project(libssh-examples C CXX)

set(examples_SRCS
  authentication.c
  knownhosts.c
  connect_ssh.c
)

include_directories(
  ${LIBSSH_PUBLIC_INCLUDE_DIRS}
  ${CMAKE_BINARY_DIR}
)

if (ARGP_INCLUDE_DIR)
    include_directories(${ARGP_INCLUDE_DIR})
endif()

if (UNIX AND NOT WIN32)
    add_executable(libssh_scp libssh_scp.c ${examples_SRCS})
    target_compile_options(libssh_scp PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
    target_link_libraries(libssh_scp ${LIBSSH_SHARED_LIBRARY})

    add_executable(scp_download scp_download.c ${examples_SRCS})
    target_compile_options(scp_download PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
    target_link_libraries(scp_download ${LIBSSH_SHARED_LIBRARY})

    add_executable(sshnetcat sshnetcat.c ${examples_SRCS})
    target_compile_options(sshnetcat PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
    target_link_libraries(sshnetcat ${LIBSSH_SHARED_LIBRARY})

    if (WITH_SFTP)
        add_executable(samplesftp samplesftp.c ${examples_SRCS})
        target_compile_options(samplesftp PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
        target_link_libraries(samplesftp ${LIBSSH_SHARED_LIBRARY})
    endif (WITH_SFTP)

    add_executable(ssh-client ssh_client.c ${examples_SRCS})
    target_compile_options(ssh-client PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
    target_link_libraries(ssh-client ${LIBSSH_SHARED_LIBRARY})

    if (WITH_SERVER AND (ARGP_LIBRARY OR HAVE_ARGP_H))
        if (HAVE_LIBUTIL)
            add_executable(ssh_server_fork ssh_server_fork.c)
            target_compile_options(ssh_server_fork PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
            target_link_libraries(ssh_server_fork ${LIBSSH_SHARED_LIBRARY} ${ARGP_LIBRARY} util)
        endif (HAVE_LIBUTIL)

        if (WITH_GSSAPI AND GSSAPI_FOUND)
            add_executable(samplesshd-cb samplesshd-cb.c)
            target_compile_options(samplesshd-cb PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
            target_link_libraries(samplesshd-cb ${LIBSSH_SHARED_LIBRARY} ${ARGP_LIBRARY})

            add_executable(proxy proxy.c)
            target_compile_options(proxy PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
            target_link_libraries(proxy ${LIBSSH_SHARED_LIBRARY} ${ARGP_LIBRARY})

            add_executable(sshd_direct-tcpip sshd_direct-tcpip.c)
            target_compile_options(sshd_direct-tcpip PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
            target_link_libraries(sshd_direct-tcpip ${LIBSSH_SHARED_LIBRARY} ${ARGP_LIBRARY})
        endif (WITH_GSSAPI AND GSSAPI_FOUND)

        add_executable(samplesshd-kbdint samplesshd-kbdint.c)
        target_compile_options(samplesshd-kbdint PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
        target_link_libraries(samplesshd-kbdint ${LIBSSH_SHARED_LIBRARY} ${ARGP_LIBRARY})

    endif()
endif (UNIX AND NOT WIN32)

add_executable(exec exec.c ${examples_SRCS})
target_compile_options(exec PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
target_link_libraries(exec ${LIBSSH_SHARED_LIBRARY})

add_executable(senddata senddata.c ${examples_SRCS})
target_compile_options(senddata PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
target_link_libraries(senddata ${LIBSSH_SHARED_LIBRARY})

add_executable(keygen keygen.c)
target_compile_options(keygen PRIVATE ${DEFAULT_C_COMPILE_FLAGS})
target_link_libraries(keygen ${LIBSSH_SHARED_LIBRARY})

add_executable(libsshpp libsshpp.cpp)
target_link_libraries(libsshpp ${LIBSSH_SHARED_LIBRARY})

add_executable(libsshpp_noexcept libsshpp_noexcept.cpp)
target_link_libraries(libsshpp_noexcept ${LIBSSH_SHARED_LIBRARY})
