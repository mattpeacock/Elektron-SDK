if (BUILD_ELEKTRON-SDK-BINARYPACK OR BUILD_INTERNAL_ESDK)
	set( SOURCE_FILES
  rsslNIProvider.c         rsslNILoginProvider.c        rsslNIDirectoryProvider.c
  rsslNIItemHandler.c      rsslVANIMarketByOrderItems.c rsslVANIMarketPriceItems.c 
  ${EtaExamples_SOURCE_DIR}/Examples/VACommon/rsslVACacheHandler.c
  ${EtaExamples_SOURCE_DIR}/Examples/VACommon/rsslVASendMessage.c
  )

	add_executable( VANIProvider_shared ${SOURCE_FILES} )
	target_include_directories(VANIProvider_shared
							PUBLIC
								$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
								$<BUILD_INTERFACE:${EtaExamples_SOURCE_DIR}/Examples/VACommon>
							)
	target_link_libraries( VANIProvider_shared 
							librsslVA_shared 
								Elektron-SDK-BinaryPack::rsslVACache_shared 
                            ${SYSTEM_LIBRARIES} 
						)

	add_executable( VANIProvider ${SOURCE_FILES} )
	target_include_directories(VANIProvider
							PUBLIC
								$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
								$<BUILD_INTERFACE:${EtaExamples_SOURCE_DIR}/Examples/VACommon>
							)
	target_link_libraries( VANIProvider 
							librsslVA  
								Elektron-SDK-BinaryPack::rsslVACache
                            ${SYSTEM_LIBRARIES} 
						)

	set_target_properties( VANIProvider VANIProvider_shared PROPERTIES OUTPUT_NAME VANIProvider )

	if ( CMAKE_HOST_UNIX )
    set_target_properties( VANIProvider_shared 
                            PROPERTIES 
                                RUNTIME_OUTPUT_DIRECTORY 
                                    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/Shared )

	else() #  if ( CMAKE_HOST_WIN32 )
	target_compile_options( VANIProvider 
								PRIVATE 
									${RCDEV_DEBUG_TYPE_FLAGS_NONSTATIC}
									${RCDEV_TYPE_CHECK_FLAG}
									$<$<CONFIG:Release_MD>:${RCDEV_FLAGS_NONSTATIC_RELEASE}>
						)
    set_target_properties( VANIProvider_shared 
                            PROPERTIES 
                                RUNTIME_OUTPUT_DIRECTORY_RELEASE_MD 
                                    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE_MD}/Shared
                                RUNTIME_OUTPUT_DIRECTORY_DEBUG_MDD 
                                    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
								LIBRARY_OUTPUT_DIRECTORY_RELEASE_MD
                                    ${CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE_MD}/Shared
                                LIBRARY_OUTPUT_DIRECTORY_DEBUG_MDD
                                    ${CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
								PDB_OUTPUT_DIRECTORY_RELEASE_MD 
									${CMAKE_PDB_OUTPUT_DIRECTORY_RELEASE_MD}/Shared 
								PDB_OUTPUT_DIRECTORY_DEBUG_MDD 
									${CMAKEPDB_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared 
								ARCHIVE_OUTPUT_DIRECTORY_RELEASE_MD
                                    ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE_MD}/Shared
                                ARCHIVE_OUTPUT_DIRECTORY_DEBUG_MDD
                                    ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG_MDD}/Shared
                          )
	target_compile_options( VANIProvider_shared 
								PRIVATE 
									${RCDEV_DEBUG_TYPE_FLAGS_NONSTATIC}
									${RCDEV_TYPE_CHECK_FLAG}
									$<$<CONFIG:Release_MD>:${RCDEV_FLAGS_NONSTATIC_RELEASE}>
						)
	endif()
else()
	message(STATUS "Skipping Eta Example VANIProvider Build: BUILD_ELEKTRON-SDK-BINARYPACK:${BUILD_ELEKTRON-SDK-BINARYPACK}")
endif()


