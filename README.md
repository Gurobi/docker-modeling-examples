![Logo](https://cdn.gurobi.com/wp-content/uploads/GurobiLogo_Black.svg "Gurobi Optimization")
# Quick reference
Maintained by: [Gurobi Optimization](https://www.gurobi.com)

Where to get help: [Gurobi Support](https://www.gurobi.com/support/), [Gurobi Documentation](https://www.gurobi.com/documentation/)

# Supported tags and respective Dockerfile links

* [10.0.2, latest](https://github.com/Gurobi/docker-modeling-examples/blob/master/10.0.2/Dockerfile)
* [10.0.1](https://github.com/Gurobi/docker-modeling-examples/blob/master/10.0.1/Dockerfile)
* [10.0.0](https://github.com/Gurobi/docker-modeling-examples/blob/master/10.0.0/Dockerfile)
* [9.5.2](https://github.com/Gurobi/docker-modeling-examples/blob/master/9.5.2/Dockerfile)
* [9.5.1](https://github.com/Gurobi/docker-modeling-examples/blob/master/9.5.1/Dockerfile)
* [9.5.0](https://github.com/Gurobi/docker-modeling-examples/blob/master/9.5.0/Dockerfile)

When building a production application, we recommend using an explicit version number instead of the `latest` tag.
This way, you are in control of the upgrade process of your application.

# Quick reference (cont.)

Supported architectures: linux amd64

Published image artifact details: https://github.com/Gurobi/docker-modeling-examples

Gurobi images:
- [gurobi/optimizer](https://hub.docker.com/r/gurobi/optimizer): Gurobi Optimizer (full distribution)
- [gurobi/python](https://hub.docker.com/r/gurobi/python): Gurobi Optimizer (Python API only)
- [gurobi/python-example](https://hub.docker.com/r/gurobi/python-example): Gurobi Optimizer example in Python with a WLS license
- [gurobi/modeling-examples](https://hub.docker.com/r/gurobi/modeling-examples): Optimization modeling examples (distributed as Jupyter Notebooks)
- [gurobi/compute](https://hub.docker.com/r/gurobi/compute): Gurobi Compute Server
- [gurobi/manager](https://hub.docker.com/r/gurobi/manager): Gurobi Cluster Manager

# What is `gurobi/modeling-examples`?
The Gurobi Optimizer is the fastest and most powerful mathematical programming solver available 
for your LP, QP and MIP (MILP, MIQP, and MIQCP) problems.
More info at the [Gurobi Website](https://www.gurobi.com/products/gurobi-optimizer/).

These Python modeling examples illustrate important capabilities of the Gurobi Python API,
including adding decision variables, building linear expressions, adding constraints,
and adding an objective function. They touch on more
advanced features such as generalized constraints, piecewise-linear
functions, and multi-objective hierarchical optimization.  They also illustrate
common constraints types 
such as “allocation constraints”, “balance constraints”, “sequencing constraints”,
“precedence constraints”, and others.

The `gurobi/modeling-example` image includes a Jupyter Notebook that allows you
to browse and execute any of the Python modeling examples.

## Getting a Gurobi license

This image comes with a `Limited License` that allows you to solve small optimization problems.
To solve larger problems, you will need to get a license that works with your application
running in Docker containers.  You have a few options:

* The [Web License Service](https://www.gurobi.com/web-license-service/) (WLS) is a new Gurobi licensing service 
  for containerized environments (Docker, Kubernetes...). Gurobi components can automatically request and renew license tokens to 
  the WLS servers available in several regions worldwide. WLS only requires that your container has access to the 
  Internet. Commercial users can request an evaluation and academic users can request a free license.
  Please register to access the [Web License Manager](https://license.gurobi.com) and read the
  [documentation](https://license.gurobi.com/manager/doc/overview)

* A [Gurobi Compute Server](https://www.gurobi.com/products/gurobi-compute-server/) 
allows you to seamlessly offload your optimization computations onto one or more dedicated 
optimization servers grouped in a cluster. Users and applications can share the servers 
thanks to advanced queuing and load balancing capabilities. Users can monitor jobs, and 
administrators can manage the servers. The cluster manager provides additional features
and security management. The Compute Server, and the Cluster Manager can 
be installed outside of the Docker cluster using a standard license, or within the Docker cluster
with a WLS license.

* The [Gurobi Cloud](https://www.gurobi.com/products/gurobi-instant-cloud/) is a simple and 
cost-effective way to get up and running with powerful Gurobi optimization software running 
on cloud services. It allows you to launch one or more computers, pre-loaded with Gurobi 
software and dedicated to you on Microsoft Azure® and Amazon Web Services®. 

* A [Gurobi Token Server](https://www.gurobi.com/documentation/current/quickstart_linux/creating_a_token_server_cl.html) 
runs on a dedicated machine outside of the Docker cluster. Client programs running in Docker containers can request
tokens from the token server.

Note that other standard license types (NODE, Academic) do not work with containers.
Please contact your sales representative at [sales@gurobi.com](mailto:sales@gurobi.com) to discuss licensing options. 

## Using the client license

Except with the embedded `Limited License`, you will need to specify a set of properties to 
connect to the compute server cluster, or the Gurobi Cloud, or the token server. To do so,
you have the following options:
* Mounting the client license file:
You can store connection parameters in a client license file (typically called `gurobi.lic`) 
and mount it to the container. 
This option provides a simple approach for testing with Docker.
When using Kubernetes, the license file can be stored as a secret and mounted in the container.

* Setting parameters with the API:
When your application creates the [Gurobi environment](https://www.gurobi.com/documentation/current/refman/py_env.html) 
in Python, you can specify connection parameters. The parameter values can come from 
environment variables, a database or from other sources as required by your application. 

A quick guide to the appropriate API parameters and license file properties is available [here](https://github.com/Gurobi/docker-optimizer/blob/master/PARAMS.md).

We do not recommend adding the license file to the Docker image itself. It is not a flexible 
solution as you may not reuse the same image with different settings. More importantly, it is not secure
as some license files need to contain credentials in the form of API keys that should remain private.

# How to use this image?

## Using Docker

The following command starts a modelling example server instance with
the limited licenses installed by default.

```
$ docker run -p 8888:8888 gurobi/modeling-examples
```

If you have a different license file you can mount it from the current 
directory `$PWD`

```console
$ docker run -p 8888:8888 \
             --volume=$PWD/gurobi.lic:/opt/gurobi/gurobi.lic:ro \
             gurobi/modeling-examples
```

A Jupiter Notebook instance will start, and you can open a browser at: 

http://localhost:8888

Then you can interact with all the [modeling examples](https://github.com/Gurobi/modeling-examples).

As this is intended to run as a training environment, security checks (Token and Password) are disabled. 
If you want to implement security checks, it can be defined in the `cmd` part of the command:

`$ docker run -p 8888:8888 gurobi/modeling-examples --NotebookApp.token='token'`

## Using Docker Compose

Example `docker-compose.yml` for a modeling example server:

```
version: '3.7'
services:
  modeling-examples:
    image: gurobi/modeling-examples:latest
    ports:
      - "8888:8888"

```

Example `docker-compose.yml` for a modeling example server with license mounted:

```
version: '3.7'
services:
  modeling-examples:
    image: gurobi/modeling-examples:latest
    ports:
      - "8888:8888"
    volumes:
      - ./gurobi.lic:/opt/gurobi/gurobi.lic

```

Run `$ docker-compose up `

A Jupiter Notebook instance will start, and you can open a browser at: 

http://localhost:8888 

Then you can interact with all the [modeling examples](https://github.com/Gurobi/modeling-examples).

As this is intended to run as a training environment, security checks (Token and Password) are disabled.  

## Using Kubernetes

If you want to mount a specific license with Kubernetes, it can be done using a secret. This 
is optional for the modeling examples as a limited license will be used if not provided.

```
kubectl create secret generic gurobi-lic --from-file="gurobi.lic=$PWD/gurobi.lic"
```

Then you can start a pod that will run the modelling examples in a container and expose it as a service. 
A simple deployment file is provided as a [reference](https://github.com/Gurobi/docker-modeling-examples/blob/master/10.0.2/k8s.yaml).

```
kubectl apply -f k8s.yaml
```

A Jupiter Notebook instance will start, and you can open a browser at: 

http://localhost:8888 

Then you can interact with all the [modeling examples](https://github.com/Gurobi/modeling-examples).

As this is intended to run as a training environment, security checks (Token and Password) are disabled.  

# License

By downloading and using this image, you agree with the 
[End-User License Agreement](https://www.gurobi.com/EULA) for the Gurobi software contained in this image.

As with all Docker images, these likely also contain other software which may be under other
licenses (such as Bash, etc from the base distribution, along with any direct or indirect
dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use
of this image complies with any relevant licenses for all software contained within.

These modeling examples are distributed under the Apache 2.0 license, (c) Copyright 2020 Gurobi Optimization, LLC

https://github.com/Gurobi/modeling-examples
