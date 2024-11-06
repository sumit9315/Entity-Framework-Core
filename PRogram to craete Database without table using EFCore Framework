step 1: first create Custom DB context class

Creating MyDbContext.cs
------------------------------------------
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace EntityframeworkCoreDemo.Entities
{
    public class MyDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.LogTo(Console.WriteLine,LogLevel.Information);
            //base.OnConfiguring(optionsBuilder);
            optionsBuilder.UseSqlServer("Server=ADMINRG-;Database=EfCoreDb;User Id=sa;Password=Sil@7894;TrustServerCertificate=True");
        }


    }
}


step 2:create object of custom context class in program.cs class.
_______________________________________________________________
we can create DB using two way:
1)using migration
2)using context.Database.EnsureCreated() method.

using EntityframeworkCoreDemo.Entities;

namespace EntityframeworkCoreDemo
{
    public class Program
    {
        static void Main(string[] args)
        {
            try
            {
                using (var context = new MyDbContext())
                {
                    //context.SaveChanges();

                    context.Database.EnsureCreated();
                    Console.WriteLine("Database created.");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error:{ex.Message}");
            }
        }
    }
}

Output:

![image](https://github.com/user-attachments/assets/f9365f49-bbbb-4911-a58b-c51bf026864d)


Possible error:
-------------------
1)Error:A connection was successfully established with the server, but then an error occurred during the login process. (provider: SSL Provider, error: 0 - The certificate chain was issued by an authority that is not trusted.)

solution:
This error is related to SSL/TLS certificate validation. SQL Server is enforcing SSL encryption, but the certificate being used isn't trusted by your system. You can address this in two main ways: by either configuring Entity Framework Core to bypass certificate validation (not recommended for production) or by setting up a trusted certificate.

### Solution 1: Bypass Certificate Validation (Development Only)

You can disable SSL verification for development purposes by adding `TrustServerCertificate=True` to your connection string. This will tell SQL Server to trust the certificate, bypassing validation.

Update your connection string in `OnConfiguring`:


optionsBuilder.UseSqlServer("Server=ADMINRG;Database=EfCoreDb;User Id=sa;Password=Sil@7894;TrustServerCertificate=True");

> **Warning**: Disabling certificate validation is not recommended for production environments, as it reduces the security of the connection.

### Solution 2: Configure a Trusted Certificate (Recommended for Production)

1. **Obtain a Trusted SSL Certificate**: Get an SSL certificate from a trusted certificate authority (CA) or use one that's signed by a CA your system recognizes. 

2. **Install the Certificate**: Install this certificate on the SQL Server. If SQL Server is running on Windows, you can do this via the SQL Server Configuration Manager:
   - Open SQL Server Configuration Manager.
   - Navigate to **SQL Server Network Configuration** > **Protocols for [Your SQL Server Instance]**.
   - Right-click on **Properties**, then go to the **Certificates** tab.
   - Select your trusted certificate.

3. **Configure SQL Server to Require Encryption**:
   - In SQL Server Configuration Manager, go to the **Flags** tab of your server properties.
   - Set **Force Encryption** to **Yes**.

4. **Use Encrypted Connection**: In your connection string, set `Encrypt=True` to require an encrypted connection:

   ```csharp
   optionsBuilder.UseSqlServer("Server=ADMINRG-9LOHAID;Database=EfCoreDb;User Id=sa;Password=Silicon@7894;Encrypt=True");
   ```

After making these changes, restart your SQL Server instance and test the connection again.

By following these steps, you should be able to resolve the SSL Provider error and connect successfully.
