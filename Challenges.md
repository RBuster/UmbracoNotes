# Challenges we might face, and possible solutions

## Media storage
**problem**: Umbraco stores media (images, documents, etc.) on the file system.  
**solution**: Someone has written a [blob storage provider](https://github.com/JimBobSquarePants/UmbracoFileSystemProviders.Azure)
that allows the CMS to save files in an Azure Blob Storage Container. This provider also acts as a virtual path provider so your media
appears as if it is being served up from the CMS, but it is really transparently being served from Azure.  
**things to think about**: The blob storage used for development still needs to be somehow copied over to a container used for the
production site. At press time, Johnny has no idea how this can be done or if Azure has a utility for doing this.
