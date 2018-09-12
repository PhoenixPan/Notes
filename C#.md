# C# - Build a ASP.Core Web Application

## MVC Configuration
1. Create a new project: New  - Project - ASP.NET WebApp (.NET.Core) - empty 
2. Add new libraries to "project.json"  
    ```
    "Microsoft.AspNetCore.Mvc": "1.0.0",
    "Microsoft.AspNetCore.StaticFiles": "1.0.0"
    ```
  
3. Create the pipeline by adding these two lines in the Configure method in startup.cs. The order matters. If static files is in the second place, the default page will not able to load static resources such as .css and .js files.  
    ```
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
          loggerFactory.AddConsole();

          if (env.IsDevelopment()) {
              app.UseDeveloperExceptionPage();
          }

          app.UseStaticFiles();
          app.UseMvcWithDefaultRoute();
      }
    ```
    
4. Add MVC to ConfigureServices method in startup.cs
    ```
    public void ConfigureServices(IServiceCollection services) {
        services.AddMvc();
    }
    ```
    
5. Create MVC folders "Models", "Views", and "Controllers" under project root. 
6. Create a controller by right clicking the Controllers folder - Add - New item - .NET.Core - MVC Controller Class. The default controller is named HomeController.cs  
7. Create a view by creating a new folder under Views folder. It should has the same name as the corresponding controller, in this case, "Home". Then add a "MVC View Page" in the folder, the default name is Index.cshtml. To have a shared layout, add a "Shared" folder under Views folder, add a new item "MVC View Layout Page". Link your static files to the layout. Then under the Views root, or anywhere you want the view, add a new item "MVC View Start Page".  
8. Create a class in Models. For example:  
    ```
    namespace PhoenixWeek3.Models {
        public enum TypeEnum {
            NEW,
            OLD
        }
        public class Course {
            // The database should have exact 3 columns
            public int CourseId { get; set; }
            public string Name { get; set; }
            public string Details { get; set; }
            public TypeEnum Type { get; set; }
        }
    }
    ```
9. Put and organize your static files under wwwroot.  

### Create a service
1. Create a "Services" folder under project root. Create a class as well as a corresponding interface. Follow the naming convensions. For example: 
    DataService.cs
    ```
    using PhoenixWeek3.Models;

    namespace PhoenixWeek3.Services {
        public class DataService : IDataService {
            public IEnumerable<Course> GetAllCourses() {
                return new List<Course> {
                    new Models.Course { CourseId = 1, Name = "C#", Details = "This is a C# course"},
                    new Models.Course { CourseId = 2, Name = "Andriod", Details = "This is an Android course"},
                    new Models.Course { CourseId = 3, Name = "Angular", Details = "This is an Angular course"} 
                };
            }
        }
    }
    ```
    IDataService.cs  
    public is required, since controller has to be able to access it.  
    ```
    using PhoenixWeek3.Models;

    namespace PhoenixWeek3.Services {
        public interface IDataService {
            IEnumerable<Course> GetAllCourses();
        }
    }
    ```
2. Add the service to the container, which is found in ConfigureServices method in startup.cs.  
    ```
    public void ConfigureServices(IServiceCollection services) {
        services.AddMvc();
        services.AddScoped<IDataService, DataService>();
    }
    ```

3. Implement **Dependency injection**. Add the service to the constructor of the controllder. Why? loose coupling. We never use a concrete class in controller. Objects are created in container (factory pattern).  
    ```
    private IDataService _dataService;
    public HomeController(IDataService dataService) {
        _dataService = dataService;
    }
    ```

#### Section summary
MVC request services through interface. When an interface is requested, the container creates an object and respond with it.    
![mvc1](https://user-images.githubusercontent.com/14355257/29198818-aa12ae7a-7e8a-11e7-8a8a-fa633e8bb254.PNG)  

Controllder's job is to get the object and pass to the view. That's it.   
```
public IActionResult Index() {
    IEnumerable<Course> data = _dataService.GetAllCourses();
    return View(data);
}
```
    
### Use data in View
1. Create a MVC View Import page for packages, so you can cite Course directly
    ```
    @using PhoenixWeek3.Models
    ```
    
2. Otherwise you have to use `IEnumerable<PhoenixWeek3.Models.Course>`, which is ugly
    ```
    @model IEnumerable<Course>
    @foreach(var item in Model)
    {
        <p>@item.CourseId</p>
        <p>@item.Name</p>
        <p>@item.Details</p>
        <br/>
    }
    ```
        
## MVVM: Model-View-ViewModel

### MVC
![mvc2](https://user-images.githubusercontent.com/14355257/29201709-286fb95c-7ea4-11e7-85e8-23c09dbbc959.PNG)

### MVVM
![mvvm1](https://user-images.githubusercontent.com/14355257/29201692-f30f3e2c-7ea3-11e7-894d-f2f4dc7cd0a9.PNG)

In case we have to services in home controller showed in the following code. How should we return multiple data? Here we introduce another module, View Model, where we save our objects. 
```
public class HomeController : Controller
{
    private IDataService _dataService;
    private IWelcomeService _welcomeService;

    public HomeController(IDataService dataService, 
                          IWelcomeService welcomeService) {
        _dataService = dataService;
        _welcomeService = welcomeService;
    }

    public IActionResult Index() {
        IEnumerable<Course> courses = _dataService.GetAllCourses();
        DateTime today = _welcomeService.GetToday();
        string msg = _welcomeService.GetWelcomeMessage();

        return View(courses);
    }
}
```

1. Create a new folder "ViewModels" under project root.  
2. Within the ViewModels folder, we create a new class.      
    ```
    // Naming: Controller name - Method name - View Model
    public class HomeIndexViewModel
    {
        public string message { get; set;}
        public DateTime todayDate { get; set; }
        public IEnumerable<Course> courses { get; set; }
    }
    ```
        
3. Now, go back to HomeController and change what we return.         
    ```
    public IActionResult Index() {

        HomeIndexViewModel vm = new HomeIndexViewModel
        {
            message = _welcomeService.GetWelcomeMessage(),
            courses = _dataService.GetAllCourses(),
            todayDate = _welcomeService.GetToday()
        };
        return View(vm);
    }
    ```
        
 4. Change the View      
     ```
     @model HomeIndexViewModel
    <h1>Home page</h1>

    <h2>@Model.todayDate</h2>
    <h2>@Model.welcomeMsg</h2>
    <table>
        @foreach (var item in Model.courses)
        {
            <tr>
                <td>@item.CourseId</td>
                <td>@item.Name</td>
                <td>@item.Details</td>
            </tr>
        }
    </table>
     ```
5. A view with perimeters:
    in controller:   
    ```
    public IActionResult Details(int id) {
        Product product = _dataService.getById(id);
        HomeDetailsViewModel vm = new HomeDetailsViewModel {
            name = product.name,
            details = product.details
        };
        return View(vm);
    }
    ```
    in index.cshtml:  `(displaying text, action, view, perimeter)`      
    ```
    <tr>
        <td>@item.id</td>
        <td>@item.name</td>
        <td>@Html.ActionLink("details", "Details", "Home", new { id = item.id})</td>
    </tr>
    ```
    in details.cshtml
    ```
    @model HomeDetailsViewModel

    <h1>Home Page</h1>
    <h2>Details</h2>

    <h3>Get product info by ID</h3>
    <p>Product Name: @Model.name</p>
    <p>Product Details: @Model.details</p>
    
    <p>@Html.ActionLink("Back","Index","Home")</p>
    ```


## Optimization
We can't write a new method for every kind of request, so we need a generic solution. Instead of writing the filters in View(.cshtml) or even the controller(wrong), we introduce a new Viwe Model to deal with it.

1. Create a new View Model. It's actually very similar to the previous View Model, but we need it for the concern of separation.  
    ```
    public class HomeCategoryViewModel
    {
        public IEnumerable<Product> categories { get; set; }
    }
    ```
2. Add the query method in Service class and interface
    DataService
    ```
    public class DataService : IDataService {
        public IEnumerable<Product> GetAllProducts() {...}
        
        public IEnumerable<Product> Query(Func<Product, bool> anyQuery) {
            return GetAllProducts().Where(anyQuery);
        }
    }
    ```
    IDataService
    ```
    public interface IDataService {
        IEnumerable<Product> GetAllProducts();

        IEnumerable<Product> Query(Func<Product, bool> anyQuery);
    }
    ```
 3. Create a separate action in HomeController. 
    ```
    public IActionResult Category() {
        HomeCategoryViewModel vm = new HomeCategoryViewModel {
            categories = _dataService.Query(p => p.Category != "").Distinct()
        };
        return View(vm);
    }
    ```
4. Essentially, we create a new action based on a new View Model with one sepcific query, for each type of request. 

## Form submissio
n
### Add new items
We will allow user to create a new object in a web form, then collect it and display it in the home index page, together with pre-made objects.  

1. In service and interface, create another method for adding an item:
    ```
    private static List<Product> _list;
    static DataService() {
        _list = new List<Product> {
            new Product { id = 1, name = "Product1", details = "Details of product 1", type = TypeEnum.NEW },
            new Product { id = 2, name = "Product2", details = "Details of product 2", type = TypeEnum.NEW },
            new Product { id = 3, name = "Product3", details = "Details of product 3", type = TypeEnum.OLD },
            new Product { id = 4, name = "Product4", details = "Details of product 4", type = TypeEnum.NEW },
            new Product { id = 5, name = "Product5", details = "Details of product 5", type = TypeEnum.OLD },
        };
    }

    public IEnumerable<Product> getAllProducts() {
            return _list;
        }

    public Product getById(int id) {
        return _list.FirstOrDefault(p => p.id == id);
    }
    
    public void add(Product p) {
        p.id = _list.Max(x => x.id) + 1;
        _list.Add(p);
    }
    ```
    
2. In HomeController, create methods to handle the request
    ```
    [HttpGet]
    public IActionResult Create() {
        return View();
    }

    [HttpPost]
    public IActionResult Create(HomeCreateViewModel vm) {
        if(ModelState.IsValid) {
            // map mv to Product
            Product p = new Product {
                name = vm.name,
                details = vm.details,
                type = vm.type
            };

            // pass to service
            _dataService.add(p);

            // go home
            return RedirectToAction("Index", "Home");
        }

        // if not valid ([Required])
        return View(vm);
    }
    ```
    
3. Create a View Model
    ```
    using System.ComponentModel.DataAnnotations;

    namespace PhoenixWeek4Last.ViewModels {
        public class HomeCreateViewModel {
            [Required] // needs DataAnnotations library
            [DataType(DataType.Text), MaxLength(5, ErrorMessage = "Length < 5")]
            public string name { get; set; }

            [Required]
            [DataType(DataType.Text), MaxLength(100)]
            public string details { get; set; }
            public TypeEnum type { get; set; }
        }
    }
    ```
    
4. Import a library in view improt page
    ```
    @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
    ```
    
5. Create a view
    ```
    @model HomeCreateViewModel

    <form asp-controller="Home" asp-action="Create" method="post">
        <div>
            <label asp-for="name"></label>
            <input asp-for="name" />
            <span asp-validation-for="name"></span>
            <br />
            <label asp-for="details"></label>
            <input asp-for="details" />
            <span asp-validation-for="details"></span>
            <br />
            <label asp-for="type">Type</label>
            <select asp-for="type" asp-items="@Html.GetEnumSelectList(typeof(PhoenixWeek4Last.Models.TypeEnum))"></select>
            <input type="submit" value="Save" />
        </div>
    </form>
    ```
### Edit existing item

1. Create a new View Model
    ```
    public class HomeEditViewModel {
        [Required] // needs DataAnnotations library
        [DataType(DataType.Text), MaxLength(10, ErrorMessage = "Length < 10")]
        public string name { get; set; }

        [Required]
        [DataType(DataType.Text), MaxLength(100)]
        public string details { get; set; }
        public TypeEnum type { get; set; }
    }
    ```
    
2. Create methods in HomeController
    ```
    // Get product info to input box   
    [HttpGet]
    public IActionResult Edit(int id) {
        Product product = _dataService.getById(id);
        HomeEditViewModel vm = new HomeEditViewModel {
            name = product.name,
            details = product.details,
            type = product.type
        };
        return View(vm);
    }

    [HttpPost]
    public IActionResult Edit(int id, HomeEditViewModel vm) {
        if (ModelState.IsValid) {
            Product product = _dataService.getById(id);
            product.name = vm.name;
            product.details = vm.details;
            product.type = vm.type;

            // go home
            return RedirectToAction("Index", "Home");
        }

        // if not valid ([Required])
        return View(vm);

    }
    ```
    
3. Create a View, which is the same as create a new item.  
4. Add new line in index page
    ```
    <td>@Html.ActionLink("edit", "Edit", "Home", new { id = item.id })</td>
    ```


# DB connection and manipulation

## Add json libraries
In dependencies:  
```
"Microsoft.EntityFrameworkCore.SqlServer": "1.0.0",
"Microsoft.EntityFrameworkCore.Tools": "1.0.0-preview2-final"
```
In tools: 
```
"Microsoft.EntityFrameworkCore.Tools": "1.0.0-preview2-final"
```

## Create classes for tables

Assume we have two classes for two tables
```csharp
public class Product
{
    public int productId { get; set; }
    public string name { get; set; }
    public double price { get; set; }

    // foreign key
    public int categoryId { get; set; }
}
```
```csharp
public class Category
{
    public int categoryId { get; set; } // every model has to have id
    public string name { get; set; }
    public string details { get; set; }
    public ICollection<Product> products { get; set; }
}
```

## Create a database: DBContext
which extends DbContect
```csharp
public class DbService: DbContext
{
    public DbSet<Category> tableCategory { get; set; }  // create table category with each variable as a column
    public DbSet<Product> tableProduct { get; set; }  // create table product with each variable as a column

    protected override void OnConfiguring(DbContextOptionsBuilder option)
    {
        // create a test database
        option.UseSqlServer("Server=(localdb)\\MSSQLLocalDB;Database=TestDb;Trusted_Connection=True"); 
    }
}
```

## Initialize the service
Open Package Control Console and enter `add-migration initMigration` to initialize the database. You can name migrations whatever you want. Then enter `update-database`. You should now be able to see the newly generated databse after you refreshed the database. Every time you make changes to the database, you need another migration, for example `add-migration anotherMigration`. Do not manually modify "Migration" folder.  



## Implement database operations in other service: DataService
```csharp
public class DataService<T>: IDataService<T> where T: class
{
    private DbService _context;
    private DbSet<T> _dbSet;

    public DataService()
    {
        _context = new DbService();
        _dbSet = _context.Set<T>();
    }

    public IEnumerable<T> getAll()
    {
        return _dbSet.ToList();
    }

    public void add(T entity)
    {
        _dbSet.Add(entity);
        _context.SaveChanges(); // commit changes
    }

    public T getSingle(Expression<Func<T, bool>> predicate) // allow us to obtain the object through any condition, not only PK
    {
        //return _context.Set<T>().FirstOrDefault(predicate);
        return _dbSet.FirstOrDefault(predicate);
    }

    public IEnumerable<T> getList(Expression<Func<T, bool>> predicate)
    {
        //return _context.Set<T>().Where(predicate);
        return _dbSet.Where(predicate);
    }
}
```

## Home controller to perform the operations

```csharp
public class CategoryController : Controller
{
    private IDataService<Category> _categoryService;
    private IDataService<Product> _productService;
    public CategoryController(IDataService<Category> categoryService, IDataService<Product> productService)
    {
        this._categoryService = categoryService;
        this._productService = productService;
    }

    [HttpGet]
    public IActionResult Add()
    {
        return View();
    }

    [HttpPost]
    public IActionResult Add(CategoryAddViewModel vm)
    {
        if (ModelState.IsValid)
        {
            Category c = new Category
            {
                name = vm.name,
                details = vm.details
            };

            _categoryService.add(c);
            return RedirectToAction("Index", "Home");
        }
        return View(vm);
    }

    [HttpGet]
    public IActionResult More(int id)
    {
        // retrieve data from records
        Category category = _categoryService.getSingle(c => c.categoryId == id);
        IEnumerable<Product> productList = _productService.getList(p => p.categoryId == id);

        CategoryMoreViewModel vm = new CategoryMoreViewModel
        {
            id = category.categoryId,
            name = category.name,
            details = category.details,
            products = productList,
            total = productList.Count()
        };
        return View(vm);
    }
  }
```
