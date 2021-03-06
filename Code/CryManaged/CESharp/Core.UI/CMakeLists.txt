if(OPTION_BUILD_CSHARP_WITH_MCS)
	project(CESharp NONE)

	set(SDK_DIR ${CMAKE_CURRENT_LIST_DIR}/../../../SDKs)
	set(MCS_PATH ${SDK_DIR}/Mono/bin/mcs.bat)
	
	if(MONO_WIN64)
		set(CRYMONOBRIDGE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../../../../bin/win_x64)
	else() 
		set(CRYMONOBRIDGE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../../../../bin/win_x86)
	endif()
	create_mono_compiler_settings()
	
	set(Component_FILES Components/ButtonCtrl.cs Components/CheckBoxCtrl.cs Components/CollapseCtrl.cs Components/ComboBoxCtrl.cs Components/Image.cs Components/RectTransform.cs Components/ScrollBarCtrl.cs Components/SplitBarCtrl.cs Components/Text.cs Components/TextCtrl.cs Components/UIComponent.cs)
	source_group("Components" FILES ${Component_FILES})

	set(Resource_FILES Resources/ImageSource.cs Resources/Resource.cs Resources/Texture.cs)
	source_group("Resources" FILES ${Resource_FILES})

	set(Root_FILES Button.cs Canvas.cs CheckBox.cs ComboBox.cs HorizontalLayoutGroup.cs IUIComponent.cs LayoutGroup.cs Panel.cs SceneManager.cs SceneObject.cs ScrollPanel.cs Splitter.cs TextElement.cs TextInput.cs Transform.cs UIElement.cs VerticalLayoutGroup.cs Window.cs)
	source_group("" FILES ${Root_FILES})

	set(SOURCE ${Component_FILES} ${Resource_FILES} ${Root_FILES})

	add_custom_target(CryEngine.Core.UI ALL
		COMMAND ${MCS_PATH} -target:library -langversion:${MONO_LANGUAGE_VERSION} -platform:${MONO_CPU_PLATFORM} -optimize -debug -define:${MONO_PREPROCESSOR_DEFINE} -lib:${MONO_LIB_PATH} -r:System -r:System.Data -r:System.Drawing -r:System.Runtime.Serialization -r:System.Windows.Forms -r:System.Xml -r:${CRYMONOBRIDGE_PATH}/CryEngine.Common.dll -r:${CRYMONOBRIDGE_PATH}/CryEngine.Core.dll ${SOURCE} -out:${CRYMONOBRIDGE_PATH}/CryEngine.Core.UI.dll
		WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
		SOURCES ${SOURCE})

	set_target_properties(CryEngine.Core.UI PROPERTIES VS_DOTNET_REFERENCES "System;System.Data")
else()
	include_external_msproject(CryEngine.Core.UI ${CMAKE_CURRENT_LIST_DIR}/Core.UI.csproj)
endif()

add_dependencies(CryEngine.Core.UI CryEngine.Core)

set_property(TARGET CryEngine.Core.UI PROPERTY FOLDER "CryMono")