# Operations

## Debug

If you want to have more detailed output set the `DEBUG` env variable to non-zero:

```
DEBUG=1 cbd some_command
```

## Troubleshoot

You can use the `doctor` command to diagnose your environment.
It can reveal some common problems with your docker or boot2docker configuration and it also checks the cbd versions.

```
cbd doctor
```

## Logs

The aggregated logs of all the Cloudbreak components can be checked with:

```
cbd logs
```

It can also be used to check the logs of an individual docker container. To see only the logs of the Cloudbreak backend:

```
cbd logs cloudbreak
```

You can also check the individual logs of `uluwatu`, `periscope`, and `identity`.

## SSH to the hosts

In the current version of Cloudbreak all the nodes can have a public IP address and all the nodes can be accessible via SSH.
The public IP addresses of a running cluster can be checked on the Cloudbreak UI under the *Nodes* tab.
Only key-based authentication is supported - the public key can be specified when creating a cloud credential.

Each cloud provider has different SSH user. In order to figure out the username you can try it wit the `root` user - that will tell you the correct username.

As an example:

```
ssh -i ~/.ssh/private-key.pem USER_NAME@<public-ip>
```

## Accessing HDP client services

The main difference between general HDP clusters and Cloudbreak-installed HDP clusters is that each host runs an Ambari server or agent Docker container and the HDP services will be installed in this container as well.
It means that after `ssh` the client services won't be available instantly, first you'll have to `enter` the ambari-agent container.
Inside the container everything works the same way as expected. In order to do so there are a few options.

## Data volumes

The disks that are attached to the instances are automatically mounted to `/hadoopfs/fs1`, `/hadoopfs/fs2`, ... `/hadoopfs/fsN` respectively.

## Selected Ambari Server node

This instance has some special tasks:

- it runs the Ambari server and its database
- it runs an nginx proxy that is used by the Cloudbreak API to communicate with the cluster securely
- it runs a Kerberos KDC container if Kerberos is configured

**Logs**

*Hadoop logs*

Hadoop logs are available from the host and from the container as well in the `/hadoopfs/fs1/logs` directory.

*Ambari logs*

You can check the Ambari logs on the host instance under the ``/hadoopfs/fs1/logs` folder as well.

**Ambari database**

To access Ambari's database SSH to the selected node and run the following command:
