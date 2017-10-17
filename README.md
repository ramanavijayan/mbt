![mbt](assets/logo-m.png)
# mbt
>> Build utility for monorepos

![build status](https://travis-ci.org/buddyspike/mbt.svg?branch=master)
[![Build status](https://ci.appveyor.com/api/projects/status/vm1lat73uo80ckoj?svg=true)](https://ci.appveyor.com/project/buddyspike/mbt)
[![Go Report Card](https://goreportcard.com/badge/github.com/buddyspike/mbt)](https://goreportcard.com/report/github.com/buddyspike/mbt)
[![Coverage Status](https://coveralls.io/repos/github/buddyspike/mbt/badge.svg?branch=master)](https://coveralls.io/github/buddyspike/mbt?branch=master)

Have a git repository with source for multiple applications? 
mbt is a simple utility to produce predictable, versioned 
build artifacts out of your git source tree.

[This blog post](https://buddyspike.github.io/blog/post/building-modular-systems-with-mbt/) covers some initial thinking behind the tool.

## How it works
mbt reads your git tree looking for directories with a spec file. The 
spec file must be named `.mbt.yml`. It is used to 
instruct how to build the application in that directory.

### An example of spec file 
```yaml
name: my-cool-app   # name of the app
build:              # list of build commands to execute in each platform
  darwin:
    cmd: ./build.sh # build command
    args: [a, b]    # optional list of arguments to be passed when invoking the build command
  linux:
    cmd: ./build.sh
    args: [a, b]
properties:         # dict of arbitrary values that can be used in templates when running mbt apply
  foo: bar
```

mbt uses the information in the spec file and the repository to generate a 
manifest of all applications available at a given revision.
Manifest contains a content based - SHA1 version for each application.
Unlike git commit sha, application version sha does not change due to operations
such as rebase (that results in a change to parent commit). This property of 
the system comes in handy when building pull-requests (or feature branches).
Build scripts can tag the artifacts with the application version and publish them
to your package manager of choice. Your testing and verification could take 
place in those packages. Some point later, when changes are rebased (or merged)
on master, we don't need to produce a new build. 

Finally, mbt checks out the revision and invokes the build command 
for the platform. Various attributes in the manifest including the version 
are populated in the environment for build scripts to access.

After the build, we have to worry about getting those bits deployed. 
Typically, deployments are also modelled in text documents 
(scripts, cloud formation templates, etc).
Manifest generated by mbt can be applied over go templates. 
This way, mbt allows you to generate consistent and predictable build 
artifacts and deployments.

## Usage Examples
```sh
# Display manifest in default branch 
mbt describe branch --in [path to repo]

# Display manifest for a specific branch
mbt describe branch [branch name] --in [path to repo]

# Display manifest for a commit
mbt describe commit [full commit sha] --in [path to repo]

# Display manifest for a pull request
mbt describe pr --src [source branch name] --dst [destination branch name] --in [path to repo]

# Build default branch
mbt build branch --in [path to repo]

# Build specific branch 
mbt build branch [branch name] --in .

# Build a pull request
mbt build pr --src [source branch name] --dst [destination branch name] --in [path to repo]

# Apply the manifest from default branch over a go template
# Template is read out from git storage. Therefore must be committed.
mbt apply branch --to [relative path to template in tree] --in . 

# Apply the manifest from a branch over a go template
mbt apply branch [branch name] --to [relative path to template in tree] --in .

# Apply the manifest and write the output to a file
mbt apply branch --to [relative path to template in tree] --out [path to output file] --in .
```
## Install
```sh
curl -L -o /usr/local/bin/mbt [get the url for your target from the links below]
chmod +x /usr/local/bin/mbt
```
## Builds

|OS               |Download|
|-----------------|--------|
|darwin x86_64    |[![Download](https://api.bintray.com/packages/buddyspike/bin/mbt_darwin_x86_64/images/download.svg)](https://bintray.com/buddyspike/bin/mbt_darwin_x86_64/_latestVersion)|
|linux x86_64     |[![Download](https://api.bintray.com/packages/buddyspike/bin/mbt_linux_x86_64/images/download.svg)](https://bintray.com/buddyspike/bin/mbt_linux_x86_64/_latestVersion)|
|windows          |[ ![Download](https://api.bintray.com/packages/buddyspike/bin/mbt_windows_x86/images/download.svg) ](https://bintray.com/buddyspike/bin/mbt_windows_x86/_latestVersion)|

## Credits
### mbt is powered by these cool libraries
- [git2go](https://github.com/libgit2/git2go)
- [libgit2](https://github.com/libgit2/libgit2) 
- [yaml] (https://github.com/go-yaml/yaml)
- [cobra](https://github.com/spf13/cobra)
- [logrus](https://github.com/sirupsen/logrus)

Icons made by [Freepik](http://www.freepik.com) from [www.flaticon.com](https://www.flaticon.com/) is licensed by [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/)