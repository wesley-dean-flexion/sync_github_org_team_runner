# GitHub Organization to Team Synchronization Runner

## Overview

This repository exists as a "runner" to run a scheduled
task to perform synchronization of Flexion's GitHub
Organization to a team under the Flexion organization.

## What

This will synchronize Flexion's GitHub organization
with a team under that organization named `everyone`.

## Why

GitHub does not allow users to add organizations as
collaborators to repositories, only teams.  Public
GitHub does not include native functionality to
synchronize a GitHub organization with a team.  So,
the [tool](https://github.com/wesley-dean-flexion/sync_github_org_team)
called by this runner performs the synchronization.

## How

The tool runs in a containerized environment based on the
[published image](https://hub.docker.com/r/wesleydeanflexion/sync_github_org_team)
built from the aforementioned repository.  

## When

The task may be run manually; otherwise, it runs via scheduled task
(cron job), initially set to every 4 hours.  The task is configured
to delay 0.5 seconds between API calls; with 189 users (the current
count), that's less than 2 minutes for the task to run.  Running the
task while another run is happening will cancel the first run.  The
synchroniation task has a timeout of 10 minutes.

## Who

The tool allows for the specification of filters to
generate a subset of users from the Flexion organization
to add to the team.  This is described on the tool's README's
[filtering users](https://github.com/wesley-dean-flexion/sync_github_org_team#filtering-users)
section.

The tool itself runs with a Personal Access Token (PAT)
which is passed to the tool as a secret.  The PAT is
associated with
[Wes Dean's account](https://github.com/wesley-dean-flexion/)
so if Wes is no longer with Flexion (i.e., if the
`wesley-dean-flexion` account is closed), it would be best to
update that PAT.

## Where

The container runs on GitHub's actions infrastructure.  The
container does not require disk access (it is configured
with environment variables), so it could run anywhere that
has network access to the GitHub API.
