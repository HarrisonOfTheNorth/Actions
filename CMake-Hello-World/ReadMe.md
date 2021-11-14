## Call to Action

The [Action](https://github.com/HarrisonOfTheNorth/Actions/blob/master/.github/workflows/cmake-hello-world.yml) file for this project is building as follows:

![cmake-hello-world workflow](https://github.com/HarrisonOfTheNorth/Actions/actions/workflows/cmake-hello-world.yml/badge.svg)

# CMake Hello World

Buckle down, it's 'Monkey-See, Monkey-Do' time!

- Follow the instructions, line by line, however if you are not on a Mac, you will have to accommodate the slight platform differences.

There's also an advanced section at the bottom to show how to use a Github Content Integration (CI) pipeline, to build and then execute the C++ file!

The current status is reported in the Call-to-Action badge above, which reports the result after every commit.

## Overview

- Keep in mind when you do the following 'Monkey-See, Monkey-Do', that the object of the CMakeLists.txt file is to auto generate a Makefile, and that having done so, and also having navigated into the Makefile's directory, when you run `make`, that it should output an executable file in the build/debug directory that you can then execute.

## Instructions

- Create a project directory and in terminal, navigate into it.
- Create a main.cpp project shell.

`main.cpp`

```
#include <iostream>

int main()
{
	std::cout << "Hello World!" << std::endl;
	return 0;
}
```

- run `cmake --version` and record the version number. We got `3.21.4`.
- If you did not get a result, install CMake and repeat the last line.
- Create a file called CMakeLists.txt and put the following content in it (but using your own version number):

`CMakeLists.txt`

```
cmake_minimum_required(VERSION 3.21.4)
project(MyProjectName)
add_executable(${PROJECT_NAME} main.cpp)
```

- create a build path, we did `mkdir -p build/debug`
- Run `cmake -S . -B build/debug`, realising that `-S` stands for source directory, and `-B` stands for build directory. You can call this command from anywhere, so long as you get the paths right.
- Change into the source directory, in our case it was `cd build/debug`
- Run `make`
- Execute the executable, in our case we ran `./MyProjectName`, which was the name we specified above in our CMakeLists.txt file
- The output should be `Hello World!`
- Look around, find an ape, and get him to pat you on the back. I learnt this exactly the same way!

## Next Step

- In the build/debug directory, run `make clean` and notice that your executable is now gone.
- As the `-S` points to your source directory, and the `-B` points to your build directory, create a `src` directory, move both the `main.ccp` file and `CMakeLists.txt` file into it, and from the project root directory, run `cmake -S src -B build/debug`.
- Navigate back into the build directry, run `make`, and execute the executable again.

## Be a Monkey Prince

- If you check your project into your own GitHub account, you can configure it to build, every time you commit - and it will report a success or failure!
- We go the extra mile here by piping the output ('Hello World!) to a text file, which is then uploaded as an attachment to the (not publicly visible) job build page.

![cmake-hello-world workflow](https://github.com/HarrisonOfTheNorth/Actions/actions/workflows/cmake-hello-world.yml/badge.svg)

This badge above indicates the current build-status of main.cpp, and the [Workflow Action File](https://github.com/HarrisonOfTheNorth/Actions/blob/master/.github/workflows/cmake-hello-world.yml) is as follows, which you too can set up.

```
name: C++ CMake-Hello-World Build

on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Get current date
        id: date
        run: echo "::set-output name=date::$(date +'%Y%m%d%I%M%p')" # Used in a filename, later
      - name: Checkout repository content
        uses: actions/checkout@v2 # Checkout the repository content to github runner.
      - name: tree
        run: tree # render a filename tree of the checkout
      - name: cmake
        run: cmake -S CMake-Hello-World/src -B CMake-Hello-World/build/debug # Generates Makefile
      - name: tree
        run: tree # render a new filename tree to see the differences in what was generated from the previous tree
      - name: make
        run: make -C CMake-Hello-World/build/debug # Runs Makefile
      - name: tree
        run: tree # render a new filename tree to see the differences in what was generated from the previous tree
      - name: run executable
        run: (cd CMake-Hello-World/build/debug && ./MyProjectName) # Runs executable that Makefile generated
      - name: Upload Artefact
        run: (cd CMake-Hello-World/build/debug && ./MyProjectName) > CMake-Hello-World/helloworld.txt # Creates text file with executable output in it
      - name: tree
        run: tree # render a new filename tree to see the differences in what was generated from the previous tree
      - name: Hello Secret
        run: |
          echo $HELLOSEC # merely echoes the stored secret, then stores it in a file, to show how it's done! Otherwise, not neccessary here!
          mkdir -p CMake-Hello-World/tmp
          echo "The arbitrary GitHub secret that was retrieved from our account is: ${{ secrets.HELLO_SECRET_KEY }}" > "CMake-Hello-World/tmp/${{ steps.date.outputs.date }}_hellosecret.txt"
          tree # list
        env:
          HELLOSEC: ${{ secrets.HELLO_SECRET_KEY }}
      - name: curl # the advantage of curl is that you can GET and POST and also add headers. We will just retrieve a simple GET to show how it is done
        run: |
          curl https://www.google.com > "CMake-Hello-World/tmp/${{ steps.date.outputs.date }}_www.google.com.html"
      - name: tree
        run: tree # show a tree that has both the secret file and curl file
      - uses: actions/upload-artifact@v2 # Uploads the path to the Action Build Output screen, with the name identified in name
        with:
          name: my-artifact
          path: CMake-Hello-World/helloworld.txt
      - uses: actions/upload-artifact@v2 # Uploads the path to the Action Build Output screen, with the name identified in name
        with:
          name: my-secret
          path: "CMake-Hello-World/tmp/${{ steps.date.outputs.date }}_hellosecret.txt"
      - name: Deploy
        uses: s0/git-publish-subdir-action@develop # Uploads the given folder (tmp) to the given branch (cmake-hello-world) of this repo
        env:
          REPO: self
          BRANCH: cmake-hello-world # This branch has to already exist
          FOLDER: CMake-Hello-World/tmp # This folder, and it's contents, were created above
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

I put extensive tree commands in so that I can look at the output file, when it does not build, to see where the problem occurred.

Guess what the build output is:

`Hello World!`

_... exactly what main.cpp outputs!_

- But wait, there's a bonus!

If you look at the .yml file, you will see that we also commit a GitHub secret, in a text file, to another branch - [here](https://github.com/HarrisonOfTheNorth/Actions/tree/cmake-hello-world) it is! The secret is inside the file, click on it!

And the icing on the cake is that we also retrieve an arbitrary webpage using curl (which is capable of both GET and POST as well as sending headers) and commit it with the above file, to the same other branch.

## Conclusion

We have demonstrated the capacity to build an application in our own repo using a GitHub Action, to execute it, to upload the output of the application to a privately accessible file, to also access a GitHub secret, to upload that to a file in a separate branch in the same repo so that it is publicly available, and to retrieve an arbitrary webpage from the internet, similarly saving the response of that.

Thus, we have demonstrated the capability to create a program, to execute it, to interact with any webpage on the internet, with both GET and POST capabilty, including the prospect of sending headers - and to save the response in either a publicly or privately accessible artifact.

All-in-all, we have proven that we are capable of using a GitHub Action to intract with any resource on the web, either using the Linux CLI, or using an executable of our own design.

## Next Demo

The trigger of these actions was a `git push` to a repo, and the natural next step would be to discover how to trigger an action periodically, saving the response to a branch that accrued each response instead of only displaying the last run.
