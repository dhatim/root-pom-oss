[![Build Status](https://travis-ci.org/dhatim/business-hours-java.svg?branch=master)](https://travis-ci.org/dhatim/root-pom-oss)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.dhatim/root-oss/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.dhatim/root-oss)

# Maven root POM for Open Source Dhatim projects

## Rationale

The goal of this project is to provide unified and centralised version
management for project dependencies and maven plugins. It's also well
suited to include some default configuration that would otherwise get
copied/pasted from project to project.

## Update Policy

The dependency and plugin versions should be kept reasonably
up-to-date; We think it's better to update a bit all the time and
catch problems early than let obsolescence creep in.

Frequent releases allow child projects to keep a stable dependency
set.

## How to Release

TBD

<!--
This project is set up according to the
[Maven Releases on Steroids](https://axelfontaine.com/blog/final-nail.html)
way. You can read by yourself how it works, but day to day it means that:

- If you edit anything in this project, keep the version `0-SNAPSHOT`.

- Releases are
[jenkins](http://jenkins.dhatim.it/job/root-pom/)-driven. When
["Performing Maven Release"](http://jenkins.dhatim.it/job/root-pom/m2release),
fill in the desired "Release Version" field. The "Development version"
field, on the other hand, is unused.
-->

### Semantic versioning

We apply the [semantic versioning rules](http://semver.org/) with the following adaptations:
- Project version is x.y.z.
- Increment major version x if:
  - You upgrade the major version of an existing plugin or dependency.
  - You perform any change impacting current users: for example, version enforcement rules.
- Increment minor version y if:
  - You upgrade the minor version of an existing plugin or dependency.
  - You add new plugins or dependencies.
- Increment patch version z if:
  - You upgrade the patch version of an existing plugin or dependency.
  - You fix some bug and this does not impact current users.

Before releasing, verify the current version of the project by listing tags. For example:
```shell
$ git fetch
...
$ git tag
...
3.2.0
4.0.0
```

## How to Contribute

Since this project centralizes dependencies for the entire company, it
is a good place to apply scrutiny to those dependencies. So:

- as a contributor, try to facilitate review. Have your PR state what
  downstream project it relates to (if applicable). Try to split
  logically-related modifications in distincts PRs and/or in separate
  commits. If somehow the dependency you're using displeases
  reviewers, maybe the right course of action is to keep it downstream
  for now (instead of "let's get the PR accepted anyway and deal with
  it later").

- as a reviewer, please be extra careful. Ask questions, research
  proposed new dependencies, don't blindly accept them. Check if
  dependencies are maintained, widespread, if there are alternatives
  or better options.

- as both contributor and reviewer, keep dependency convergence in
  mind.

## Usage

In your project top-level pom, instead of e.g.:

```xml
<project>

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.dhatim</groupId>
    <artifactId>my-oss-project</artifactId>
    <version>x.y.z-SNAPSHOT</version>
    <packaging>pom</packaging>
        …/…
```

set it up so it inherits this artifact:

```xml
<project>

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.dhatim</groupId>
        <artifactId>root-oss</artifactId>
        <version>0.0.2</version>
    </parent>
    
    <artifactId>my-oss-project</artifactId>
    <version>x.y.z-SNAPSHOT</version>
    <packaging>pom</packaging>
        …/…
```

:point_up: Since the parent is a (kind of) dependency, your project
should inherit a RELEASE version (i.e. not a SNAPSHOT one) in order to
get a stable dependency set.

Then, remove any dependency and plugin version information from your
project.

## FAQ

### the "most recent" version of some dependency or plugin actually isn't the one I, or anybody else, want!

`rules.xml` allow to exclude specific versions from the version checks.

### My project use dependencies not listed in there

If some dependencies or plugins versions are missing from the root
POM, add them to it and release a new version. While you're at it,
check wether other versions need upgrading.

### My project breaks when I use the most recent version of...

If you really wish to depend on some specific version, keep a local
override in your POM (but you should strive to fix that situation
ASAP).

### Some default plugin configuration gets in my way

Try to find out and understand why it's there, and to merge your way
with the default's. Make it evolve if needed; Talk to other users and
document what you break or remove so it's the least surprising for the
other projects.

### Sort dependencies

Dependencies declaration must be defined by lexical order by groupId,
then artifactId.  The
[Sortpom plugin](https://github.com/Ekryd/sortpom) verifies this order
when building.  You can sort manually when adding new dependencies:

```shell
$ mvn sortpom:sort
$ mvn sortpom:verify
```

The sort command will change the `pom.xml` file, so don't forget to
re-format it after sorting.
