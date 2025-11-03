# MSVC (Microsoft Visual Studio) sln & vcxproj synchronization and update tooling

The shell scripts (`bash`, mostly, as I generally work git-for-windows based) used in my MuPDF monolith to generate, update and otherwise maintain all MSVC sln+vcxproj project files: keeping them up to date and all with the exact same compiler settings in order to prevent a class of bugs that has bitten me way too often when working on commercial C/C++ projects: slightly different compiler/interface compiler config can cause hard-to-diagnose run-time errors.

## Which of these does what exactly...?

<sup>And already the documentation is lagging behind the codebase... 🤡</sup>

### `mknew.sh`

Most frequently used is `mknew.sh` while does not only create a fresh VCXPROJ for any directory (library) you add to the `/thirdparty/owemdjee/...` directory tree, but it is also used to *update* (regenerate/synchronize) already existing VCXPROJ files:

    ./mknew.sh 1234 param

will seek out all projects containing `param` in their name, run their VCXPROJ files through the project settings synchronization scripts (RTFC: `*vcxproj*.sh`, which inh turn drive `*vcxproj*.js` node/JS scripts, where necessary) and update/regenerate the source file set for said VCXPROJ project by *globbing* the related source directory, while following the guidance in the `<project>.spec` file (where the project file is named `<project>.vcxproj`) in a way somewhat similar to a gitignore file, but *different syntax*.


### `sync-sln-files.sh`

Not used often but important in its own way: some very large projects of mine have multiple SLN files: one major/master, plus a bunch of derivatives for various purposes: partial builds, test builds, experimentation, less noise/clutter while I work on a specific part, anything. 

This script takes the set of SLN files found and 'synchronizes' them, ensuring a bunch of basic layout and content is shared among them, plus project dependencies are synchronized (i.e. projects needed by other projects but *not yet listed in this particular SLN* are added to that particular SLN, etc.)


### `*.sh`

Sorry, RTFC (particulary the head comments of various scripts) while this README doc remains incomplete and running-after-the-facts-as-they-are-today, anyway.

