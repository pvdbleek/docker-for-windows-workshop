# Docker EE for Windows - Workshop, Lab 1

For this simple example a ‘Hello World’ container image will be created and deployed. For the best experience run these commands in an elevated Windows CMD shell or PowerShell.
Windows PowerShell ISE does not work for interactive sessions with containers. Even though the container is running, it will appear to hang.
First, start a container with an interactive session from the nanoserver image. Once the container has started, you will be presented with a command shell from within the container.
Copy
none
docker run -it microsoft/nanoserver cmd
Inside the container we will create a simple ‘Hello World’ script.
Copy
none
powershell.exe Add-Content C:\helloworld.ps1 'Write-Host "Hello World"'
When completed, exit the container.
Copy
none
exit
You will now create a new container image from the modified container. To see a list of containers run the following and take note of the container id.
Copy
none
docker ps -a
Run the following command to create the new ‘HelloWorld’ image. Replace with the id of your container.
Copy
none
docker commit <containerid> helloworld
When completed, you now have a custom image that contains the hello world script. This can be seen with the following command.
Copy
none
docker images
Finally, to run the container, use the docker run command.
Copy
none
docker run --rm helloworld powershell c:\helloworld.ps1


## Download App and Extract App
```
#From Windows Server 2016 node with Docker and access to Docker Trusted Registry (DTR)
C:\> Invoke-WebRequest -Uri https://broyal.blob.core.windows.net/1eae3d83-fc8b-4eba-8702-4bb20fcd6105/demoapp.zip -OutFile demoapp.zip
-OutputPath <source_code_path>
```
## Build Demo App Image
```
C:\> docker build -t <docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version> .
```
