

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


<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/97ab382e-ab7a-490d-a2b8-1f0d55ca039a" />

<img width="300" height="300" alt="Screenshot 2025-08-04 121846" src="https://github.com/user-attachments/assets/6c869c6f-5ff7-4275-8322-cb3971ce6fc7" />




<img width="300" height="300" alt="Screenshot 2025-08-04 121925" src="https://github.com/user-attachments/assets/2b1802a7-9e1c-4184-8924-80f5fc2b9e99" />


<img width="300" height="300" alt="Screenshot 2025-08-04 121942" src="https://github.com/user-attachments/assets/aa3ba5de-86a3-411b-b23b-fcaea81a7927" />


<img width="300" height="300" alt="Screenshot 2025-08-04 122001" src="https://github.com/user-attachments/assets/0c702189-f0dc-43b0-a0e3-d3869f872910" />


<img width="300" height="300" alt="Screenshot 2025-08-04 122033" src="https://github.com/user-attachments/assets/0c743402-a23e-42c3-b32b-de39a4350c2d" />


<img width="300" height="300" alt="Screenshot 2025-08-04 122052" src="https://github.com/user-attachments/assets/b00fc9f1-6e29-47f4-82ea-8dc6c68c7d66" />



Factory Method Pattern Example

Business Requirement 
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/41c54f9d-3e77-4cc2-bd71-ee456e812e8d" />

1. Differentiate employees as permanent and contract and segregate their pay scales as well as bonus based on their employee types.  ( We have achieved this using simple factory 
2. Calculate Permanent employee house rent allowance
3. Calculate Contract employee medical allowance

Steps to solve the above business requirement

Step 1: Add HouseAllowance and MedicalAllowance to the existing Employee table.
CREATE TABLE [dbo].[Employee] (
    [Id]               INT          IDENTITY (1, 1) NOT NULL,
    [Name]             VARCHAR (50) NOT NULL,
    [JobDescription]   VARCHAR (50) NOT NULL,
    [Number]           VARCHAR (50) NOT NULL,
    [Department]       VARCHAR (50) NOT NULL,
    [HourlyPay]        DECIMAL (18) NOT NULL,
    [Bonus]            DECIMAL (18) NOT NULL,
    [EmployeeTypeID]   INT          NOT NULL,
    [HouseAllowance]   DECIMAL (18) NULL,
    [MedicalAllowance] DECIMAL (18) NULL,
    PRIMARY KEY CLUSTERED ([Id] ASC),
    CONSTRAINT [FK_Employee_EmployeeType] FOREIGN KEY ([EmployeeTypeID]) REFERENCES [dbo].[Employee_Type] ([Id]) );

Step 2: Open EmployeePortal.edmx under the Models folder of the solution and update the model from the database 

Step 3: Create FactoryMethod folder under existing Factory folder and add BaseEmployeeFactory class.

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/0c4a26a9-5074-4f48-99b1-42b8327ebd18" />

Step 4: Create ContractEmployeeFactory class under FactoryMethod folder.

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/6c130a87-69cf-49db-aecd-47d24146615f" />
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/472f319b-1764-4de5-a11b-93ec124991b1" />
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/f5c1275b-ad54-45f3-b83d-dd44d22c095e" />
<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/d5d83a8d-8139-44a3-b7b7-342d1d00aa44" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/200d377f-431e-44dd-a862-370a78b37785" />

<img width="300" height="300" alt="Screenshot 2025-08-04 112319" src="https://github.com/user-attachments/assets/7607f2ed-b45e-423b-bbc2-ffba2be96310" />



  
