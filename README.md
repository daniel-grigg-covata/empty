# Covata .NET SDK #

Windows .NET SDK for the [Covata Platform](https://www.covata.com/products/).
Supports .NET 4.5 and higher.  The SDK makes it simple for .NET developers 
to access all of the Covata Platform REST API features.

As an example, the following lines create a secure object, upload it and share it 
with two collaborators in a single step:

```c#
api.CreateAndUploadSecureObject(new SecureObjectFileArgs() {
    Source = unprotectedFile, 
    AddCollaborators = new List<Collaborator>() { new Collaborator("alice@email_domain.org") }
    });
```


## Getting started


### Pre-requisites

Before you can utilise the Covata Platform via the SDK you'll need:

+ A Covata Platform instance.  
  - If you a new user, you can register for free at [Register for SafeShare](http://go.covata.com/l/33552/2014-03-13/7xsc).
  - For existing Covata Cloud deployments this will be [covata.io](https://covata.io). 
  - For on-premise deployments/hybrid deployments consult your system administrator.
+ A registered client application on your Covata platform
+ One or more registered users.


### Installation

To install the Covata.Api, run the following command in the Package Manager Console

    PM> Install-Package Covata.Api

Alternatively add the package via 'Manage NuGet Packages...' in Visual Studio.


### Quick Usage Guide

The following gives a short motivational example demonstrating the ease 
of use afforded by the SDK.

```c#

    // Instance the API
    var api = new CovataApiClient("YOUR-SERVER-URL", 
        new GrantPassword(APP_CLIENT_ID, APP_CLIENT_SECRET));

    // Sign-in as an originator to create some content.
    api.Authorize(new PasswordCredentials( USER_NAME, USER_PASSWORD));

    // Creating an objects
    var myObject = api.CreateSecureObject(new SecureObjectArgs {
        Name = "my-object"});

    // Encrypt a file and link its content to the object.
    // Nothing's been uploaded yet.
    myObject = api.UpdateSecureObjectFile(
                myObject,
                new SecureObjectFileArgs {
                    Source = "important-document",
                    Name = "important-document"
                });

    // Share the object with a collaborator, giving them download access.
    myObject = api.UpdateSecureObject(myObject,
        new SecureObjectArgs {
            AddCollaborators =
                new List<Collaborator> {new Collaborator("alice@somewhere.org")},
            AccessControls = 
                new AccessControls {AllowDownload = true, AllowPrint = true}
    });

    // Upload an object's encrypted content
    api.UploadSecureObject(myObject.Id, "important-document.cov");
    
    // Creating a collection
    var myCollection = api.CreateCollection(new CollectionArgs {
        Name = "my-collection"});
    
    // Move an object to a collection
    api.UpdateCollectionAsync(myCollection, new CollectionArgs {
        Name = "my-collection-with-objects"}); 

    // Signin as a collaborator and download
    api.Authorize(new PasswordCredentials("alice@somewhere.org", "its-a-seekret"));
    
    // Download a shared object
    api.DownloadSecureObjectContent(sharedItem.Id, "important-document");  

```


### Examples

The examples solution underneath the topmost folder of the sdk-csharp
repository contains a number of different examples to further help you
get started with the SDK:

+ HelloWorld - Uses the helper API to create & upload and download an object.
+ WorldTour - Quick tour of many of the file-based API functions.


### Tests

The SDK comes with an extensive set of acceptance tests under 
Covata.Api.AcceptanceTests.  The tests are written using the 
[Specflow BDD testing framework](http://www.specflow.org).  
The tests are also runnable via the builtin test runners.

More extensive documentation on the SDK and the Covata Platform 
can be found [here](https://www.covata.com/doc/).


## Dependencies

The dependencies are automatically resolved by Nuget.

These include:

+ [RestSharp](http://restsharp.org)
+ [ServiceStack](https://servicestack.net)
+ [WebSocket4Net](https://websocket4net.codeplex.com)
+ [FParsec](http://www.quanttec.com/fparsec/)


## Contact

For information regarding the SDK please contact the authors:

daniel.grigg@covata.com
Dimitri.Koubaroulis@covata.com

For sales or general inquiries, please visit [Covata](https://www.covata.com/contact/).

Copyright(c) 2014, Cocoon Holdings Data Limited
