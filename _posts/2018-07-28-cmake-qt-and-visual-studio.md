---
layout: post
title: CMake, Qt, and Visual Studio
date: 2018-07-28 16:41 -0400
tags: programming cmake c++ qt build
---

I have been wanting to do cross platform desktop applications for a while. I know there exists plenty of programming languages and out of box solutions for being able to do cross platform applications easily. However, I wanted to see how it was done through C++ along with build systems. I already have done cross platform before utilizing Qt, but that was done through Qt and its Qt Creator application, it was very automated and easy. Also, it was mostly someone else in my group getting it all setup and creating the UI.


There are two ways that I know of that you can make C++ code work on multiple platforms like Linux and Windows. The first way was through cross-compilation in which the compiler compiles for other target systems, e.g. a Linux host machine would compile for Windows. I have read that this is very difficult and not worth it. The second way is just compiling it on each system, so the Windows version would need to be compiled in Windows and a Linux version would need to be compiled in Linux through their respective toolchains. A way to keep your project working across all platforms effortlessly is through CMake which is the build system. So let’s say you want to work on windows, you first configure CMake and then it would generate the files needed for the specified toolchain where you can compile on that system.


Unfortunately, there wasn't that many guides on utilizing CMake, Qt, and Visual Studio together. I first started with learning the basics of CMake, or at least getting a gist of what it's about by doing [CMake's tutorial](https://cmake.org/cmake-tutorial/). The tutorial wasn't as good as I hoped because I couldn't type word for word what was happening in each step to compile and generate, a lot of info was left out. Luckily they had a zip of all the steps; the tutorial felt more like a reference. I then took a look at [Qt's CMake tutorial](http://doc.qt.io/qt-5/cmake-manual.html) to see if they had a decent HelloWorld tutorial. Again, it was basically another reference sheet and was not really tutoring a new person on everything that was happening. So I had to turn to youtube which brought me to [these series](https://www.youtube.com/watch?v=9KHSa-fZC94&list=PLXTq1obpeHem3h424u4G4wS9FGwPNk5mM&index=1). I am lucky to come across such a tutorial series for doing exactly what I wanted to do. When I tried mimicking the videos, I had done some things differently.  
  
<br>

----

<br>

You require a `CMakeLists.txt` that tells CMake on how to generate everything.  

`cmake_minimum_required(VERSION 3.12.0)`  
You must first tell it what version you want to limit this txt file to run on. Typically you would pick the current version of your CMake, this prevents problems from running newer CMake commands on older CMake versions.  

`project(HelloQt LANGUAGES CXX)`  
You name your project and say what language it is in. Typing `LANGUAGES CXX` disables C and restricts it to C++ files only. Without specifying the `LANGUAGES` both C and C++ are enabled.

`set(CMAKE_INCLUDE_CURRENT_DIR ON)`  
This also enables `CMAKE_CURRENT_SOURCE_DIR` and `CMAKE_CURRENT_BINARY_DIR`, you need this for `ui_mainwindow.h` that gets generated later on. Reason being is that `ui_mainwindow.h` gets generated in the build directory. We're currently in the source directory setting everything up. `ui_mainwindow.h` derives from your `mainwindow.ui` I believe it is a translation into C++. `mainwindow.ui` is created through Qt Creator where you design your UI/windows.  

`set(CMAKE_AUTOMOC ON)`  
This is for Visual Studio so it doesn't give out errors and such before compiling. For instance, I think you can still compile when intellisense gives you errors, but you can still compile and it will stop giving errors. This should stop the fake errors in the first place. This is just speculation.  

`set(CMAKE_AUTOUIC ON)`  
This generates the `ui_mainwindow.h`. Also, this is the simpler version of other commands/macros like `qt5_wrap_ui()`, etc.    

`find_package(Qt5 REQUIRED COMPONENTS Core Widgets Gui)`  
This line gave me quite a lot of trouble as I seen different ways on how it was used. For instance, there was a `CONFIG` option which apparently puts `find_package` into CONFIG mode instead of MODULE, a different prototype of the command. However, I was told by *ngladitz#cmake@freenode* that it shouldn't matter since both use package configuration files. `REQUIRED` just makes it so CMake fails if it can't find all of the given components, and `COMPONENTS` is self-explanatory. The way it is written is probably the simplest way to get the needed things from Qt5.  
```
set(project_ui
	mainwindow.ui)

set(project_headers
	mainwindow.h)

set(project_sources
	main.cpp
	mainwindow.cpp)
```

Afterwards I have a bunch of sets that are basically variables that store the ui, cpp, and h files that are needed for `add_executable`. You could just automatically add everything by directories, but the video series I watched explained it's good practice to detail each file so people notice new things being added in.  

`add_executable(${PROJECT_NAME} ${project_headers} ${project_ui} ${project_sources})`  
This actually says what the .exe should be named and all the stuff that is needed to compile it. Imagine using gcc/g++ and typing all those extra needed files.  
```
target_link_libraries(${PROJECT_NAME}
	PUBLIC
	Qt5::Core
	Qt5::Gui
	Qt5::Widgets)
```
You need to manually link your stuff in, and you need to use the right namespace for Qt5. You can make it privately or publicly linked, I still don't have a solid grasp on this.  

```
macro(qt5_copy_dll APP DLL)
    # find the release *.dll file
    get_target_property(Qt5_${DLL}Location Qt5::${DLL} LOCATION)
    # find the debug *d.dll file
    get_target_property(Qt5_${DLL}LocationDebug Qt5::${DLL} IMPORTED_LOCATION_DEBUG)

    add_custom_command(TARGET ${APP} POST_BUILD
       COMMAND ${CMAKE_COMMAND} -E copy_if_different $<$<CONFIG:Debug>:${Qt5_${DLL}LocationDebug}> $<$<NOT:$<CONFIG:Debug>>:${Qt5_${DLL}Location}> $<TARGET_FILE_DIR:${APP}>)
endmacro()

qt5_copy_dll(${PROJECT_NAME} Core)
qt5_copy_dll(${PROJECT_NAME} Gui)
qt5_copy_dll(${PROJECT_NAME} Widgets)
```
At the end is a macro that I took from somewhere else on the web. The executable needs the DLLs copy pasted beside, especially if you're using debug mode.  
<br>
 
****
<br>

For the UI creation, here are the screenshots of the creation of the UI files. The video tutorial said to use Qt Designer Form Class and then create a Main Window template, make sure the path is set to where your `CMakeLists.txt` is. You can put whatever you want in the MainWindow, I just used a label and put it in the center of the window as it's a simple HelloWorld program.

![]({{ "/images/2018-07-28-cmake-qt-and-visual-studio/1.png"}})  
  
![]({{ "/images/2018-07-28-cmake-qt-and-visual-studio/2.png"}})  
  
![]({{ "/images/2018-07-28-cmake-qt-and-visual-studio/3.png"}})
<br>
<br>
 
****
<br>

Now you should be able to generate everything with CMake. You can use the command line or GUI for CMake, I used the GUI. You set the source to where the `CMakeLists.txt` is, the rest of the ui and code should be there as well. You can set a build folder somewhere else where it won't interfere with the source. You click the Config button where you should get red boxes indicating new information has appeared. A critical step is to tell CMake where Qt5's CMake files are for the compiler that you are generating for. I installed Qt in C:\ and I was using Visual Studio 2017 so my path was `C:\Qt\5.11.1\msvc2017_64\lib\cmake\Qt5`. I had a problem earlier because I used `C:\Qt\5.11.1\winrt_x64_msvc2017\lib\cmake\Qt5`, notice the winrt. Now you can Generate where you pick the respective compiler you are using, I used Visual Studio 2017 x64. With that, you have generated the necessary files for Visual Studio and can finally open up the project. Make sure once it is open that you right click the project in Visual Studio and select 'Set as StartUp Project', this tells Visual Studio what project to build and debug first. I am skipping what I put in my `main.cpp` but you can view the whole end result [here](https://github.com/Davidj361/HelloQt).
