# Trigger Buildkite Pipeline GitHub Action

A [GitHub Action](https://github.com/actions) for triggering a build on a [Buildkite](https://buildkite.com/) pipeline.


<img src="screenshot.png" alt="Screenshot of the Trigger Buildkite GitHub Action Node" width="298" />

## Features

* Creates builds in Buildkite pipelines, setting commit, branch, message.
* Provides the build JSON response and the build URL as outputs for downstream actions.

## Usage

Create a [Buildkite API Access Token](https://buildkite.com/docs/apis/rest-api#authentication) with `write_builds` scope, and save it to your GitHub repository’s **Settings → Secrets**. Then you can configure your Actions workflow with the details of the pipeline to be triggered, and the settings for the build.

For example, the following workflow creates a new Buildkite build on every commit:

```
on: [push]

steps:
  - name: Trigger a Buildkite Build
    uses: "buildkite/trigger-pipeline-action@v1.3.0"
    env:
      BUILDKITE_API_ACCESS_TOKEN: ${{ secrets.TRIGGER_BK_BUILD_TOKEN }} 
      PIPELINE: "my-org/my-deploy-pipeline"
      BRANCH: "master"
      COMMIT: "HEAD"
      MESSAGE:  ":github: Triggered from a GitHub Action"
```

## Configuration Options

The following environment variable options can be configured:

|Env var|Description|Default|
|-|-|-|
|PIPELINE|The pipeline to create a build on, in the format `<org-slug>/<pipeline-slug>`||
|COMMIT|The commit SHA of the build. Optional.|`$GITHUB_SHA`|
|BRANCH|The branch of the build. Optional.|`$GITHUB_REF`|
|MESSAGE|The message for the build. Optional.||
|BUILD_ENV_VARS|Additional environment variables to set on the build, in JSON format. e.g. `{"FOO": "bar"}`. Optional. ||

## Outputs

The following outputs are provided by the action:

|Output var|Description|
|-|-|
|url|The URL of the Buildkite build.|
|json|The JSON response returned by the Buildkite API.|

## Development

To run the test workflow, you use [act](https://github.com/nektos/act) which will run it just as it does on GitHub:

```bash
act
```

## Testing

To run the tests locally, use the plugin tester (that has everything already installed) by running the Docker command

```bash
docker run --rm -ti -v "$PWD":/plugin buildkite/plugin-tester:v4.0.0
```

## Contributing

* Fork this repository
* Create a new branch for your work
* Push up any changes to your branch, and open a pull request. Don't feel it needs to be perfect — incomplete work is totally fine. We'd love to help get it ready for merging.

## Releasing

* Create a new GitHub release. The version numbers in the readme will be automatically updated.

## Roadmap

* Add a `WAIT` option for waiting for the Buildkite build to finish.
* Support other properties available in the [Buildkite Builds REST API](https://buildkite.com/docs/apis/rest-api/builds#create-a-build), such as environment variables and meta-data.

Contributions welcome! ❤️
