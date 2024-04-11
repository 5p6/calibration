### 1.Introdction
This is a C++library for camera calibration. The types of camera sensors that can be calibrated include `pinhole` cameras and `fisheye` cameras, as well as `monocular` and `stereo` camera calibration. Based on this library, `RGBD` camera calibration and registration can be reopened
This library can be used for both `Linux` and `Windows` operating systems, but the `Windows` operating system must be configured with an opencv environment,If the directory of your opencv library is as follows
```shell
- opencv
    - include
    - x64
        - vc17
            - bin
            - lib
```

you need change the context about `CMakeLists.txt` like this :
```CMake
### 链接库
# 参杂着debug和release的链接库，利用config可以设置链接库的类型
file(GLOB OpenCV_LIBS "../opencv/x64/vc17/lib/*.lib" )
set(OpenCV_INCLUDE_DIR "../opencv/include")
message(STATUS "Using Microsoft Visual C++ compiler -- MSVC")
```

### 2.Dependcies
* OpenCV 4.x

### 3.Build
If the environment are linux,you need to change some varable in the `CMakeLists` ,which include the fmt and opencv path,the 
```shell
cd your_calib_path
```

and
```shell
cmake -S . -B ./build
cmake --build ./build --config release
```

So you could make use of the executable file to calib

### 4.Example
The number of camera could be recongized by the code,so the only thing that you need to do is to check the calibration blank type and the camera typy,which involved:
*  calibration blank type
   *  CircleGrid 
   *  CornerGrid 
*  camera typy
   *  Pinhole
   *  Fisheye

then,you need to confirm the blank size(mXn) and square size(mm),just like this
#### 4.1 Monocular Camera
Assuming your data directory is as follows
```shell
- data
    -calib
        - img1.png
        - img2.png
        ...
```
then you need to confirm the parameters like
* board_width : assume `m`
* board_height : assume `n`
* squre_size : assume `l(mm)`
* Camera_SensorType : you can choose `[Pinhole,Fisheye]`
* Chessboard_Type : you can choose `[Corner,Circle]`
* Param_path : the file you save the parameter

Run using the command line
```shell
example_calib.exe Mono ./data/calib ${board_width} ${board_height} ${squre_size} ${Camera_SensorType} ${Chessboard_Type} ${Param_path}
# for example
example_calib.exe Mono ./data/calib m n l Pinhole Corner calibconfig.yaml
```

the just run it!

#### 4.2 Stereo Camera
Assuming your data directory is as follows
```shell
- data
    - calib
        - left
            - img1.png
            - img2.png
            ...
        - right
            - img1.png
            - img2.png
            ...
```
then you need to confirm the parameters like
* board_width : assume `m`
* board_height : assume `n`
* squre_size : assume `l(mm)`
* Camera_SensorType : you can choose `[Pinhole,Fisheye]`
* Chessboard_Type : you can choose `[Corner,Circle]`
* Param_path : the file you save the parameter

Run using the command line
```shell
example_calib.exe Stereo ./data/calib ${board_width} ${board_height} ${squre_size} ${Camera_SensorType} ${Chessboard_Type} ${Param_path}
# for example
example_calib.exe Stereo ./data/calib m n l Pinhole Corner test.yaml
```

the just run it!

#### 4.3 Rectify Image
Using program `example_rectify.exe` ,you can rectify the Monocular/Stereo Image,like this
```shell
## Monocular
example_rectify.exe calib.yaml ./data/img1.png 

## Stereo
example_rectify.exe calib.yaml ./data/left/left1.png ./data/right/right1.png 
```