# Development Notes

[<- home](README.md)

## Content

All "content" on the site is treated basically the same.

Content Starts as a document type and then is made into what it needs to be (Page, Content from database etc.)

Document types can come with pre-made umbraco templates, which are all MVC based. 
For example, you can make a doc type of master, have it have a master template with navigation, header and such. Then create a doc type of home page with a template, and have that home page inherit from the master page doc type.

On the front end pane you will see the master page as content, which can be annoying coming from something liek sitefinity, but you can have the master pages url redirect to the home page to give the same user experience. 

## Customization

Everything in umbraco is customizable with typical MVC and .Net standards. Controllers can be hijacked for custom implementation, and partials rendered in the same fashion

Umbraco is MVC first, with the application layered on. This means that there are no real layers of applicaiton to work through to get custom implementations through. 

While Umbraco does offer an API, it is familiar to MVC developers and is built off the back of traditional MVC framwork ideas.

Customization is all over the place with Umbraco, and could take up weeks of talks about it. From normal css and js, to back end screens it all follows the same pattern. 

## Data Storage (Dynamic Modules for the sitefinity dev)

Data storage works in a similar way to sitefinity. A user will create a document type (as referenced above) and then build what they need in the doc type for data storage fields. 

After a user has created a doc type, they need to create a content item. Navigating to content, they can create it at the root (if they have given permission for that), or under another item. This is useful for things like categories or banner items as they can be grouped nicely and hidden from the navigation of the site at a top level.

This can be accessed just like any other piece of content. All content has an ID (int) and is shown in the backend. For a developer, that just means using the correct methods and interfaces to retrive that content, and then treat it as a regular object, given the correct configurations.

## Configurations

Umbraco can be configured in a billion different ways. For the sake of this document not going off the rails, I will focus mainly on the configuring of data storage.

Umbraco has a neat way of constucting concrete classes based on document types. In the web.config you can set the Umbraco.ModelsBuilder.ModelsMode value to LiveDll, and Umbraco will convert your dynamic document type to a concrete class for use in server side code.

This works just as easily as you would expect it to. Simply get the object you need, and dot off for the properties you want to access. There are a few properties like documents and images that will need extra helpers involved, but that is the case pretty much everywhere. Get used to it.

There are other ways, like regular Dll mode, but that means the developer will need to generate the dll themselves after a doc type has been created. 

## Add ons

Perhaps the best thing about Umbraco is how light weight it is. On a clean install, you get exactly that, a completely clean install. That means no snazzy features like blogs and news, but all of that is plug-in-able as you need it. So if you need a single page app with nothing fancy, you can get exactly that with nothing more slowing you down.

Umbraco has a market place, both in the backend of the application itself, as well as on the web. Users can create their own packages for private or public use, and anyone can download packages for their own use. The power of open source.

## Styling and Scripting

While you can do themes in Umbraco, the easier way (for dev at least) is to create your sheets and insert them where you need it.

Using @RenderSection works just like in MVC and bundling/minifying works as well. Nothing fancy to do or install for it to work, outside of vs extensions. 

## Pricing

Umbraco is free, for everyone. 

There really doesn't need to be much more here, it's free, straight up. 

Umbraco does have paid plans for upgrades and hosting, as well as enterprise support plans for much cheaper than other .Net cms's.

## Hosting/Build servers
Since Umbraco is based on a single NuGet package, it is quite simple to integrate into a continuous-integration pipeline. There is no special package source that needs to be configured.

Likewise, Umbraco works well on Azure and seems to have no problems.
