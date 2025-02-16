set(kvClient_sources)

list(APPEND kvClient_sources kv/2pc.cc)
list(APPEND kvClient_sources kv/Backoff.cc)
list(APPEND kvClient_sources kv/Cluster.cc)
list(APPEND kvClient_sources kv/LockResolver.cc)
list(APPEND kvClient_sources kv/RegionCache.cc)
list(APPEND kvClient_sources kv/RegionClient.cc)
list(APPEND kvClient_sources kv/Rpc.cc)
list(APPEND kvClient_sources kv/Snapshot.cc)
list(APPEND kvClient_sources kv/Scanner.cc)
list(APPEND kvClient_sources pd/Client.cc)
list(APPEND kvClient_sources coprocessor/Client.cc)
list(APPEND kvClient_sources RedactHelpers.cc)

set(kvClient_INCLUDE_DIR ${kvClient_SOURCE_DIR}/include)

# Only enable fiu under test mode.
if (ENABLE_TESTS)
    add_definitions(-DFIU_ENABLE)
endif()

add_library(kv_client ${kvClient_sources})
target_include_directories(kv_client PUBLIC ${kvClient_INCLUDE_DIR} ${fiu_include_dirs} ${Protobuf_INCLUDE_DIR} ${gRPC_INCLUDE_DIRS})
target_link_libraries(kv_client kvproto ${Poco_Foundation_LIBRARY} fiu ${Protobuf_LIBRARY} ${gRPC_LIBRARIES})

if (ENABLE_TESTS)
    add_subdirectory (test)
endif()
