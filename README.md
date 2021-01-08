![Logo](https://www.gurobi.com/wp-content/uploads/2018/12/logo-final.png "Gurobi Optimization")
# Quick reference
Maintained by: [Gurobi Optimization](https://www.gurobi.com)

Where to get help: [Gurobi Support](https://www.gurobi.com/support/), [Gurobi Documentation](https://www.gurobi.com/documentation/)

# Supported tags and respective Dockerfile links
## Simple Tags
* [9.1.1, latest](https://github.com/Gurobi/docker-modeling-examples/blob/master/9.1.1/Dockerfile)

# Quick reference (cont.)
Where to file issues: https://github.com/Gurobi/docker-modeling-examples/issues

Supported architectures: linux amd64

Published image artifact details: https://github.com/Gurobi/docker-modeling-examples

Gurobi images:
- [gurobi/optimizer](https://hub.docker.com/repository/docker/gurobi/optimizer): Gurobi Optimizer (full distribution)
- [gurobi/python](https://hub.docker.com/repository/docker/gurobi/python): Gurobi Optimizer (Python API only)
- [gurobi/modeling-examples](https://hub.docker.com/repository/docker/gurobi/modeling-examples): Optimization modeling examples (distributed as Jupyter Notebooks)
- [gurobi/compute](https://hub.docker.com/repository/docker/gurobi/compute): Gurobi Compute Server
- [gurobi/manager](https://hub.docker.com/repository/docker/gurobi/manager): Gurobi Cluster Manager

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

* A [Gurobi Compute Server](https://www.gurobi.com/products/gurobi-compute-server/) 
allows you to seamlessly offload your optimization computations onto one or more dedicated 
optimization servers grouped in a cluster. Users and applications can share the servers 
thanks to advanced queuing and load balancing capabilities. Users can monitor jobs, and 
administrators can manage the servers. The cluster manager provides additional features
and security management. The Compute Server and the Cluster Manager must 
be installed outside of the Docker cluster.

* The [Gurobi Cloud](https://www.gurobi.com/products/gurobi-instant-cloud/) is a simple and 
cost-effective way to get up and running with powerful Gurobi optimization software running 
on cloud services. It allows you to launch one or more computers, pre-loaded with Gurobi 
software and dedicated to you on Microsoft Azure® and Amazon Web Services®. 

* A [Gurobi Token Server](https://www.gurobi.com/documentation/current/quickstart_linux/creating_a_token_server_cl.html) 
runs on a dedicated machine outside of the Docker cluster. Client programs running in Docker containers can request
tokens from the token server.

* Please contact your sales representative at [sales@gurobi.com](mailto:sales@gurobi.com) to discuss additional options. 

Note that other standard license types (NODE, Academic) do not work with Docker.

## Using the client license

Except with the embedded `Limited License`, you will need to specify a set of properties to 
connect to the compute server cluster, or the Gurobi Cloud, or the token server. To do so,
you have the following options:
* Mounting the client license file:
You can store connection parameters in a client license file (typically called `gurobi.lic`) 
and mount it to the container. This option provides a simple approach for testing.

* Setting parameters with the API:
When your application creates the [Gurobi environment](https://www.gurobi.com/documentation/current/refman/py_env.html) 
in Python, you can specify connection parameters. The parameter values can come from 
environment variables, a database or from other sources as required by your application. 
This is the recommended approach for production applications.

A quick guide to the appropriate API parameters and license file properties is available [here](https://github.com/Gurobi/docker-optimizer/blob/master/PARAMS.md).

# How to use this image?

## Start a `modeling-examples` server instance
`$ docker run -p 8888:8888 gurobi/modeling-examples`

... this will make use of the limited licenses installed by default.

If you have a different valid `Gurobi Client` license file:

```console
$ docker run -p 8888:8888 
             --volume=$PWD/gurobi.lic:/opt/gurobi/gurobi.lic:ro 
             gurobi/modeling-examples
```

... where `$PWD` is the current directory.

A Jupiter Notebook instance will start, and you can open a browser at: 

http://localhost:8888

Then you can interact with all the [modeling examples](https://github.com/Gurobi/modeling-examples).

As this is intended to run as a training environment, security checks (Token and Password) are disabled. 
If you want to implement security checks, it can be defined in the `cmd` part of the command:

`$ docker run -p 8888:8888 gurobi/modeling-examples --NotebookApp.token='token'`



## Via docker stack deploy or docker-compose
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

# License

View [End User License Agreement](https://www.gurobi.com/wp-content/uploads/2020/11/EULA_standard.pdf) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other
licenses (such as Bash, etc from the base distribution, along with any direct or indirect
dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use
of this image complies with any relevant licenses for all software contained within.

### Modeling Examples
These modeling examples are distributed under the Apache 2.0 license, (c) Copyright 2020 Gurobi Optimization, LLC

https://github.com/Gurobi/modeling-examples
