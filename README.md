# Cloud Foundry Sleepy Buildpack

Cloud Foundry buildpack that sleeps at *run* time, prior to the app starting.

For sleeping at *build* time, see https://github.com/emalm/investigation-buildpack.

## Usage

Push the app with the sleepy buildpack, before other buildpacks:
```
cf push my-app -b https://github.com/Gerg/sleepy-buildpack.git -b other_buildpack
```

Make sure the app's health check type is `process`, otherwise the app will fail
health checks, since it will not be running:
```
cf set-health-check my-app process
```

### Configuration

By default, the buildpack sleeps for 300 seconds. To change this value, set the
BP_SLEEP_TIME environment variable for the app:
```
cf set-env my-app BP_SLEEP_TIME 1800
```

## How it Works

The Cloud Foundry Buildpack App Lifecycle [sources buildpack-provided
`profile.d`
files](https://github.com/cloudfoundry/buildpackapplifecycle/blob/cb243e1d2a5f4ca908e6a18f355e7c6953bb6738/launcher/launcher_unix.go#L19-L33)
prior to launching the application. This buildpack adds a `profile.d` file
that sleeps when sourced. This can be helpful for debugging applications, prior
to the app starting or the other buildpack or app pre-start scripts running.
