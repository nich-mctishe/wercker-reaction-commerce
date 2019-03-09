
build status: [![wercker status](https://app.wercker.com/status/881fe9d430fdbe1b2e1f5f4091f99c37/s/master "wercker status")](https://app.wercker.com/project/byKey/881fe9d430fdbe1b2e1f5f4091f99c37)

## SYSTEM REQUIREMENTS

- Docker 18.09.1
- Wercker 1.0.1467

### Getting Started

* Clone this repo
* cd into the root of the repo and run `tools/dev`

The application should now be running on port 3000. You will be attached to the running container and the log viewer will run.

If you need to run adhoc commands, hit `ctrl+c` to exit the log viewer. You can return to the log viewer by simply running `log`

To exit fully from the container, hit `ctrl+c` to exit the log viewer and then type `exit` to exit the container.

### Development Credentials

**EMAIL/LOGIN:** admin@localhost
**PASSWORD:** r3@cti0n


## DOCKER REQUIREMENTS

### DEV

- 4 CPU's
- 3 GB RAM
- 1.5 GB SWAP

Startup time: 11 min.

******************

- 8 CPU's
- 8 GB RAM
- 4 GB SWAP

start: 19:29:30
end: 19:41:00
startup time: 10.5 mins

### MINIMUM LIVE REQUIREMENTS

$5 DO instance:
- 1 CPU
- 1 GB RAM
- 4 GB SWAP

1:
start: 18:05:00
end: 18:20:00
startup time: 15 min.
notes: requests are processed very slowly, may need to get upgraded package.

2 (with --production flag):
start: 18:27:15
end: 19:06:00
startup time: 40 min.
notes: long to start in dev mode. would have serious issues passing wercker checks.
may have to abandon wercker set up if this is true.

******************

$10 DO instance:
- 1 CPU
- 2 GB RAM
- 4 GB SWAP

1:
start: 09:24:30
end: 09:33:00
startup time: 8.5mins
notes: quick! (relatively) also browsing not to bad once cached.

2 (with --production flag):
start: 09:53:30
end: 10:10:00
startup time: 17-20 mins
notes: manageable

## Deploy to dokku

watch out for checks, will need to make these long in order to ensure long initial setup does not time out.

### Docker base images

You should periodically update the node base image as new versions are released. To do so, update the node box in `wercker.yml` and the base image in `dokku/Dockerfile` for deployment.

### Wercker Project Setup

1. Navigate to your project and open the `Workflows` tab
2. Click `Add new pipeline`
3. Fill in the form:
    - **Name** - [staging/production]
    - **YML Pipeline name** - deploy
    - **Hook type** - default
4. Click the `Create` button
5. On the resulting page, create a new SSH key by clicking the `Generate SSH Keys` link below and to the right of the Pipeline environment variables section:
    - **SSH key name** - DOKKU_KEY
6. Create three environment variables:
    - **DOKKU_HOST** - String - [your dokku host ip or fqdn]
    - **DOKKU_APP** - String - reaction
7. Go back to the `Workflows` tab
8. click the `+ icon` next the the `build` workflow.
9. Enter the branch you wish to deploy, select the pipeline you just created for the execute pipeline, and click `Add`

### Dokku Server Setup

Once you've booted your Dokku server, on the installation screen, copy and paste the public SSH key from the Wercker Project target. Then run the following steps:

```bash
# firstly, set up hydra for auth
apt install docker-compose
docker network create auth.reaction.localhost
git clone https://github.com/reactioncommerce/reaction-hydra.git .hydra
cd .hydra
docker-compose up -d

# Set some environment variables
export MONGO_IMAGE_VERSION="3.4.4"

# Install Mongo Dokku plugin
dokku plugin:install https://github.com/dokku/dokku-mongo.git

# Create our reaction app
dokku apps:create reaction

# Create our Mongo service and link to our reaction app
dokku mongo:create reaction-db
dokku mongo:link reaction-db reaction

# Create our volumes

# Mount our volumes

# Restart our application on failure
dokku docker-options:add reaction deploy --restart=always

# Set our config values
dokku config:set reaction DEBUG="req,app,app:*"
dokku config:set reaction DEBUG_COLORS="1"
dokku config:set reaction COOKIE_SECRET="$(< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c64)"
dokku config:set reaction GOOGLE_ANALYTICS_ID="[ga_id]"
dokku config:set reaction METEOR_ALLOW_SUPERUSER=true
dokku config:set reaction HYDRA_ADMIN_URL=http://hydra:4445
dokku config:set reaction HYDRA_TOKEN_URL=http://hydra:4444/oauth2/token
dokku config:set reaction HYDRA_OAUTH2_INTROSPECT_URL=http://hydra:4445/oauth2/introspect
dokku config:set reaction OAUTH2_CLIENT_DOMAINS=http://localhost:4000
dokku config:set reaction METEOR_WATCH_POLLING_INTERVAL_MS=10000
dokku config:set reaction REACTION_EMAIL="youradmin@yourdomain.com"
dokku config:set reaction REACTION_USER="admin-username"
dokku config:set reaction REACTION_AUTH="admin-password"

# Setup out domains
dokku domains:add reaction example.com
dokku domains:add reaction www.example.com

```

currently having issues with buildpack selection on dokku
