# Docker EE for Windows - Workshop, Lab 2

# Adding a Windows worker node to a Docker EE swarm cluster

docker join ....

netsh advfirewall firewall add rule name="docker_ucp" dir=in action=allow protocol=TCP localport=12376
Stop-Service docker
dockerd --unregister-service
dockerd -H npipe:// -H 0.0.0.0:12376 --tlsverify --tlscacert=c:\ProgramData\docker\ucp\ca.pem --tlscert=c:\ProgramData\docker\ucp\cert.pem --tlskey=c:\ProgramData\docker\ucp\key.pem --register-service
Start-Service docker


# Create Image Repository in Docker Trusted Registry (DTR)
1) Log into Docker Trusted Registry
2) [Create a Repository](https://docs.docker.com/datacenter/dtr/2.2/guides/user/manage-images/)

# Login and Push App to Docker Trusted Registry (DTR)
```
C:\> docker login <docker_trusted_registry_url>

C:\> docker push <docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version>
```

# Deploy App
1) Login to Docker Universal Control Plane
2) Resources >> Services >> Create a Service
3) Configure Details
    * Name Service
    * Enter Image Name as "<docker_trusted_registry_url>/<user_or_organization>/<app_name>:<app_version>"
4) Configure Scheduling
    * Add Constraint "node.platform.os==windows"
5) Configure Resources
    * Add Port
        * Internal Port: `80`
        * Protocol: `tcp`
        * Publish Mode: `host`
        * Public Port: `80`
6) Deploy now!

# View App
1) Browse `wrk_pip` public IP address
2) Website should appear

# Scale App
1) Resources >> Services
2) Select service you'd like to scale
3) Scheduling >> Scale
4) Update Scale value to 3, clicking check mark to confirm
5) Save Changes