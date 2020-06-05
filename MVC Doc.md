# **MVC Doc**

⚠️ Work In Progress ⚠️

## Action Methods

All the methods that are present inside the controller are by default treated as action methods.
An action method typically executes whenever the browser sends request.

## App_Start

It Contains the files that needs to be executed on the first request.
When we send a first request from the browser all the files under App_Start folder gets executed.
They should be called from the Application_Start() method of Global.asax file.

## App_Data

It contains SQL Server LocalDb Database file.<br>
App_Data folder can contain application data files like LocalDB, .mdf files, xml files and other data related files. 
IIS will never serve files from App_Data folder.

## Views/web.config

This file contains settings for all the views. Generally we will use this file to add the necessary namespaces that are needed to be imported in all the views by default.

## GLobal.asax

This file contains all the application level and session level events.

## packages.config

This file contains list of nuget packages currently installed in the project.

## web.config

Contains configuration settings that are related to the entire project.

## ActionResult

ActionResult is a class, that represents `"result of an action method"`.<br>
AspNet MVC recemmends to specify action methods return type as `Action Result`.
Action Result is an abstract class that has several child classes, you can return an object of any of the child class.

Child classes are:

- ContentResult
- ViewResult
- FileResult
- PartialViewResult
- RedirectResult
- RedirectToRouteResult
- JsonResult

### ContentResult

Used to send plain text or custom information to the browser.

```c#
 public ActionResult GetEmpName(int EmpId)
    {
        var employees = new[]
        {
            new {EmpId = 1, EmpName = "Scott"},
            new {EmpId = 2, EmpName = "Mike"},
            new {EmpId = 3, EmpName = "John"},
        };

        string matchEmpName = null;
        foreach(var item in employees)
        {
            if(item.EmpId == EmpId)
            {
                matchEmpName = item.EmpName;
            }
        }
        return Content(matchEmpName, "text/plain");
    }
```

### ViewResult

Used to send rendered html output of the view to the browser.

### FileResult

Used to send the content of the file to the browser.

```c#
 public ActionResult GetPaySlip(int EmpId)
    {
        string fileName = "~/PaySlip" + EmpId + ".pdf";
        return File(fileName, "application/pdf");
    }
```

### PartialViewResult

Used to send content of a partial view to an action method.

### RedirectResult

Used to redirect to outside of the website.

```c#
 public ActionResult GetRedirectUrl(int EmpId)
    {
        string fbLink = "https://www.facebook.com/emp" + EmpId;
        return Redirect(fbLink);
    }
```

Browser ---request1---> Server
<---response1--- 302 redirect
----request2--->  
 <---response2--- 200 Ok

Browser requests to server, and server sends 302 redirect link. Browser sends request to new link and server responds with 200.

### RedirectToRouteResult

Used to redirect to specific action method.
This class object reperents redirection from one action method to another action method.<br>
It sends HTTP 302 response to the browser, so the browser sends another request to the specific action method.

```c#
  public ActionResult Login(string Username, string Password)
    {
        if(Username == "admin" && Password == "manager")
        {
            return RedirectToAction("Dashboard", "Admin");  // Dashboard action method is in another controller, so we have to specify the controller name
        } else
        {
            return RedirectToAction("InvalidLogin"); // InvalidLogin is in same controller, so no need of controller name.

        }
    }

    public ActionResult InvalidLogin()
    {
        return View();
    }
```

### JsonResult

Used to send an object that is converted as json.

## Razor View Engine

View Engine provides a set of syntaxes to write c# code in the view. View engine is also responsible to render the view as html.<br>
ASP.Net MVC supports two type of view engine

- ASPX View Engine
- Razor View Engine

## Razor Expression

Razor expressions start with `@` followed by C# code:

```html
<div>@ViewBag.Title</div>
<p>@DateTime.Now</p>
```

With the exception of the C# `await` keyword, implicit expressions must not contain spaces. If the C# statement has a clear ending, spaces can be intermingled:

```html
<p>@await DoSomething("hello", "world")</p>
```

## Razor If Statement

```html
@{ var price=50; }

<html>
  <body>
    @if (price>30) 
    {
      <p>The price is too high.</p>
    } 
    else {
      <p>The price is OK.</p>
    }
  </body>
</html>
```

Here if and else block must contain `{}` , event if there is only a single statement inside the blocks.

## Razor For Loop

``` html
@{
    Layout = null;
    int repeat = 5;
}

<html>
    <body>
        <ul>
            @for(int i = 1; i <= repeat; i++)
            {
                <li>@i</li>
            }
        </ul>
    </body>
</html>
```

## Razor foreach

```c#
@{
    string[] peoples = new string[5] { "John", "Jane", "Mark", "Kevin", "Scott" };
}

<html>
<body>
    <div> 
        <ul>
           @foreach(string person in peoples)
           {
               <li>@person</li>
           }
        </ul>

    </div>
</body>
</html>
```

## Shared View

Shared views are present in the `Views\Shared` folder.
Shared views are the views that can be called from any controller of the entire project.
The view that belongs to all the controller are created as shared view.<br>
When we call a view, it checks for the view in the `Views\Controllername` folder forst, if not found, it will search in the `Views\Shared` folder.

## Layout View

### Sharing data from view to layout view

 -
 -

### Section in layout view

 - Sections are used to display view specific content in the layout view.
 - Sections are defined in the view and rendered in layout view.

 > In View

 ``` c#
@section sectionname
{
    content here
}
 ```

> In Layout View

``` c#
@RenderSection("sectionname")
```

 - If we want to make the section optional (ie, display in case of some view and don't display on other views), then we can give `required:false` as second parameter of `@RenderSection`.

 ``` c#
@RenderSection("sectionname", required:false)
 ```

## _ViewStart.cshtml

 - It defined the default layout view of all the views.
 - It contain only one statement ie, layout path.
 - It cannot contain any other code.
 - Its name is fixed, we cannot change it.

> In `_ViewStart.cshtml`
 ```
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
 ```

 - If we specify the path to layout in `_ViewStart.cshtml` we don't need to specify the path to the layout view in each of the controller view.
 - If the `_ViewStart.cshtml` is present inside Views folder, it will be applied to all the views.
 - If the `_ViewStart.cshtml` is present in `subfolder` of the `Views` folder then it will be applied to only the view of that subfolder.
 - If `_ViewStart.cshtml` is present in `Views` folder as well as `Views\subfolder` then `_ViewStart.cshtml` present in subfolder will be applied to the views of subfolder and `_ViewStart.cshtml` present  in the `Views` folder will be applied to the rest of the views.

 ## Partial Views

 - Partial view is a small view that contains content that can be shared among multiple views.
 - Can be present in `Views\Controllername` folder or in `VIews\Shared` folder.
 - Partial view cannot contain the layout page.
 - Partial view doesn't contain any html structured element such as html, head, body etc. Beacuse it is always called inside the view.

First Create a partial view then invoke it inside the view.
> Inside View
 ```
@{
    Html.RenderPartial("PartialViewName")
}
 ```

## Attribute Routing

- New and preffered way of routing.

``` c#
["url"]
public ActionResult MethodName()
{

}
```

- Applicable for specific action method. Each action method must have `Route` defined.
- Should be enabled by using: <br>
    &nbsp; &nbsp;&nbsp; &nbsp; `routes.MapMvcAttributesRoutes();`
- Also supports parameters and constraints.

Example: <br>

> In RouteConfig.cs
``` c#
routes.MapMvcAttributesRoutes();
```

> In controller

``` c#
public class ProductsController : Controller
    {
        [Route ("Products/Details/{id:int?}")]
         public ActionResult Details()
        {
            return View();
        }
    }
```

## Models

- Model is a class that defines the structure of the data that you want to store/display.
- Also contains business logic.
- Model will be called by Controller and View.

> View Model
``` c#
public class ViewModel 
{
    public dataType propertyName { get; set; }
}
```
Represents the structure of the data that you want to display to user.

> Domain Model
``` c#
public class DomainModel 
{
    public dataType propertyName { get; set; }
}
```
Represents the structure of the data that you want to store in the database table.

> Service Model
``` c#
public class ServiceModel 
{
    public dataType methodName {...}
}
```
Represents business logic (code that needs to be exicuted before inserting/ updating).

## Model Binding

- 

## Bind Attribute

-

## Custom Model Binding

-

## Entity Framework

### DbContext and DbSet

- DbContect is a class based on which you can write LINQ queries to perform CRUD operations on table.
- DbCOntext is a collection of DbSet's.
- DbSet object that represents a table.
- DbSet provides a set of methods such as `WHERE`, `ORDER BY` etc, to manipulate the tables.
- Generally for 1 database we need to create 1 DbContext and for 1 table we need to create 1 DbSet.

## Calling Stored Procedures using EF

> Step 1: Create procedure in SQL Server:

``` sql
create procedure getProductsByBrandID
(@BrandID bigint)
as begin
	select * from Products where BrandID=@BrandID
end
```

> Step 2: Invoke stored procedure directly using the object of DbContext:

``` c#
SqlParameter[] sqlParameters = new SqlParameter[]
    {
        new SqlParameter("@BrandID", 2)
        //you can add more parameters here
    };
    List<Product> products = db.Database.SqlQuery<Product>("exec getProductsByBrandID @BrandID", sqlParameters).ToList();
```

## DbFirstApproach

-

## CodeFirstApproach

-

## HTML Helper

HtmlHelper class generates html elements using the model class object in razor view. It binds the model object to html elements to display value of model properties into html elements and also assigns the value of the html elements to the model properties while submitting web form. So always use HtmlHelper class in razor view instead of writing html tags manually.

> In server

``` c#
@Html.TextBoxFor(model => model.EmpName)
```

> In Browser

``` html
<input type="text" name="EmpName">
```

For more [click here](https://www.tutorialsteacher.com/mvc/html-helpers).

### List of HtmlHelper methods 
| HtmlHelper        | Strongly Typed HtmlHelpers | Html Control          |
| ----------------  |:-------------:             | -----:                |
| Html.ActionLink   | -                          | Anchor link           |
| Html.TextBox      | Html.TextBoxFor            | Textbox               |
| Html.TextArea     | Html.TextAreaFor           | TextArea              |
| Html.CheckBox     | Html.CheckBoxFor           | Checkbox              |
| Html.RadioButton  | Html.RadioButtonFor        | Radio button          |
| Html.DropDownList | Html.DropDownListFor       | Dropdown, combobox    |
| Html.ListBox      | Html.ListBoxFor            | multi-select list box |
| Html.Hidden       | Html.HiddenFor             | Hidden field          |
| Password          | Html.PasswordFor           | Password textbox      |
| Html.Display      | Html.DisplayFor            | Html text             |
| Html.Label        | Html.LabelFor              | Label                 |
| Html.Editor       | Html.EditorFor             | Generates Html controls based on data type of specified model property e.g. textbox for string property, numeric field for int, double or other numeric type. |

---

The difference between calling the HtmlHelper methods and using an html tags is that the HtmlHelper method is designed to make it easy to bind to view data or model data. <br>	

Example :- 

>Using HTML Helper

``` c#
@Html.TextBox("Search", ViewBag.search as string, new { @class = "form-control mr-sm-2", placeholder = "Search" })
```

> Normal HTML

``` html
<input class="form-control mr-sm-2" type="search" placeholder="Search" name="search" value="@ViewBag.search">
```

Example 2 :- 

>Using HTML Helper

``` html
<td>@Html.DisplayFor(temp => item.ProductName)</td>
```

> Normal HTML

``` html
 <td>@item.ProductName</td>
```

## Client side validation

ASP.NET MVC uses DataAnnotations attributes to implement validations. DataAnnotations includes built-in validation attributes for different validation rules, which can be applied to the properties of model class.

<!-- ### Need 3 packages

- jQuery
- jQuery.Validation
- Microsoft.jQuery.Unobtrusive.Validation -->

> Step 1 :- First of all, apply DataAnnotation attribute on the properties of Student model class.

``` c#
public class Student
{
    [Required]
    public string StudentName { get; set; }   
}
```

> Step 2 :- In Post Action method  inside Controller add a condition checking ModelState.IsValid

``` c#
[HttpPost]
public ActionResult Edit(Student std)
{
    if (ModelState.IsValid) { 
    
        //write code to update student 
    
        return RedirectToAction("Index");
    }
    
    return View();
}
```

> Step 3 :- In View add a validation message using HTML Helper

``` c#
 @Html.ValidationMessageFor(model => model.StudentName, "", new { @class = "text-danger" })
```

### Custom error message

Use the ErrorMessage parameter of the DataAnnotation attributes to provide your own custom error message as shown below.

``` c#
public class Student
{
    [Required(ErrorMessage="Please enter student name.")]
    public string StudentName { get; set; }
}
```

Also, you can specify a message as a second parameter in the ValidationMessage() method as shown below.

``` c#
@Html.ValidationMessageFor(m => m.StudentName, "Please enter student name.", new { @class = "text-danger" })
```

### Validatio summary

The ValidationSummary can be used to display all the error messages for all the fields.
It can also be used to display custom error messages.

>  ValidationSummary should be inside the form.

``` c#
@Html.ValidationSummary()
```


For more [click here](https://www.tutorialsteacher.com/mvc/implement-validation-in-asp.net-mvc).

## ASPNET Identity

- ASPNET Identity is a framework to store and manage user accounts in web application.
- It has four major component / packages.

 1. Microsoft.AspNet.Identity.Core

 2. Microsoft.AspNet.Identity.EntityFramework

 3. Microsoft.AspNet.Identity.Owin

 4. Microsoft.Owin.Host.SystemWeb



























⚠️ Work In Progress ⚠️