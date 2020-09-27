---
title: "Releasing Go Modules"
date: 2020-09-27T10:01:08-06:00

tags:
    - go
    - releasing

categories:

draft: true
---

There are 2 main methods for releasing Go modules: Copying all sources to a version directory in the root of the repository _or_ creating a version branch. I don't like copying my full source to a subdirectory so I'm focusing this article on creating a version branch which is a common approach for releasing software.

## Prelude: Semantic Import Versioning

Go uses [semantic versioning](https://semver.org) (semver) for dependency management. Additionally, Go has a simple rule when importing packages.

> If an old package and a new package have the same import path, the new package must be backwards compatible with the old package.

Written from the point of view of a package maintainer.

> When I release a backwards incompatible change, I must ensure the import path is different from the previous major release.

The change can be achieved with a v2 suffix on the import path defined in `go.mod` file.

```
module github.com/chrisdoherty4/greeter/v2
```

This results in import paths for the v2 major release requiring the v2 suffix.

```go
// Version 0 or 1 import
import "github.com/chrisdoherty4/example"

// Version 2 import.
import "github.com/chrisdoherty4/example/v2
```

[](#v1-no-prefix) Note the suffix isn't required for v0 or v1 because many packages are not released beyond v1. For a complete answer see [Russ Cox's FAQ post](https://github.com/golang/go/issues/24301#issuecomment-371228664).

## Releasing a Go module with branches

When starting development we create a `go.mod` file with the v0/v1 module name.

```
module github.com/chrisdoherty4/greeter
```

The Go tooling looks for the latest semantic version tag in the repository and checks that the requested module exists.

If the repository is yet to be tagged, the Go tooling will pull the tip of the default branch.

### v0

Until we're ready for our first release, v0, Go will pull the tip of the default branch.

> Major version zero (0.y.z) is for initial development. Anything MAY change at any time. The public API SHOULD NOT be considered stable.

_- [semver item 4](https://semver.org/#spec-item-4)_

Given v0 is considered unstable, and that the Go tooling pulls the tip of the default branch, we can develop our initial code on the default branch tagging v0 minor and patch numbers as we go. 

### v1

When we're ready to release v1, we create a v1 major branch, prepare the branch and create a semantic version tag.

```shell
$ git checkout -b v1
$ git tag v1.0.0
$ git push --tags origin v1
```

_We haven't altetered the module name with v1 as noted in [the prelude](#v1-no-prefix)._

Now we have a branch that we can apply backwards compatible changes to, we have our default branch representing the latest copy of our code and we haven't duplicated the source.

### v2 and beyond

The v2 release process is essentially the same as v1 except we must change the module name in our `go.mod` to include a `v2` suffix.

```
module github.com/chrisdoherty4/greeter/v2
```

We can apply this change on the `v2` branch and then tag it as `v2.0.0`.