# Docker EE for Windows - Workshop, Lab 2

## Deploying the App as a Swarm service

Log into the Docker UCP webinterface through the URL and User ID provided by your instructor.

1) Login to Docker Universal Control Plane

2) Resources >> Services >> Create a Service

3) Configure Details

- Name Service
- Enter Image Name as ``<docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version>``
    
4) Configure Scheduling

- Add Constraint "node.platform.os==windows"

5) Configure Resources

- Add Port
       - Internal Port: `80`
       - Protocol: `tcp`
       - Publish Mode: `host`
       - Public Port: `8X`

****
Important, each student should use a different port: the X in the port number should be the same as the number in your user ID, so ports 81-86!
****

6) Deploy now!

# View your App in a webbrowser
1) Browse the `loadbalancer` public IP address/URL

2) The app's website should appear

# Scaling the App to run across all nodes

Remove the service you have created.
All containers should be removed quickly.

Now redeploy your service but this time, instead of scheduling one replica, deploy it as a global service.

Your app should now be deployed across all nodes available to the cluster.

