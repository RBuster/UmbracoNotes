# Challenges we might face, possible solutions, and some things to consider

[<- home](README.md)

## Media storage
**problem**: Umbraco stores media (images, documents, etc.) on the file system.  
**solution**: Someone has written a [blob storage provider](https://github.com/JimBobSquarePants/UmbracoFileSystemProviders.Azure) that allows the CMS to save files in an Azure Blob Storage Container. This provider also acts as a virtual path provider so your media appears as if it is being served up from the CMS, but it is really transparently being served from Azure.  
**things to think about**: The blob storage used for development still needs to be somehow copied over to a container used for the production site. At press time, Johnny has no idea how this can be done or if Azure has a utility for doing this easily.

## Template storage
**problem**: Umbraco stores templates on the file system.  
**solution**: Some things we generally consider to be *CMS work* will need to be completed locally and checked in to version control.  
**things to consider**: For clients who need to modify templates but also want to use Umbraco, we may need to develop a sort of hybrid workflow where the client has access to version control and the ability to check in their changes.

## Document type storage
**problem**: Umbraco creates strongly typed models which map to [document types](https://our.umbraco.org/documentation/tutorials/creating-basic-site/document-types) in the CMS. These models need to be compiled using MSBuild and have build artifacts (linked object code) created.  
**solution**: This is another thing that we would generally consider *CMS work* but will need to be completed locally and checked into version control.  
**things to consider**: As with the template storage problem, clients who want to modify document types will need to be integrated into a sort of workflow.
