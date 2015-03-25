cmake_minimum_required(VERSION 2.8)

project(htkutils_project)

SET(START_DIR .)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${START_DIR}/builds/win32 )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${START_DIR}/builds/win32/lib )

# Importing Infra
SET(INFRA_DIR ${START_DIR}/libs/infra/src)
add_library(infra STATIC ${INFRA_DIR}/infra_vv_funcs.cpp ${INFRA_DIR}/infra_vm_funcs.cpp ${INFRA_DIR}/infra_mm_funcs.cpp ${INFRA_DIR}/infra_exception.cpp ${INFRA_DIR}/infra_binary.cpp)
target_include_directories (infra PUBLIC ${INFRA_DIR})

# Importing cmd_line
SET(CMD_LINE_DIR ${START_DIR}/libs/cmd_line/src)
add_library(cmd_line STATIC ${CMD_LINE_DIR}/cmd_line.cpp)
target_include_directories (cmd_line PUBLIC ${CMD_LINE_DIR})

#Executables
set (EXEC_LIST 
htk_shuffle
htk_stats
htk_split
htk2db
htk2txt
txt2htk
htk_ceps_dist
htkcat
htk_contract
htk_split_from_phn)

FOREACH(EXEC ${EXEC_LIST})
  add_executable(${EXEC} ${START_DIR}/src/${EXEC}.cpp ${START_DIR}/src/HtkFile.cpp)
  target_link_libraries(${EXEC} LINK_PUBLIC cmd_line)
  target_link_libraries(${EXEC} LINK_PUBLIC infra)
ENDFOREACH()