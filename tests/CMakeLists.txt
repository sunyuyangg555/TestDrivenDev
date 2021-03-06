project(TestDrivenDev)

# include project source
include_directories(${PROJECT_SOURCE_DIR}/../include)
include_directories(${PROJECT_SOURCE_DIR}/../include/LedDriver)
include_directories(${PROJECT_SOURCE_DIR}/../include/HomeAutomation)
include_directories(${PROJECT_SOURCE_DIR}/../include/util)

# include mocks only for test code
include_directories(${PROJECT_SOURCE_DIR}/../mocks)

# include CppUTest headers
include_directories($ENV{CPPUTEST_HOME}/include)

# add cpputest library
add_library(imp_cpputest STATIC IMPORTED)
set_property(TARGET imp_cpputest PROPERTY
        IMPORTED_LOCATION $ENV{CPPUTEST_HOME}/src/CppUTest/libCppUTest.a)

# build test library for LedDriver
add_library(LedDriverTest ./LedDriver/LedDriverTest.cpp)

# build test library for LightControllerSpcTest
add_library(LightControllerSpcTest ./HomeAutomation/LightControllerSpyTest.cpp)

# build test library for FakeTimeServiceTest
add_library(FakeTimeServiceTest ./HomeAutomation/FakeTimeServiceTest.cpp)

# build test library for LightSchedulerTest
add_library(LightSchedulerTest ./HomeAutomation/LightSchedulerTest.cpp)

add_executable(RunAllTests RunAllTests.cpp)
target_link_libraries(RunAllTests imp_cpputest
        LedDriverTest LedDriver RuntimeErrorStub
        LightSchedulerTest LightScheduler
        FakeTimeServiceTest FakeTimeService
        LightControllerSpcTest LightControllerSpc
        )