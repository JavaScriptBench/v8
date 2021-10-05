V8 JavaScript Engine
=============



==============================================================

Just 

```
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git 
// suggest get the working version from here https://github.com/JavaScriptBench/v8/releases/download/9.3.344/depot_tools-master.tar.gz
cd depot_tools/
export PATH=~/depot_tools:$PATH
#vim ~/.bashrc

mkdir chrome
cd chrome
fetch chromium
gclient sync
cd src
./build/install-build-deps.sh --no-chromeos-fonts
gclient sync


cd ../..
mkdir v8
cd v8
fetch v8
cd v8
git checkout master
git pull && gclient sync
tools/dev/gm.py x64.release
tools/dev/gm.py x64.release.check
~/v8/v8/out/x64.release/d8 test.js

```

The other way:

```
# In this repo
./install-build-deps.sh --no-chromeos-fonts
tools/dev/gm.py x64.release
tools/dev/gm.py x64.release.check
~/v8/v8/out/x64.release/d8 test.js
```


Ref:

https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html#_setting_up

https://v8.dev/docs/build


==============================================================

V8 is Google's open source JavaScript engine.

V8 implements ECMAScript as specified in ECMA-262.

V8 is written in C++ and is used in Google Chrome, the open source
browser from Google.

V8 can run standalone, or can be embedded into any C++ application.

V8 Project page: https://v8.dev/docs


Getting the Code
=============

Checkout [depot tools](http://www.chromium.org/developers/how-tos/install-depot-tools), and run

        fetch v8

This will checkout V8 into the directory `v8` and fetch all of its dependencies.
To stay up to date, run

        git pull origin
        gclient sync

For fetching all branches, add the following into your remote
configuration in `.git/config`:

        fetch = +refs/branch-heads/*:refs/remotes/branch-heads/*
        fetch = +refs/tags/*:refs/tags/*
        
        ```
Building V8
Make sure that you are in the V8 source directory on the master branch.

cd /path/to/v8
Pull in the latest changes and install any new build dependencies:

git pull && gclient sync
Compile the source:

tools/dev/gm.py x64.release
Or, to compile the source and immediately run the tests:

tools/dev/gm.py x64.release.check
```

```
Building V8 using gm (the convenience workflow)
gm is a convenience all-in-one script that generates build files, triggers the build and optionally also runs the tests. It can be found at tools/dev/gm.py in your V8 checkout. We recommend adding an alias to your shell configuration:

alias gm=/path/to/v8/tools/dev/gm.py
You can then use gm to build V8 for known configurations, such as x64.release:

gm x64.release
To run the tests right after the build, run:

gm x64.release.check
gm outputs all the commands it’s executing, making it easy to track and re-execute them if necessary.

gm enables building the required binaries and running specific tests with a single command:

gm x64.debug mjsunit/foo cctest/test-bar/*
Building V8: the raw, manual workflow
Step 1: generate build files
There are several ways of generating the build files:

The raw, manual workflow involves using gn directly.
A helper script named v8gen streamlines the process for common configurations.
Generating build files using gn
Generate build files for the directory out/foo using gn:

gn args out/foo
This opens an editor window for specifying the gn arguments. Alternatively, you can pass the arguments on the command line:

gn gen out/foo --args='is_debug=false target_cpu="x64" v8_target_cpu="arm64" use_goma=true'
This generates build files for compiling V8 with the arm64 simulator in release mode using goma for compilation.

For an overview of all available gn arguments, run:

gn args out/foo --list
Generate build files using v8gen
The V8 repository includes a v8gen convenience script to more easily generate build files for common configurations. We recommend adding an alias to your shell configuration:

alias v8gen=/path/to/v8/tools/dev/v8gen.py
Call v8gen --help for more information.

List available configurations (or bots from a master):

v8gen list
v8gen list -m client.v8
Build like a particular bot from the client.v8 waterfall in folder foo:

v8gen -b 'V8 Linux64 - debug builder' -m client.v8 foo
Step 2: compile V8
To build all of V8 (assuming gn generated to the x64.release folder), run:

ninja -C out/x64.release
To build specific targets like d8, append them to the command:

ninja -C out/x64.release d8
Step 3: run tests
You can pass the output directory to the test driver. Other relevant flags are inferred from the build:

tools/run-tests.py --outdir out/foo
You can also test your most recently compiled build (in out.gn):

tools/run-tests.py --gn
```

Contributing
=============

Please follow the instructions mentioned at
[v8.dev/docs/contribute](https://v8.dev/docs/contribute).
