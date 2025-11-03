# MSVC (Microsoft Visual Studio) sln & vcxproj synchronization and update tooling

The shell scripts (`bash`, mostly, as I generally work git-for-windows based) used in my MuPDF monolith to generate, update and otherwise maintain all MSVC sln+vcxproj project files: keeping them up to date and all with the exact same compiler settings in order to prevent a class of bugs that has bitten me way too often when working on commercial C/C++ projects: slightly different compiler/interface compiler config can cause hard-to-diagnose run-time errors.

## Notes / used with...

Requires:

- MSVC2022 (it worked with MSVC2019, but things change and I don't check older MSVC rigs unless I really have to)
- `bash` ([git for windows](https://git-scm.com/install/windows), rather)
- Node (please *do* use [NVM for Windows](https://github.com/coreybutler/nvm-windows) for this one: it makes some other stuff easier to deal with when you're doing frontend work alongside)
- [`developer-utility-commands`: some utility scripts of mine for working with git, etc.](https://github.com/GerHobbelt/developer-utility-commands)

These scripts are personalized, expect to run on a `SUBST`ed[^subst] `Z:\` drive, which points to local NTFS storage. The `developer-utility-commands` are expected to reside in `Z:\tools\` a.k.a. `/z/tools/` in `bash`-speak on Windows.


[^subst]: I use a SUBST-ed drive so I can set up the same dev environment on various machines; none of those have a `Z:` drive before I touch them, thus far, so that makes life easier. ... What would have made life *neater* would have been an NTFS Junction `Z` instead of this DOS-like old skool `SUBST` approach but two things have been learned from experience: (a) git, MSVC and the rest of the scripts sometimes barf a fatal hairball when going through a NTFS Junction-ed path; don't ask me why, as those were very machine and platform version/revision specific issues, which `SUBST` never had, oddly enough; a neat `net use` shared drive also didn't fly as one might expect, so `SUBST` it is, forever... (b) as you will have learned elsewhere, **never run your development environment, nor your source tree, from a "networked drive"**, wether it is Linux/Samba based or from a 'neat' Windows Server and properly administrated Windows Active Directory Network: you'll die... or your IDE, scripts and compile/link/build runs will, anyway. Local disk, some sort of SSD/NVMe preferably, and SUBST-ed. And, yes, the drive wear will be noticable after a while, if you run very large projects with a zillion files. *So be it.*

Incidentally, this is the `bash` *alias* list as I keep in my `~/.profile`:

```
echo "map virtual drive to development base dir (may produce an error...): "
#subst 'Z:'  'I:\Projects\.........ov\80'
subst 'Z:'  'I:\Z'
# net use 'Z:'  '\\localhost\I$\Projects\.........ov\80'
# ^^^^^^^^^ the 'net use' based drive mapping causes fsync fatal errors in git-for-windows!
#
# net use

alias gpp='/z/tools/git_pull_push.sh'
alias ll='ls -l'
alias ls='ls -F --color=auto --show-control-chars'
alias node='winpty node.exe'
alias npp='/c/Program\ Files/Notepad++/notepad++.exe'
alias winget='winpty winget.exe'
```

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

