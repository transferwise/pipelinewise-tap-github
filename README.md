# pipelinewise-tap-github

[![PyPI version](https://badge.fury.io/py/pipelinewise-tap-github.svg)](https://badge.fury.io/py/pipelinewise-tap-github)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/pipelinewise-tap-github.svg)](https://pypi.org/project/pipelinewise-tap-github/)
[![License: MIT](https://img.shields.io/badge/License-AGPLv3-yellow.svg)](https://opensource.org/licenses/AGPL-3.0)

[Singer](https://singer.io) tap that produces JSON-formatted data from the GitHub API following the [Singer spec](https://github.com/singer-io/getting-started/blob/master/SPEC.md).

This is a [PipelineWise](https://transferwise.github.io/pipelinewise) compatible tap connector.

This tap:
- Pulls raw data from the [GitHub REST API](https://developer.github.com/v3/)
- Extracts the following resources from GitHub for a single repository:
  - [Assignees](https://developer.github.com/v3/issues/assignees/#list-assignees)
  - [Collaborators](https://developer.github.com/v3/repos/collaborators/#list-collaborators)
  - [Commits](https://developer.github.com/v3/repos/commits/#list-commits-on-a-repository)
  - [Issues](https://developer.github.com/v3/issues/#list-issues-for-a-repository)
  - [Pull Requests](https://developer.github.com/v3/pulls/#list-pull-requests)
  - [Comments](https://developer.github.com/v3/issues/comments/#list-comments-in-a-repository)
  - [Reviews](https://developer.github.com/v3/pulls/reviews/#list-reviews-on-a-pull-request)
  - [Review Comments](https://developer.github.com/v3/pulls/comments/)
  - [Stargazers](https://developer.github.com/v3/activity/starring/#list-stargazers)
- Outputs the schema for each resource
- Incrementally pulls data based on the input state

## Quick start

1. Install

   We recommend using a virtualenv:

    ```bash
    python3 -m venv venv
    . venv/bin/activate
    pip install --upgrade pip
    pip install .
    ```

2. Create a GitHub access token

    Login to your GitHub account, go to the
    [Personal Access Tokens](https://github.com/settings/tokens) settings
    page, and generate a new token with at least the `repo` scope. Save this
    access token, you'll need it for the next step.

3. Create the config file

    Create a JSON file containing the start date, access token you just created
    and the path to one or multiple repositories that you want to extract data from. Each repo path should be space delimited. The repo path is relative to
    `https://github.com/`. For example the path for this repository is
    `singer-io/tap-github`. 

    ```json
    {"access_token": "your-access-token",
     "repository": "singer-io/tap-github singer-io/getting-started",
     "start_date": "2021-01-01T00:00:00Z"}
    ```
4. Run the tap in discovery mode to get properties.json file

    ```bash
    tap-github --config config.json --discover > properties.json
    ```
5. In the properties.json file, select the streams to sync

    Each stream in the properties.json file has a "schema" entry.  To select a stream to sync, add `"selected": true` to that stream's "schema" entry.  For example, to sync the pull_requests stream:
    ```
    ...
    "tap_stream_id": "pull_requests",
    "schema": {
      "selected": true,
      "properties": {
        "updated_at": {
          "format": "date-time",
          "type": [
            "null",
            "string"
          ]
        }
    ...
    ```

6. Run the application

    `tap-github` can be run with:

    ```bash
    tap-github --config config.json --properties properties.json
    ```


## To run tests

1. Install python test dependencies in a virtual env and run nose unit and integration tests
```
  python3 -m venv venv
  . venv/bin/activate
  pip install --upgrade pip
  pip install -e .[test]
```

2. To run unit tests:
```
  pytest tests/unittests
```
