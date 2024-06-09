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
