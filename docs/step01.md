# `step01`: Ensure basic prerequisites and familiarize with the `lazygit` sandbox

To execute this tutorial step, you need a box having git, docker-compose and access to internet allowing for docker build command to work as needed.

In this box, create a tutorial folder, for example `~/mft-tutorial` on Mac / Linux or `%USERPROFILE%\mft-tutorial` or `${env:USERPROFILE}/mft-tutorial` on Windows. We will refer to this folder as `${TUTORIAL_HOME}` or `%TUTORIAL_HOME%` or `${env:TUTORIAL_HOME}` in the following steps according to the user environment:

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


Open a shell or command window in that folder and execute the commands:

```sh
git clone -b step01 https://github.com/ibm-webmethods-continuous-delivery/1c-mft-hybrid-example.git
```

Then for MacOS, Linux or Powershell

```pwsh
cd 1c-mft-hybrid-example/.sbx/1c-mft-example
```

OR for Windows CMD

```bat
cd 1c-mft-hybrid-example\.sbx\1c-mft-example
```

For simplicity, from now on the code snippets are provided for MacOS, Linux or Powershell, as the syntax is almost the same. The user is expected to correct the small inaccuracies according to the shell used.

Now copy EXAMPLE.env into .env and change the values in .env according to your needs. Mind that the user and group ids should be the same as the user executing the docker commands.

Run the following commands:

```sh
docker compose up -d
docker exec -it 1c-mft-example sh
```

From now on we are in a container shell. The `git fetch` command makes all future steps tags and branches visible locally inside the container, so you can navigate between steps with `lazygit`.

```sh
git fetch
lazygit
```

When the tutorial step execution finishes, the user may throw away immediately the git exploration sandbox:


```sh
# On the host
cd ${TUTORIAL_HOME}/1c-mft-hybrid-example/.sbx/1c-mft-example
docker compose down -t 0 -v
```

or directly from any path:

```sh
# On the host
docker compose -f ${TUTORIAL_HOME}/1c-mft-hybrid-example/.sbx/1c-mft-example/docker-compose.yml down -t 0 -v
```

For convenience, some quick commands reference is kept in [this file](docs/QUICK_COMMANDS.md).


---

← [Back to README](../README.md) | Next: [`step02`](step02.md) →
