# docker-byteball-hub
Docker image for byteball-hub

## Usage

The simplest way to create the docker image and run byteball-hub is below.

```console
$ docker build -t byteball-hub .
$ docker run -d byteball-hub
```

although it is always a good idea to give a container a name you can remember:

```console
$ docker run -d --name my-byteball-hub byteball-hub
```

To stop the container and then restart it again, use:

```console
$ docker stop my-byteball-hub
$ docker start my-byteball-hub
```

### Using volumes

Although the byteball-hub docker image has been set up to create a volume
and store the byteball runtime files on the host filesystem, using a named volume
is recommended so containers can be dropped and recreated easily by referencing
the existing storage by a simple name:

```console
$ docker volume create --name byteball-hub
$ docker run -d --name my-byteball-hub -v byteball-hub:/byteball byteball-hub
```

NOTE: The configuration files are stored in the `/byteball` folder inside the container. 

### Changing the configuration

In order to change the configuration file, stop the byteball-hub container
and start a new one like below:

```console
$ docker run -it --rm -v byteball-hub:/byteball byteball-hub vi /byteball/conf.json
```

This will mount the named byteball volume and open/create the conf.json file in the
`vi` text editor. When you quit from `vi` the container will automatically
delete itself due to the `--rm` flag.

Now you can start the container again and the app will start up with the 
changed configuration.

See configuration options here:
* [byteball/byteballcore](https://github.com/byteball/byteballcore)
* [byteball/byteball-hub](https://github.com/byteball/byteball-hub)

### Checking the log file

In case you need to check the log files you can use the following command:

```console
$ docker run -it --rm -v byteball-hub:/byteball byteball-hub less /byteball/log.txt
```

### Exposing port to the host system

If you enabled the default websocket port (6611) you may want to map it a port
on your host system. You have to create the container as below, but you may
first want to stop and remove the running container before creating a new one.

```console
$ docker stop my-byteball-hub
$ docker rm my-byteball-hub
$ docker run -d --name my-byteball-hub -v byteball-hub:/byteball -p 6611:6611 byteball-hub
```

This will map the 6611 port of the host system to the 6611 port of the container.

