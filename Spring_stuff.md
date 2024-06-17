# what is the use of service interface and service class, how they are connected to each other ?
Ans -> when we annotaed @Autowired inside our controller then spring automatically searches in its application context a candidate to be injected in the controller . A valid 
candidate should be concrete class marked as a sprin bean . using annotation @Service. 

so now why are using redudent service interface , rather then using serviceImpl directly . 
his is not specific to spring but it is a general practice in the Object Oriented Programming. Whenever we are consuming the unknown other object (in your case Service), 
we want to refer it via the contract i.e Interface.

Let's take a real life example.If you want to use the Washing Machine, You would like to use it via its Interface because it will hide the complexity involved inside and 
provide you the implementation (Abstraction).
Ex : in spring way if we write inside controller 
@Autowired 
private Service service;
it means we are indirectly saying 
Service service = new ServiceImpl();

But it is possible that we have different ServiceImpl for same service that have different implementation based on different use case. So by using service interface it becomes
easy to handle things . 
For Ex: 
let say we have 3 service  serviceA,serviceB,ServiceC  and inside our controller we just want to user ServiceC we can specify by using @Qualifier annotation . 

@Autowired
@Qualifier("ServiceC")
private Service service;

# How many types of variable in java ?
There are mainly three types of variable in java . 
## 1. Local Variable : Local variables are defined inside a method, constructor, or block. They are implemented at stack level internally. There is no default value for local variables, so local variables should be declared and an initial value should be assigned before the first use.
For Example : 
'''java
public class Test{
 void printAge(){
  int age;
  age=age+10;
  System.out.println("age : {}",age);
  }
  public static void main(String args[]){
  Test test=new Test();
  // this line will give error as there is no predefined value assigned by the java compiler at the time of variable creation so this will throw error . 
  test.printAge();
  }
}

## 2. Instance Variable : These variables are defined outside of the fucntion and it is part of class not a any methond . so if we create two object then inside heap memory section separate space will be created for both of the object variable . 

## 3. Static Variable: These variable are part of the class . it means every object will share a single copy of that object and to access this variable we don't need any object of the class. 
