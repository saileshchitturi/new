✋inheritance mapping :
👍Faculty.java

package inheritancemapping;
import javax.persistence.*;

@Entity
@Table(name="faculty_table")
//@DiscriminatorValue(value="faculty")
public class Faculty extends Person
{
	private String qualification;
	private double salary;
	private String course;
	public String getQualification() {
		return qualification;
	}
	public void setQualification(String qualification) {
		this.qualification = qualification;
	}
	public double getSalary() {
		return salary;
	}
	public void setSalary(double salary) {
		this.salary = salary;
	}
	public String getCourse() {
		return course;
	}
	public void setCourse(String course) {
		this.course = course;
	}
}

👍Inheritance mapping demo
package inheritancemapping;
import org.hibernate.*;
import org.hibernate.cfg.*;

public class InheritanceMappingDemo 
{
	public static void main(String[] args) 
	{
		Configuration configuration = new Configuration();
		configuration.configure("hibernate.cfg.xml");
		SessionFactory sessionFactory = configuration.buildSessionFactory();
		Session session = sessionFactory.openSession();
		
		Transaction transaction = session.beginTransaction();
		
		Person p = new Person();
		p.setName("Soumya");
		p.setGender("Male");
		
		Faculty f = new Faculty();
		f.setName("Surya");
		f.setGender("Male");
		f.setQualification("Ph.D");
		f.setSalary(5600.00);;
		f.setCourse("JFSD");
		
		Student s = new Student();
		s.setName("Sravan");
		s.setGender("Male");
		s.setProgram("Btech");
		s.setCgpa(8.95);
		s.setBacklogs(0);
		
		session.save(p);
		session.save(f);
		session.save(s);
		
		System.out.println("Success");
		
		transaction.commit();
		session.close();
		sessionFactory.close();
	}
}

👍person
package inheritancemapping;

import javax.persistence.*;

@Entity
@Table(name="person_table")
//@Inheritance(strategy=InheritanceType.SINGLE_TABLE)
//@DiscriminatorColumn(name="type",discriminatorType=DiscriminatorType.STRING,length = 100)
//@DiscriminatorValue(value="person")
//@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
@Inheritance(strategy = InheritanceType.JOINED)
public class Person
{
	@Id
	@GeneratedValue
	private int id;
	private String name;
	private String gender;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
}

👍student
package inheritancemapping;
import javax.persistence.*;

@Entity
@Table(name="student_table")
// @DiscriminatorValue(value="student") 
public class Student extends Person
{
	private String program;
	private double cgpa;
	private int backlogs;
	public String getProgram() {
		return program;
	}
	public void setProgram(String program) {
		this.program = program;
	}
	public double getCgpa() {
		return cgpa;
	}
	public void setCgpa(double cgpa) {
		this.cgpa = cgpa;
	}
	public int getBacklogs() {
		return backlogs;
	}
	public void setBacklogs(int backlogs) {
		this.backlogs = backlogs;
	}
}

✋Hybernate crud operations:
👍product
package com.klef.jfsd;

public class Product
{
	private int id;
	private String category;
	private String name;
	private String description;
	private Double price;
	private String manufactureddate;
	private boolean expiry;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getCategory() {
		return category;
	}
	public void setCategory(String category) {
		this.category = category;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getDescription() {
		return description;
	}
	public void setDescription(String description) {
		this.description = description;
	}
	public Double getPrice() {
		return price;
	}
	public void setPrice(Double price) {
		this.price = price;
	}
	public String getManufactureddate() {
		return manufactureddate;
	}
	public void setManufactureddate(String manufactureddate) {
		this.manufactureddate = manufactureddate;
	}
	public boolean isExpiry() {
		return expiry;
	}
	public void setExpiry(boolean expiry) {
		this.expiry = expiry;
	}
}

👍product crud operations
package com.klef.jfsd;

import org.hibernate.*;
import org.hibernate.cfg.*;

public class ProductCRUDOperations 
{
   public static void main(String args[])
   {
	   ProductCRUDOperations crudOperations = new ProductCRUDOperations();
	   //crudOperations.insertProduct();
	   //crudOperations.viewProduct();
	   //crudOperations.deleteProduct();
	   //crudOperations.updateProduct();
	   crudOperations.updateProductRecord();
	    //crudOperations.saveorupdateProduct();
   }
   public void insertProduct() // Insert Product
   {
	   Configuration configuration = new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory = configuration.buildSessionFactory();
	   Session session = sessionFactory.openSession(); 
	  
	   
	   Transaction transaction = session.beginTransaction();
	   
	   //transient state
	   
	   Product product = new Product();
	   product.setId(103);
	   product.setCategory("Groceries");
	   product.setName("Cucumber");
	   product.setDescription("It is a Vegetable");
	   product.setPrice(10.00);
	   product.setManufactureddate("22/07/2022");
	   product.setExpiry(false);
	   
	   session.save(product);  //persistent state
	   
	   transaction.commit(); // which will save the results into database
	   
	   System.out.println("Product Object Saved Successfully");
	   
	   session.close(); //detached state
	   sessionFactory.close();
	   
   }
   
   public void viewProduct()
   {
	   Configuration configuration = new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory = configuration.buildSessionFactory();
	   Session session = sessionFactory.openSession();
	   
	   Object obj=session.load(Product.class, 101);
	  // Object obj=session.load(Product.class, 105);
	   //Object obj=session.get(Product.class, 101);
	   //Object obj=session.get(Product.class, 105);
	   Product product=(Product)obj;
	   
	   if(product!=null)
	   { 
	   System.out.println(product.getId());
	   System.out.println(product.getCategory());
	   System.out.println(product.getName());
	   System.out.println(product.getDescription());
	   System.out.println(product.getPrice());
	   System.out.println(product.getManufactureddate());
	   System.out.println(product.isExpiry());
	   }
	   else
	   {
		   System.out.println("Product Object Not Found");
	   }
	 
	   session.close(); //detached state
	   sessionFactory.close();
   }
   public void deleteProduct()
   {
	   Configuration configuration = new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory = configuration.buildSessionFactory();
	   Session session = sessionFactory.openSession(); 
	  
	   
	   Transaction transaction = session.beginTransaction();
	   
	  
	   //Object obj = session.load(Product.class,101);
	   //Object obj = session.load(Product.class,110);
	   //Object obj = session.get(Product.class,102);
	   Object obj = session.get(Product.class,101);
	   Product product = (Product)obj;
	   
	   if(product!=null)
	   {
	   session.delete(product);
	   System.out.println("Product Object Deleted Successfully");
	   }	   
	   else
	   {
		   System.out.println("Product Object Not Found to Delete");
	   }
	   transaction.commit(); // which will save the results into database
	   
	   
	   
	   session.close(); //detached state
	   sessionFactory.close();
   }
   public void updateProduct() //Actual Update Operation using load() or get() method
   {
	   Configuration configuration = new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory = configuration.buildSessionFactory();
	   Session session = sessionFactory.openSession(); 
	  
	   
	   Transaction transaction = session.beginTransaction();
	   
	  
	   Object obj = session.load(Product.class, 101);
	   Product product = (Product)obj;
	   
	   product.setCategory("Vegetables");
	   product.setPrice(100.49);
	   product.setExpiry(true);
	   
	   
	   transaction.commit(); // which will save the results into database
	   
	   System.out.println("Product Object Updated Successfully");
	   
	   session.close(); //detached state
	   sessionFactory.close();
   }
   public void updateProductRecord() //using update() method
   {
	   Configuration configuration = new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory = configuration.buildSessionFactory();
	   Session session = sessionFactory.openSession(); 
	  
	   
	   Transaction transaction = session.beginTransaction();
	   
	   // if you don't set values for all properties then NULL will taken for properties. 
	   // That means if you want update using this way, you need to update values for all properties.
	
	  
	   Product product = new Product();
	   product.setId(101); // This id must be there in the table
	   product.setName("Brinjal");
	   product.setManufactureddate("25/10/1993");
	   product.setExpiry(false);
	   
	   session.update(product);
	   
	   transaction.commit(); // which will save the results into database
	   
	   System.out.println("Product Object Updated Successfully");
	   
	   session.close(); //detached state
	   sessionFactory.close();
   }
   public void saveorupdateProduct()
   {
	   Configuration configuration = new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory = configuration.buildSessionFactory();
	   Session session = sessionFactory.openSession(); 
	  
	   
	   Transaction transaction = session.beginTransaction();
	   
	   //if id is there, values will be updated for ID row otherwise new record will be inserted
	  
	   Product product = new Product();
	   product.setId(101); // This id must be there in the table
	   product.setName("Brinjal");
	   product.setExpiry(true);
	   
	   session.saveOrUpdate(product);
	   
	   transaction.commit(); // which will save the results into database
	   
	   System.out.println("Product Object Updated Successfully");
	   
	   session.close(); //detached state
	   sessionFactory.close();
   }
}

👍customer
package com.klef.jfsd;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="customer_table")
public class Customer 
{
  @Id
  @Column(name="customer_id")
  private int id;
  @Column(name="customer_name",nullable = false , length = 100)
  private String name;
  @Column(name="customer_gender",nullable = false)
  private String gender;
  @Column(name="customer_username",nullable = false,unique = true, length = 100)
  private String username;
  @Column(name="customer_password",nullable = false,length = 100)
  private String password;
  @Column(name="customer_age",nullable = false)
  private Double age;
  @Column(name="customer_salary",nullable = false)
  private Double salary;
  @Column(name="customer_location",nullable = false,length = 200)
  private String location;
  @Column(name="customer_marriedstatus",nullable = false)
  private boolean marriedstatus;
public int getId() {
	return id;
}
public void setId(int id) {
	this.id = id;
}
public String getName() {
	return name;
}
public void setName(String name) {
	this.name = name;
}
public String getGender() {
	return gender;
}
public void setGender(String gender) {
	this.gender = gender;
}
public String getUsername() {
	return username;
}
public void setUsername(String username) {
	this.username = username;
}
public String getPassword() {
	return password;
}
public void setPassword(String password) {
	this.password = password;
}
public Double getAge() {
	return age;
}
public void setAge(Double age) {
	this.age = age;
}
public Double getSalary() {
	return salary;
}
public void setSalary(Double salary) {
	this.salary = salary;
}
public String getLocation() {
	return location;
}
public void setLocation(String location) {
	this.location = location;
}
public boolean isMarriedstatus() {
	return marriedstatus;
}
public void setMarriedstatus(boolean marriedstatus) {
	this.marriedstatus = marriedstatus;
}
@Override
public String toString() {
	return "Customer [id=" + id + ", name=" + name + ", gender=" + gender + ", username=" + username + ", password="
			+ password + ", age=" + age + ", salary=" + salary + ", location=" + location + ", marriedstatus="
			+ marriedstatus + "]";
}
}

👍customer crud operations
package com.klef.jfsd;
import org.hibernate.*;
import org.hibernate.cfg.*;
public class CustomerCRUDOperations {

	public static void main(String[] args) 
	{
		CustomerCRUDOperations crudOperations = new CustomerCRUDOperations();
		crudOperations.insertCustomer();
		//crudOperations.viewCustomerById();
		//crudOperations.deleteCustomer();
		//crudOperations.updateCustomer();
		
	}
	public void insertCustomer() 
	{
		Configuration configuration = new Configuration();
		configuration.configure("hibernate.cfg.xml");
		
		SessionFactory sessionFactory = configuration.buildSessionFactory();
		Session session = sessionFactory.openSession();
		
		Transaction transaction = session.beginTransaction();
		
		Customer customer = new Customer();
		customer.setId(105);
		customer.setName("Sailesh");
		customer.setGender("MALE");
		customer.setUsername("Sai1313");
		customer.setPassword("123");
		customer.setAge(17.6);
		customer.setSalary(587666.5);
		customer.setLocation("Vij");
		customer.setMarriedstatus(false);
		
		session.save(customer);
		transaction.commit();
		System.out.println("Customer object saved successfully");
		
		session.close();
		sessionFactory.close();
		
	}
	
	public void viewCustomerById()
	{
		Configuration configuration =new Configuration();
		configuration.configure("hibernate.cfg.xml");
		
		SessionFactory sessionFactory=configuration.buildSessionFactory();
		Session session = sessionFactory.openSession();
		Object obj=session.get(Customer.class, 103);
		Customer customer = (Customer)obj;
		if(customer!=null) {
			System.out.println(customer.getId());
			System.out.println(customer.getName());
			System.out.println(customer.getUsername());
			System.out.println(customer.getGender());
			System.out.println(customer.getAge());
			System.out.println(customer.getSalary());
			System.out.println(customer.getLocation());
			System.out.println(customer.isMarriedstatus());
		}
		else {
			System.out.println("Customer object not found");
		}
		session.close();
		sessionFactory.close();
		
	}
	public void deleteCustomer()
	{

		Configuration configuration =new Configuration();
		configuration.configure("hibernate.cfg.xml");
		
		SessionFactory sessionFactory=configuration.buildSessionFactory();
		Session session = sessionFactory.openSession();
		
		Transaction transaction = session.beginTransaction();
		Object obj=session.get(Customer.class, 101);
		Customer customer = (Customer)obj;
		
		if(customer!=null) {
			session.delete(customer);
			System.out.println("Customer object delted successfully");
		}
		else {
			System.out.println("Customer not found");
		}
		
		transaction.commit();
		
		session.close();
		sessionFactory.close();

	}
	public void updateCustomer()
	{
		Configuration configuration =new Configuration();
		configuration.configure("hibernate.cfg.xml");
		
		SessionFactory sessionFactory=configuration.buildSessionFactory();
		Session session = sessionFactory.openSession();
		
		Transaction transaction = session.beginTransaction();
		Object obj=session.get(Customer.class, 101);
		Customer customer = (Customer)obj;
		
		if(customer!=null) {
			
			customer.setUsername("Soum1313");
			customer.setSalary(466666.6);
			customer.setMarriedstatus(true);
			customer.setLocation("vskp");
			
			session.save(customer);
			transaction.commit();
		}
		else {
			System.out.println("Customer object not found");
		}
	
		
		
		session.close();
		sessionFactory.close();
	}
}

✋HCQL:
package com.klef.jfsd;

import java.util.List;

import org.hibernate.Criteria;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import org.hibernate.criterion.Order;
import org.hibernate.criterion.Projections;
import org.hibernate.criterion.*;

public class HCQLDemo 
{
  public static void main(String args[])
  {
    HCQLDemo hcqlDemo  = new HCQLDemo();
    //hcqlDemo.RestrictionsDemo();
    //hcqlDemo.ProjectionswithAggregateFunctions();
    //hcqlDemo.ProjectionswithProperties();
    //hcqlDemo.OrderDemo();
    //hcqlDemo.HCQLConditions();
    hcqlDemo.HCQLall();
    
    
  }
  public  void RestrictionsDemo()
  {
    Configuration configuration=new Configuration();
    configuration.configure("hibernate.cfg.xml");
       
    SessionFactory sessionFactory=configuration.buildSessionFactory();
    Session session=sessionFactory.openSession();
      
    Criteria criteria1 = session.createCriteria(Customer.class);
    //criteria1.add(Restrictions.eq("id",102)); //id=102
    criteria1.add(Restrictions.eq("name","Sravan"));//name=Sravan
    List<Customer> list1 = criteria1.list();
    System.out.println("Total Records where name is Sravan="+list1.size());
    if(list1.size()>0)
    {
      for(Customer c:list1)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }
    
    
    Criteria criteria2 = session.createCriteria(Customer.class); 
    criteria2.add(Restrictions.lt("salary",50000000.00));//salary < 50000000
    List<Customer> list2 = criteria2.list();
    System.out.println("Total Records where salary < 5000000.000="+list2.size());
    if(list2.size()>0)
    {
      for(Customer c:list2)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }
    
    
    Criteria criteria3 = session.createCriteria(Customer.class); 
    criteria3.add(Restrictions.gt("salary",5000000.00));//salary>50000.00 (greater than)
    List<Customer> list3 = criteria3.list();
    System.out.println("Total Records whre salary > 500000.000="+list3.size());
    if(list3.size()>0)
    {
      for(Customer c:list3)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }


    Criteria criteria4 = session.createCriteria(Customer.class); 
    criteria4.add(Restrictions.le("salary",5000000.00));//salary<=50000.00 (less than or equal to)
    List<Customer> list4 = criteria4.list();
    System.out.println("Total Records where salary<=5000000.000="+list4.size());
    if(list4.size()>0)
    {
      for(Customer c:list4)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }

    Criteria criteria5 = session.createCriteria(Customer.class); 
    criteria5.add(Restrictions.ge("salary",50000.00));//salary>=50000.00 (greater than or equal to)
    List<Customer> list5 = criteria5.list();
    System.out.println("Total Records where salary>=50000000.000="+list5.size());
    if(list5.size()>0)
    {
      for(Customer c:list5)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }

    Criteria criteria6 = session.createCriteria(Customer.class); 
    criteria6.add(Restrictions.between("salary",10000.00,70000000.00));//salary>=10000.00 and salary<=70000.00
    List<Customer> list6 = criteria6.list();
    System.out.println("Total Records where salary is between= "+list6.size());
    if(list6.size()>0)
    {
      for(Customer c:list6)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }

    Criteria criteria7 = session.createCriteria(Customer.class); 
    criteria7.add(Restrictions.ne("salary",50000.00));//salary!=50000.00 (not equal)
    List<Customer> list7 = criteria7.list();
    System.out.println("Total Records where salary is not equal to 50000="+list7.size());
    if(list7.size()>0)
    {
      for(Customer c:list7)
      {
        System.out.println(c.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }
    
    
    Criteria criteria8 = session.createCriteria(Customer.class); 
    criteria8.add(Restrictions.idEq(101)); // id = 101
  List<Customer> list8 = criteria8.list();
   System.out.println("Total Records="+list8.size());
    if(list8.size()>0)
    {
      System.out.println("Customer Data Found");
      for(Customer Customer:list8)
      {
        System.out.println(Customer.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }
    
    
    Criteria criteria9 = session.createCriteria(Customer.class); 
    criteria9.add(Restrictions.isNotNull("salary"));
    //criteria9.add(Restrictions.isNull("salary"));
   // criteria9.add(Restrictions.eqOrIsNull("salary", 30000.00));
   // criteria9.add(Restrictions.neOrIsNotNull("salary", 30000.00));
    criteria9.add(Restrictions.isNull("salary"));
  List<Customer> list9 = criteria9.list();
   System.out.println("Total Records="+list9.size());
    if(list9.size()>0)
    {
      System.out.println("Customer Data Found");
      for(Customer Customer:list9)
      {
        System.out.println(Customer.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }
    
    Criteria criteria10 = session.createCriteria(Customer.class); 
    //criteria10.add(Restrictions.like("name", "S%"));
    criteria10.add(Restrictions.like("name", "%n"));
    //criteria10.add(Restrictions.like("name", "____"));
  List<Customer> list10 = criteria10.list();
   System.out.println("Total Records="+list10.size());
    if(list10.size()>0)
    {
      System.out.println("Customer Data Found");
      for(Customer Customer:list10)
      {
        System.out.println(Customer.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }
    
    Criteria criteria11= session.createCriteria(Customer.class); 
    //criteria11.add(Restrictions.in("id", new Integer[] {101,102,105}));
    criteria11.add(Restrictions.in("name", new String[] {"CBIT","KLU","Sravan"}));
    List<Customer> list11 = criteria11.list();
   System.out.println("Total Records="+list11.size());
    if(list11.size()>0)
    {
      System.out.println("Customer Data Found");
      for(Customer Customer:list11)
      {
        System.out.println(Customer.toString());
      }
    }
    else
    {
      System.out.println("Customer Data Not Found");
    }

    session.close();
    sessionFactory.close();
  }
  
  public void ProjectionswithAggregateFunctions()
  {
    Configuration configuration=new Configuration();
    configuration.configure("hibernate.cfg.xml");
         
    SessionFactory sessionFactory=configuration.buildSessionFactory();
    Session session=sessionFactory.openSession();
    
    Criteria c1 = session.createCriteria(Customer.class);
    c1.setProjection(Projections.rowCount());
    List list1 = c1.list();
    System.out.println("Total Records Count="+list1.get(0));
    
    Criteria c2 = session.createCriteria(Customer.class);
    c2.setProjection(Projections.count("salary"));
    List list2 = c2.list();
    System.out.println("Total Records Count="+list2.get(0));
    
    Criteria c3 = session.createCriteria(Customer.class);
    c3.setProjection(Projections.min("age"));
    List list3 = c3.list();
    System.out.println("Minimum Age="+list3.get(0));
    
    Criteria c4 = session.createCriteria(Customer.class);
    c4.setProjection(Projections.max("age"));
    List list4 = c4.list();
    System.out.println("Maximum Age="+list4.get(0));
    
    Criteria c5 = session.createCriteria(Customer.class);
    c5.setProjection(Projections.sum("salary"));
    List list5 = c5.list();
    System.out.println("Total Salary="+list5.get(0));
    
    Criteria c6 = session.createCriteria(Customer.class);
    c6.setProjection(Projections.avg("salary"));
    List list6 = c6.list();
    System.out.println("Average Salary="+list6.get(0));
    
    session.close();
    sessionFactory.close();
  }
  
  public void ProjectionswithProperties()
  {
    Configuration configuration=new Configuration();
    configuration.configure("hibernate.cfg.xml");
           
    SessionFactory sessionFactory=configuration.buildSessionFactory();
    Session session=sessionFactory.openSession();
     
    Criteria c1 = session.createCriteria(Customer.class);
    c1.setProjection(Projections.id());
    List list1 = c1.list();
    System.out.println("Total Records Size="+list1.size());
    for(int i=0;i<list1.size();i++)
    {
      System.out.println(list1.get(i));
    }
    
    Criteria c2 = session.createCriteria(Customer.class);
    c2.setProjection(Projections.property("name"));
    List list2 = c2.list();
    System.out.println("Total Records Size="+list2.size());
    for(int i=0;i<list2.size();i++)
    {
      System.out.println(list2.get(i));
    }
    
    Criteria c3 = session.createCriteria(Customer.class);
    c3.setProjection(Projections.projectionList().add(Projections.property("id")).add(Projections.property("name")).add(Projections.property("salary")) );
    List<Object[]> list3 = c3.list();
    System.out.println("Total Records Size="+list3.size());
      for(Object[] obj:list3)
      {
        System.out.println(obj[0]+","+obj[1]+","+obj[2]);
      }
      
    session.close();
    sessionFactory.close();
  }
  
  
  public void OrderDemo() // asc or desc
  {
    Configuration configuration=new Configuration();
    configuration.configure("hibernate.cfg.xml");
           
    SessionFactory sessionFactory=configuration.buildSessionFactory();
    Session session=sessionFactory.openSession();
    
    Criteria c = session.createCriteria(Customer.class);
    c.addOrder(Order.asc("name"));
    //c.addOrder(Order.desc("name"));
    List<Customer> customerlist = c.list();
    System.out.println("Total Records Size="+customerlist.size());
    for(Customer customer:customerlist)
    {
      System.out.println(customer.toString());
    }
    
    session.close();
    sessionFactory.close();
  }
  
  public void HCQLConditions()
  {
	  Configuration configuration=new Configuration();
	    configuration.configure("hibernate.cfg.xml");
	           
	    SessionFactory sessionFactory=configuration.buildSessionFactory();
	    Session session=sessionFactory.openSession();
	    
	    Criteria c = session.createCriteria(Customer.class);
	    
	    //c.add(Restrictions.not(Restrictions.lt("salary", 100000000.00)));
	    //c.add(Restrictions.not(Restrictions.like("name", "N%")));

	    c.add(Restrictions.or(Restrictions.lt("salary",30000000.00),Restrictions.like("name", "S%")));
	    
	    List<Customer> customerlist = c.list();
	    System.out.println("Total Records Size="+customerlist.size());
	    for(Customer customer:customerlist)
	    {
	      System.out.println(customer.toString());
	    }
	    
	    session.close();
	    sessionFactory.close();
  }
  
  public void HCQLall() {
	  Configuration configuration=new Configuration();
	    configuration.configure("hibernate.cfg.xml");
	           
	    SessionFactory sessionFactory=configuration.buildSessionFactory();
	    Session session=sessionFactory.openSession();
	    
	    Criteria c = session.createCriteria(Customer.class);
        c.add(Restrictions.like("name", "S%")); // salary>30000
        c.setProjection(Projections.property("name")); // selecting only name property
        c.addOrder(Order.asc("name")); // descending order
        List list = c.list();
        System.out.println("Total Records="+list.size());
        for(int i=0;i<list.size();i++)
        {
          System.out.println(list.get(i));
        }
	    
	    session.close();
	    sessionFactory.close();
  }
}


✋HQL:
package com.klef.jfsd;

import java.util.List;

import org.hibernate.*;
import org.hibernate.cfg.*;

//import org.hibernate.Query;
//import org.hibernate.Criteria;

public class HQLDemo 
{
   public static void main(String args[])
   {
	   HQLDemo demo = new HQLDemo();
	   //demo.viewallCustomers();
	  //demo.viewallCustomersPartialObject();
	   demo.AggregateFunctions();
	   //demo.updatedeletepostionalparameters();
	   //demo.updatedeletenamedparameters();
	   //demo.selectpostionalparameters();
	   //demo.selectnamedparameters();

	   
   }
   public void viewallCustomers() //complete entity or object
   {
	   Configuration configuration=new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory=configuration.buildSessionFactory();
	   Session session=sessionFactory.openSession();
	   
	   Query qry = session.createQuery("from Customer");
	   List<Customer> customerlist=qry.list();
	   System.out.println("***Using Complete Object***");
	   System.out.println("Total Records="+customerlist.size());
	   for(Customer customer : customerlist)
	   {
		   System.out.println(customer.toString());
	   }
	   session.close();
	   sessionFactory.close();
   }
   public void viewallCustomersPartialObject() // partial entity or object
   {
	   Configuration configuration=new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory=configuration.buildSessionFactory();
	   Session session=sessionFactory.openSession();
	   
	   Query qry = session.createQuery("select c.id,c.name,c.salary,c.username,c.location from Customer c");
	   List<Object[]> customerlist=qry.list();
	   System.out.println("***Using Partial Object***");
	   System.out.println("Total Records="+customerlist.size());
	   for(Object[] obj:customerlist)
	   {
		   System.out.println("Customer ID="+obj[0]);
		   System.out.println("Customer Name="+obj[1]);
		   System.out.println("Customer Salary="+obj[2]);
		   System.out.println("Customer Username="+obj[3]);
		   System.out.println("Customer Location="+obj[4]);
		   System.out.println();
	   }
	   session.close();
	   sessionFactory.close();
   }
   
   public void AggregateFunctions()
   {
	   Configuration configuration=new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory=configuration.buildSessionFactory();
	   Session session=sessionFactory.openSession();
	   
	   Query q1 = session.createQuery("select count(*) from Customer");
	   List list1 = q1.list();
	   System.out.println("***count(*)***");
	   System.out.println("Total Records Count="+list1.get(0));
	   
	   Query q2 = session.createQuery("select sum(salary) from Customer");
	   List list2 = q2.list();
	   System.out.println("***sum(salary)***");
	   System.out.println("Total Salary="+list2.get(0));
	   
	   Query q3 = session.createQuery("select min(age) from Customer");
	   List list3 = q3.list();
	   System.out.println("***min(age)***");
	   System.out.println("Mininum Age="+list3.get(0));
	   
	   Query q4 = session.createQuery("select max(age) from Customer");
	   List list4 = q4.list();
	   System.out.println("***max(age)***");
	   System.out.println("Maximum Age="+list4.get(0));
	   
	   
	   Query q5 = session.createQuery("select avg(salary) from Customer");
	   List list5 = q5.list();
	   System.out.println("***avg(salary)***");
	   System.out.println("Average Salary="+list5.get(0));
	   
	   session.close();
	   sessionFactory.close();
   }
   
   public void updatedeletepostionalparameters()
   {
	   Configuration configuration=new Configuration();
	   configuration.configure("hibernate.cfg.xml");
	   
	   SessionFactory sessionFactory=configuration.buildSessionFactory();
	   Session session=sessionFactory.openSession();
	   
	   Transaction transaction=session.beginTransaction();
	   
	   Query q1 = session.createQuery("update Customer set name=?1,salary=?2 where id=?3 ");
	   q1.setString(1, "Soumya Prasad");
	   q1.setDouble(2, 100000.00);
	   q1.setInteger(3, 105);
	   
	   int count = q1.executeUpdate();
	   System.out.println(count+" Record(s) Updated");
	   
	   Query q2 = session.createQuery("delete from Customer where id=?1");
	   q2.setInteger(1, 105);
	   
	   int count1 = q2.executeUpdate();
	   System.out.println(count1+" Record(s) Deleted");
	   transaction.commit();
	   session.close();
	   sessionFactory.close();
   }
   
   public void updatedeletenamedparameters()
   {
	   Configuration configuration=new Configuration();
	     configuration.configure("hibernate.cfg.xml");
	     
	     SessionFactory sessionFactory=configuration.buildSessionFactory();
	     Session session=sessionFactory.openSession();
	     
	     Transaction transaction=session.beginTransaction();
	     
	     Query q1 = session.createQuery("update Customer set name=:v1 , salary =:v2 where id = :v3");
	     q1.setParameter("v1", "Sravan");
	     q1.setParameter("v2", 50000.00);
	     q1.setParameter("v3", 101);
	     
	     int count = q1.executeUpdate();
	     System.out.println(count + " Record(s) Updated");
	     
	     Query q2 = session.createQuery("delete from Customer where id=:v1");
	     q2.setParameter("v1", 104);
	     
	     int count2 = q2.executeUpdate();
	     System.out.println(count2 + " Record(s) Deleted");
	
	     transaction.commit();
	     session.close();
	     sessionFactory.close();
   }
   
   public void selectpostionalparameters()
   {
     Configuration configuration=new Configuration();
     configuration.configure("hibernate.cfg.xml");
     
     SessionFactory sessionFactory=configuration.buildSessionFactory();
     Session session=sessionFactory.openSession();
     
     Query qry = session.createQuery("from Customer where id=?1");
     qry.setInteger(1, 103);
     
     List<Customer> customerlist = qry.getResultList();
     System.out.println("Total Records : " +customerlist.size());
     for( Customer c : customerlist)
    	 System.out.println(c.toString());
     
     Query qry2 = session.createQuery("from Customer where username=?1 and password=?2");
     qry2.setParameter(1, "Soum1313");
     qry2.setParameter(2, "123");
     
     List<Customer> customerlist2 = qry2.getResultList();
     if(customerlist2.size()==1)
    	System.out.println(customerlist2.get(0));
     else 
    	 System.out.println("Invalid Credentials");
     
     session.close();
     sessionFactory.close();
   } 
   
   public void selectnamedparameters()
   {

     Configuration configuration=new Configuration();
     configuration.configure("hibernate.cfg.xml");
     
     SessionFactory sessionFactory=configuration.buildSessionFactory();
     Session session=sessionFactory.openSession();
     
     Query qry1 = session.createQuery("from Customer where id=:v");
     qry1.setParameter("v", 101);
     List<Customer> customerlist = qry1.getResultList();
     System.out.println("Total records = "+customerlist.size());
     for(Customer c: customerlist)
    	 System.out.println(c.toString());
     
     Query qry2 = session.createQuery("from Customer where username=:v1 and password=:v2");
     qry2.setParameter("v1","Soum1313");
     qry2.setParameter("v2", "123");
     
     List<Customer> customerlist2 = qry2.getResultList();
     if(customerlist2.size()==1)
     {
       System.out.println("Customer Record Found");
       System.out.println(customerlist2.get(0));
     }
     else
     {
       System.out.println("Customer Record Not Found");
     }
     
     session.close();
     sessionFactory.close();
   }
   
}











