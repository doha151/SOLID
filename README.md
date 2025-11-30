# SOLID 
// SOLID peincipels :

// 1- Single Responsibility Principle:

// Bad 

public class Employee
{
    public double CalculateSalary(int hours)
    {
        return hours * 50;
    }

    public void SaveToDatabase(Employee emp)
    {
        Console.WriteLine("Saving to DB...");
    }
}

// Good 

public class SalaryCalculator
{
    public double CalculateSalary(int hours)
    {
        return hours * 50;
    }
}

public class EmployeeRepository
{
    public void Save(Employee emp)
    {
        Console.WriteLine("Saving to DB...");
    }
}


// 2- Open/Closed Principle :

// Bad 

public class Payment
{
    public void Pay(string type)
    {
        if (type == "paypal")
            Console.WriteLine("Pay with PayPal");
        else if (type == "visa")
            Console.WriteLine("Pay with Visa");
    }
}


// Good 

public abstract class PaymentMethod
{
    public abstract void Pay();
}

public class PayPal : PaymentMethod
{
    public override void Pay()
    {
        Console.WriteLine("Pay with PayPal");
    }
}

public class Visa : PaymentMethod
{
    public override void Pay()
    {
        Console.WriteLine("Pay with Visa");
    }
}

public class PaymentProcessor
{
    public void Process(PaymentMethod method)
    {
        method.Pay();
    }
}


// 3- Liskov Substitution Principle

// Bad 

public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int Area()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}

// Good 

public interface IShape
{
    int Area();
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public int Area()
    {
        return Width * Height;
    }
}

public class Square : IShape
{
    public int Side { get; set; }

    public int Area()
    {
        return Side * Side;
    }
}


// 4- Interface Segregation Principle

// Bad 
public interface IWorker
{
    void Work();
    void Eat();
}

public class Robot : IWorker
{
    public void Work() { }

    public void Eat()      
    {
        throw new NotImplementedException();
    }
}


// Good 
public interface IWorkable
{
    void Work();
}

public interface IEatable
{
    void Eat();
}
public class Human : IWorkable, IEatable
{
    public void Work() { }
    public void Eat() { }
}

public class Robot : IWorkable
{
    public void Work() { }
}


// 5- Dependency Inversion Principle

// Bad 
public class EmailService
{
    public void SendEmail(string msg)
    {
        Console.WriteLine("Sending email: " + msg);
    }
}

public class Notification
{
    private EmailService email = new EmailService();

    public void Send(string message)
    {
        email.SendEmail(message);
    }
}


//Good 
public interface IMessageService
{
    void Send(string message);
}

public class EmailService : IMessageService
{
    public void Send(string message)
    {
        Console.WriteLine("Sending email: " + message);
    }
}

public class SmsService : IMessageService
{
    public void Send(string message)
    {
        Console.WriteLine("Sending SMS: " + message);
    }
}

public class Notification
{
    private readonly IMessageService _service;

    public Notification(IMessageService service)
    {
        _service = service;
    }

    public void Send(string message)
    {
        _service.Send(message);
    }
}
