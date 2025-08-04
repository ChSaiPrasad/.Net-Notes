In this session we will learn
1. What is Factory Design Pattern
2. Implementation Guidelines
3. Simple factory implementation

What is Factory Design Pattern : Gang of Four Definition 
Define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory method lets a class defer instantiation it uses to subclasses

Factory pattern is one of the most used design patterns in real world applications. Factory pattern creates object without exposing the creation logic to the client and refer to newly created object using a common interface

Implementation Guidelines

We need to choose Factory Pattern when
1. The Object needs to be extended to subclasses
2. The Classes doesn’t know what exact sub-classes it has to create
3. The Product implementation tend to change over time and the Client remains unchanged

Simple Factory Example : Business Requirement

Differentiate employees as permanent and contract and segregate their pay scales as well as bonus based on their employee types

We can address the above requirement with the below implementations
1. Implement without Factory Pattern
2. Use a Simple Factory
3. Enhance Simple factory to Factory Method Pattern

We will be working on the Employee Portal that we used in the Singleton tutorials. Please refer to them before proceeding 

Prerequisite steps 
1. Enhance the DB model to add Employee_Type Table
2. Add Permanent and Contract Employees as Master Data.
3. Add new columns EmployeeTypeID, Bonus, HourlyPay to Emplyee Table and add Foreign key constraint to the Emp
4. Update the Emloyee Model edmx file with the latest changes.
5. Create new BaseController and Move the Singleton Exception logic  to the base controller

   <pre>
    public class BaseController : Controller
    {
        private ILog _ILog;
        public BaseController()
        {
            _ILog = Log.GetInstance;
        }
        protected override void OnException(ExceptionContext filterContext)
        {
            _ILog.LogException(filterContext.Exception.ToString());
            filterContext.ExceptionHandled = true;
            this.View("Error").ExecuteResult(this.ControllerContext);
        }
    }
    </pre>
7. Regenerate the EmployeesController and its corresponding views
8. Comment the code in Create and Update views which accepts inputs for Bonus and HourlyPay

Solution 1: Implement without Factory Pattern
<pre>
EmployeeController.cs

        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "Id,Name,JobDescription,Number,Department,HourlyPay,Bonus,EmployeeTypeID")] Employee employee)
        {
            if (ModelState.IsValid)
            {
                if (employee.EmployeeTypeID == 1)
                {
                    employee.HourlyPay = 8;
                    employee.Bonus = 10;
                }
                else if (employee.EmployeeTypeID == 2)
                {
                    employee.HourlyPay = 12;
                    employee.Bonus = 5;
                }
                db.Employees.Add(employee);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            ViewBag.EmployeeTypeID = new SelectList(db.Employee_Type, "Id", "EmployeeType", employee.EmployeeTypeID);
            return View(employee);
        }

  </pre>

The above code introduces 
1. Tight coupling between Controller class and Business logic
2. For any new employee type addition, we end up modifying the controller code adding extra over heads in the development and testing process

Using a simple factory eliminates the above drawbacks.

Solution 2: Implement with Simple Factory

1. Add new Manager folder and add the below interface and classes
<pre>
IEmployeeManager.cs

    public interface IEmployeeManager
    {
        decimal GetBonus();
        decimal GetPay();
    }

  </pre>


<img width="1051" height="610" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/97ab382e-ab7a-490d-a2b8-1f0d55ca039a" />

<img width="1042" height="571" alt="Screenshot 2025-08-04 121846" src="https://github.com/user-attachments/assets/6c869c6f-5ff7-4275-8322-cb3971ce6fc7" />




<img width="300" height="300" alt="Screenshot 2025-08-04 121925" src="https://github.com/user-attachments/assets/2b1802a7-9e1c-4184-8924-80f5fc2b9e99" />


<img width="300" height="300" alt="Screenshot 2025-08-04 121942" src="https://github.com/user-attachments/assets/aa3ba5de-86a3-411b-b23b-fcaea81a7927" />


<img width="300" height="300" alt="Screenshot 2025-08-04 122001" src="https://github.com/user-attachments/assets/0c702189-f0dc-43b0-a0e3-d3869f872910" />


<img width="300" height="300" alt="Screenshot 2025-08-04 122033" src="https://github.com/user-attachments/assets/0c743402-a23e-42c3-b32b-de39a4350c2d" />


<img width="300" height="300" alt="Screenshot 2025-08-04 122052" src="https://github.com/user-attachments/assets/b00fc9f1-6e29-47f4-82ea-8dc6c68c7d66" />






  
