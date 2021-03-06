option( PLUGIN_STANDARD_QSRA "Install qSRA (Surface of Revolution Analysis) plugin" OFF )

# CloudCompare 'Surface of Revolution Analysis' plugin
if( PLUGIN_STANDARD_QSRA )
	# DXF support should be enabled for this plugin! (not mandatory though)
	if( NOT OPTION_USE_DXF_LIB )
		message( WARNING "qSRA plugin works best with dxflib support! (enable OPTION_USE_DXF_LIB)" )
	endif()
	
	project( QSRA_PLUGIN )

    AddPlugin( NAME ${PROJECT_NAME} )

	add_subdirectory( include )
	add_subdirectory( src )
	add_subdirectory( ui )
    
    set_target_properties( ${PROJECT_NAME} PROPERTIES
        # FIXME Remove after everything has moved to targets
        # This is an exception to find ccColorScaleEditorDlg which means we have an odd dependency
        AUTOUIC_SEARCH_PATHS "${CloudCompare_SOURCE_DIR}/ui_templates;${CMAKE_CURRENT_SOURCE_DIR}/ui"
    )

    # FIXME These are required because we have dependencies outside of the API
	target_sources( ${PROJECT_NAME}
		PRIVATE
			# we need the ccRenderToFileDlg class
			${CloudCompare_SOURCE_DIR}/ccRenderToFileDlg.cpp
			${CloudCompare_SOURCE_DIR}/ccRenderToFileDlg.h
			${CloudCompare_SOURCE_DIR}/ui_templates/renderToFileDialog.ui
	)
    target_include_directories( ${PROJECT_NAME}
        PRIVATE
            ${CloudCompare_SOURCE_DIR}
    )
	
	if( OPTION_USE_DXF_LIB )
		target_link_DXFLIB( ${PROJECT_NAME} )
	endif()
endif()
