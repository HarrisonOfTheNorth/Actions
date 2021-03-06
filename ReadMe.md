# Actions

## CMake-Hello-World

![cmake-hello-world workflow](https://github.com/HarrisonOfTheNorth/Actions/actions/workflows/cmake-hello-world.yml/badge.svg)

- A C++ Hello World that is automatically built on commit to GitHub using the GitHub Action .yml file below; when the executable is run:
  - it's output ('Hello World!') is uploaded in an attached file to the build run output
  - a secret is retrieved from our GitHub Secrets stash, which is placed in a file, and that file is committed to another branch
  - we GET an arbitrary webpage using curl (which is capable of GET and POST, with headers if need be), which we place in a file and commit that, with the previous file, to another branch
  - If we didn't break the build, the badge above ls labelled as passing.
- [Detailed ReadMe](CMake-Hello-World/ReadMe.md)
- [Action YML](.github/workflows/cmake-hello-world.yml)
- [Secret and retrieved webpage](https://github.com/HarrisonOfTheNorth/Actions/tree/cmake-hello-world) files on another branch.
