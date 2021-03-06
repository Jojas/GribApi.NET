# data/CMakeLists.txt
add_subdirectory(tigge)

# Download all the binary GRIB data from website

file(READ "grib_data_files.txt" files_to_download)
string(REGEX REPLACE "\n" ";" files_to_download "${files_to_download}")
if( HAVE_AEC )
    list(APPEND files_to_download ccsds.grib2)
endif()


# Download all data files doing md5 check on each
ecbuild_get_test_multidata( TARGET get_gribs
                            NAMES ${files_to_download} )

# Copy other files - e.g. reference data, text files etc from the source data dir
LIST(APPEND other_files
    60_model_levels
    bitmap.diff
    ieee_test.good
    index.ok
    index_f90.ok
    julian.out.good
    list_points
    local.good.log
    ls.log
    multi.ok
    multi_step.txt
    no_bitmap.diff
    read_any.ok
    scan_x_regular_gg_5_7_good.dump
    scan_x_regular_ll_5_4_good.dump
    scan_x_regular_ll_5_7_good.dump
    scan_x_regular_ll_8_4_good.dump
    scan_x_regular_ll_8_7_good.dump
    scan_x_rotated_ll_5_4_good.dump
    scan_x_rotated_ll_5_7_good.dump
    scan_x_rotated_ll_8_4_good.dump
    scan_x_rotated_ll_8_7_good.dump
    scan_y_regular_ll_5_4_good.dump
    scan_y_regular_ll_5_7_good.dump
    scan_y_regular_ll_8_4_good.dump
    scan_y_regular_ll_8_7_good.dump
    scan_y_rotated_ll_5_4_good.dump
    scan_y_rotated_ll_5_7_good.dump
    scan_y_rotated_ll_8_4_good.dump
    scan_y_rotated_ll_8_7_good.dump
    spherical_model_level.grib1.good
    spherical_model_level.grib1_32.good
    statistics.out.good
    step.log
    step_grib1.filter
    step_grib1.log
    typeOfProcessedData.ok
    tigge_pf_ecmwf.grib2.ref
)

foreach( file ${other_files} )
   execute_process( COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/${file} ${CMAKE_CURRENT_BINARY_DIR} )
endforeach()

# Another dummy target which depends on all previous targets
#add_custom_target( get_gribs ALL DEPENDS ${tgts} )
#add_custom_target( get_gribs DEPENDS ${tgts} )
