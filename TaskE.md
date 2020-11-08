# CS3219 Task E: Object Interaction Design Patterns

This article is written for CS3219 Individual assignment Task E to explain other Object Interaction Design Patterns not explained in lecture.

Reference is made from [Refactoring.Guru](https://refactoring.guru/refactoring) and [TutorialsPoint](https://www.tutorialspoint.com/design_pattern) and may include examples found in these websites.

I will cover one design pattern from each of the **Creational** and  **Structural ** pattern since only two patterns are covered in lecture.



## Creational Design Pattern: Factory Method

The **Factory Method** is a Creational Design Pattern that is also known as *Virtual Constructor*.

It provides an interface for the creation of objects in a superclass, but provides flexibility for derived classes to change the type of objects created.

Imagine an application is built for a logistics management application. The first version of the application can only handle land transportation and so a majority of code lives in `Truck` class.

As the application becomes more popular and application become more widely used, more requests for other categories such as air logistics are brought up by users.\.


Adding `Airplane` to the application would require making changes to the codebase since most of the code is coupled to `Truck` class. Furthermore, if another form of transport is to be added into the logistics management application such as sea logistics, it would require another class `Ships` which will result in change to codebase again. 



### Solution: Factory Method

This design pattern advices replacing direct object construction calls with calls to a special *factory* method. Objects returned by a factory method are often referred to as *products*



![image-20201108155515570](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201108155515570.png)

The subclass can override the factory method and change the class of products being created y the method `makeTransport()`

The subclasses may return different types of products only if these products are derived from a common base class/interface. The factory method in the base class should have its return type declared in the interface. 

![image-20201108160148409](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201108160148409.png)

In the above example, `Truck` and `Airplane` implement the `Transport` interface which declares a method called `deliver`. `Truck` and `Airplane` implement this method differently, with `Truck` delivering items by land and airplanes delivering items by air. The factory method in `RoadLogistics` class return `Truck` objects whereas `AirLogistics` returns `Airplane` objects using the factory method. 

The *Client* code which uses the *factory* method will not be able to tell the difference between actual products returned by various subclasses. The client treats all the products as abstract `Transport` and know that they have a `deliver` method but is not concerned with how it exactly works.



### General Structure

![image-20201108161412969](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201108161412969.png)

1. The **Product** declares interface that is common to all objects which can be produced by creator and its subclasses
2. **Concrete Products** represents different implementations of the product interface
3. The factory method is declared in the **Creator** class which returns new product objects and the return type must be that of the Product interface
   The factory method can be declared as abstract to make subclasses implement the method on their own. 
4. **Concrete Creators** will implement their own factory method to return different types of products. In the example above, `ConcreteCreator1` has a factory method called `createProduct()` which returns an object of class `ConcreteProduct1`

### Pros vs Cons

| Pros                                                         | Cons                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| There is **no tight coupling** between the creator and the concrete products | Code will become **complicated** since a large number of new subclasses have to be introduced to implement the patterns. |
| **Single Responsibility Principle** is ensured and the product creation code is placed in one part of the program. | -                                                            |
| **Open/Closed principle** is ensured and new types of products can be introduced into the program without breaking the client code | -                                                            |



## Structural Design Pattern: Proxy

**Proxy** is a structural design pattern which allows a substitute/placeholder for another object. It controls access to the original object, allowing one to perform something either before or after the request gets through to the original object. 

The Proxy pattern advices one to create a new proxy class with the same interface as the original service object. Proxy objects are passed to all of the original object's clients and upon receiving request from client, the proxy creates a real service object and delegates all the work to it. 

An example would be using a credit card as a proxy for bank account which in turn is a proxy for cash. Both of them would implement the same interface in which they are used for making payment. 

![image-20201108162854899](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201108162854899.png)

### General Structure

![image-20201108163221270](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201108163221270.png)

1. The **ServiceInterface** declares interface of the Service. The Proxy follows this interface to disguise itself as a service object
2. The **Service** class provides some business Logic
3. **Proxy** class has a reference field that points to a service object. After the proxy finishes processing, it passes the request to the service object
4. The **Client** should work with both services and proxies through the same interface which will allow one to pass a proxy into any code that expects a service object.

### Pros vs Cons

| Pros                                                         | Cons                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Clients do not have to know about controlling the service object | Code will become **complicated** as new classes are introduced |
| The lifecycle of the service object can be managed without the clients caring about it | Response from service might get delayed                      |
| Proxy works even if service object is not ready              | -                                                            |
| **Open/Closed Principle** is ensured. New proxies can be introduced without changing the service or clients | -                                                            |