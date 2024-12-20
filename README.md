

# ARchitecture

## Demo
This is a small demo video for this application. Click the video below:

[![Click this](https://img.youtube.com/vi/IomnGpExPE4/hqdefault.jpg)](https://www.youtube.com/embed/IomnGpExPE4)

or click [here](https://www.youtube.com/watch?v=IomnGpExPE4).



## Members
- Eber Christer [@eberfrogster](https://github.com/eberfrogster)
- Jessica Sumargo [@jessicasmrg](https://github.com/jessicasmrg)
- Piotr Nobis [@piotrnobis](https://github.com/piotrnobis)


## Description

ARchitecture is an application that aids in room planning and visualization. The user is provided with a set of markers, each representing different common objects that one would find in a room (walls, tables, bed, chairs, TV, and other furnitures). By placing these markers in front of the camera, a 3D visualization of the aforementioned objects are rendered into the scene.


## Running ARchitecture
#### Without IDE
1. Clone this repository
2. Open `ARchitecture/src/main.cpp` and change the `VIDEOPATH` and `MARKERPATH` macro in **line 13-14** to adapt to the user's system<sup>a</sup>.
3. Change current directory to ARchitecture `cd <yourpath>/ARchitecture`
4. Build the program using CMake `cmake .`
5. Compile the program using either the generated or the provided `makefile`  `make`
6. Run generated executable to start the program<sup>b</sup> `./ARchitecture`

Note: If after running the program compiled and built with CMake the user receives this error:  
```
[CV] No Webcam detected, searching for video file
[CV] No video file detected, exiting
``` 
please use the provided `makefile`! Since the provided `makefile` will be replaced by the one generated by CMake, the user can retrieve this back by copying the `makefile` in `ARchitecture/resources/makefile`.

---
#### With XCode
1. Clone this repository
2. Open `ARchitecture/src/main.cpp` and change the `VIDEOPATH` and `MARKERPATH` macro in **line 13-14** to adapt to the user's system<sup>a</sup>.
3. Open the `CMakeLists.txt` given and adjust the following:
	- Adjust the `IncludePath`
	- Adjust the `target_link_libraries`:<br> `/opt/homebrew/cellar/glfw/<GLFW_VERSION>/lib/libglfw.3.3.dylib` for M1 Macs or `/usr/local/Cellar/glfw/3.3/lib/libglfw.3.3.dylib` for Intel Macs
4. Open CMake and set the path to `ARchitecture` (where `CMakeLists.txt` is located) and then for the binaries add a “.../build” to the previous path. 
5. Press “Configure” and choose Xcode as IDE
6. Press “Generate”
7. Press “Open Project”
8. Run the code from XCode <sup>b</sup>
---
#### With Visual Studio 2022
1. Clone this repository
2. Create an empty C++ Project in Visual Studio 2022
3. Move the `ARchitecture/src` and `ARchitecture/resources` to the project directory `/<project-name>/<project-name>`
4. Add the `.cpp | .h`  inside the `ARchitecture/src` directory to the current project in the VisualStudio Solution Explorer
5. Set the dependencies under **Project -> Project Properties**
	- Language Standard: 
		- **Configuration Properties -> General -> C++ Language Standard**: `ISO C++20`
	- OpenCV:
		- **Configuration Properties -> VC++ Directories -> Include Directories -> Edit**: `C:\Libraries\OpenCV_4_7_0\opencv\build\include`
		- **Configuration Properties -> Linker -> General -> Additional Library Directories -> Edit**: `C:\Libraries\OpenCV_4_7_0\opencv\build\x64\vc16\lib`
		- **Configuration Properties -> Input -> Additional Dependencies -> Edit**: `opencv_world470d.lib`
	- GLFW:
		- **Configuration Properties -> VC++ Directories -> Include Directories -> Edit**: `C:\Libraries\glfw-3.3.8.bin.WIN64\include`
		- **Configuration Properties -> Linker -> General -> Additional Library Directories -> Edit**: `C:\Libraries\glfw-3.3.8.bin.WIN64\lib-vc2019`
		- **Configuration Properties -> Input -> Additional Dependencies -> Edit**: `glfw3.lib; glfw3dll.lib; opengl32.lib`
	- GLEW:
		- **Configuration Properties -> VC++ Directories -> Include Directories -> Edit**: `C:\Libraries\glew-2.1.0\include`
		- **Configuration Properties -> Linker -> General -> Additional Library Directories -> Edit**: `C:\Libraries\glew-2.1.0\lib\Release\x64`
		- **Configuration Properties -> Input -> Additional Dependencies -> Edit**: `glew32.lib`
	-	GLM
		- **Configuration Properties -> VC++ Directories -> Include Directories -> Edit**: `C:\Libraries\glm-0.9.9.7\glm`
6. Change the `VIDEOPATH` and `MARKERPATH` macro in **line 13-14** to adapt to the user's system<sup>a</sup>.
7. Run the program <sup>b</sup>

Note: The user might have to add `.string()` function in **line 59**. `markerPaths.push_back(entry.path().*string()*);`

---
Default: Immediately after the program runs, it will automatically start the webcam built into the device. Otherwise, it will read the contents of `resources/MarkerMovie.MP4`, and display the AR functionality on the video instead,

<font size="2"> <sup>a</sup> Can be relative or absolute path. 

<font size="2"> <sup>b</sup> If somehow there is an error concerning the video encoding, the user can remove the `cv::CAP_FFMPEG` in line 32 and 37 in `ARchitecture/src/main.cpp`. If somehow there is an error mentioning that no webcam/video file can be detected, use the provided `makefile` instead of CMake.


## Files
This is an overview of this repository's file structure:
``` txt
ARchitecture
├── src
│   ├── main.cpp
│   ├── MarkerDetection.(cpp|h)
│   ├── ObjectRender.(cpp|h)
├── resources
│   └── markers
│       ├── marker<x>.png
│   └── MarkerMovie.MP4	
│   └── markers_all.png	
├── CMakeLists.txt
├── makefile
```
`MarkerDetection.(cpp|h)` contains a class and helper classes that essentially takes care of the necessary OpenCV implementation, which includes marker detection, marker identification, and pose estimation. 

`ObjectRender.(cpp|h)` contains a class that takes care of visualization and object creation with OpenGL. This includes helper functions to convert OpenCV coordinates into OpenGL coordinates, vector algebra, as well as furniture object creation.

`main.cpp` implements all the modules mentioned above.

`resources` stores all the necessary resources for the program to function. `resources/markers` contains multiple unique arUco markers that will be used to create a marker dictionary for the marker detection and object creation. The video files are also stored here.


## Frameworks
- [OpenGL](https://www.genome.gov/) : Object creation & 3D rendering.
- [OpenCV](https://opencv.org/) : Marker detection & Pose estimation





## CMakeLists.txt
``` CMake
cmake_minimum_required(VERSION 3.4)

project(ARchitecture)
set(IncludePath "/usr/include")
set(ARchitecture_SOURCES src/MarkerDetection.cpp src/MarkerDetection.h src/main.cpp)

# GLEW
message(STATUS "Locating GLEW...")
find_package(GLEW REQUIRED)

# OpenGL
message(STATUS "Locating OpenGL...")
find_package(OpenGL REQUIRED)

# OpenCV
message(STATUS "Locating OpenCV...")
find_package(OpenCV REQUIRED)

if (GLEW_FOUND AND OPENGL_FOUND AND OpenCV_FOUND)
message(STATUS "All required packages found!")

include_directories( ${GLEW_INCLUDE_DIRS}  IncludePath)
include_directories( ${OpenCV_INCLUDE_DIRS} )
include_directories( ${OPENGL_INCLUDE_DIRS} )

add_executable(output ${ARchitecture_SOURCES})

# select one of these based on the user's operating system
# for Linux
target_link_libraries (output ${GLEW_LIBRARIES} "/usr/lib/x86_64-linux-gnu/libglfw.so" ${OPENGL_LIBRARIES} ${OpenCV_LIBS}) 

# for MacOS
target_link_libraries (output GLEW::GLEW "/opt/homebrew/cellar/glfw/3.3.8/lib/libglfw.3.3.dylib" ${OPENGL_LIBRARIES} ${OpenCV_LIBS}) 

endif()
```
