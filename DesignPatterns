
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
            /*
Assuming Singleton is created from employee class
we refer to the GetInstance property from the Singleton class
             */
            Singleton fromEmployee = Singleton.GetInstance;
            fromEmployee.PrintDetails("From Employee");
            /*
Assuming Singleton is created from student class
we refer to the GetInstance property from the Singleton class
             */
            Singleton fromStudent = Singleton.GetInstance;
            fromStudent.PrintDetails("From Student");

            Console.ReadLine();
        }
    }
}

Singleton.cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace SingletonDemo
{
    /*
Sealed ensures the class being inherited and
object instantiation is restricted in the derived class
     */
    public sealed class Singleton
    {
        private static int counter = 0;

        /*
Private property initilized with null
ensures that only one instance of the object is created
based on the null condition
         */
        private static Singleton instance = null;
       
        /*
public property is used to return only one instance of the class
leveraging on the private property
         */
        public static Singleton GetInstance
        {
            get
            {
                if (instance == null)
                    instance = new Singleton();
                return instance;
            }
        }
        /*
Private constructor ensures that object is not
instantiated other than with in the class itself
         */
        private Singleton()
        {
            counter++;
            Console.WriteLine("Counter Value " + counter.ToString());
        }
        /*
Public method which can be invoked through the singleton instance
         */
        public void PrintDetails(string message)
        {
            Console.WriteLine(message);
        }
    }
