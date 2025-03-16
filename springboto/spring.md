dipendency injection is the implementation of ioc ....

### spring framework , solves challenges which exists with Servlets . 

# removal of web.xml 
1. this web.xml over the time become too big and becomes very difficult to manage and understand ..
2. spring framework introduced "Annotation" based onfiguration .  

# Inversion of Control ( IoC)
1. servlets dependends on the servlet container to create object and maintain lifecycle.  
2. IoC is more flexible way to manage object dependencies and its lifeCycle ( though Dependency injection) ...


# what is the differnce between controller and rest-controller ? 

RestController = Controller + ResponseBody 
ResponseBody => denotes the return value of the controller method should be serialized to HTTP response body . 
if we do not provide ResponseBody , spring will conside response as name for the view and tries to resolve and render it ( in case we are using the @Controller annotation ) 

RequestMapping - 
 1. value , path (both are same )
 2. Method
 3. consumes , produces
 4. @Mapping
 5. @Reflective ( {ControllerMappingReflectiveProcessor.class})
Example - @RequestMapping ( path="/fetchUser" , method=RequestMapping.GET , consumes= "application/json",produces="application/json")

Controller  - it indicate that the class is responsible for handing incoming http requests . 
Here @ResponseBody tells that it is the http response not the name of the some view that is rendered by the UI 
for example Hello.jsp

RequestParam - used to bind , request paramerer to controller method parameter  . 
The Framework automitacilly performs type conversion from the request parameter's string representation to the specified type. 
1. primitive types - such as int long float double boolean etc.
2. wrapper class - sush as Interger , Long , Float , Double , Boolean etc.
3. String - Request param as inherently treated as string only .
4. Enums - you can bind request param to enum types .
5. custome object type - we can do it using registered propertyeditor.

   How to use PropertyEditor -

``` java
@RestController
@RequestMapping( path= "/api/")
public class SampleController {
  @InitBinder
  protected void initBinder(DataBinder binder){
    binder.registerCustomerEditor( String.class , "firstName", new FirstNamePropertyEditor());
                                   return type  |  RequestParam | class name that will do that work 
  } 
  @GetMapping ( path="/fetchUser")
  public String getUserDetails(@RequestParam ( name="firstName") String firstName ,
                                @RequestParam ( name ="lastName", required ="false)  String last_name) ) {
   return "fetching and returning user details";
  }
}
public class FirstNamePropertyEditor extends PropertyEditorSupport {
  @Override
  public void setAsText ( String text ){
    setValue(text.trim().toLowerCase());
} 
```
PathVariable - used to extract values from the path of the URL and help to bind it to controller method parameter . 

``` java
@RestController
@RequestMapping(value="/api/")
public class SampleController { 
   @GetMapping(path="/fetchUser/{firstName}")
   public String getUserDetails(@PathVariable ( value="firstName" ) String firstName ) {
        return "fetching and returning user details based on first name " ;
   }
} 
```
@RequestBody  - bind the body of HTTP request ( typically JSON ) to controller method parameter ( java objcet ) 
``` java
@RestController
@RequestMapping(value="/api/")
public class SampleController { 
   @GetMapping(path="/fetchUser/")
   public String getUserDetails(@RequestBody User user ) {
        return "fetching and returning user details based on first name " ;
   }
} 
```
ResponseEntity - it represent the entire HTTP response 
Header , status , response body etc . 
``` java 
@RestController
@RequestMapping(value="/api/")
public class SampleController { 
   @GetMapping(path="/fetchUser/")
   public String getUserDetails(@RequestBody User user ) {
        return ResponseBody.status(HttpStatus.Ok).body(user) ;
   }
} 



Q. why ResponseBody is not required in the RestController . 
ans - RestController is nothing but controller with response body so it mean it has the inbuild implementation that if the class is treated as a restcontroller then all the mapping will return the responseBody . 
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

===========================
@RestController
@RequestMapping( path= "/api/")
public class SampleController {
  @GetMapping ( path="/fetchUser")
  public String getUserDetails(){
   return "fetching and returning user details";
  }
  @PostMapping(path="/saveUser")
  public String saveUserDetails(){
    return "sucessfully saved the user details";
 }
}

================================
@RestController
@RequestMapping( path= "/api/")
public class SampleController {
  @GetMapping ( path="/fetchUser")
  public String getUserDetails(@RequestParam ( name="firstName") String firstName ,
                                @RequestParam ( name ="lastName", required ="false)  String last_name) ) {
   return "fetching and returning user details";
  }
  @PostMapping(path="/saveUser")
  public String saveUserDetails(){
    return "sucessfully saved the user details";
 }
}
```

