cmake_minimum_required(VERSION 3.5)


set(SOURCES_C
  libsrtp/srtp/ekt.c
  libsrtp/srtp/srtp.c
)


set(CIPHERS_SOURCES_C
  libsrtp/crypto/cipher/cipher.c
  libsrtp/crypto/cipher/null_cipher.c
)


if (ENABLE_OPENSSL)
  list(APPEND CIPHERS_SOURCES_C
    libsrtp/crypto/cipher/aes_icm_ossl.c
    libsrtp/crypto/cipher/aes_gcm_ossl.c
  )
else()
  list(APPEND  CIPHERS_SOURCES_C
    libsrtp/crypto/cipher/aes.c
    libsrtp/crypto/cipher/aes_icm.c
  )
endif()


set(HASHES_SOURCES_C
    libsrtp/crypto/hash/auth.c
    libsrtp/crypto/hash/null_auth.c
)


if (ENABLE_OPENSSL)
  list(APPEND HASHES_SOURCES_C
    libsrtp/crypto/hash/hmac_ossl.c
  )
else()
  list(APPEND  HASHES_SOURCES_C
    libsrtp/crypto/hash/hmac.c
    libsrtp/crypto/hash/sha1.c
  )
endif()


set(KERNEL_SOURCES_C
  libsrtp/crypto/kernel/alloc.c
  libsrtp/crypto/kernel/crypto_kernel.c
  libsrtp/crypto/kernel/err.c
  libsrtp/crypto/kernel/key.c
)


set(MATH_SOURCES_C
  libsrtp/crypto/math/datatypes.c
  libsrtp/crypto/math/stat.c
)


set(REPLAY_SOURCES_C
  libsrtp/crypto/replay/rdb.c
  libsrtp/crypto/replay/rdbx.c
  libsrtp/crypto/replay/ut_sim.c
)


set(SOURCES_H
  libsrtp/crypto/include/aes.h
  libsrtp/crypto/include/aes_icm.h
  libsrtp/crypto/include/alloc.h
  libsrtp/crypto/include/auth.h
  libsrtp/crypto/include/cipher.h
  libsrtp/crypto/include/cipher_types.h
  libsrtp/crypto/include/crypto_kernel.h
  libsrtp/crypto/include/crypto_types.h
  libsrtp/crypto/include/datatypes.h
  libsrtp/crypto/include/err.h
  libsrtp/crypto/include/hmac.h
  libsrtp/crypto/include/integers.h
  libsrtp/crypto/include/key.h
  libsrtp/crypto/include/null_auth.h
  libsrtp/crypto/include/null_cipher.h
  libsrtp/crypto/include/rdb.h
  libsrtp/crypto/include/rdbx.h
  libsrtp/crypto/include/sha1.h
  libsrtp/crypto/include/stat.h
  libsrtp/include/srtp.h
  libsrtp/include/srtp_priv.h
  libsrtp/include/ut_sim.h
  esp-port/config.h
)


set(SRTP2_SRCS
  ${SOURCES_C}
  ${CIPHERS_SOURCES_C}
  ${HASHES_SOURCES_C}
  ${KERNEL_SOURCES_C}
  ${MATH_SOURCES_C}
  ${REPLAY_SOURCES_C}
  ${SOURCES_H}
)


set(SRTP2_INCLUDE_DIRS
	esp-port
	libsrtp/include
	libsrtp/crypto/include
	)


idf_component_register(SRCS ${SRTP2_SRCS} INCLUDE_DIRS ${SRTP2_INCLUDE_DIRS})
target_compile_definitions(${COMPONENT_LIB} PUBLIC "-DHAVE_CONFIG_H")

# Reduce paranoia while building the sources
set_source_files_properties(libsrtp/crypto/kernel/crypto_kernel.c PROPERTIES COMPILE_FLAGS -Wno-maybe-uninitialized)
set_source_files_properties(libsrtp/srtp/srtp.c PROPERTIES COMPILE_FLAGS -Wno-incompatible-pointer-types)
set_source_files_properties(libsrtp/crypto/cipher/cipher.c PROPERTIES COMPILE_FLAGS -Wno-incompatible-pointer-types)
