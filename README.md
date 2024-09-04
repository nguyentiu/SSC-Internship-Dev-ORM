# SSC-Internship-Dev-ORM
# Hướng Dẫn Sử Dụng Entity Framework Core
## 1. Giới Thiệu Về Entity Framework Core
### 1.1. Entity Framework Core là gì?
Entity Framework Core (EF Core) là một ORM (Object-Relational Mapping) hiện đại, mã nguồn mở cho .NET. ORM là một kỹ thuật lập trình cho phép các nhà phát triển tương tác với cơ sở dữ liệu bằng cách sử dụng các đối tượng trong mã nguồn thay vì phải làm việc trực tiếp với các bảng và câu lệnh SQL. EF Core hỗ trợ các nhà phát triển trong việc xây dựng ứng dụng với các thao tác CRUD (Create, Read, Update, Delete) dễ dàng hơn, đồng thời duy trì sự linh hoạt và hiệu suất cao.

EF Core được phát triển bởi Microsoft và là phiên bản mới hơn của Entity Framework truyền thống, với nhiều cải tiến và hỗ trợ tốt hơn cho các ứng dụng hiện đại.

### 1.2. Tại sao sử dụng Entity Framework Core?
EF Core mang lại nhiều lợi ích vượt trội so với việc tương tác trực tiếp với cơ sở dữ liệu bằng cách sử dụng SQL thuần túy:

- Tăng tốc phát triển: EF Core tự động hóa nhiều tác vụ phức tạp như tạo, xóa, cập nhật bảng, do đó giúp giảm thiểu thời gian phát triển.

- Đơn giản hóa quản lý dữ liệu: Với EF Core, bạn có thể làm việc với cơ sở dữ liệu bằng cách thao tác trực tiếp trên các đối tượng (classes) trong .NET, giúp mã nguồn dễ hiểu và dễ bảo trì hơn.

- Độc lập với cơ sở dữ liệu: EF Core hỗ trợ nhiều loại cơ sở dữ liệu khác nhau như SQL Server, SQLite, MySQL, PostgreSQL, v.v. Điều này có nghĩa là bạn có thể chuyển đổi giữa các cơ sở dữ liệu mà không cần thay đổi nhiều trong mã nguồn của mình.

- Tính năng LinQ: EF Core hỗ trợ LinQ (Language Integrated Query), cho phép bạn viết các truy vấn phức tạp một cách dễ hiểu và an toàn.

- Hỗ trợ tốt cho môi trường đa nền tảng: EF Core có thể chạy trên các hệ điều hành khác nhau như Windows, macOS, và Linux, giúp mở rộng phạm vi ứng dụng của bạn.

### 1.3. Các Thành Phần Chính của Entity Framework Core
EF Core bao gồm nhiều thành phần quan trọng giúp quản lý các thao tác với cơ sở dữ liệu:

- DbContext: Là lớp trung tâm trong EF Core, đại diện cho phiên làm việc với cơ sở dữ liệu. Mỗi `DbContext` sẽ đại diện cho một cơ sở dữ liệu và được sử dụng để truy vấn cũng như lưu các thực thể (entities).

- DbSet: Mỗi `DbSet<TEntity>` trong `DbContext` đại diện cho một bảng trong cơ sở dữ liệu và cung cấp các phương thức để thêm, xóa, và truy vấn dữ liệu.

- Migrations: EF Core hỗ trợ quản lý schema của cơ sở dữ liệu thông qua tính năng Migrations, giúp bạn dễ dàng tạo, cập nhật hoặc xóa các bảng và cột mà không cần phải viết SQL thủ công.

- Entity Types: Đây là các lớp .NET mà EF Core sử dụng để ánh xạ đến các bảng trong cơ sở dữ liệu. Mỗi lớp đại diện cho một thực thể (entity) trong cơ sở dữ liệu.

- Relationships: EF Core hỗ trợ ánh xạ mối quan hệ giữa các thực thể như One-to-One, One-to-Many, và Many-to-Many, giúp bạn dễ dàng thiết lập và quản lý các liên kết giữa các bảng.

### 1.4. Cách Thức Hoạt Động của Entity Framework Core
EF Core hoạt động bằng cách ánh xạ (mapping) các lớp C# đến các bảng trong cơ sở dữ liệu và các thuộc tính (properties) của lớp đó đến các cột trong bảng. Khi bạn thao tác với đối tượng (object) của lớp trong C#, EF Core sẽ tự động dịch các thao tác đó thành các câu lệnh SQL tương ứng và thực thi chúng trên cơ sở dữ liệu.

- Querying Data: Bạn có thể truy vấn dữ liệu bằng cách sử dụng LINQ hoặc các phương thức như `Find`, `Where`, `FirstOrDefault`, v.v. EF Core sẽ chuyển các truy vấn này thành SQL và thực thi chúng trên cơ sở dữ liệu.

- Saving Data: Khi bạn thêm, sửa, hoặc xóa đối tượng trong C#, EF Core sẽ theo dõi các thay đổi đó và khi bạn gọi `SaveChanges()`, nó sẽ dịch các thay đổi này thành các câu lệnh SQL và gửi chúng tới cơ sở dữ liệu.

- Migrations: Khi bạn thay đổi cấu trúc của lớp (entity), bạn có thể sử dụng Migrations để cập nhật schema của cơ sở dữ liệu mà không mất dữ liệu.

## 2. Cài Đặt Entity Framework Core
Bạn có thể thêm EF Core vào dự án của mình bằng cách sử dụng NuGet Package Manager:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```
## 3. Cấu Hình Database Context
Tạo một lớp `ApplicationDbContext` kế thừa từ `DbContext` để cấu hình kết nối cơ sở dữ liệu:

```csharp
using Microsoft.EntityFrameworkCore;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    public DbSet<Student> Students { get; set; }
}

public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
```
## 4. Thiết Lập Kết Nối Database Trong appsettings.json
Thêm chuỗi kết nối vào file appsettings.json:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=EFCoreSample;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```
## 5. Đăng Ký DbContext Trong Program.cs
Đăng ký ApplicationDbContext trong Program.cs:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();
```
## 6. Thực Hiện Migration
Sử dụng Migration để tạo bảng trong cơ sở dữ liệu:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```
## 7. CRUD Operations với EF Core
CRUD (Create, Read, Update, Delete) là các thao tác cơ bản mà bất kỳ ứng dụng nào cũng phải thực hiện trên cơ sở dữ liệu. Chúng ta sẽ tạo các thao tác này trong một lớp `StudentService`, nơi chúng ta thực hiện logic nghiệp vụ.

Step 1: Tạo Model `Student`
Trước tiên, chúng ta cần tạo lớp `Student` trong thư mục `Models`:

```csharp
// Models/Student.cs
public class Student
{
    public int Id { get; set; }       // Khóa chính
    public string Name { get; set; }  // Tên sinh viên
    public int Age { get; set; }      // Tuổi sinh viên
}
```
Step 2: Tạo Database Context
Tạo lớp `ApplicationDbContext` trong thư mục `Data` để đại diện cho phiên làm việc với cơ sở dữ liệu:

```csharp
// Data/ApplicationDbContext.cs
using Microsoft.EntityFrameworkCore;
using SampleBackendApp.Models;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    public DbSet<Student> Students { get; set; }  // Đại diện cho bảng Students trong cơ sở dữ liệu
}
```
Step 3: Cấu Hình Database Connection trong `appsettings.json`
Thêm chuỗi kết nối vào file `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=EFCoreSample;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```
Step 4: Đăng Ký DbContext Trong Program.cs
Đăng ký ApplicationDbContext trong Program.cs:

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Đăng ký DbContext với chuỗi kết nối từ appsettings.json
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Đăng ký các service khác
builder.Services.AddControllers();

var app = builder.Build();

// Cấu hình ứng dụng
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
app.Run();
```
Step 5: Tạo `StudentService` để thực hiện CRUD Operations
Tạo lớp `StudentService` trong thư mục `Services` để xử lý logic nghiệp vụ:

```csharp
// Services/StudentService.cs
using Microsoft.EntityFrameworkCore;
using SampleBackendApp.Models;
using SampleBackendApp.Data;

public class StudentService
{
    private readonly ApplicationDbContext _context;

    public StudentService(ApplicationDbContext context)
    {
        _context = context;
    }

    // Create - Thêm mới một sinh viên
    public async Task<Student> CreateStudentAsync(Student student)
    {
        _context.Students.Add(student);
        await _context.SaveChangesAsync();  // Lưu thay đổi vào cơ sở dữ liệu
        return student;
    }

    // Read - Lấy danh sách tất cả sinh viên
    public async Task<List<Student>> GetAllStudentsAsync()
    {
        return await _context.Students.ToListAsync();  // Lấy tất cả sinh viên từ bảng Students
    }

    // Update - Cập nhật thông tin sinh viên
    public async Task<Student> UpdateStudentAsync(int id, Student updatedStudent)
    {
        var student = await _context.Students.FindAsync(id);
        if (student != null)
        {
            student.Name = updatedStudent.Name;
            student.Age = updatedStudent.Age;
            await _context.SaveChangesAsync();  // Lưu thay đổi vào cơ sở dữ liệu
        }
        return student;
    }

    // Delete - Xóa sinh viên khỏi cơ sở dữ liệu
    public async Task<bool> DeleteStudentAsync(int id)
    {
        var student = await _context.Students.FindAsync(id);
        if (student != null)
        {
            _context.Students.Remove(student);
            await _context.SaveChangesAsync();  // Lưu thay đổi vào cơ sở dữ liệu
            return true;
        }
        return false;
    }
}
```
Step 6: Tạo `StudentsController` để Giao Tiếp với API
Tạo `StudentsController` trong thư mục `Controllers` để xử lý các yêu cầu HTTP:

```csharp
// Controllers/StudentsController.cs
using Microsoft.AspNetCore.Mvc;
using SampleBackendApp.Services;
using SampleBackendApp.Models;

[ApiController]
[Route("api/[controller]")]
public class StudentsController : ControllerBase
{
    private readonly StudentService _studentService;

    public StudentsController(StudentService studentService)
    {
        _studentService = studentService;
    }

    // POST: api/Students - Tạo mới sinh viên
    [HttpPost]
    public async Task<IActionResult> CreateStudent([FromBody] Student student)
    {
        var createdStudent = await _studentService.CreateStudentAsync(student);
        return CreatedAtAction(nameof(GetStudentById), new { id = createdStudent.Id }, createdStudent);
    }

    // GET: api/Students - Lấy tất cả sinh viên
    [HttpGet]
    public async Task<IActionResult> GetAllStudents()
    {
        var students = await _studentService.GetAllStudentsAsync();
        return Ok(students);
    }

    // GET: api/Students/{id} - Lấy thông tin sinh viên theo ID
    [HttpGet("{id}")]
    public async Task<IActionResult> GetStudentById(int id)
    {
        var student = await _studentService.UpdateStudentAsync(id, student);
        if (student == null)
        {
            return NotFound();
        }
        return Ok(student);
    }

    // PUT: api/Students/{id} - Cập nhật thông tin sinh viên
    [HttpPut("{id}")]
    public async Task<IActionResult> UpdateStudent(int id, [FromBody] Student student)
    {
        var updatedStudent = await _studentService.UpdateStudentAsync(id, student);
        if (updatedStudent == null)
        {
            return NotFound();
        }
        return Ok(updatedStudent);
    }

    // DELETE: api/Students/{id} - Xóa sinh viên
    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteStudent(int id)
    {
        var success = await _studentService.DeleteStudentAsync(id);
        if (!success)
        {
            return NotFound();
        }
        return NoContent();
    }
}
```
Giải Thích
- Model `Student`: Định nghĩa lớp `Student` đại diện cho bảng `Students` trong cơ sở dữ liệu.
- ApplicationDbContext: Đại diện cho phiên làm việc với cơ sở dữ liệu, kết nối và thao tác với bảng `Students`.
- Cấu hình DbContext: Cấu hình chuỗi kết nối và đăng ký `ApplicationDbContext` trong `Program.cs`.
- StudentService: Thực hiện các thao tác CRUD với cơ sở dữ liệu thông qua ApplicationDbContext.
- StudentsController: Cung cấp các API endpoint để tạo, đọc, cập nhật và xóa sinh viên.
