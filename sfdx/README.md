# Buildkite to Github Actions

The purpose of this work is to move off of Buildkite and simplify the sfdx Build / Test / Deploy process. Moving to Github Actions allows for 100% of the s.p.a. and appexchange package to be fully automated inside of Github. Now for the details of how it works and how to add/modify the process.

## The Action is broken into 4 parts which lives inside .github folder. 
```
main.workflow
sfdx_action/Dockerfile
sfdx_action/entrypoint.sh
sfdx_action/bin/{shell scripts to do work}
```

## Workflows are defined HCL [HCL](https://github.com/hashicorp/hcl) and relates to Buildkite pipeline files which looks like this

```
action "Download Static Assets" {
  uses = "./.github/sfdx_action"
  needs = ["Dependencies / JWT Token"]
  args = ["build_apps"]
  secrets = [  ]
  env = {  }
}
```

uses: Points to the Dockerfile to load (this can be published as a repo in future)
needs: Requires a previous step to complete before staring this step
args: The arg passed is the name of the script you would like to run inside of .github/sfdx_action/bin
secrets: Secrets needed for step (tokens/api) (Must use GH UI to add/delete)
env: Environment variables for the current step (Can use GH UI or commit code)

## The entrypoint.sh is what allows args to be passed and run the current step. 

```
#!/bin/sh
set -e

sh -c "$*"
```


And finally let's look at a single step/script

```
#!/bin/sh
set -e

echo "Get Latest Static Resources"

echo 'Download Static Assets'
tools/build-apps
```


That's it. If you need a new process or step just make a shell script inside of .github/sfdx_action/bin then chmod to make executable and finally create/name and pass the arg of the script you want inside of main.workflow