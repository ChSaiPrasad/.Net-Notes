

#Design Patterns

Singleton Patterns
------------------
Singleton Pattern belongs to Creational type pattern. Gang of four have defined five design patterns that belongs to creational design type category. 
Singleton is one among them and the rest are Factory, Abstract Factory, Builder and Prototype patterns. As the name implies, creational design type deals with object creation mechanisms. 
Basically, to simplify this, creational pattern explain us the creation of objects in a manner suitable to a given situation. 
Singleton design pattern is used when we need to ensure that only one object of a particular class is Instantiated. That single instance created is responsible to coordinate actions 
across the application. 

Advantages and Guidelines for Singleton implementation.
Concurrent access to the resource is well managed by singleton design pattern.
As part of the Implementation guidelines we need to ensure that only one instance of the class exists by declaring all constructors of the class to be private. 
Also, to control the singleton access we need to provide a static property that returns a single instance of the object.


Singleton Class Implementation Example 
<pre> 
Program.cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// First version of Singleton demo
namespace SingletonDemo
{
    class Program
    {
        static void Main(string[] args)
        {
         
            Singleton fromEmployee = Singleton.GetInstance;
            fromEmployee.PrintDetails("From Employee");
 
            Singleton fromStudent = Singleton.GetInstance;
            fromStudent.PrintDetails("From Student");

            Console.ReadLine();
        }
    }
}
 </pre>

 <pre>
Singleton.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace SingletonDemo
{
    
    public sealed class Singleton
    {
        private static int counter = 0;
        private static Singleton instance = null;
    
        public static Singleton GetInstance
        {
            get
            {
                if (instance == null)
                    instance = new Singleton();
                return instance;
            }
        }
        
        private Singleton()
        {
            counter++;
            Console.WriteLine("Counter Value " + counter.ToString());
        }
       
        public void PrintDetails(string message)
        {
            Console.WriteLine(message);
        }
    }

     </pre>



This pattern is not suited for multithread conditions

 
In our previous video we discussed 
Why we need to create a singleton class and how we can apply singleton design pattern to a class
We have changed the class to restrict external instantiation by changing the public constructor to private and then we provided a public 
property to access this class 
We have proved that adding Private constructor will prevent us instantiating a new class
We have further sealed down this class to avoid any inheritance
 
You might be wondering why we need to seal the class when a private constructor is present. 

Let’s first remove the sealed keyword and check that. Let’s create another class called DerivedSingleton and Inherit the singleton class. Let's compile the code and look at that it has thrown an error saying Singleton is inaccessible due to its protection level. This error is because of private constructor.
 
Now you might be thinking that when a private constructor is restricting the inheritance then why we need to apply sealed keyword to the class. 
 
Let’s just move this new class inside the Singleton class. By moving this class inside the Singleton class it has now become nested or child class of the main singleton class 
<pre>
Singleton.cs 
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace SingletonDemo
{
    public class Singleton
    {
        private static int counter = 0;
        private static object obj = new object();
 
        private Singleton()
        {
            counter++;
            Console.WriteLine("Counter Value " + counter.ToString());
        }
        private static Singleton instance = null;

        public static Singleton GetInstance
        {
            get
            {
                if (instance == null)
                    instance = new Singleton();
                return instance;
            }
        }

        public void PrintDetails(string message)
        {
            Console.WriteLine(message);
        }

        public class DerivedSingleton : Singleton
        {

        }
    }
}

    </pre>
 
What is a nested class? A class with in another class is called a nested class. At this moment we will skip any further discussions related to nested class as it is out of scope for this tutorial. However, for more details please refer to MSDN article about nested classes and types.
 
Now that we have moved the derived class to nested class lets compile the program and check. Look at that we are able to compile this successfully. 
 
Now, let’s switch to main program and access the nested class.
<pre>
Program.cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace SingletonDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Singleton fromStudent = Singleton.GetInstance;
            fromStudent.PrintDetails("From Student");
            
            Singleton fromEmployee = Singleton.GetInstance;
            fromEmployee.PrintDetails("From Employee");

            Console.WriteLine("-------------------------------------");
            
            Singleton.DerivedSingleton derivedObj = new Singleton.DerivedSingleton();
            derivedObj.PrintDetails("From Derived");
           
            Console.ReadLine();
        }
    }
}
 </pre>
Lets run the program. Look at that the counter value has incremented to 2 proving that we are able to create multiple instances of the singleton using the nested derived class.
 
This violates the principle of singleton.  Let’s go back to the Singleton and make the class as sealed. Let’s compile the program
 
Look at that we have got an error when we compile the program saying we cannot derive a sealed class. With this we have proved that private constructor helps in preventing any external instantiations of objects and sealed will prevent the class inheritances.
