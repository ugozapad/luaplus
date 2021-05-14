option(USE_LUAPLUS_EXTENSIONS "Use luaplus extensions for lua core." OFF)

if(USE_LUAPLUS_EXTENSIONS)
	set(LUA_PATH "${CMAKE_CURRENT_SOURCE_DIR}/lua53-luaplus")
	set(LUA_TARGET_NAME "lua53-luaplus")
else()
	set(LUA_PATH "${CMAKE_CURRENT_SOURCE_DIR}/lua53")
	set(LUA_TARGET_NAME "lua53")
endif()

file(GLOB LUA_SRC
	"${LUA_PATH}/src/*.c"
	"${LUA_PATH}/src/*.h"
)

list(REMOVE_ITEM LUA_SRC "${LUA_PATH}/src/lua.c")
list(REMOVE_ITEM LUA_SRC "${LUA_PATH}/src/luac.c")

# We using lua only as static lib.
add_library(${LUA_TARGET_NAME} STATIC ${LUA_SRC})
target_include_directories(${LUA_TARGET_NAME} PUBLIC "${LUA_SRC}/src")

#########
# LuaPlus

set(LUAPLUS_PATH "${CMAKE_CURRENT_SOURCE_DIR}")

file(GLOB LUAPLUS_SRC
	"${LUAPLUS_PATH}/*.cpp"
	"${LUAPLUS_PATH}/*.h"
	"${LUAPLUS_PATH}/*.inl"
)

# We using luabind as static library too!
add_library(luaplus STATIC ${LUAPLUS_SRC})
target_include_directories(luaplus PUBLIC ${LUAPLUS_PATH} PUBLIC "${LUA_PATH}/src")

# For user convenience 
target_link_libraries(luaplus ${LUA_TARGET_NAME})