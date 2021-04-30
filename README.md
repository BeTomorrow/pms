Sync a Postman cloud collection with a local file in a git project folder to
keep a versioned export of a collection.

# Requirements

Bash compatible shell, curl, jq, optionally git.
You also need a [Postman API key](https://learning.postman.com/docs/developer/intro-api/).

# Homebrew Install

```
brew install betomorrow/draft/pms
```

# Manual Install

Checkout this repo, add `+x` permission to `pms` if needed and add the
directory to your `PATH` variable or add a symbolic link for example in your
`/usr/local/bin`:

```sh
chmod +x pms
ln -s `pwd`/pms /usr/local/bin/pms
```

# Use

## Configuration

You can use the `init` command to help set up the configuration files.

### User configuration

Create a `~/.pms` user configuration for your Postman API key:

```shell
touch ~/.pms && chmod 0600 ~/.pms
```

```properties
api-key=MY-API-KEY
```

### Project configuration

Create a `pms.config` configuration in your project folder:

```properties
# General name, used for default collection and environment file names
name=MyProject

# Name of the collection (in Postman), optional
# Collection is skipped if not configured
collection-name=Project

# Collection file name, optional
# Defaults to {name}.postman_collection.json
collection-file=something-else.json

# Name of the environment (in Postman), optional
# Environment is skipped if not configured
environment-name=Project - Sample

# Environment file name, optional
# Defaults to {name}.postman_environment.json
environment-file=something-else.json

# Remote repository, optional
# Used for load-remote to retrieve files to load into Postman cloud
remote-repository=git@somehost.com:repo/project.git

# Remote repository branch
# Required to use the remote repository
remote-repository-branch=main
```

## Commands

Use the tool in a project directory with configuration file set as detailed above.

### init

```
pms init
```

Help set up user and project configuration files if missing.

### save

```
pms save
```

Download collection and environment from Postman cloud and save them locally.
Environment file is scrubbed from values and ids. 

### load

```
pms load
```

Upload local collection and environment to Postman cloud.

### load-remote

```
pms load-remote
```

Retrieve collection and environment files from a git `remote-repository`
master branch, and uploads to Postman cloud. This is meant as a lightweight
way to keep a collection up-to-date without using a local clone of the
repository. You still need a `pms.config` configuration, and `~/.pms`.

# Note about environment

Environment is expected to be a sample to hint at properties that can be set
for a collection, not to manage environments directly. Environment initial
values will be purged if you load the scrubbed environment file from a save.
In doubt, leave the `environment-name` not set to skip operations.
