cmake_minimum_required(VERSION 3.10)
project(STM32F4_FOC_Control C)

# 设置C标准
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# 编译选项
add_compile_options(-Wall -Wextra -O2)

# 包含目录
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# 主库
add_library(stm32f4_foc_control STATIC
    STM32F4_FOC_Control.c
)

# 测试可执行文件
add_executable(stm32f4_foc_test
    STM32F4_FOC_Test.c
    STM32F4_FOC_Control.c
)

# 链接数学库
target_link_libraries(stm32f4_foc_test m)

# 示例可执行文件（需要修改main函数以避免冲突）
add_executable(stm32f4_foc_example
    STM32F4_FOC_Example.c
    STM32F4_FOC_Control.c
)

target_link_libraries(stm32f4_foc_example m)

# 测试目标
add_custom_target(test
    COMMAND stm32f4_foc_test
    DEPENDS stm32f4_foc_test
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Running FOC control module tests..."
)

# 安装规则
install(TARGETS stm32f4_foc_control
    ARCHIVE DESTINATION lib
)

install(FILES STM32F4_FOC_Control.h
    DESTINATION include
)

# 打印配置信息
message(STATUS "===========================================")
message(STATUS "STM32F4 FOC Control Module Configuration")
message(STATUS "===========================================")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "C Compiler: ${CMAKE_C_COMPILER}")
message(STATUS "C Standard: ${CMAKE_C_STANDARD}")
message(STATUS "===========================================")
