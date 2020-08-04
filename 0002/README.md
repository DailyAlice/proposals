---
id: 0002
title: Set the mood around releasing code
status: ideation
authors: Gianluca Arbezzano <gianarb92@gmail.com>
---

## Summary

This proposal sets the expectation about what it means to release code part of
the Tinkerbell organisation.

## Goals and not Goals

Goals are:

1. What is a release
2. Set a few rules that all the projects should follow
3. Quality assertion.

Not-Goals:

1. How each project should be released
2. Define who own releases
3. Distribution via `apt`, `apk` and so on.

## Content

A release is a tested and stable point in time snapshot from a codebase.

We follow the [Semantic Versioning (semver) 2.0.0](https://semver.org).
Summarized as follows:

Given a version number MAJOR.MINOR.PATCH, increment the:

* MAJOR version when you make incompatible API changes,
* MINOR version when you add functionality in a backwards compatible manner, and
* PATCH version when you make backwards compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions
to the MAJOR.MINOR.PATCH format.

### Schedule

Every subproject should be able to follow its own cadence in terms of how often
or regularly a new release should be cut, as long as it is documented and
shared with the community.

A new `MINOR` version for Tinkerbell should be released following a proper
schedule, right now I think every two months sounds reasonable looking at the
code written and merged in the last couple of weeks. In this way the
subprojects will have an intrinsic release target if they want to get their new
code included in the Tinkerbell release.

`PATCH` release for Tinkerbell can be done as required by the subprojects
bug fixes.

`MAJOR` release right now are not even evaluated because the project is too young.

### GitHub release and release website

Git supports tag and GitHub has the concept of a release based on a particular
tag.

The artifact generated as part of the release will be published as part of the
release website, using as a guidance how [HashiCorp
Release](https://releases.hashicorp.com/) works.

I like the idea to use a release website vs having to push them as manifest in
GitHub release because it gives a single place where the community can
programmatically lookup what they are looking for, without having to look for
repositories, versions and so on.

As part of the Release page on GitHub we will publish a generated changelog.

### Architectures and operating systems

We know there are different architectures and Go supports cross compilations.
When possible we should cross compile binaries at least for:

* Linux
* macOS
* Windows

For `arm` and `x86_64`.

### Ownership

The contributors and maintainers for each project have the responsibility for
the quality of the release itself.

Tinkerbell as a project will have its group of interest around release
management that will take care of it (this is out of scope for this proposal but
has to be discussed in another one).

### Quality assertion

Every project should be covered by tests: unit tests, functional tests and
integration as needed.

If all the test suite succeed the project can be released, otherwise tests
should be fixed first.

The documentation contains in tinkerbell.org covers the entire project and it
has to be reviewed before any Tinkerbell release. Each Tinkerbell MINOR release will
have its own documentation and users need a way to visualize the version they
are looking for.

### Tinkerbell

Tinkerbell itself is more like a meta-project that glues all the subprojects it
depends on. In terms of quality assertion we should have a strong and reliable
pipeline of E2E tests to guarantee that the new version for the subprojects are
working fine in integration.

The subproject Tinkerbell depends on will be pinned as part of the Tinkerbell
release but their version are a "suggestion". SemVer guarantees that all the
PATCH release are working as long as the MINOR release is the same.

Tinkerbell `v0.4.0` depends from boot `v1.2.0` every PATCH release for boot
should work.

SemVer guarantees that any MINOR release after the one pinned from Tinkerbell
should work as well.

Tinkerbell `v0.4.0` depends from boot `v1.2.0` every MINOR release for
boot should work with Tinkerbell `v0.4.0` for example:

* boot `v1.1.0` is not guaranteed to work
* boot `v1.2.0` should work
* boot `v1.2.4` should work
* boot `v1.23.0` should work.

### Support lifecycle

TODO: I need some help here, personally I am thinking about 1 year (6 Tinkerbell
releases)

### Continuous Delivery

All the project should have in place a continuous delivery pipeline capable of
creating a release from a tag.

The process needs to be documented with the possibility to run it locally for
testing purpose and for troubleshooting.

### Documentation

The documentation is versioned as well following the release cadence Tinkerbell
has. A new version of Tinkerbell goes out when E2E tests are green and
documentation is ready.

## System-context-diagram

## APIs

## Alternatives

I didn't find a well explained release process that can be reused. Every project
has its own way even if, at the end the outcome is similar. I am listing here
some successful release process I am involved with, please add yours if
you have because I am sure this proposal will be a glue of the best experiences
we have as a team:

* [Kubernetes Release](https://github.com/kubernetes/sig-release/tree/master/release-team)
* [Docker release page on GitHub](https://github.com/docker/docker-ce/releases)
* [Kubernetes Release page on GitHub](https://github.com/kubernetes/kubernetes/releases)
