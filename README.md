# Drone 

This repository contains a docker-compose file and ENV vars that allow you to locally deploy the Drone CI server and runner. This setup is meant for the local instance of GitLab.

Default port mappings, which can be changed in the `docker-compose.yaml` file:

- Drone server:
    - 80:8180
    - 443:8143

- Drone runner:
    - 3000:8300

To start the deployment, configure GitLab and update the ENV files by following the instructions in the next section. Once you're done run the below command:

`docker-compose up -d`

After the server comes up you should be able to navigate to the `http://<drone-server-ip>:8180` address from your browser.

If everything is configured correctly you should be redirected to the GitLab login page after clicking `Continue` on the Drone welcome page.

After successful GitLab login, you should be sent back to Drone where you can simply click `Submit` without filling any fields to be taken to Dashboard.

Once in Dashboard click on `Sync` in the top-right corner. If there is full connectivity between Drone and GitLab your GitLab repositories should appear in Drone.

To kick off a pipeline run Drone will look for `.drone.yml` in the root of your repository.

You can find a very basic example pipeline in the `.drone.yaml.example` file.

And that's it. Have fun with Drone!

## Configure GitLab

You need to add Drone to applications in GitLab. To do that follow instructions in the Drone docs: [https://docs.drone.io/server/provider/gitlab/](https://docs.drone.io/server/provider/gitlab/)

Once you configured GitLab you should have `Application ID` and `Secret` that you will add to ENV VARS for Drone.

## Update variables in ENV files

There are two ENV files, located in `./env/`, used in this deployment.

### runner.env

`runner.env` configures the Drone runner service.

Edit the file and set `DRONE_RPC_SECRET` variable to be a shared secret used between the Drone server and runner.

You can generate a shared secret using the below command:

```
openssl rand -hex 16
```

### server.env

`server.env` file is used to configure the server service.

You need to set the below variables:

- `DRONE_GITLAB_CLIENT_ID` - set this to the  `Application ID` you got from GitLab.
- `DRONE_GITLAB_CLIENT_SECRET` - Set this to the `Secret` you got from GitLab .
- `DRONE_GITLAB_SERVER` - This should be set to the _IP:port_ or _hostname_ of your GitLab instance. This must be reachable by your Drone server.
- `DRONE_RPC_SECRET` - secret shared between the Drone server and runner.
- `DRONE_SERVER_HOST` - _IP/hostname:port_ of your Drone server. This must be the same as what you configured in GitLab when adding Drone to the applications.

## References

- [Drone docs](https://docs.drone.io/)