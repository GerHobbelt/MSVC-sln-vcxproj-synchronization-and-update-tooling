# MSVC (Microsoft Visual Studio) sln & vcxproj synchronization and update tooling

The shell scripts (`bash`, mostly, as I generally work git-for-windows based) used in my MuPDF monolith to generate, update and otherwise maintain all MSVC sln+vcxproj project files: keeping them up to date and all with the exact same compiler settings in order to prevent a class of bugs that has bitten me way too often when working on commercial C/C++ projects: slightly different compiler/interface compiler config can cause hard-to-diagnose run-time errors.

