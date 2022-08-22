# backup
Repository for backup images, scripts, and actions

## rclone
Simple rclone docker image for accessing cloud storage providers using a `RCLONE_CONF` environment variable
that defines all rclone remotes, appropriate for CI/CD cloud backup operations. Based on https://github.com/wei/rclone.

To use, first develop an `rclone.conf` with all desired remotes defined. This repository can be used locally for this 
purpose by commenting the `ENTRYPOINT` invocation in the `Dockerfile` and running 
```
docker run --rm -e "RCLONE_CONF=$(cat rclone/rclone.conf)" $(docker build -q rclone/.) bash
```
and then running [rclone config](https://rclone.org/docs/) to generate the .conf file. Once it's working, `rclone.conf`
can be used with docker commands below (`ENTRYPOINT` uncommented) or by pasting the contents into a Github secret with 
key `RCLONE_CONF` and using in an Action.

### Local docker usage
```
docker run --rm -e "RCLONE_CONF=$(cat rclone/rclone.conf)" $(docker build -q rclone/.) \
    ls <remote>:<path>
```
```
docker run --rm -e "RCLONE_CONF=$(cat rclone/rclone.conf)" $(docker build -q rclone/.) \
    sync --progress <source-remote>:<path> <destination-remote>:<path>
```

### Github Actions usage

```
name: Sync backups
on:
  schedule:
    - cron: "0 0 * * *"  # Runs daily
  workflow_dispatch:
jobs:
  rclone:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: rclone
      uses: data-mermaid/backup/rclone@v1
      env:
        RCLONE_CONF: ${{ secrets.RCLONE_CONF }}
      with:
        args: sync <source-remote>:<path> <destination-remote>:<path>
```
