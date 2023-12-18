# 如何在不用catkin_make的情况下，使用ros

catkin_make 不重要，重要的是要引入catkin的编译依赖，和运行时依赖，如此这般就可以正常编译，如下所示，这是一个正常的一个完成编译的系统

![image-20231218155900733](../10-文章图片/image-20231218155900733.png)

其中cmake 文件如下写法

```cmake
cmake_minimum_required(VERSION 2.8.3)
project(monitor_msgs)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "-w")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -g -ggdb ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS_DEBUG "-g ${CMAKE_CXX_FLAGS}")

add_definitions("-DCATKIN_ENABLE_TESTING=0") # 禁用catkin测试 不禁用也可以编译

find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        message_generation
        std_msgs
        geometry_msgs
        )

add_message_files(
        FILES
        fault_info.msg
        fault_vec.msg
)

generate_messages(
        DEPENDENCIES
        geometry_msgs
        std_msgs
)

catkin_package(
        CATKIN_DEPENDS message_runtime
)
```

写法与ros体系中别无二致，但是CATKIN_DEPENDS需要一个没有在cmake中提到的文件`package.xml`

否则会报错