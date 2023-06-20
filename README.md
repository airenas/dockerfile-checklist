# dockerfile-checklist

The file contains a simple checklist for manually checking a Dockerfile for security issues and other recommended practices

## Security

- [ ] starts the main process as a non-root user, has `USER` command
- [ ] the final image contains only a minimal set of SW to run the service:
    - [ ] uses a light, slim base image
    - [ ] packages - no unneeded 3rd-party packages installed
    - [ ] code - no source code for compiler languages, just a binary
    - [ ] or for interpreted languages - just needed scripts
    - [ ] check with snyk for vulnerabilities
    - [ ] check downloaded SW with curl or wget using checksums
## Reproducibility
- [ ] has an exact version of the base image set
- [ ] compiles binary inside docker, uses two-step docker file 
- [ ] takes the version of the service as a `docker` parameter and add/compiles into binary

## Other

- [ ] has the `EXPOSE` command listed if the service needs some ports
- [ ] the service responds to terminate command
- [ ] the main process rips zombies
- [ ] `apt-get install` commands alphabetically sorted
- [ ] Always combine RUN apt-get update with apt-get install in the same RUN statement.
- [ ] if using pipes in commands - `set -o pipefail &&`: `RUN set -o pipefail && wget -O - https://some.site | wc -l > /number`
- [ ] the main command with `ENTRYPOINT` and parameters as `CMD` 
- [ ] has `VOLUME` for database-like services that persist data
