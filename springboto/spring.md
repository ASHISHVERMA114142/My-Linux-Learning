dipendency injection is the implementation of ioc ....

### spring framework , solves challenges which exists with Servlets . 

# removal of web.xml 
1. this web.xml over the time become too big and becomes very difficult to manage and understand ..
2. spring framework introduced "Annotation" based onfiguration .  

# Inversion of Control ( IoC)
1. servlets dependends on the servlet container to create object and maintain lifecycle.  
2. IoC is more flexible way to manage object dependencies and its lifeCycle ( though Dependency injection) ...


# what is the differnce between controller and rest-controller ? 

Controller  - it indicate that the class is responsible for handing incoming http requests . 
``` java
@Controller
public class SampleController {
  @RequestMapping(path="/fetchUser",method = RequestMethod.GET)
  @ResponseBody
  public String getUserDetails(){
   return "fetching and returning user details";
  }
  @RequestMapping(path="/saveUser",method= RequestMethod.POST)
  @ResponseBody
  public String saveUserDetails(){
    return "sucessfully saved the user details";
 }
}
```

