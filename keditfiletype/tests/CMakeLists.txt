include_directories( ${CMAKE_CURRENT_SOURCE_DIR}/.. )

########### filetypestest ###############

add_executable(filetypestest
 filetypestest.cpp
 ../mimetypedata.cpp
 ../mimetypewriter.cpp
)
ecm_mark_as_test(filetypestest)
add_test(NAME filetypestest COMMAND filetypestest)
target_link_libraries(filetypestest
   KF6::KIOCore
   KF6::Service
   KF6::Parts
   Qt::Core
   Qt::Gui
   Qt::Test
)

# The kcmshell6 of the KCMUtils built against, never another desktop's on PATH.
find_program(KCMSHELL6_EXECUTABLE kcmshell6
    PATHS ${KF6KCMUtils_DIR}/../../../bin ${KF6KCMUtils_DIR}/../../../../bin
    NO_DEFAULT_PATH REQUIRED
)
add_test(NAME kcm_smoketest COMMAND ${KCMSHELL6_EXECUTABLE} --smoke-test kcm_filetypes)
set_tests_properties(kcm_smoketest PROPERTIES
    ENVIRONMENT_MODIFICATION QT_PLUGIN_PATH=path_list_prepend:${CMAKE_BINARY_DIR}/bin
)
