# Quick Repeatable Commands

## Initialize a new shell to execute steps (from step 2 onwards)

```sh
# For Linux or MacOS
export TUTORIAL_HOME=~/mft-tutorial
```

```bat
@REM For Windows CMD / BAT files
SET TUTORIAL_HOME=%USERPROFILE%/mft-tutorial
```

```pwsh
# For Windows Powershell
${TUTORIAL_HOME}=${env:USERPROFILE}/mft-tutorial
```

## Start the tutorial sandbox

```sh
# On the host
docker compose -f ${TUTORIAL_HOME}/1c-mft-hybrid-example/.sbx/1c-mft-example/docker-compose.yml up -d
```

## New shell in tutorial's container

```sh
# On the host
docker exec -it 1c-mft-example sh
```


## Close the tutorial sandbox

```sh
# On the host
docker compose -f ${TUTORIAL_HOME}/1c-mft-hybrid-example/.sbx/1c-mft-example/docker-compose.yml down -t 0 -v
```
