# Dockerfile checklist

Checklist for dockerfile security issues and best practices

## Security

### Final image

- [ ] should contain only a minimal set of SW needed to run the service:
    - [ ] uses a light, slim base image
    - [ ] no unneeded 3rd-party packages are installed
- [ ] should contain no know at-the-time vulnerabilities - check using Snyk

### Main service

- [ ] no source code for compiler languages included in the image, just a binary
- [ ] or just needed scripts - for interpreted languages
- [ ] starts the main process with non-root privileges
    *the main process inside a docker container, if possible, should not run under root privileges*
- [ ] config files, env variables included in the dockerfile must contain no secrets.
    
### SW

- [ ] if SW is installed manually (by downloading from external sources using `curl`, `wget`) in dockerfile then the downloaded package must be verified using checksum


## Reproducibility

*It is very important to keep the dockerfile stable and not breakable on SW changes*

- [ ] version of the base image is explicitly specified 

    *tag **latest** should be avoided*

- [ ] compiles binary inside docker, uses two-step docker file

    *no binaries or other compilable artifacts are added from a local machine. Just the code*

- [ ] takes the version of the service as a `docker` build parameter and adds/compiles it into binary

## Other

- [ ] if the service exposes/listens to ports then these ports should be explicitly provided in the dockerfile's `EXPOSE` command
- [ ] the service responds to terminate command
- [ ] the main process rips zombies if it creates other Unix processes
- [ ] `apt-get install` commands should be alphabetically sorted

    *for better readability*
- [ ] any `apt-get install` should precede `apt-get update` in the same `RUN` statement
- [ ] if using pipes in command then errors should be correctly propagated - `set -o pipefail &&`: `RUN set -o pipefail && wget -O - https://some.site | wc -l > /number`
- [ ] the main command should be provided in the `ENTRYPOINT` command and parameters in the `CMD`

    *it allows inspect the main service process without any additional knowledge*
- [ ] if the service uses a filesystem for persistence then used directories should be explicitly specified in the Dockerfile with the `VOLUME` command.
