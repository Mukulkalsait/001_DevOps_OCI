## Dockerfile =>

> Dockerfile Instructions

```lua
  x.
  FROM Create a new build stage from a base image.
  SHELL Set the default shell of an image.
  RUN Execute build commands.
  WORKDIR Change working directory Default for => { RUN,CMD,ADD,COPY,ENTRYPOINT } commands .
 *USER Set "User" and "group IDs". [default => root] *(RUN adduser -D username). 
  ENV Set environment variables key=value key=value (for next line use " \ ") .
  COPY Copy files and directories.
  ADD Add local or remote files and directories.
  EXPOSE Describe which ports your application is listening on. to check => [docker inspect mynewimg - -format="{{json.Config.ExposedPorts}/"{"5000/tcp:{}"}]  cmd.
  VOLUME Create volume mounts .
  LABEL Add metadata to an image. {key=val} {Writer=mukuldk}.

  -- Y: Executable fields
  ENTRYPOINT Specify default executable NON-MODIFIABLE. -- G: ["nmp"], ["pnpm"] , ["/bin/sh"]? ,
  CMD Specify default commands MODIFIABVLE. -- G: ["exicutable","arg1","arg2"]
  -- DX: With "docker run" + command we can override the cmd



ARG Use build-time variables.
HEALTHCHECK Check a containers health on startup.
MAINTAINER Specify the author of an image.
ONBUILD Specify instructions  for_when the image is used in a build.
STOPSIGNAL Specify the system call signal for_exiting a container.
```

# DOCKER FILE =>

```Dockerfile
FROM alpine:3.18
SHELL ["/bin/sh","-c"] # default
SHELL ["pwsh","-command"] # for windows

RUN apk add curl
WORKDIR /downloads

RUN adduser -D mukuldk
USER mukuldk[:wheel]

ENV eSECRET_API_KEY=75C57EA3C2B5C5EA \
 url=https://vihaanaitech.com/main/Apis/app_key_verify.php \
 eVERIFICATION_KEY=8A9AB63614582D29

ENV app_host="0.0.0.0"
ENV sys_host="5.5.5.5"
ENV app_port=5000

COPY . . (everythign from . to new . => WORKDIR )
COPY app.sh /downloads/ (absolute path)
ADD . .
ADD https://github.com/Mukulkalsait/nvim /home/mukuldk/.config/

EXPOSE 3000  (+ use in => docker run -it -p 8080:3000 myimg )
EXPOSE 5000

VOLUME
LABEL CREATER="mukuldk"


CMD ["bun","start"] // G: 1
 or
ENTRYPOINT  ["bun"] or ["pnpm"] // G:2
CMD ["start"]
 or
ENTRYPOINT  ["sleep"]  // G: 3
CMD ["5"]

// DX: OVERRIDING CMD Limitations
// Y:
//  in (1) docker run -it -d -p 8080:3000 myimg npm i
//  in (2) docker run -it -d -p 8080:3000 myimg (i/start/stop) [because entrypoint cant be overridestopstop]
//  in (3) docker run -it myimg (1/2/3/..) [again because entrypoint cant be overridestopstop]




```

## Core Concepts

- Docker Image: Read-only snapshot of a container.
- Docker Container: Executable package with software and dependencies.
- Docker Client: Tool to interact with Docker.
- Docker Daemon: Service managing Docker objects.
- Docker Registry: Storage for Docker images.

Perfect 👌 you’re at the stage where you know the **basics of Docker** (run, build, compose, Dockerfile, multistage builds).
If you want to go **deep** (DevOps-level mastery), you’ll need to cover:

---

## 🔹 `docker run` – Full Options (Important Flags)

You already know the most common ones. Here’s a deeper list:

<!-- Y: Basic-->

- `docker run <name> <any COMMAND of os> atribute` -> directly pass COMMAND inside the os.
  docker run myimg { pwd || cat index.html || apt add curl || uname -a || cat /etc/os-release }
- `--name <name>` → Name container.
- `<name> env` -> show total env set in file.

- `-d, --detach` → Run in background.
- `-i` → Keep STDIN open && `-t` → Allocate TTY (usually used as `-it`).

- `--rm` → Remove container on exit.

- `-p, --publish hostPort:containerPort` → Publish container ports.

- `--network <network>` → Attach to a network.

<!-- IMP: USED IMP -->

- `-e, --env KEY=VALUE` → Set env vars.
- `--env-file <file>` → Load env vars from file.

- `-v, --volume <host:container>` → Mount a volume.
- `--mount type=bind|volume|tmpfs,...` → More advanced mounting.

- `--network-alias <alias>` → Alias inside custom networks.
- `--ipc` → Share IPC namespace.
- `--pid` → Share PID namespace.
- `--hostname` → Set hostname.

- `--workdir <dir>` → Set working directory.
