Steps:
1. Create a `3rd_party/<library-name>` folder
2. Copy include and lib64 folder of the library to the `3rd_party/<library-name>` folder
3. Create a `cmake/modules` folder
4. Create `Find<Library-name>.cmake` file inside `cmake/modules` folder
5. Define paths and libraries needed:
    set(`<LIBRARY_NAME>`_PATH ${CMAKE_SOURCE_DIR}/3rd_party/`<library-name>`)
    set(`<LIBRARY_NAME>`_INCLUDE_PATH ${`<LIBRARY_NAME>`_PATH}/include) -> To the include folder
    set(`<LIBRARY_NAME>`_LIB_PATH ${`<LIBRARY_NAME>`_PATH}/lib64/...) -> To the folder that contains the .so files needed in lib64
    set(`<LIBRARY_NAME>`_LIB file1 file2 ...) -> The .so files needed. Remember to remove the `Lib` prefix and `.so` extension
6. Create `CMakeLists.txt` file in the original folder
7. Add these contents:
    set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules) -> Point to folder in step 3 that contains all cmake files of 3rd party libraries
    find_package(`<Library-name>` REQUIRED) -> The file name in step 4, removing the `Find` prefix and `.cmake` extension
    target_include_directories(main PUBLIC ${`<LIBRARY_NAME>`_INCLUDE_PATH}) -> the variable set in step 5, link to include folder
    target_link_directories(main PUBLIC ${`<LIBRARY_NAME>`_LIB_PATH}) -> the variable set in step 5, link to lib64 folder
    target_link_libraries(main PRIVATE ${`<LIBRARY_NAME>`_LIB}) -> the variable set in step 5, link to needed libraries