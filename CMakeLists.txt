#
# project: fips-lz4
#
option(LZ4_MINIMAL "Build minimal version of LZ4 library" ON)

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")

    fips_setup(PROJECT fips-lz4)
    fips_begin_app(lz4 cmdline)
else()
    fips_begin_lib(lz4)
endif()

include_directories(lz4/lib)
fips_dir(lz4/lib GROUP ".")

fips_files(
    lz4.c
    lz4.h
)

if (NOT LZ4_MINIMAL)
    fips_files(
        lz4frame.c
        lz4frame.h
        lz4hc.c
        lz4hc.h
    )
endif()

if (NOT FIPS_IMPORT)
    if (LZ4_MINIMAL)
        fips_files(
            lz4frame.c
            lz4frame.h
            lz4hc.c
            lz4hc.h
        )
    endif()

    fips_dir(lz4/programs GROUP "cli")

    fips_files(
        bench.c
        bench.h
        datagen.c
        datagen.h
        lz4cli.c
        lz4io.c
        lz4io.h
        platform.h
        util.h
    )

    fips_libs(xxhash)

    fips_end_app()
    fips_finish()
else()
    fips_end_lib()
endif()
