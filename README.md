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

In this box, create a tutorial folder, for example `~/mft-tutorial` on Mac / Linux or `%USERPROFILE%\mft-tutorial` on Windows.

Open a shell or command window in that folder and execute the commands:

```sh
git clone -b step01 https://github.com/ibm-webmethods-continuous-delivery/1c-mft-hybrid-example.git

cd 1c-mft-hybrid-example/.sbx/1c-mft-example
```

Now copy EXAMPLE.env into .env and change the values in .env according to your needs. Mind that the user and group ids should be the same as the user executing the docker commands.

Run the following commands:


```sh
docker compose up -d

docker exec -it 1c-mft-example sh

git fetch

lazygit
```

The `git fetch` command makes all future step tags visible locally inside the container, so you can navigate between steps with `lazygit`.

