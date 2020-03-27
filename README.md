# Reproduce the issue

This repository is related to a [StackOverflow post](https://stackoverflow.com/questions/60874456) and deals with the problem that I can't rename my Bazel workspace file to `WORKSPACE.bazel`

## 1. Install dependencies with `yarn install`

## 2. Run `yarn build`

### Everything works fine

```
$ bazelisk build //...
INFO: Analyzed 3 targets (103 packages loaded, 7392 targets configured).
INFO: Found 3 targets...
INFO: Elapsed time: 11.415s, Critical Path: 1.52s
INFO: 19 processes: 18 linux-sandbox, 1 worker.
INFO: Build completed successfully, 48 total actions
Done in 11.72s.
```

## 3. Rename `WORKSPACE` to `WORKSPACE.bazel`

## 4. Run `yarn build` again

### It breaks! 😮

```
$ bazelisk build //...
INFO: Call stack for the definition of repository 'com_github_pkg_errors' which is a go_repository (rule definition at /home/flolu/.cach
e/bazel/_bazel_flolu/9d1b897d04e9a8aa4b1c82ee6b746d0c/external/bazel_gazelle/internal/go_repository.bzl:189:17):
 - <builtin>
 - /home/flolu/.cache/bazel/_bazel_flolu/9d1b897d04e9a8aa4b1c82ee6b746d0c/external/io_bazel_rules_docker/repositories/go_repositories.bzl:44:9
 - /home/flolu/.cache/bazel/_bazel_flolu/9d1b897d04e9a8aa4b1c82ee6b746d0c/external/io_bazel_rules_docker/nodejs/image.bzl:44:5
 - /home/flolu/Desktop/workspace-rename-issue/WORKSPACE.bazel:45:1
ERROR: An error occurred during the fetch of repository 'com_github_pkg_errors':
   Not a regular file: /home/flolu/Desktop/workspace-rename-issue/WORKSPACE
ERROR: /home/flolu/.cache/bazel/_bazel_flolu/9d1b897d04e9a8aa4b1c82ee6b746d0c/external/io_bazel_rules_docker/container/go/cmd/join_layer
s/BUILD:3:1: @io_bazel_rules_docker//container/go/cmd/join_layers:go_default_library depends on @com_github_pkg_errors//:go_default_library in repository @com_github_pkg_errors which failed to fetch. no such package '@com_github_pkg_errors//': Not a regular file: /home/flolu/Desktop/workspace-rename-issue/WORKSPACE
ERROR: Analysis of target '//service:image' failed; build aborted: no such package '@com_github_pkg_errors//': Not a regular file: /home
/flolu/Desktop/workspace-rename-issue/WORKSPACE
INFO: Elapsed time: 0.815s
INFO: 0 processes.
FAILED: Build did NOT complete successfully (25 packages loaded, 665 targets configured)
    Fetching @com_github_google_go_containerregistry; fetching
error Command failed with exit code 1.
```
