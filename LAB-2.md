# Docker EE for Windows - Workshop, Lab 2

## Adding a Windows worker node to a Docker EE swarm cluster

Access the Swarm-node-to-be through RDP with the address and credentials provided by your instructor. 

Log into the Docker UCP webinterface through the URL and User ID provided by your instructor.

Navigate to the Resources > Nodes pane and select "Add node", you will be prompted with the command and secure token to join a new node.

Please make sure you join your Windows node as a Worker only.

Next, run the provided command from the command line on your Windows node.

```
docker swarm join ....
```

After a few seconds you should see the newly added node in the UCP node overview. It will prompt you to run some post-join config commands.
Run the following command from the command line on your worker node:

```
netsh advfirewall firewall add rule name="docker_ucp" dir=in action=allow protocol=TCP localport=12376
Stop-Service docker
dockerd --unregister-service
dockerd -H npipe:// -H 0.0.0.0:12376 --tlsverify --tlscacert=c:\ProgramData\docker\ucp\ca.pem --tlscert=c:\ProgramData\docker\ucp\cert.pem --tlskey=c:\ProgramData\docker\ucp\key.pem --register-service
Start-Service docker
```
When the node has succesfully been added to the swarm you should be able to see the ucp-agent container running on the node by doing:

```
docker ps
```

# Deploying the App as a Swarm service
1) Login to Docker Universal Control Plane

2) Resources >> Services >> Create a Service

3) Configure Details

- Name Service
- Enter Image Name as ``<docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version>``
    
4) Configure Scheduling

- Add Constraint "node.platform.os==windows"

5) Configure Resources

- Add Port
       - Internal Port: `8X`
       - Protocol: `tcp`
       - Publish Mode: `host`
       - Public Port: `8X`

****
Important, each student should a different port: the X in the port number should be the same as the number in your user ID, so ports 81-86!
****

6) Deploy now!

# View your App in a webbrowser
1) Browse the `loadbalancer` public IP address/URL

2) The app's website should appear

# Scaling the App to multiple containers
1) Resources >> Services

2) Select service you'd like to scale

3) Scheduling >> Scale

4) Update Scale value to a number of choice (max. 6), clicking check mark to confirm

5) Save Changes