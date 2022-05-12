# DemoApp
Generic aspnet core app that can be easily rebranded for demos.

## To Build
- Just click the DemoApp button in the Build toolbar

## To Publish
- Right click on the DemoApp project in the Solution Explorer in the tree and choose Publish...
- Verify the configuration settings, especially which registry the container image will be published to
- Click Publish

## To Deploy
- Change the registry, repository, and image tag as needed in the /GitOps/yaml/app-deployment.yaml file
- Change the storage class and service type as needed in the app-deployment.yaml file
- Run `kubectl create -f <path to app-deployment.yaml file> -n <namespace>`
- Wait for the db pod to come up completely
- Get the 'web-app' service endpoint by running `kubectl get services -n <namespace>`
- Connect to the app service endpoint in your browser - port 80
- To redploy first run `kubectl delete -f <path to app-deployment.yaml file> -n <namespace>` and then `kubectl create -f <path to app-deployment.yaml file> -n <namespace>` again.

## To Rebrand:

Generally, you can search through the entire solution for "DEMO_CUSTOMIZATON" to find all the places where the code can be customized.

### Rebrand container registry
You will need to create your own container registry where you can push your modified images to and then you will need to change a few things as follows to publish to your registry and then deploy from there:
- Create a container image repository someplace like Azure Container Registry (ACR)
- Change the container registry in the GitOps/yaml/app-deployment.yaml file.

		```
		image: contosobooks.azurecr.io/contosobooks:<tag>
		```

- Change the Publish profile configuration to also point to the container registry
- Publish to make sure that end:end building and publishing to the container registry works

### Create new data models as needed for the demo scenario
- Add new models in the Models folder (instructions)[https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-6.0&tabs=visual-studio#add-a-data-model-class]. Group new models by creating a new scenario-specific folder under Models.  Example: /Models/MiningOperations/MiningCart
- Add additional DBContext classes in the DemoApp.Data namespace in DataSource.cs.  See detailed instructions in the DataSource.cs file.
- Add a new Initialize method overload on the DbInitializer class in DbInitializer.cs  See detailed instructions in the DbInitializer.cs file.
- Change the DbContext class name in Startup.cs in *2* places and in Program.cs in 1 place if you want to change to a different DbContext

### Scaffold up the views and controllers for the new data model classes
- Right click on Controllers, select Add -> New Scaffold Item -> choose 'MVC Controller with views, using Entity Framework' -> fill out the form by selecting the class and DbContext. (Example)[https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-6.0&tabs=visual-studio#scaffold-movie-pages]

### Customize display strings and images
- Find a different home page branding image on the web taking into consideration intellectual property rights.  Add the new branding image to /DemoApp/wwwroot/images.
- Modify the display strings and cover branding image in appsettings.json.
- Change the database name in appsettings.json if needed

### Customize menu
- In /DemoApp/Views/Shared/_Layout.cshtml, modify the menu layout by adding or commenting out different menu options/links to views as needed.