find_package(epoxy REQUIRED)

set(POLYGLDEVICE_SRCS
	Src/BlinnPhongRenderingPass.cpp
	Src/DebugNormalsRenderingPass.cpp
	Src/GLMeshDeviceProxy.cpp
	Src/GLRenderingDevice.cpp
	Src/GLShaderProgram.cpp
	Src/GLTextFieldBufferDeviceProxy.cpp
	Src/GLTextureDeviceProxy.cpp
	Src/GLWorldRendering.cpp
	Src/PostprocessRenderingPass.cpp
	Src/PostprocessQuad.cpp
	Src/RenderingPassBase.cpp
	Src/Text2DRenderingPass.cpp
	Src/TransparentRenderingPass.cpp
)
set(POLYGLDEVICE_INCLUDE Src)
set(POLYGLDEVICE_H_FOR_IDE
	Src/BlinnPhongRenderingPass.hpp
	Src/DebugNormalsRenderingPass.hpp
	Src/GLMeshDeviceProxy.hpp
	Src/GLRenderingDevice.hpp
	Src/GLShaderProgram.hpp
	Src/GLTextFieldBufferDeviceProxy.hpp
	Src/GLTextureDeviceProxy.hpp
	Src/GLUtils.hpp
	Src/PostprocessRenderingPass.hpp
	Src/PostprocessQuad.hpp
	Src/RenderingPassBase.hpp
	Src/Text2DRenderingPass.hpp
	Src/TransparentRenderingPass.hpp
)
set(POLYGLDEVICE_LIBS polycore polyengine epoxy::gl)

# macOS Obj-C sources
if(APPLE)
	find_library(Cocoa_LIBRARY Cocoa)
	mark_as_advanced(Cocoa_LIBRARY)

	list(APPEND POLYGLDEVICE_SRCS
            Src-macOS/CocoaImpl.mm)
	list(APPEND POLYGLDEVICE_INCLUDE Src-macOS)
	list(APPEND POLYGLDEVICE_H_FOR_IDE
			Src-macOS/CocoaImpl.h
            Src-macOS/ObjCInterface.hpp)
	list(APPEND POLYGLDEVICE_LIBS ${Cocoa_LIBRARY})
endif(APPLE)

add_library(polygldevice SHARED ${POLYGLDEVICE_SRCS} ${POLYGLDEVICE_H_FOR_IDE})
target_compile_definitions(polygldevice PRIVATE _DEVICE)
target_include_directories(polygldevice PUBLIC ${POLYGLDEVICE_INCLUDE})
target_link_libraries(polygldevice PRIVATE ${POLYGLDEVICE_LIBS})

cotire(polygldevice)
