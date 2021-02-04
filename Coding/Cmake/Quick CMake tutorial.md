# Quick CMake tutorial﻿

Last modified: 25 January 2021

This tutorial will guide you through the process of creating and developing a simple CMake project. Step by step, we will learn the basics of [CMake](https://cmake.org/) as a build system, along with the CLion settings and actions for CMake projects.

> ### 
>
> 
>
> The source code of the sample project used below is available on [GitHub](https://github.com/MarinaKalashina/CMake-Tutorial-sample).

## 1. Basic CMake project﻿

[CMake](https://cmake.org/) is a meta build system that uses scripts called **CMakeLists** to generate build files for a specific environment (for example, makefiles on Unix machines). When you create a new CMake project in CLion, a **CMakeLists.txt** file is automatically generated under the project root.

Let’s start with creating a new CMake project. For this, go to **File | New Project** and choose **C++ Executable**. In our example, the project name is *cmake_testapp* and the selected language standard in *C++14*.

By default, we get the project with a single source file **main.cpp** and the automatically created root [CMakeLists.txt](https://www.jetbrains.com/help/clion/cmakelists-txt-file.html) containing the following commands:

![new cmake project](https://resources.jetbrains.com/help/img/idea/2020.3/cl_simple_cmakeprj.png)



| Command                                  | Description                                                  |
| ---------------------------------------- | ------------------------------------------------------------ |
| `cmake_minimum_required(VERSION 3.13)`   | [Specifies](https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html) the minimum required version of CMake. It is set to the version of CMake bundled in CLion (always one of the newest versions available). |
| `project(cmake_testapp)`                 | Defines the project name according to what we provided during project creation. |
| `set(CMAKE_CXX_STANDARD 14)`             | [Sets](https://cmake.org/cmake/help/latest/command/set.html) the *CMAKE_CXX_STANDARD* variable to the value of *14*, as we selected when creating the project. |
| `add_executable(cmake_testapp main.cpp)` | [Adds](https://cmake.org/cmake/help/latest/command/add_executable.html) the *cmake_testapp* executable target which will be built from **main.cpp**. |

## 2. Build targets and Run/Debug configurations﻿

[Target](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#binary-targets) is an executable or a library to be built using a CMake script. You can define multiple build targets in a single script.

For now, our test project has only one build target, *cmake_testapp*. Upon the first project loading, CLion automatically adds a Run/Debug configuration associated with this target:

![default configuration for a new cmake project](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_defaultconf1.png)



Click **Edit Configurations** in the switcher or select **Run | Edit Configurations** from the main menu to view the details. The target name and the executable name were taken directly from the **CMakeLists.txt**:

![details of the default configuration for a new cmake project](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_defaultconf2.png)



Notice the **Before launch** area of this dialog: **Build** is set as a before launch step by default. So we can use this configuration not only to debug or run our target but also to perform the build. To learn more about various build actions available in CLion, see [Build actions](https://www.jetbrains.com/help/clion/build-actions.html).

## 3. Adding targets and reloading the project﻿

Now let’s add another source file **calc.cpp** and create a new executable target from it.

Right-click the root folder in the Project tree and select **New | C/C++ Source File**. CLion prompts to add the file to an existing target:

![add a new source file](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_newfile_addtotargets.png)



Since our goal is to create a new target, we clear the **Add to targets** checkbox. Accordingly, CLion notifies us that the new file currently does not belong to any target:

![file not belonging to the project](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_notbelong_warning.png)



Now let's declare a new target manually in the **CMakeLists.txt**. Note that CLion treats **CMake scripts as regular code** files, so we can use code assistance features like syntax highlighting, auto-completion, and navigation:

![completion in cmake script](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmakelists_completion.png)

When we make changes in **CMakeLists.txt**, CLion needs to reload it in order to update the project structure:

![reload cmake project after adding a new target](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_addfile_reload.png)



We can either reload the project once (**Reload changes**) or enable automatic reload to let CLion silently apply all the changes in **CMakeLists.txt**. The option for enabling/disabling auto-reload is also available in **Settings / Preferences | Build, Execution, Deployment | CMake**.

After reloading the project, CLion adds a Run/Debug configuration for the new target:

![configuration for the newly added target](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_addtarget_conf.png)



### Library targets﻿

Up to this point, the targets we added were executables, and we used `add_executable` to declare them. For library targets, we need another command - [add_library](https://cmake.org/cmake/help/latest/command/add_library.html). As an example, let's create a static library from the **calc.cpp** source file:

```cmake
add_library(test_library STATIC calc.cpp)
```

Copied!



As well as for executables, CLion adds a Run/Debug configuration for the library target after reloading the project:

![configuration for the newly added library target](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_addlibrary_conf.png)



However, this is a non-executable configuration, so if we attempt to run or debug it, we will get the ![Artwork studio icons common error](https://resources.jetbrains.com/help/img/idea/2020.3/artwork.studio.icons.common.error_dark.svg)*Executable not specified* error message.

To obtain the library file, we need to build the **test_library** target. For this, we can switch to the corresponding configuration and press ![Icons actions compile](https://resources.jetbrains.com/help/img/idea/2020.3/icons.actions.compile_dark.svg), or call **Build | Build "test_library"**. The **libtest_library.a** file will appear in the **cmake-build-debug** folder.

## 4. Build types and CMake profiles﻿

All the Run/Debug configurations created by far were *Debug* configurations, which is the default build type of the [CMake profile](https://www.jetbrains.com/help/clion/cmake-profile.html) that was automatically configured for our project. **CMake profile** is a set of options for the project build. It specifies the [toolchain](https://www.jetbrains.com/help/clion/how-to-create-toolchain-in-clion.html), build type, CMake flags, path for storing build artifacts, *make* build options, and environment variables.

For example, to separate the *Debug* and *Release* builds, we can add ![Icons general inline add hover](https://resources.jetbrains.com/help/img/idea/2020.3/icons.general.inlineAddHover.svg) a new CMake profile in **Settings / Preferences | Build, Execution, Deployment | CMake** and set its build type to **Release**:

![Adding a CMake profile](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_addprofile.png)



Notice the **Build directory** field that specifies the location of build results. The default folders are **cmake-build-debug** for *Debug* profiles and **cmake-build-release** for *Release* profiles. You can always set ![Icons actions menu open](https://resources.jetbrains.com/help/img/idea/2020.3/icons.actions.menu-open_dark.svg) other locations of your choice.

Now the Run/Debug configuration switcher shows two available profiles:

![cmake profiles in the configuration switcher](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_profiles_configs.png)



Switching configurations or CMake profiles may affect preprocessor definitions used while resolving the code. For example, when there are separate flags for Debug and Release builds, or when there are some variables that take different values depending on the build type. This is called [resolve context](https://www.jetbrains.com/help/clion/switching-resolve-context.html).

Resolve context defines how CLion performs syntax highlighting and other code insights like Find Usages, refactorings, and code completion. When you switch between configurations, the resolve context for the current file is changed automatically. Also, you can select it manually in the context switcher (**<Select automatically>** restores the automatic selection):

![Resolve context switcher](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_resolvecontext.png)



## 5. Adding include directories﻿

In order to use additional headers located in separate directories, we need to add them either to all the targets or to some specific ones.

For example, let’s create three directories under the project root, **includes**, **includes/general**, **includes/math**, and write the following commands in **CMakeLists.txt**:

```cmake
include_directories(includes/math)
include_directories(includes/general)
```

Copied!



These two commands make the headers located in **general** and **math** available for including from the sources of all targets. For example, we can write `#include "header_math.h"` in **calc.cpp**.

> ### 
>
> 
>
> Headers and sources that you add to the project will be resolved correctly only if you include them explicitly in **CMakeLists.txt** or if you include them in other files already belonging to the project (see [Managing CMake project files](https://www.jetbrains.com/help/clion/managing-cmake-project-files.html)).

## 6. Linking libraries﻿

### Static libraries﻿

On [step 3](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html#lib-targets), we created a static library called *test_library* (the default filename is **libtest_library.a**).

Let's create a **lib** directory under the project root and copy **libtest_library.a** from its default location (**cmake-build-debug**) to this folder.

We will use two commands to link our static library to the *cmake_testapp* target: [find_library](https://cmake.org/cmake/help/latest/command/find_library.html) provides the full path, which we then pass directly into the [target_link_libraries](https://cmake.org/cmake/help/latest/command/target_link_libraries.html) command via the `${TEST_LIBRARY}` variable:

![linking a static library](Quick%20CMake%20tutorial.assets/cl_cmake_link_staticlib.png)



**Note**: make sure to place `target_link_libraries` after the `add_executable` command, so that CMake actually builds the target before linking the library.

### Dynamic libraries (Boost example)﻿

To illustrate linking dynamic libraries, we will take an example of using [Boost.Test framework](https://www.boost.org/doc/libs/1_45_0/libs/test/doc/html/utf.html).

Let's write a simple function `int add_values (int a, int b) { return a+b;}` in **calc.cpp** and create the associated header **calc.h** with the function declaration. We will test this function with the help of Boost.Test framework.

> ### 
>
> 
>
> For details on working with Boost.Test, see [Unit testing tutorial](https://www.jetbrains.com/help/clion/unit-testing-tutorial.html).

As our project gets more complicated, the root **CMakeLists.txt** file can become difficult to maintain. To avoid this, and to build a transparent project structure, we will extract the tests into a subproject.

Let's add a directory called **test** and create a source file **tests.cpp** within it. Also, we need to provide this directory with its own **CMakeLists.txt** file (right-click **test** in the Project tree and select **New | CMakeLists.txt**):

![subproject for tests](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_subprojects.png)



The subdirectory **test/CMakeLists.txt** script is initially empty. We can start filling it up by inserting a [live template](https://www.jetbrains.com/help/clion/using-live-templates.html) for *Boost with libs*. Press Ctrl+J or click **Code | Insert Live Template**, and choose `boost_with_libs`:

![boost live template](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_boosttemplate.png)



Adjust the inserted code to the following:

```cmake
set(Boost_USE_STATIC_LIBS OFF) #enable dynamic linking

# search for unit_test_framework
find_package(Boost REQUIRED COMPONENTS unit_test_framework)

include_directories(${Boost_INCLUDE_DIR})

# create a cmake_testapp_boost target from test.cpp
add_executable(cmake_testapp_boost tests.cpp)

# link Boost libraries to the new target
target_link_libraries(cmake_testapp_boost ${Boost_LIBRARIES})

# link Boost with code library
target_link_libraries(cmake_testapp_boost test_library)
```

Copied!



> ### 
>
> 
>
> Also, we need to place the `add_subdirectory(test)` command in the **root** **CMakeLists.txt** to make our test target *cmake_testapp_boost* available for the main build.
>
> This command, when placed in the root CMake script, declares a subproject *test* that has its own **CMakeLists.txt**.

After reloading the changes in both **CMakeLists.txt** files, CLion creates a Run/Debug configuration for the *cmake_testapp_boost* target. This is a regular CMake Application configuration which we could run/debug right away. However, to be able to use the built-in test runner, let's create another configuration out of the [Boost.Test](https://www.jetbrains.com/help/clion/boost-test-support.html) template:

![boost test run/debug configuration](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_boosttest_config.png)



Now we can run this configuration and get test results. Test runner shows the tree of tests in the suite, their output, status, and duration:

![boost test runner](https://resources.jetbrains.com/help/img/idea/2020.3/cl_cmake_boost_testrunner.png)