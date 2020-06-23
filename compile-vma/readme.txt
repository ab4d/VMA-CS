Compiling VMA as a dll for Windows with Visual Studio:

1) Download and install Vulkan SDK (https://vulkan.lunarg.com/)

2) Download VMA source code from https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator and locate the vk_mem_alloc.h file.

3) Add a new file vk_mem_alloc.cpp to the same folder - set its content to:
#pragma comment(lib, "vulkan-1.lib")
#define VMA_IMPLEMENTATION
#include "vk_mem_alloc.h"

The first line specifies to use the vulkan-1.lib that is needed to compile with VMA_STATIC_VULKAN_FUNCTIONS=1 (by default; this uses the Vulcan functions defined in vulkan.h and vulkan-1.lib (in C:\VulkanSDK\1.1.114.0\Lib or ...\Lib32). 

4) Open the  vk_mem_alloc.h file and uncomment the following two lines (line 1819 in v2.3.0):
#define VMA_CALL_PRE  __declspec(dllexport)
#define VMA_CALL_POST __cdecl

This will compile the library with exporting the VMA functions so that they can be called from other libraries.

5) Start "compile win-x64.bat" or "compile win-x86.bat". This will crate the vk_mem_alloc.dll in the same folder.


cl.exe compiler command line options:
(https://docs.microsoft.com/en-us/cpp/build/reference/compiler-options-listed-by-category?view=vs-2019):

/LD - creates a dynamic-link library.
/W3 - verbosity level - use W4 to get more info
/Iheader_folder_name - adds include files folder (.h header files in the same folder are automatically loaded)
/link - parameters after that will be used by the linker (defined in  https://docs.microsoft.com/en-us/cpp/build/reference/linker-options?view=vs-2019)
