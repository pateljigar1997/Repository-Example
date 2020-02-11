# Repository-Example

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace DEMO.Models
{
    public interface IEmployeeRepository
    {
        Employee GetEmployee(int Id);
        IEnumerable<Employee> GetAllEmployee();
        Employee Add(Employee employee);
        Employee Update(Employee employeeeChanges);
        Employee Delete(int id);
        void Save();
        
    }
}

----------------Then Implement of Repository---------------

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace DEMO.Models
{
    public class SQLEmployeeRepository : IEmployeeRepository
    {
        private readonly DBContext _context;

        public SQLEmployeeRepository(DBContext context)
        {
            this._context = context;
        }
        public Employee Add(Employee employee)
        {
            _context.Employees.Add(employee);
            _context.SaveChanges();
            return employee;
        }

        public Employee Delete(int id)
        {
            Employee employee = _context.Employees.Find(id);
            if(employee !=null) 
            {
                _context.Employees.Remove(employee);
                _context.SaveChanges();
            }
            return employee;
        }

        public IEnumerable<Employee> GetAllEmployee()
        {
            return _context.Employees;
        }

        public Employee GetEmployee(int Id)
        {
            return _context.Employees.Find(Id);
        }

        public void Save()
        {
            _context.SaveChanges();
        }

        public Employee Update(Employee employeeeChanges)
        {
           var employee = _context.Employees.Attach(employeeeChanges);
            employee.State = Microsoft.EntityFrameworkCore.EntityState.Modified;
            _context.SaveChanges();
            return employeeeChanges;
        }
    }
}
