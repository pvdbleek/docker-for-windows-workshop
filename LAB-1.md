# Docker EE for Windows - Workshop, Lab 1

## Verify your installation

You have received access to a Windows Server 2016 VM running on Azure.

Before we continue with the labs, let's verify that docker is installed and that we have pulled all needed images.

To verify docker is correctly running, issue:

```
docker version
```

To verify which images are available:

```
docker images
```
You should have a minimum of two images available for these labs:

- microsoft/windowsservercore
- microsoft/nanoserver

## Running our first container

For this simple example a ‘Hello World’ container image will be created and deployed. For the best experience run these commands in an elevated Windows CMD shell or PowerShell.

**** 

Windows PowerShell ISE does not work for interactive sessions with containers. 
Even though the container is running, it will appear to hang.

****

First, start a container with an interactive session from the nanoserver image. Once the container has started, you will be presented with a command shell from within the container.

````
docker run -it microsoft/nanoserver cmd
````
Inside the container we will create a simple ‘Hello World’ script.

````
powershell.exe Add-Content C:\helloworld.ps1 'Write-Host "Hello World"'
````

When completed, exit the container.

````
exit
````
You will now create a new container image from the modified container. To see a list of containers run the following and take note of the container id.

```
docker ps -a
```

Run the following command to create the new ‘HelloWorld’ image. Replace with the id of your container.

```
docker commit <containerid> helloworld
```

When completed, you now have a custom image that contains the hello world script. This can be seen with the following command.

```
docker images
```

Finally, to run the container, use the docker run command.

```
docker run --rm helloworld powershell c:\helloworld.ps1
```

## Building an image for a .NET demo application

From your Windows Server 2016 VM with Docker and access to Docker Trusted Registry (DTR) 

Download and unzip the demo app.

```
Invoke-WebRequest -Uri https://broyal.blob.core.windows.net/1eae3d83-fc8b-4eba-8702-4bb20fcd6105/demoapp.zip -OutFile demoapp.zip
-OutputPath <source_code_path>

Expand-Archive -Path demoapp.zip
```

Inside the demoapp directory you will find the Dockerfile for this project.
You might want to inspect the Dockerfile to find out what happens during the image build. The extracted files contain a pre-compiled application, so there's no need to build them at this stage.

To build the image:

```
docker build -t <docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version> .
```
Now let's run our newly created container and expose it to port 80 (HTTP).

```
docker run -ti -p 80 <docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version>
```

Open your webbrowser and point it to the hostname of your VM, you should see the website of your demo application.

Once you have succesfully deployed your app, login into to DTR (login and URL provided by your instructor) through the webbrowser and create a repo called ``<user_or_organization>/<app_name>`` 


[Create a Repository](https://docs.docker.com/datacenter/dtr/2.2/guides/user/manage-images/)

```
docker login <docker_trusted_registry_url>
```

Now push your image to the DTR:

```
docker push <docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version>
``` 