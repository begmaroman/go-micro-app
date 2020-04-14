[go]: https://golang.org/doc/install
[nats]: https://nats.io/
[docker]: https://www.docker.com/
[dockercompose]: https://docs.docker.com/compose/
[protobuf]: https://developers.google.com/protocol-buffers
[protocgenmicro]: github.com/micro/protoc-gen-micro
[protoc]: https://github.com/google/protobuf/releases
[gomodules]: https://blog.golang.org/using-go-modules

# Go Micro App

This is the CLI application to setup and manage [GoLang][go] micro-services based project.

Using this app, you don't need to do a trivial work to setup a new project 
that is going to be using [GoLang][go] as the main programming language 
and micro-services architecture.
What you need is to install this app on your environment and use its interface.

## The following tools gonna be used in your project:

- [GoLang][go] as the main programming language.
- [Docker][docker] to containerize services.
- [docker-compose][dockercompose] to orchestrate all a service containers together.
- [NATS][nats] as discovery service, broker service, and transport.
- [Protobufs][protobuf] as the mechanism for serializing structured data that passes between services.
- [protoc][protoc] to generate GoLang protobuf models.

Use the instructions below to prepare your environment to create a new project 
with micro-services architecture.

### Install Go

*NOTE: Skip this section if you already have installed Go.*

First, [install Go][go]. 

To set up a working Go environment you need to set two environment
variables: `GOROOT` and `GOPATH`. You will want to define these
variables so that both your terminal and your GUI editor picks them
up. For example, if you use zsh and Emacs.app (with the
`exec-path-from-shell` package) on macOS, you can put these
definitions into your `~/.zshenv` file:

```sh
export GOROOT=/usr/local/opt/go/libexec
export GOPATH=~/gopath
```

It does not matter where your `$GOPATH` is located as long as you have
permissions to it. You will probably want to make it easy to type, or
use your shell to alias it (e.g. in zsh `hash -d go=$GOPATH`).

You will also want to place both `/bin` directories into your `$PATH`.
For zsh users this will be another line in `~/.zshenv`:

```sh
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

Also, for Go versions less than 1.13, you need to enable the module-aware mode
by setting the following environment variable:

```sh
export GO111MODULE=on
```

After this is done **you will be able to use `go get`**, the built-in Go
package manager. It is idiomatic in Go-land to install _all_
repositories through `go get`.

You will, of course, have to tweak things depending on your OS and shell.

## Install [protoc][protoc]

We use Protobufs as part of our toolchain so you need 
to [grab a release][protoc] from upstream before
you can create a project. 
On macOS you can do this with just `brew install protobuf`, if you'd like.
On Linux try to do the following:
```bash
$ cd ./~
$ export PROTOBUF_VERSION=3.11.4
$ curl -sOL "https://github.com/google/protobuf/releases/download/v${PROTOBUF_VERSION}/protoc-${PROTOBUF_VERSION}-linux-x86_64.zip" && \
$ unzip protoc-*.zip              && \
$ mv bin/protoc /usr/local/bin    && \
$ mv include/* /usr/local/include && \
$ rm -f protoc-*.zip
```

### Install `go-micro-app`

Open your bash terminal and execute the following command to install `go-micro-app`:
```bash
$ go install github.com/begmaroman/go-micro-app 
```

To make sure all went well, check the version of the app:
```bash
$ go-micro-app --version
go-micro-app v1.0.0
```

Congrats!

### Let's create a new project!

If you gonna create your own repository in GitHub or similar services, 
you need to go to the directory that represents the root of your account in a visioning control.
For example, if you have an account in GitHub with username `gopher`, you need to do the following:
```bash
$ cd $GOPATH/src/github.com/gopher
```
If you don't have a directory with such path, you have to create it.

So now you've prepared all things that are needed to create your own project using `go-micro-app`.
As an example, we will create an project with name `my-microservices-project`. 
You can change this name, it's up to you.
Let's do so!
```bash
# We are here $GOPATH/src/github.com/gopher
$ go-micro-app new my-microservices-project
```
This command created a new directory with the name `my-microservices-project` 
in the path from where it has been executed, i.e. `$GOPATH/src/github.com/gopher/my-microservices-project`

Let's go to `my-microservices-project` directory and see what we have there.
```bash
$ cd my-microservices-project
$ ls 
```

**This is the structure of the created project:**

**`./proto`** contains protocol buffers models of each service where it exists.
Your services use protocol buffer messages as the main communication objects.


**`./scripts`** contains bash scripts. You may see `generate.sh` scripts there. 
It's needed to generate GoLang models of proto messages.
To generate proto files, execute the following command from the root of the project 
```bash
$ ./scripts/generate.sh .
```

    
**`./services`** contains the logic of all of the services.

    
**`./utils`** contains common packages used across the services.


**`./vendor`** (not exists yet) contains dependencies. 
Execute the following from the root of the project to setup `vendor`:
```bash
$ export GO111MODULE=on
$ go mod download
$ go mod vendor
```
    

**`./docker-compose.yaml`** contains configuration of services to run inside docker containers.
Useful commands:
- `docker-compose up -d --build` - restart all services.
- `docker-compose up -d --build <service-name>` - restart a certain service.
- `docker-compose down` - destroy all services. All data will be removed.
- `docker-compose stop` - stop all services.
    

**`./go.mod`** and **`./go.sum`** needed to manage dependencies using [go modules][gomodules].


**`./.env`** contains environment variables that are passing to the services 
when we run them using docker-compose (see `docker-compose.yaml`).


**`./README.md`** recommended to describe your project.


