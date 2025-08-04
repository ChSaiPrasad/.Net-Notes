
Gang Of Four Definition : "The Abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes"

The Abstract Factory Pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes

Abstract Factory pattern belongs to creational patterns and is one of the most used design patterns in real world applications

Abstract factory is a super factory that creates other factories

Implementation Guidelines
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/c06401cc-695a-4f9d-a22d-e69e8b781306" />

We need to Choose Abstract Factory Pattern when
1. The application need to create multiple families of objects or products
2. We need to use only one of the subset of families of objects at a given point of time
3. We want to hide the implementations of the families of products by decoupling the implementation of each of these operations

Business Requirement : Handout computers to Contract and Permanent employees based on the designation and employee type with below specifications

Permanent Employee 
1. Managerial Position is eligible for Apple MAC Book Laptop
2. Non Managerial Position is eligible for Apple IMac desktop

Contract Employee
1. Managerial Position is eligible for Dell Laptop
2. Non Managerial Position is eligible for Dell desktop

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/f6a0556f-1015-47c4-aee7-51cf5d149f35" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/6d0f4fe3-6d95-4693-8870-a43f98514899" />
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/28278b6f-8989-4d31-97f9-780e15126cce" />
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/4d4563c5-0ed1-4614-bb7b-d04410e907cd" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/b9525368-13d3-4745-8818-7a2cc384569e" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/4a0a19bc-1734-4cb1-b952-d26569c26778" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/a4eb23ac-cfa4-4cd0-937d-7ed683a758cc" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/9244167c-cd72-4ec7-915f-187e152e187b" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/b2c464cc-4547-46d5-ab6c-cfa5227147df" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/20187b7b-30b4-4520-85d5-823a17b75efb" />
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/90eaeff9-b863-4747-9699-15fd0cb1803e" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/a5383af7-118c-4ec4-b3be-39c793a4aeb1" />








Abstract Factory Representation
1. Client is a class which use AbstractFactory and AbstractProduct interfaces to create a family of related objects.
2. AbstractFactory is an interface which is used to create abstract product
3. ConcreteFactory is a class which implements the AbstractFactory interface to create concrete products.
4. AbstractProduct  is an interface which declares a type of product.
5. ConcreteProduct is a class which implements the AbstractProduct interface to create product.

Difference between Abstract Factory and Factory Method
1. Abstract factory pattern adds a layer of abstraction to the factory method pattern
2. Abstract factory pattern implementation can have multiple factory methods
3. Similar products of a factory implementation are grouped in Abstract factory
4. Abstract Factory uses object composition to decouple applications form specific implementations
5. Factory Method uses inheritance to decouple applications form specific implementations
