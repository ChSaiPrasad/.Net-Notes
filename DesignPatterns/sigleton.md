

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


<img width="300" height="300" alt="Slide3" src="https://github.com/user-attachments/assets/26c0ebd7-78aa-4a47-8295-7328a5427dfe" />
<img width="300" height="300" alt="Slide4" src="https://github.com/user-attachments/assets/d5ecebf0-c0eb-4777-ab86-fc41a695d468" />
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



---------------------------------------------------------

Lazy Initialization in Singleton : GetInstance Property is responsible for the Singleton Instance creation. Singleton object is not instantiated until and unless GetInstance is invoked. Hence, there is a delay in instance creation till the GetInstance is accessed.
This Delay in Instance creation is called Lazy Initialization. 

How to use Multithreads in Singleton : The lazy initialization works perfectly well when we invoke the GetInstance in a Single threaded approach. However, there is a chance that we may end up creating multiple instances when multiple threads invoke the GetInstance at the same time.

This Thread racing situation causes thread safety issues in Singleton Initialization and further the current code ends up in creating multiple instances of Singleton objects in memory.

To achieve and replicate multiple threads accessing GetInstance, We have modified the main program by using Parallel.Invoke method of .NET Framework 4.0.  Please refer to Main program code below for more details.

How to implement a Thread Safe singleton class : Locks are the best way to control thread race condition and they help us to overcome the present situation. Please refer to the Singleton.cs code for lock checks and double check locking.

For more details on double check locking please refer to the below article
https://en.wikipedia.org/wiki/Double-... 

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
           /* Multi Threading */
            Parallel.Invoke(
                () => PrintStudentdetails(), 
                () => PrintEmployeeDetails()
                );
            Console.ReadLine();
        }

        private static void PrintEmployeeDetails()
        {
            Singleton fromEmployee = Singleton.GetInstance;
            fromEmployee.PrintDetails("From Employee");
        }

        private static void PrintStudentdetails()
        {
           
            Singleton fromStudent = Singleton.GetInstance;
            fromStudent.PrintDetails("From Student");
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
        private static readonly object obj = new object();
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
                {
                    lock (obj)
                    {
                        if (instance == null)
                            instance = new Singleton();
                    }
                }
                return instance;
            }
        }
        /*
Public method which can be invoked through the singleton instance
         */
        public void PrintDetails(string message)
        {
            Console.WriteLine(message);
        }
    }
}
    </pre>


Lazy Initialization : The lazy initialization of an object improves the performance and avoids unnecessary computation till the point the object is accessed. Further, it reduces the memory footprint during the startup of the program. Reducing the memory print will help faster loading of the application. 

Non-Lazy or Eager Loading : Eager loading is nothing but to initialize the required object before it’s being accessed.  Which means, we instantiate the object and keep it ready and use it when we need it. This type of initialization is used in lower memory footprints. Also, in eager loading, the common language runtime takes care of the variable initialization and its thread safety. Hence, we don’t need to write any explicit coding for thread safety.

Singleton with Lazy keyword (.NET 4.0) : Lazy keyword provides support for lazy initialization. In order to make a property as lazy, we need to pass the type of object to the lazy keyword which is being lazily initialized. 

By default, Lazy[T] objects are thread-safe.  In multi-threaded scenarios, the first thread which tries to access the Value property of the lazy object will take care of thread safety when multiple threads are trying to access the Get Instance at the same time. 

Therefore, it does not matter which thread initializes the object or if there are any thread race conditions that are trying to access this property.
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
            Parallel.Invoke(
                () =] PrintStudentDetails(),
                () =] PrintEmployeeDetails()
            );
            Console.ReadLine();
        }

        private static void PrintEmployeeDetails()
        {
            Singleton fromEmployee = Singleton.GetInstance;
            fromEmployee.PrintDetails("From Employee");
        }

        private static void PrintStudentDetails()
        {  
            Singleton fromStudent = Singleton.GetInstance;
            fromStudent.PrintDetails("From Student");
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
       
        private Singleton()
        {
            counter++;
            Console.WriteLine("Counter Value " + counter.ToString());
        }
        private static readonly Lazy[Singleton] instance = new Lazy[Singleton](()=]new Singleton());
       
        public static Singleton GetInstance
        {
            get
            {
                return instance.Value;
            }
        }
       
        public void PrintDetails(string message)
        {
            Console.WriteLine(message);
        }       
    }

    </pre>

Differences between Singleton and static classes
1. Static is a keyword and Singleton is a design pattern
2. Static classes can contain only static members
3. Singleton is an object creational pattern with one instance of the class
4. Singleton can implement interfaces, inherit from other classes and it aligns with the OOPS concepts
5. Singleton object can be passed as a reference
6. Singleton supports object disposal
7. Singleton object is stored on heap
8. Singleton objects can be cloned

Static class example - Temperature Converter 
We are pretty sure that the formulas for foreign heat to Celsius conversion and vice versa will not change at all and hence we can use 
static classes with static methods that does the conversion for us. Please refer to the below code for more details.

Real world usage of Singleton : Listed are few real world scenarios for singleton usage
1. Exception/Information logging
2. Connection pool management 
3. File management
4. Device management such as printer spooling
5. Application Configuration management
6. Cache management
7. And Session based shopping cart are some of the real world usage of singleton design pattern

<pre>
Program.cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace StaticDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            double celcius = 37; double fahrenheit = 98.6;
            Console.WriteLine("Value of {0} celcius to fahrenheit is {1}",
                celcius, Converter.ToFahrenheit(celcius));
            Console.WriteLine("Value of {0} fahrenheit to celcius is {1}",
                fahrenheit, Converter.ToCelcius(fahrenheit));
            Console.ReadLine();
        }
    }
}
</pre>
<pre>
Converter.cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace StaticDemo
{
    public static class Converter
    {
        public static double ToFahrenheit(double celcius)
        {
            return (celcius * 9 / 5) + 32;
        }
        public static double ToCelcius(double fahrenheit)
        {
            return (fahrenheit - 32) * 5 / 9;
        }
    }
}
</pre>
