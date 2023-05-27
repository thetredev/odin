#  Container image used to build Odin projects
Based on Ubuntu 22.04 and LLVM 14. Check out https://github.com/odin-lang/Odin for more information about Odin.

# Usage
Pull the image from either registry:
- Docker Hub: `docker pull thetredev/odin:<version>`
- GitHub Container Registry: `docker pull ghcr.io/thetredev/odin:<version>`

Check available versions here:
- Docker Hub: https://hub.docker.com/r/thetredev/odin
- GitHub Container Registry: https://github.com/thetredev/odin/pkgs/container/odin

## Interactive
```
docker run --rm -it ghcr.io/thetredev/odin:<version>

odin@6e556c30d770:~$ odin
odin is a tool for managing Odin source code
Usage:
        odin command [arguments]
Commands:
        build             compile directory of .odin files, as an executable.
                          one must contain the program's entry point, all must be in the same package.
        run               same as 'build', but also then runs the newly compiled executable.
        check             parse, and type check a directory of .odin files
        strip-semicolon   parse, type check, and remove unneeded semicolons from the entire program
        test              build and runs procedures with the attribute @(test) in the initial package
        doc               generate documentation on a directory of .odin files
        version           print version
        report            print information useful to reporting a bug

For further details on a command, invoke command help:
        e.g. `odin build -help` or `odin help build`

odin@6e556c30d770:~$ odin version
odin version dev-2023-05:0c352213
```

## Non-interactive
```
docker run --rm -v <source code directory>:/home/odin/src ghcr.io/thetredev/odin:<version> odin build src
```

# What's inside?
```
- wget + curl
- git
- build-essential
- cmake
- LLVM 14 via https://apt.llvm.org/llvm.sh
- dependencies to build Odin itself
- Odin + libraries (built as part of generating the container image)
- non-root user "odin" with GID/UID 1000
```

Full source: [image/Dockerfile](image/Dockerfile)
