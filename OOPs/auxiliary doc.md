- By default, members are private in a class.
- Class: user-defined datatype, blueprint for creating objects
- Object: It is an entity that has a state and a behavior, anything that exists in the physical world
- Size of empty class : 1 byte
- Reasons for 1 Byte Size : Object Identity, Memory Alignment
- Padding: Extra space added to the structure to align data in memory, done to ensure that the data is stored in a way that is efficient for the computer to access and manipulate.


- in case of inheritance, when we have a derived class that is inherited from a base class, and then, we have some properties for the derived class which are not present in the base class, and we have declared parameterized constructors for both, if we don't give in the correct parameters(i.e., we use some properties from the base class and some from the derived for our object), and we have the right parameterized constructor for that in our derived class but for the properties belonging to the base class, they need to be initialized in the base class's constructor, but as there is a parameterized constructor there(doesn't take only those parameters that were used in the object's initialization), there is no default constructor there and so we need to define one, or else error will be thrown.
	- Derived Class : ![[Pasted image 20241009230858.png]]
	- Base Class : ![[Pasted image 20241009230925.png]] 
	- New Base Class(with a default constructor):  ![[Pasted image 20241009231534.png]]

