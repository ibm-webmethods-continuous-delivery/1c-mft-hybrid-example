# Hybrid Managed File Transfer Utilization Example

This example is provided with the purpose of showing how hybrid deployments work when using edge runtimes in a data exchange flow using IBM Integration (IWHI) SaaS.

The scenario described here is expressed in terms of data flows and servicing relation with the diagram below:

![Overall Data Flow](./img/01.OverallDataFlow.png)

In terms of continuous delivery, this example shows how the general framework principles work with edge code, where they find good applicability, as opposed to the general SaaS approach, which is much more oriented towards a "click on web UI" approach.

The current git repository offers a step by step approach, where the steps are tagged git commits instead of separate folders. This way differences between steps can be seen in the commit differences themselves.

Steps are numerated with two decimal digits, including a leading zero in case the step number is lower than 10.

This tutorial itself is built in an incremental manner. Markdown files describing the steps themselves evolves as the tutorial is created.

At the moment of its initial planning, that is at `step01`, the tutorial contains the following steps:

- `step01`: Ensure basic prerequisites and familiarize with the `lazygit` sandbox
- `step02`: Ensure all prerequisites and build edge images with debugging and jdbc adapter
- `step03`: Create MFT SaaS user, virtual folder and reception rule. Test using web client
- `step04`: Create the test harness automation to send a zip file fixture and enable our TDD approach
- `step05`: Extend MFT processing of the file to produce notifications towards IBM Integration SaaS
- `step06`: Extend IBM Integration to propagate the notification towards and edge instance
- `step07`: Explore in detail packages types and local development arrangements
- `step08`: Extend Edge capabilities to unzip the file and verify its integrity
- `step09`: Extend Edge capabilities to parse and validate inbound records
- `step10`: Extend Edge capabilities to upsert records in a local database

Working in an agile mode, this plan may change as we go ahead and learn further details.

## `step01`: Ensure basic prerequisites and familiarize with the `lazygit` sandbox

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

> **Note for Rootless Docker on Linux:**
> When running with rootless Docker on Linux, the container non-root user needs additional group permissions to access bind mounts properly. Ensure you set `CONSIDER_ADDITIONAL_GROUP=true` and `ADDITIONAL_GROUP_ID=0` (root group) in your `.env` file, and grant write permissions for the group on the host folder if necessary.

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
