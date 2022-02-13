# Getting Started with ColdBrew

Let's pretend you want to create a project called "echoserver". Rather than starting from scratch maybe copying some files and then editing the results to include your name, email, and various configuration issues that always get forgotten until the worst possible moment, get cookiecutter to do all the work.

First, get Cookiecutter. Trust me, it's awesome:

```text
$ pip install cookiecutter
```

Alternatively, you can install `cookiecutter` with homebrew:

```text
$ brew install cookiecutter
```

Finally, to run it based on this template, type:

```text
$ cookiecutter gh:go-coldbrew/cookiecutter-coldbrew
```

You will be asked about your basic info \(name, project name, app name, etc.\). This info will be used to customise your new project.

_Warning: After this point, change 'github.com/ankurs', 'MyApp', etc to your own information._

Answer the prompts with your own desired options. For example:

```text
source_path [github.com/ankurs]: github.com/ankurs
app_name [MyApp]: MyApp
grpc_package [github.com.ankurs]: github.com.ankurs
service_name [MySvc]: MySvc
project_short_description [A Golang project.]: A Golang project
docker_image [alpine:latest]:
docker_build_image [golang]:
Select docker_build_image_version:
1 - 1.17
2 - 1.18
Choose from 1, 2 [1]: 2
```

Enter the project and take a look around:

```text
$ cd MyApp/
$ ls
```

Run `make help` to see the available management commands, or just run `make build` to build your project.

```text
$ make run
```
