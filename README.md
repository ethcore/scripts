# scripts

Various helpful scripts

## Dockerfiles for images used in GitLab CI

[Documentation](dockerfiles/README.md).
Rust and tools for:

- Substrate-based projects CIs
- android
- arm64
- armv7
- i686
- docs autogeneration
- various tools
- tools for kubernetes

## GitLab CI for building docker images

Pipelines are triggered by schedule. Can be launched manually though.
To launch the pipeline go to the project's CI/CD -> Schedules menu.
To change, edit/create the new schedule:
    set the required variable and cron schedule.


