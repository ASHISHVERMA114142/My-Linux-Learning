### one to many mapping ( uni-directional )

    - One entity is associated with the multiple records of the another entity . 
    - by default lazy loading 
    - one parent have multiple child entity in one to many relationship 
    - if we don't provide the owning side and inverse side then it will create new table having primary key of both parent and child 

Basic example

```java
import com.HotelService.HotelService.JpaMapping.Model.UserDetails;
import jakarta.persistence.*;

import java.util.ArrayList;

@Entity
@Table("user_details")
public class UserDetals {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;
    private String name;
    private Integer age;

    @OneToMany(cascade = CascadeType.ALL)
    private List<UserDetails> userDetailsList = new ArrayList<>();
}

@Entity
@Table
public class UserAddress {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private Integer pinCode;
}

```
so if we look on the above code , it will create new table with two column that will be the primary key of both the Entity ( userId and Id). 

what if we don't want to create the new table in case of one to many ?
ans  - so we can use @JoinColumn , this also tells jpa that we want to store the FK into the child table not on the parent table 
Below is the example for this - 

```java
import com.HotelService.HotelService.JpaMapping.Model.UserDetails;
import jakarta.persistence.*;

import java.util.ArrayList;

@Entity
@Table("user_details")
public class UserDetals {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;
    private String name;
    private Integer age;

    @OneToMany(cascade = CascadeType.ALL)
    @JoinColumn(name = "user_details_fk" , referencedColumnName = "userId")  // here we have added new thing that will tell that we want to add the userId that is from the userDetails table into the userAddress Table .
    private List<UserDetails> userDetailsList = new ArrayList<>();
}

@Entity
@Table
public class UserAddress {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private Integer pinCode;
}

```

what is meaning of orphan removal ? 
ans - in one to many relateion we usually have list of entity ( basically the child entity ) on the parent side , so this collection will have FK set as the parent PK , but in case if we remove the 
        entry from this collection then it will automatically removed from the DB , but we have to enable one value from the mapping annotation "orphanRemoval = true" as it is set to false by default 

```java
import com.HotelService.HotelService.JpaMapping.Model.UserDetails;
import jakarta.persistence.*;

import java.util.ArrayList;

@Entity
@Table("user_details")
public class UserDetals {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;
    private String name;
    private Integer age;

    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true) // here we are setting orphan removal to true so that once the collectiion entity will remove then the DB will also update ....
    @JoinColumn(name = "user_details_fk" , referencedColumnName = "userId")  
    private List<UserDetails> userDetailsList = new ArrayList<>();
}

@Entity
@Table
public class UserAddress {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private Integer pinCode;
}
```

=========================================== one to many bidirectional ==================================================

what is meaning of bidirectional 
  - each parent will have reference to child 
  - each child will have reference to parent 


so which one will be the inverse side and which one the owing side 

1. inverser side - ( parent ) 
        - no FK is created in table 
        - only hold the reference of owing side 

2. owing side - ( child )
       - hold the FK in the table 

example :

```java
import com.HotelService.HotelService.JpaMapping.Model.UserDetails;
import com.fasterxml.jackson.annotation.JsonIdentityInfo;
import com.fasterxml.jackson.annotation.ObjectIdGenerator;
import com.fasterxml.jackson.annotation.ObjectIdGenerators;
import jakarta.persistence.*;

import java.util.ArrayList;

@Entity
@Table("user_details")
@JsonIdentityInfo(
        generator = ObjectIdGenerators.PropertyGenerator.class,
        property = "userId"
)
public class UserDetals {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;
    private String name;
    private Integer age;

    @OneToMany(mappedBy = "userDetails", cascade = CascadeType.ALL, orphanRemoval = true)
    // here we are setting orphan removal to true so that once the collectiion entity will remove then the DB will also update ....
    private List<UserDetails> userDetailsList = new ArrayList<>();
}

@Entity
@Table
@JsonIdentityInfo(
        generator = ObjectIdGenerators.PropertyGenerator.class,
        property = "id"
)
public class UserAddress {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private Integer pinCode;
    
    @ManyToOne
    @JoinColumn(name =  "userDetals_FK", referencedColumnName = "userId")
    private UserDetails userDetails;
}
```


=================================== Many to one Unidirectional ===================
  - many order can be placed by 1 user 
  - so still user is considered as parent and child is considered as child 
  - parent don't have reference to child 
  - each child will have reference to parent

```java

import jakarta.persistence.*;

@Entity
@Table("user_details")
public class UserDetails {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;
    private String name;
    private Integer age;
}

@Entity
@Table(name = "userAddress")
public class UserAddress{
    @Id
    @GeneratedValue(generator = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private String pinCode;
    
    @ManyToOne
    @JoinColumn(name = "user_id_FK" , referencedColumnName = "userId")
    private UserDetails userDetails
}
```


============================= Many to many unidirectional =====================================
  - reference will have only in one direction 

```java
import jakarta.persistence.*;

@Entity
@Table("user_details")
public class UserDetails {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long userId;
    private String name;
    private Integer age;
    @ManyToMany(cascade =  CascadeType.ALL)
    @JoinTable{
        name = "user_details_user_address",
        joinColumns = @JoinColumn(name = "userId"), 
        inverseJoinColumn= @JoinColumn(name = "id")
    }
    private UserAddress userAddress;
}

@Entity
@Table(name = "userAddress")
public class UserAddress{
    @Id
    @GeneratedValue(generator = GenerationType.IDENTITY)
    private Long id;
    private String address;
    private String pinCode;
}
```

