# Entity-Framework-Core

Problem with ADO .NET:
- we have to create conncyion object.
  open the connection object.
  create the command object.
  Execute the commnad 
  Manage the transaction 
  Retrieve the data
  Close the connection

These things are done by DbContext class object in EFcore and we don't have to do it manually ny writing code..

Entity Framework  core is  a data access technology provided by Microsoft.
It is an ORM(object relation mapping) tool.

SQL server uses sql  language to create relational databse.
C# uses object oriented programming language to create object.

EF CORE IS TOOLS WHICH CREATES MAPPING BETWEEN C# OBJECT TO sql SERVER TABLE (rELATIONAL DATABSE)
for this ,EF core provides two packages :1) for sql server(EF CORE) DB provider to connect with 2) EF CORE TOOL to create migration so that keep in sync with sql sever db.

Code 1st approach:
========================
1)Create a model/Entities class.
-----------------------
ex. student,brnach,address class for which we need to craete db table in db

  public class student
  {
    
  }

2)identify primary and foriegn key in model i.e refernace navigation property and collection navigation property
-------------------------------

  class student:
  
  A student can belong to a single branch=====>one to one relation
  -------------
  => Reference navigation property and this model will contains foriegn key.
  public  Branch Branches{get; set;} ==> property of other class type(Branch class)
    here branch is model name
        branches is table name

        foriegn key will be **Branch Id** i.e primary key of Branch table

class branch:

  A branch can have many student.--> one to many
  -----------
  =>Collection Navigation property andf this model will have primary key.
  public list<student> students{get;set;}

  here student is model name
      students is table name
      
3)Now create a custom DB context class
-----------------
i create a class with proper name.
  **ex. EFcoreDbContext.cs**

add **Microsoft.EntityframeworkCore** namespace  which contain DbContext class.
  **using Microsoft.EntityframeworkCore**

ii Inherit the class from DbContext class
  **public class EFcoreDbContext : DBContext**
    {
    }

iii Now we need to create connection between DB and our DOT NET application.For this DbContect class has **OnConfiguring** method which is virtual and of void type(as it create connection and doesn't return anything) so we need to overide the **OnConfiguring** menthod and it takes one parameter of **DbContextOptionBuilder** class object and **DbContextOptionBuilder** class has a method **UseSqlServer**(provider specific) which takes connection string to connect to db.
ex.
  
    public class EFcoreDbContext : DBContext
  
      {
      
        protected override void OnConfiguring(DbContextOptionBuilder Builder)
        
        {
        
            builder.UseSqlServer("connection string)
            
        }
        
      }

iv Now connection is made.

v Now we need to create  table .
for this we have to DbSet property of DbContext class.
ex. public DbSet <student> students{get;set;}

it will create a dbo.Students table in databse from student model and all property of student model will be column of students table.


vi Program to create a student table in sql server db.
----------------------------------------------------
student.cs
-------------

    public class student

      {
         public int Id{get;set}
         
         public string? Name {get;set;}
      }

EFcoreDbContext.cs
-----------------

  using Microsoft.EntityframeworkCore

    public class EFcoreDbContext : DBContext

    {
    
      protected override void OnConfiguring(DbContextOptionBuilder Builder)
      
      {
      
          builder.UseSqlServer("connection string)
          
      }
  
      public DbSet <student> students{get;set;}
      
    }

Vii Now before creating table in DB ,create migration
        add-migration migration_name

viii Now to add table update migration 
        update migration


  ![image](https://github.com/user-attachments/assets/697e7dc0-ed5f-475f-a820-3d5eef4762fe)

 ![image](https://github.com/user-attachments/assets/a90d4c0f-57e6-4d84-8f85-5cd34c809205)






      


  
