PROJECT(TinyXML)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/../common/cmake/)
if(USE_LOCAL_TINYXML)
    set(TINYXML_FOUND "NO")
    message(STATUS "Using local TinyXML")
else()
    find_package(TINYXML)
endif()
if (NOT TINYXML_FOUND)
    file (GLOB TinyXML_src src/*.c* src/*.def)
    file (GLOB TinyXML_headers include/*.h*)

    set(TinyXML_HEADERS_DIR ${TinyXML_SOURCE_DIR}/include)

    include_directories(${TinyXML_HEADERS_DIR})

    if(APPLE)
        set(CMAKE_OSX_ARCHITECTURES "i386;x86_64")
    endif()

    add_library(tinyxml ${TinyXML_src} ${TinyXML_headers})
    
    if(VISIBILITY_HIDDEN)
        SET(COMPILE_FLAGS "${COMPILE_FLAGS} -fvisibility=hidden")
    endif()    
    if(UNIX OR APPLE)
        SET(COMPILE_FLAGS "${COMPILE_FLAGS} -fPIC")
    endif()
    set_target_properties(tinyxml PROPERTIES COMPILE_FLAGS "${COMPILE_FLAGS}")
	 set_property(TARGET tinyxml PROPERTY FOLDER "third_party")
    pack_static(tinyxml)
else()
    set(TinyXML_HEADERS_DIR ${TINYXML_INCLUDE_DIRS})
endif()
