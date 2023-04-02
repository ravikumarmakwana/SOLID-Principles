    namespace SingleResponsibilityPrinciple
    {
        // 1. Single Responsibility Principle
        public class Student
        {
            public void AddStudent()
            {
                // Register new student.
            }

            public void EnterSubjectsMarks()
            {
                // Enter subject marks of students
            }

            public void GetResults()
            {
                // Generate result base on marks
            }
        }

        /*
         * 
         * Here, 
         * If we want to change logic of student registration, then we need to update this class.
         * If we want to change logic of storing subject marks, then we need to update this class.
         * If we want to change logic of calculating result of students, then we need to update this class.
         * so, here there are many reason to change this class, which is violation of Single Responsibility Principle.
         * 
         */

        public class StudentRegistration
        {
            public void RegisterStudent()
            {
                // Student registration logic.
            }
        }

        public class SubjectMarks
        {
            public void StoringMarks()
            {
                // Logic of subject marks management.
            }
        }

        public class ResultManagement
        {
            public void CalculateResult()
            {
                // Logic of calculating result.
            }
        }
    }

    namespace OpenClosedPrinciple
    {
        /*
         * 2. Open-Closed Principle
         * This principle state that Open for Extension, closed for modification.
         * 
         */
        public interface ILogger
        {
            void Log();
        }
        public class FilerLogger : ILogger
        {
            public void Log()
            {
                // Log to file.
            }
        }
        public class DatabaseLogger : ILogger
        {
            public void Log()
            {
                // Log to database.
            }
        }
        public class ConsoleLogger : ILogger
        {
            public void Log()
            {
                // Log to console.
            }
        }

        public class LogFactory
        {
            public ILogger GetLogger(string loggerSource)
                => loggerSource switch
                {
                    "File" => new FilerLogger(),
                    "Database" => new DatabaseLogger(),
                    "Console" => new ConsoleLogger(),
                    _ => throw new Exception("Invalid logger source.")
                };
        }
    }

    namespace LiskovSubstitutionPrinciple
    {
        /*
         * 3. Liskov Substitution Principle
         * This principle state that, subclass should be substitution for their base class.
         * It states that a superclass object should be replaceable with a subclass object
         * without breaking the functionality of software.
         * The Liskov Substitution Principle makes heavy use of inheritance and polymorphism and 
         * commonly leverages language features such as interface when multiple inheritance is not supported.
         */

        public class Rectangle
        {
            public int Height;
            public int Width;

            public Rectangle()
            {

            }

            public Rectangle(int height, int width)
            {
                Height = height;
                Width = width;
            }

            public void SetHeight(int value)
            {
                this.Height = value;
            }

            public void SetWidth(int value)
            {
                this.Width = value;
            }

            public double CalculateArea()
            {
                return Height * Width;
            }
        }
        public class Square : Rectangle
        {
            public Square()
            {

            }

            public Square(int size)
            {
                this.Height = this.Width = size;
            }

            public void SetHeight(int value)
            {
                this.Height = this.Width = value;
            }

            public void SetWidth(int value)
            {
                this.Height = this.Width = value;
            }
        }
        public class ShapePrinter
        {
            public void PrintShape(Rectangle rectangle)
            {
                rectangle.SetHeight(10);
                Console.WriteLine($"Area = {rectangle.CalculateArea()}");
            }
        }
    }

    namespace InterfaceSegregationPrinciple
    {
        /*
         * 4. Interface Segregation Principle
         * 'Segregation' means keeping things separated.
         * This principle states that, Many clients specific interfaces are better than one general purpose interface.
         * Clients should not to implement a function they do not need.
         * 
         */
        public interface IParkingLot
        {
            void Park();
            void Unpark();
            void CalculateFee();
            void FeePayment();
        }
        public class PaidParkingLot : IParkingLot
        {
            public void Park()
            {
                // Park your car
            }

            public void Unpark()
            {
                // Un-park your car
            }

            public void CalculateFee()
            {
                // Calculate fee for parking lot.
            }

            public void FeePayment()
            {
                // Parking lot fee payment.
            }
        }
        public class FeeParkingLot : IParkingLot
        {
            public void Park()
            {
                // Park your car.
            }

            public void Unpark()
            {
                // Unpark your car.
            }

            public void CalculateFee()
            {
                // It's not require.
            }

            public void FeePayment()
            {
                // It's not require.
            }
        }
        // Instead of general-purpose interface, use specific interface like IFeeParkingLot
        public interface IFeeParkingLot
        {
            void Park();
            void Unpark();
        }
    }

    namespace DependencyInversionPrinciple
    {
        /*
         * 5. Dependency Inversion Principle
         * # High-level modules should not depends on lower-level modules. Both depends on abstraction.
         * # Abstraction should not depends on details. Details should be depend of abstraction.
         */
        public enum Gender
        {
            Male, Female
        }
        public class Employee
        {
            public string Name { get; set; }
            public Gender Gender { get; set; }
        }
        /*
        public class EmployeeManager
        {
            public List<Employee> Employees { get; set; }

            public EmployeeManager()
            {
                Employees = new List<Employee>(); 
            }
            public void AddEmployee(Employee employee)
            {
                Employees.Add(employee);
            }
        } 
        public class EmployeeStatistics
        {
            private readonly EmployeeManager _employeeManager;

            public EmployeeStatistics(EmployeeManager employeeManager)
            {
                _employeeManager = employeeManager;
            }

            public int GetCountOfFemales()
                => _employeeManager.Employees.Count(e => e.Gender == Gender.Female);
        } */

        // Here, High-lever module is strictly depend on lower-level module.
        public interface IEmployeeSearchable
        {
            IEnumerable<Employee> GetFemaleEmployees();
        }
        public class EmployeeManager : IEmployeeSearchable
        {
            public List<Employee> Employees { get; set; }

            public EmployeeManager()
            {
                Employees = new List<Employee>();
            }
            public void AddEmployee(Employee employee)
            {
                Employees.Add(employee);
            }
            public IEnumerable<Employee> GetFemaleEmployees()
                => Employees.Where(e => e.Gender == Gender.Female);
        }
        public class EmployeeStatistics
        {
            private readonly EmployeeManager _employeeManager;

            public EmployeeStatistics(EmployeeManager employeeManager)
            {
                _employeeManager = employeeManager;
            }

            public int GetCountOfFemales()
                => _employeeManager.GetFemaleEmployees().Count();
        }
    }
    public class Program
    {
        public static void Main(string[] args)
        {
            var shapePrinter = new LiskovSubstitutionPrinciple.ShapePrinter();

            LiskovSubstitutionPrinciple.Rectangle rectangle = new LiskovSubstitutionPrinciple.Rectangle();
            rectangle.SetWidth(30);
            shapePrinter.PrintShape(rectangle);

            LiskovSubstitutionPrinciple.Rectangle square = new LiskovSubstitutionPrinciple.Square();
            square.SetWidth(40);
            shapePrinter.PrintShape(square);
        }
    }
