# SOLIDSoftwareNotes

SOLID principles are the foundation on which we can build clean, maintainable architectures.

Problems when SOLIDS are not used
-

Code Fragility
- Fragility is the tendency of the software to break in many places every time it is changed.

Code Rigidity
- Rigidity is the tendency for software to be difficult to change, even in simple ways.
  Every change cause a cascade of subsequent changes in dependent modules.


Fragility and rigidity are symptoms of high technical debt.


Technical Debt
-
The cost of prioritizing fast delivery over code quality for long periods of time.

The Choices you have to Make:
- Fast delivery 
  - Easiest fix/change
  - Fast
  - Poor written code
- Code quality
  - Takes more time
  - Adds a bit more of complexity
  - Maintainable

Facts:
- No matter how good the team is, technical debt will accumulate over time
- Left uncontrolled, it will kill your project
- The key is to keep it under control

Controlling Technical Debt
- Write code
- Pay debt (refactor)
- Write more code
- Pay debt (refactor)


SOLID
-

SOLID Principles:
- Acronym for 5 software design principles that help us to keep technical debt under control.
  - Single Responsibility Principle
  - Open Closed Principle
  - Liskov Substitution Principle
  - Interface Segregation Principle
  - Dependency Inversion Principle

Top Benefits:
- Easy to understand and reason about
- Changes are faster and have minimal risk level
- Highly maintainable over long periods of time
- Cost effective

Other ways to keep it clean:
- Constant refactoring
- Design patterns
- Unit Testing (TDD)


Globomantics HR

- Employee management
- Tax calculation
- Pay slip generation
- Reporting


Single Responsibility Principle
-
Every function, class or module should have one and only one reason to change

Example of Responsibilities:
- Business logic
- User Interface
- Persistence
- Logging
- Orchestration
- Users

Always identify the reasons to change that your components have and reduce them to a single one

- It makes code easier to understand, fix, and maintain
- Classes are less coupled and mor resilient to change
- More testable design

Indentify Multiple Reasons to Change

If Statements
        
        if(employee.getMonthlyIncome() > 2000) {
            // some logic here
        } else {
            // some other logic here
        }

Switch Statements

        switch(employee.getNbHoursPerWork()){
            case 40: {
                //logic for full time
            }
            case 20: {
                //logics for part time
            }
        }

Monster Method

        Income getIncome(Employee e) {
            Income income = employeeRepository.getIncome(e.id);
        StateAuthorityApi.send(income, e.fullName);
        Payslip payslip = PayslipGenerator.get(income);
        JsonObject payslipJson = convertToJson(payslip);
        EmailService.send(e.email, payslipJson);

        ...
        
        return income;
    }

God Class
        
        class Utils{
            void saveToDb(Object o){...}
            void convertToJson(Object o){...}
            byte[] serialize(Object 0){...}
            void log(String msg){...}
            String toFriendlyDate(LocalDateTime date){...}
            int roundDoubleToInt(double val){...}
        }

People 

        Report generate(){
            // method used by HR and Management actors
            // each one will want different features at some point
            // in time

SRP Example

        class ConsoleLogger{
            void logInfo(String msg){
                System.out.println(msg);
        }

        void logError(String msg, Exception e){...}
    }


Symptoms of Not Using SRP

- Code is more difficult to read and reason about
- Decreased quality due to testing difficulty
- Side effects
- High coupling

Coupling
- The level of inter-dependency between various software components

Example
    
        Income getIncome(Employee e){
            RepositroyImpl repo = new RepositoryImpl(srv, port, db);

        Income income = repo.getIncome(e.id);

        return income;
    }


        Income getIncome(Employee e, Repository repo){
            Income income = repo.getIncome(e.id);
        
        
        return income;
        
    }

If Module A knows too much about Module B, changes to the internals of Module B ,ay break functionality in Module A.


Open Closed Principle
-
Classes, functions, and modules should be closed for modification, but open for extension

- Closed for modification
  - Each new feature should not modify existing source code
- Open for extension
  - A component should be extendable to make it behave in new ways

Modifying Existing Code is a risk
- High risk of breaking 
- Can have ripple effects

Why you should apply the OCP

- New features can be added easily and with minimal cost
- Minimizes the risk of regression bugs
- Enforces decoupling by isolating changes in specific components, works along with the SRP

SOLID principles are most effective when applied together

Progressively Applying the OCP

- Start small
  - Make changes inline
  - Bug fixes can be implemented this way
- More changes
  - consider inheritance
- Many changes/dynamic decision
  - Consider interfaces and design patterns like strategy


Applying OCP for Frameworks and API's
-

API
- A contract/agreement between different software components on how they should work together

A public framework or API is under your control. But clients might use it in ways that you aren't aware of

Exposed Framework/SDK
- Changes can break the clients implementations

        public class TaxCalculator {
            public double calculate(Employee e String currency) {
                // business logic
            }
        }

Make Framework/SDK Open for Modification
            
        public interface AbstractTaxCalculator{
            public double calculate(Employee e)
        }
        
        // implemented by customer

        class CustomerUSACalc implements AbstractTaxCalculator {
            public double calculate(Employee e){
                // business logic
            }
        }

Best Practices for Changing API's

- Do not change existing public contracts: data classes, signatures
- Expose abstractions to your customers and let them add new features on top of your framework
- If a breaking change is inevitable, give your clients time to adapt


Liskov Substitute Principle
-
- If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S
without modifying the functionality of the program
- Any object of a type must be substitutable by objects of a derived typed without altering the correctness
of that program

Relationships

- Is a
- Is substitutable by
  - Is the class rectangle fully substitutable by the class square

Incorrect relationships between types cause unexpected bugs or side effects


Violations

Empty Methods/Functions

        class Ostrich extends Bird{
            @Override public void fly(int altitude){
                //Do nothing; An ostrich can't fly
        }
    }
    
        Bird ostrich = new Ostrich();
        ostrich.fly(1000);

Harden Preconditions

        class Rectangle {
            public void setHeight(int height){}
            public void setWidth(int width(){}

            public int calculateArea(){
                return this.width * this.height;
            }
        }

        class Square extends Rectangle {
            public void setHeight(int height){
                //Width must be equal to height
            this.height = height;
            this.width = height;
        }

        public void setWidth(int width){} // Same logic w = h
    }

        Rectangle r = new Square();
        r.setWidth(10);
        r.setHeight(20);

        r.calculateArea(); // Will return 400


Partial Implemented Interfaces

        interface Account{
            void processLocalTranser(double amount);
            void processInternationalTransfer(double amount);
        }

        class SchoolAccount implements Account
            void processLocalTransfer(double amount){
                // Business logic here
        }
        void processInternationalTransfer(double amount){
            throw new RuntimeException("Not Implemented");
        }
    }

        Account account = new SchoolAccount();
        account.processInternationalTransfer(10000); // will crash


Type Checking

        for (Task t: tasks) {
            if(t instanceof BugFix){
                    BugFix bf = (BugFix)t;
                    bf.intitializeBugDescription();
        }
        t.setInProgress();
        }

Fixing Incorrect Relationships

Two Ways to Refactor Code to LSP

- Eliminate incorrect relations between objects
- Use "Tell, don't ask!" principle to eliminate type checking and casting


Empty Methods/Functions

    class Ostrich extends Bird{
        @Override public void fly(int altitude){
                // Do nothing; An ostrich can't fly
        }
    }

    class Bird {
        // Bird data and capabilities
        public void fly (int altitude){...}
    }

    class Ostrich {
        // Ostrich data and capabilities. No fly method
    }

Partial Implemented Interfaces

    class SchoolAccount implements Account {
        void processLocalTransfer(double amount){
            // Business logic here
    }

    void processInternationalTransfer(double amount){
        throw new RuntimeException("Not Implemented");
        }
    }

Fixed Partial Implemented Interfaces

    class SchoolAccount implements LocalAccount{
        void processLocalTransfer(double amount){
            // Business logic here
        }
    }

Type Checking

    for (Task t : tasks){
        if(t instanceof BugFix){
            BugFix bf = (BugFix)t;
            bf.initialize BugDescription();
    }
    t.serInProgress();
    }

    class BugFix extends Task{
        @Ovveride
        public void setInProgress(){
            this.initializeBugDescription();
            super.setInProgress();
        }
    }

Tell, Don't Ask

    for (Task t : tasks){
        //Task should be replaceable by BugFix
        t.setInProgress();
    }

Apply the LSP in a Proactive Way

- Make sure that a derived type can substitute its base type completely
- Keep base classes small and focused
- Keep interfaces lean

Real life categories do not always map to OOP relationships


Interface Segregation Principle
-
Clients should not be forced to depend on methods that they do not use

The "interface" word in the principle name does not strictly mean a Java reference

The ISP Reinforces Other Principle
- By keeping interfaces small, the classes that implement them have a higher chance to fully substitute the interface
- Classes that implement small Interfaces are more focused and tend to have a single purpose

Benefits of Applying the ISP
- Lean interfaces minimize dependencies on unused members and reduce code coupling
- Code becomes more cohesive and focused
- It reinforces the use of the SRP and LSP


"Fat" Interfaces

interfaces with Many Methods
Throwing Exceptions
Increased Coupling
Not just about Interfaces
Empty Methods 

Fixing Interface Pollution

- Your own code 
  - Breaking interfaces is pretty easy and safe due to the possiblity to implement as
    many interfaces as we want
- External legacy code
  - You can't control the interfaces in the external code, so you use design patterns like "Adapter"


Decoupling Inversion Principle
-

- High level modules should not depend on low level module;
    both should depend on abstractions
- Abstractions should not depend on details. Details should depend upon abstraction.

High Level Modules
- Modules written to solve real problems amd use cases
- They are more abstract and map to the business domain
- What the software do

Low Level Modules
- Contain implementation details that are required to execute the business policies
- The are considered the "plumbing" or "internals" of an application
- How the software should do various task
- examples:
  - logging
  - newtwork communication
  - data access
  - IO