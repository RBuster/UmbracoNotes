# Surface Controllers

## Intro
This is a basic intro to surface controllers

_a site is more than just displaying data_

> a surface controller is an MVC controller that interacts with the front-end rendering of an Umbraco page.

You'll need a bit of MVC knowledge.

A surface controller is used to render child action content and handle form data submission. A regular ASP.Net MVC controller, but inherits from `Umbraco.Web.Mvc.SurfaceController`. By inheriting, it is auto-routed and has instant access to Umbraco helper methods and properties.

## Creating the model
- model is the data we'll be working with
- model is a POCO
- model is data passed between view and controller  
For this example, we will use the class below as our model.

```csharp
public class ContactFormSubmission
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string Message { get; set; }
}
```

## Setting up the view
- view controls markup
- partial view. only the markup for the form
- create strongly typed view for better Intellisense

create partial view in `~/Views/Partials/`  
use MVC input extension methods (`Html.TextBoxFor`)  

## The surface controller

- Append "SurfaceController" to name of controller _by convention_
- Create as empty controller
- Always inherit from `Umbraco.Web.Mvc.SurfaceController`
- Index action returns the partial view created previously
```csharp
public class ContactFormSurfaceController : SurfaceController
{
    public ActionResult Index()
    {
        return PartialView("ContactForm", new ContactFormViewModel());
    }
}
```

## Handling posts
We need to:
- Update controller with a method to handle posts
- Update view to post to correct method.

### Adding post-handling method
Add new method to surface controller, returning an `ActionResult`. Add the `[HttpPost]` attribute to this new method to ensure that it can only do posts. This method should also accept a single parameter that is the same type as your model. 
```csharp
public class ContactFormController : SurfaceController
{
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Submit(ContactFormViewModel viewModel)
    {
        // Do something with the form submission here...

        return CurrentUmbracoPage();
    }
}
```


### Posting to correct method
Use `Html.BeginUmbracoForm` and use the type of your surface controller as the single type argument. The name of your method should be used as the `action` argument here.

We will use the Razor code below for our view:
```csharp
@model UmbracoApp.Models.ContactFormViewModel
@using UmbracoApp.Controllers
@using (Html.BeginUmbracoForm<ContactFormController>("Submit", FormMethod.Post))
{
    @Html.AntiForgeryToken()
    @Html.TextBoxFor(m => m.Name, new { @class = "form-control", id = "name", placeholder = "Name" })

    @Html.TextBoxFor(m => m.Email, new { @class = "form-control", id = "email", placeholder = "Email" })

    @Html.TextAreaFor(m => m.Message, new { @class = "form-control", id = "message", placeholder = "Message" })
    <input type="submit" name="Submit" value="Submit" class="round-button" />
}
```
We will place this view in a file located at `~/Views/Partials/ContactForm.cshtml`

We should now be posting to the correct method on our surface controller from our view.

## Validation
Server-side validation is done by using attributes on properties of the model.
Client-side validation is done with jQuery unobtrusive validation, just like with standard MVC. Our modified model looks like this now:
```csharp
using System.ComponentModel.DataAnnotations;
public class ContactFormViewModel
{
    [Required(ErrorMessage = "Name is required.")]
    public string Name { get; set; }

    [Required(AllowEmptyStrings = false, ErrorMessage = "Email is required.")]
    public string Email { get; set; }

    [Required(ErrorMessage = "A message is required.")]
    public string Message { get; set; }
}
```

## Using Surface Controllers on templates
```csharp
@Html.Action("Index", "ContactForm")
```

### Rendering chain
Partial View Macro File or Template  
&nbsp;&nbsp;calls -> `@Html.Action("Index", "ContactForm")`  
&nbsp;&nbsp;&nbsp;&nbsp;returns -> `~/Views/Partials/ContactForm.cshtml`  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;posts to -> `ContactFormSurfaceController.Submit()`

## Presenting a form submission status
We can use `TempData` to pass a submission status to the view. We can do something like below:
```csharp
// controller
TempData["submit-success"] = true;

// view
@if(TempData["submit-success"] == null) {
    // show form
}
else {
    // show a confirmation message
}
```
