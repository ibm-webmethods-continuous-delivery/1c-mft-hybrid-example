# `step01`: Ensure basic prerequisites and familiarize with the `lazygit` sandbox

To execute this tutorial step, you need a box having git, docker-compose and access to internet allowing for docker build command to work as needed.

In this box, create a tutorial folder, for example `~/mft-tutorial` on Mac / Linux or `%USERPROFILE%\mft-tutorial` on Windows. We will refer to this folder as `<tutorial-home>` in the following steps.

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

---

← [Back to README](../README.md) | Next: [`step02`](step02.md) →
