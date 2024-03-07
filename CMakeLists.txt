cmake_minimum_required(VERSION 3.12)
project(call_center)

set(CMAKE_CXX_STANDARD 14)

# Настройки для библиотеки ImGui
set(IMGUI_DIR "${CMAKE_CURRENT_SOURCE_DIR}/imgui")  
include_directories(${IMGUI_DIR})
add_library(imgui 
    ${IMGUI_DIR}/imgui.cpp 
    ${IMGUI_DIR}/imgui_draw.cpp 
    ${IMGUI_DIR}/imgui_widgets.cpp 
    ${IMGUI_DIR}/imgui_tables.cpp 
    ${IMGUI_DIR}/imgui_impl_opengl2.cpp  # Добавлено расширение cpp
    ${IMGUI_DIR}/imgui-SFML.cpp  # Добавлено расширение cpp
)

# Настройки для библиотеки SFML
set(SFML_DIR "${CMAKE_CURRENT_SOURCE_DIR}/SFML/lib/cmake/SFML")  
find_package(SFML 2.5 COMPONENTS system window graphics REQUIRED)
include_directories(${SFML_INCLUDE_DIR} "${CMAKE_CURRENT_SOURCE_DIR}/SFML/include")  

# Настройки для SQLite3
set(SQLite3_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/sqlite3/include")  
set(SQLite3_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/sqlite3/lib/sqlite3.lib")
set(SQLite3_DLL "${CMAKE_CURRENT_SOURCE_DIR}/sqlite3/lib/SQLiteCpp.dll")
include_directories("${SQLiteCpp_DIR}/include/SQLiteCpp")


# Добавление исполняемого файла
add_executable(call_center
    call_center.cpp  
    ${IMGUI_DIR}/imgui.cpp 
    ${IMGUI_DIR}/imgui_draw.cpp 
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/imgui_tables.cpp 
    ${IMGUI_DIR}/imgui_impl_opengl2.cpp  # Добавлено расширение cpp
    ${IMGUI_DIR}/imgui-SFML.cpp  # Добавлено расширение cpp
)

# Линковка с библиотеками
target_link_libraries(call_center PRIVATE imgui sfml-system sfml-window sfml-graphics SQLiteCpp opengl32 ${SQLite3_LIBRARY})

# Подключение SQLite3
target_include_directories(call_center PRIVATE ${SQLite3_INCLUDE_DIR})

add_custom_command(
    TARGET call_center POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
    ${CMAKE_CURRENT_SOURCE_DIR}/CallCenterClients.db
    $<TARGET_FILE_DIR:call_center>
)
