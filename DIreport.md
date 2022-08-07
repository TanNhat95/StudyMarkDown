# Dependency injection

>**Dependency Injection** is a form *design pattern designed* to **prevent** dependencies between classes,
to make the code easier to understand, for the purpose of maintaining and upgrading the code.

>In projects with a high degree of complexity except for functional design of the application, organizational code always comes first. Organization makes programming easy to maintain, as well as code that expands later.

When class A uses some functions of class B, it can be said that class A has a **dependency** with class B.

*In java*, before we can use methods of another class, we have to instantiate an object of that class
(or A needs to create an instance of B). So we can understand, **handing the task of initializing that object to someone else and directly using those dependencies** is called dependency injection.

I personally understand it like this :
- the man representing the **class**
- employee's grocery store representing the **DI**

The man go to grocery store and he wants to buy **Object A & B** .
Employee's grocery store (DI) : 
- Creating Object A & B and give him .

We usually only encounter the following **three types** of *Dependency Injection*:
- **Constructor injection**: dependency *is provided* via **constructor**
- **Setter injection**: dependency will be *passed* to a class through **setter methods**
- **Interface injection**: dependency *is provided* via **interface**, which contains a function named **Inject**. **Clients** must *implement* an **Interface** that has a <span style="color:red">setter method</span> for getting the *dependency* and **passing** it to the class through calling the *Interface's Inject function*.

#### So the specific task of Dependency Injection is: ####
- Create objects.
- Know which classes need those objects.
- Give those classes the objects they need.

*Class* will not directly depend on each other, but instead they will link to each other *through* an **Interface**

*The instantiati of class* will be managed by the **Interface** because the class that depends on it .

We have class **Car**, which contains some other objects like **Wheel**, or **Battery**

```js
class Car{
  private Wheels wheel = new MRFWheels();
  private Battery battery = new ExcideBattery();
  ...
  ...
}
```

class Car is *responsible for initializing all dependency objects*. But what if we want to get rid of **MRFWheels** and replace it with **BMWWheels**.

Now we **have to recreate** the new car object with the new dependency **BMWWheels**???. And if you add something else, you have to recreate the object?
**DI** prevent the above dependency
**DI** is a man-in-the-middle responsible for creating different types of **wheels** and then providing them to the class **Car**. It make **Car** not dependent on **Wheels** & **Battery**.

```js
class Car{
  private Wheels wheel;
  private Battery battery;
  
  /*
    2 ways implement dependency injection
        1.  constructor
        2.  Setter method
  */
  
  // constructor
  Car(Wheel wh, Battery bt) {
    this.wh = wh;
    this.bt = bt;
  }
  
  // Setter method
  void setWheel(Batter bt){
    this.bt = bt;
  }
  ...  
  ...
}
```

### Benefit ###
- Easy to test and write Unit Tes
- Easily see relationships between objects
- Easier to extend applications or features
- Reduces the dependent between components, avoiding too much influence when there is a change.

### Disadvantage ###
- It's quite complicated to learn
- The biggest difficulty is that when newcomers to DI do not understand clearly, the DI making process is still confused and the injectors are bound without completely escaping according to DI's thought.