# SimpleSandbox
A simple sandbox for Java based on SecurityManager.

Currently in proposal stage. Feedback is welcome, and can be provided through the forums.

# Introduction

As per JEP 411, the SecurityManager will be deprecated for removal in the JDK 17 release. As per the JEP, the
cost-to-benefit ratio of maintaining the SecurityManager is high, since the development costs are high while
the community uptake is limited.

The uptake might be limited because
1. Configuring the SecurityManager in a truly secure way is difficult for general developers.
2. Gathering information about which permissions are required for a library is tedious.

This project is a proposal to make SecurityManager easier to use, by:

1. Creating a Java library that is easier to use than the SecurityManager API. It will use and advocate best practices
around the SecurityManager, while providing an opinionated abstraction over the raw API.

2. Creating a public database of permissions required by popular libraries, in a similar
vein as [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped), a public database for type declartion
files used by TypeScript.

3. Creating plugins for popular build tools, like gradle, maven, sbt, etc. These plugins will automatically figure out
the dependencies of the project and fetch the requisite permission lists from the public database.

Hopefully, an outcome of this exercise will be that, as more people use the SecurityManger, the cost for developing it (or a substitue for it) becomes justified and the JDK development team can allocate resources to maintain SecurityManager
or its substitute.

# Requirements
Since this is at proposal stage, only the requirements are being defined now. The design is intentionally vague, to let the community evolve the direction.

## Library
* A custom `Policy` class that controls permissions based on `CodeSource.getLocation()`. This will allow, for example,
for permissions to be specified per `jar` file.
* The definied permission set should be easy to expand or contract, so that permissions from a public database can be
refined on a per-project basis.
* An embedded Java DSL, as well as, an external DSL in a human readable format (Eg, in JSON, YAML or their variants).
* Ability to work with the module system.
* Ability to work with a "fat" jar.

## Permission Database
* Should allow permissions to be defined per library, per version.
* Version could be specified as a wild-card: For example: `1.0.*`. The closest matching version will be used to lookup the permission set.
* Permission specs should be accompanied with test-cases that exercise the permissions.
* A CI script should ensure that permissions are specified as per the tests
* The database should be publicly hosted and queryable by library name and version

## Build Tool Plugins
* Gather dependencies and versions automatically from the project definition
* Query public database for permissions required by these dependencies
* Store permissions lists in project's resource folder, so that the library can enforce them at runtime
