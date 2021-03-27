# solid-principles-for-peopleops
# SOLID Principles
Software developers have faced many fundamental problems since they started developing large-scale projects. Solutions have been put forward by bringing different approaches to each of the problems and some of them have been accepted by the software community and have formed Design Patterns and Principles that are actively used today. SOLID principles refer to recommended practices to follow for writing understandable, flexible and manageable OOP code. Just as the principles of SOLID are useful when working with OOP languages such as C # and Java, they can also provide a useful intellectual framework when working with one of the SQL languages such as PL-SQL and T-SQL as an extreme example. Solid Principles are a set of principles that are brought to the basic problems encountered in software development processes and contain 5 basic principles.
Common problems encountered in software development;
- **Stiffness:** The design used cannot be developed and added.
- **Fragility:** Changes made in one place cause problems in another.
-	**Stability:** The enhanced module is not reusable elsewhere.
-	 **Cost:** Increasing development cost and process. <br/>
Robert C. Martin, popularly known as "Uncle Bob", brought together the solutions to such problems 
and presented SOLID to the software community by gathering all the solutions to these problems under 5 main headings in the early 2000s.
Since then, it has become one of the software principles that have been accepted and used with the most attention. 
The word SOLID is formed by the combination of the initials of 5 basic titles.
 
1. **Single Responsibility:** Our classes should have a single well-defined responsibility. 
2. **Open / Closed:** Our classes are closed to change, but should be open to the addition of new behaviors.
3. **Liskov Substitution:** We should be able to use the derived classes (sub class) instead of the base class they derive from without making any changes in our code.
4. **Interface Segregation:** We should prefer to create more specialized contracts rather than a single general-purpose contract.
5. **Dependency Inversion:** In layered architectures, upper level modules should not be directly dependent on lower level modules.
 

# 1. Single Responsibility Principle (SRP)

*"Each class or method should have a single responsibility"*

Classes should only have one role and / or responsibility. A class should only deal with one operation.
For example; The user class should contain only the basic features of the user. Adding the user class to the database is the task of a separate class. For this it must be the UserDb class. The user's database operations belong to this class. In addition, after the user is added to the db or the moment of adding, all log operations that write errors that may occur should be kept in the LogDb class. The User class must be used for a single task. It may also be the retention of the User's information.
 
The interface below does more than one thing. Searching and ending the call is a separate job. Receiving and sending data is a separate job. These two jobs must be separated.
 
```
interface Modem 
{
  public void dial(String pno);
  public void hangup();

  public void send(char c);
  public char recv();
}
```
Must be;
```
 interface DataChannel{
  public void send(char c);
  public char recv();
}

interface Connection 
{
  public void dial(String phn);
  public char hangup();
}
```

# 2. Open / Closed Principle (OCP)

*"Classes should be closed to change, but open to development."*

Classes must be expandable but not modifiable. To elaborate, a new field should be able to be added to a class. But existing fields should not be changed. There may be other classes using existing fields. The change made can affect other classes.
For example; Let there be a field where we keep the user type belonging to the user class. You want to determine the password rules according to this user type. If the user registered by mail, use an 8 character password rule. If registered with Facebook or Google, no password rule. Let the GetPasswordRule method take shape according to this user type. However, we may ask users who have commented anonymously in the future to be registered in the database. This type of users will not need a password, either. For this reason, it is necessary to add anonymous users to the if in the GetPasswordRule method where Facebook google users are located. This will cause us to make changes in our current method. We cannot predict what consequences these changes will have in the future. For this reason, changing a class or method can cause undesirable results. The solution is to create a separate class for each user type. These user types use the User class as base class. Since each user type has a separate class, the GetPasswordRule method can be set separately for these users.
 
Let's examine a separate example software.
In the example below, transactions have been made according to the account type. These transactions are managed with if-else conditions. However, in every new incoming account type, a new line must be added to the if else block. This breaks the condition of being closed to change.

```
    Public class SavingAccount  
    {  
        //Other method and property and code  
        Public decimal CalculateInterest(AccountType accountType)  
        {  
            If(AccountType==”Regular”)  
            {  
                //Calculate interest for regular saving account based on rules and   
                // regulation of bank  
                Interest = balance * 0.4;  
                If(balance < 1000) interest -= balance * 0.2;  
                If(balance < 50000) interest += amount * 0.4;  
            }  
            else if(AccountType==”Salary”)  
            {  
                //Calculate interest for saving account based on rules and regulation of   
                //bank  
                Interest = balance * 0.5;  
            }  
        }  
    }
```

As a solution To examine the example below, 2 new classes were created. These are the Regular and Salary Saving Acccount classes.
If a new account type arrives, a new class of this type will be created instead of editing the if else block. These classes are derived from the ISavingAccount interface. There is a CalculateInterest method common to account types in the ISavingAccount interface. Thanks to this method, each class can make its own calculations. This way, no existing classes will be changed. Here it could be used in abstract class instead of interface.

```
    Interface ISavingAccount  
    {  
       //Other method and property and code  
       decimal CalculateInterest();  
    }  
    Public Class RegularSavingAccount : ISavingAccount  
    {  
      //Other method and property and code related to Regular Saving account  
      Public decimal CalculateInterest()  
      {  
        //Calculate interest for regular saving account based on rules and   
        // regulation of bank  
        Interest = balance * 0.4;  
        If(balance < 1000) interest -= balance * 0.2;  
        If(balance < 50000) interest += amount * 0.4;  
      }  
    }  
      
    Public Class SalarySavingAccount : ISavingAccount  
    {  
      //Other method and property and code related to Salary Saving account`  
      Public decimal CalculateInterest()  
      {  
        //Calculate interest for saving account based on rules and regulation of   
        //bank  
        Interest = balance * 0.5;  
      }  
    }
```


# 3. Liskov's Substitution Principle (LSP) (Substitution Principle)

*“Let all programs (functions) that take parameters in terms of T be P, be an o1 object of S type and an o2 object of T type. If the behavior of P does not change when o1 and o2 objects are replaced, type S is a subtype of type T! "*

Derived classes should be replaced with their base classes. Expecting subclass objects to behave the same when they replace objects of the parent. Objects derived from a base class are expected to have all properties belonging to this base class.
As an example, let's create the Bird class. Let this be our Fly () method belonging to the bird class. The Eagle class derived from this bird class can use the Fly () method without difficulty. But when we create the Penguin class, penguins should not be derived from the Bird class, as they cannot fly. Instead, a separate base class should be created for birds that can fly. This base class can also be derived from the Bird class. In this way, we will have two base classes. To be for birds and birds that can fly. The eagle will use flying birds as its base class, while the penguin will use only the birds base class. What should not be forgotten here is the base class of birds that can fly.
 
 ```
 public interface IDuck
{
   void Swim();
   // contract says that IsSwimming should be true if Swim has been called.
   bool IsSwimming { get; }
}
public class OrganicDuck : IDuck
{
   public void Swim()
   {
      //do something to swim
   }

   bool IsSwimming { get { /* return if the duck is swimming */ } }
}
public class ElectricDuck : IDuck
{
   bool _isSwimming;

   public void Swim()
   {
      if (!IsTurnedOn)
        return;

      _isSwimming = true;
      //swim logic  

   }

   bool IsSwimming { get { return _isSwimming; } }
}
 ```
 
If we examine the example below, As you can see, there are two examples of ducks. One organic duck and one electric duck. The electric duck can only swim when it's open. For this, the IsSwimming method should be removed from the interface and a new interface should be created. Get the new interface ISwimDuck with IsSwimming. The organic duck should be derived through this interface. The electric duck should be derived from this IDuck interface.

# 4. Interface Segregation Principle (Specialized Contract / Interface Separation Principle)

*"Principle of separation of interfaces"*

Thanks to this principle, large interfaces are divided into smaller parts. Lower-level classes should only use methods that work for them. If they are derived from large interfaces, they will have to use many processes that they do not need and will not use.
In short, this principle states that interfaces should be kept as small as possible.
 
The interface below does multiple jobs. Classes derived from this interface will have to use all methods. Instead, these interfaces should be divided into smaller business units.
 
 ```
  interface Worker {
  void work(); 
  void eat();
}
 ```
 
Small fragmented interfaces are more easily added to classes. In this way, classes derived from these interfaces will not receive methods they do not use.
 
 
 ```
 interface Workable {
  public void work();
}

interface Feedable{
  public void eat();
}
 ```
 

# 5. Dependency Inversion Principle

*"In layered architectures, upper level modules should not be directly dependent on lower level modules."*

All dependencies should be carried out through abstractions. These abstractions can be created with abstarct methods or interfaces. The aim here is to eliminate the problems that may arise due to the dependence of upper level modules on lower levels. In other words, the aim is to prevent any changes made at the lower level from the upper level code change or its loyalties.
 

For example, if we have a user and two classes that register this user in db, named People and PeopleDb, since these two classes are linked to each other, we will only need to operate with the People class that meets certain conditions. But if we want to register the Admin to db, we will not be able to do this because the PeopleDb class is connected to the People class. Instead, we can abstract these dependencies by deriving the People class from an interface. Thus, our Employee and Admin classes can be processed with the PeopleDb class via this interface.
 
 ```
 Interface IPerson { 
 string FirstName { get; set; }
 string LastName { get; set; }
 int EmployeeId { get; set; }
}

public class Employee : IPerson {
  int EmployeeId;
}

public class Admin : IPerson {
  int AdminId;
}

public class PersistPeople {
   IPerson person;
   // Constructor
   PersistPeople(IPerson person) {
      this.person = person;
   }
   public void SavePerson() {
      person.Save();
   }    
}
 ```
 
 ```
 public class HomeController : Controller { 

   // If we want to save an employee we can use Persist class:
   Employee empl = new Employee();
   empl.FirstName = "John";
   empl.LastName = "Doe";
   empl.EmployeeId = 247854;
   PersistPeople persist = new Persist(empl);
   persist.SavePerson();

   // Or if we want to save an admin we can use Persist class:
   Admin admin = new Admin();
   admin.FirstName = "David";
   admin.LastName = "Borax";
   admin.EmployeeId = 999888;
   PersistPeople persist = new Persist(admin);
   persist.SavePerson();
   } 
}
 ```
 
