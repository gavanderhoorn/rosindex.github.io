---
layout: page
title: Design
permalink: /about/design/
breadcrumbs: ['about']
---

# ROS Index Design

ROS Index was developed to address the limitations of the [ROS
Wiki](http://wiki.ros.org). The biggest issue plaguing the Wiki is the lack of
synchronization between documentation and changing interfaces. Originally, a
wiki system suited the ROS community well, since most ROS code was contained in
centralized SVN repositories. In order to contribute documentation, a user
could do so on the wiki, without needing access to the code. Since ROS was
first released, however, most code has migrated to distributed version control
systems which make it much easier for users to contribute back via pull or merge
requests. ROS Index focueses on presenting documentation which is coupled to
version-controlled code and on making this documentation aggregated in one
place.

{% toc 2 %}

## Index Organization

ROSIndex organizes ROS source code in two ways:

 * By repository
 * By package

### Repository Organization

The repository organization is simple: each repository identifier corresponds
to a set of repository `instances`. Repository instances are different versions
of a repository with the same name but different URIs. This enables ROSIndex
to index forks of known repositories. Within each instance, branches and tags
correspond to different ROS distributions. So versions of repositories can be
organized hierarchically like so:

 1. repository identifier
 2. repository instance
 3. ROS distribution (branch/tag)

This gives rise to urls like:

```
rosindex.github.io/r/<<REPOSITORY>>/<<INSTANCE>>/#<<DISTRO>>
```

So for the ``geometry`` repository, the default instance would be resolved by:

```
rosindex.github.io/r/geometry
```

In this case, this would be equivalent to:

```
rosindex.github.io/r/geometry/rosdistro
```

This link would resolve to the repository corresponding to the URI given in the
*latest* rosdistro index. Then the user could switch between distros of *that
repository* with the distro selector buttons.

> Note that the repository for a repository identifier can change between
> distributions. In this case, it is expected that older versions of the sources
> are tagged in the *latest* version of the repository.

Specific instances of packages can be resolved by the following:

 1. repository identifier
 2. repository instance
 3. package name
 4. ROS distribuition (branch/tag)

### Package Organization

Normally, however, users are interested in looking up ROS code by package name.
In this case, it's more subtle. Between ROS distributions, a package can migrate
from one repository to another. As such, a hierarchical organization like that
used for repositories doesn't fit as well. It could have the effect of obscuring
versions of code for a newer or older distribution because it changed
repositories.

Most importantly, people want to see the documentation for *offifical* packeges.
As such, it makes sense that when browsing packages, the default instances for a
distribution are organized like so:

 1. package name
 2. ROS distribution (repo/instance/branch/tag)

When viewing the tab for a given ROS distribution, the user can see additional
metadata about the package, as well as in which repository that package is
located.

## Finding Sources

### Official Index

The ROS Index site generator reads `rosdistro` files to get lists of released
and unreleased ROS repositories. For all repositories with source links, it
adds them to a known repositories index. 

Files are read from two places in `rosdistro`:

 * `_rosdistro/<<DISTRO>>/distribution.yaml`
 * `_rosdistro/doc/<<DISTRO>>/*.rosinstall`

ROS packages are uniquely located in rosdistro by a distribution (groovy,
hydro, indigo) and a repository identifier. In a given distribution, package
names are unique.

### Forks

Unfortunately, the *rosdistro* standard, as defined in
[REP-141](http://ros.org/reps/rep-0141.html), does not accomodate the indexing
of *forks*. Until the standard is extended to do so, rosindex will support the
indexing of this additional information.

Forks are described in the YAML-formatted markdown files in the `_repos`
directory which correspond to the repository names in *rosdistro*. Each
repository fork should be given an identifying name for easy reference on the
rosindex website.

```yaml
instances:
  - uri: 'https://github.com/jbohren/conman.git'
    type: git
    default: true
    purpose: 'original'
    distros:
      hydro: { default: true, version: 'master' }
  - uri: 'https://github.com/RCPRG-ros-pkg/conman.git'
    type: git
    purpose: 'experimentation'
    distros:
      hydro: { version: 'master' }
```

## Information Collected by ROS Index

### Repository Information

* Repository URI
* Release status
* Last commit date

### Package Information

* All information contained in the package manifest as described by
  [REP-127](http://www.ros.org/reps/rep-0127.html) and
  [REP-140](http://www.ros.org/reps/rep-0140.html).
* Last commit date

### Package Contents

* ROS Launchfiles
* ROS Message files
* ROS Service files

### ROS Index Metadata

See [ROS Index Metadata](/contribute/metadata) for more details.
