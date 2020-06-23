# VMA-CS
C# wrapper for the Vulkan Memory Allocator (VMA) library
(https://github.com/GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator)

The Vulkan Memory Allocator (VMA) library provides a simple and easy to integrate API to help you allocate memory for Vulkan buffer and image storage.

This project contains a VMA.cs file that can be included into your project and provides a .Net wrapper for the native VMA library.


## Native VMA library

VMA library is originally defiend in a single C++ header file (vk_mem_alloc.h). To use it in a .Net project, the library needs to be compiled into a native dll. 

This project comes with a precompiled library for win-x86 and win-x64 (**see compile-vma folder**).
There are also instructions and windows bat files to compile newer versions of the library or versions with different #define options.


## Usage

To use the VMA library in a .Net project, copy the VMA.cs file from this project to your project.
Then add the native vk_mem_alloc.dll to the output path of your project (use the version for your target platform).

The following code shows a usage sample with VulkanSharp library (https://github.com/mono/VulkanSharp):

```csharp
private void TestVma(Device vulkanDevice, PhysicalDevice physicalDevice)
{
    var vmaAllocatorCreateInfo = new VMA.VmaAllocatorCreateInfo()
    {
        device         = ((IMarshalling)vulkanDevice).Handle,
        physicalDevice = ((IMarshalling)physicalDevice).Handle
    };

    var result = (Vulkan.Result)VMA.vmaCreateAllocator(ref vmaAllocatorCreateInfo, out var allocatorPtr);

    if (result != Result.Success)
    {
        // Cannot create VMA allocator
    }


    var allocatorCreateInfo = new VMA.VmaAllocationCreateInfo()
    {
        usage = VMA.VmaMemoryUsage.VMA_MEMORY_USAGE_GPU_ONLY
    };

    var bufferCreateInfo = new BufferCreateInfo()
    {
        Size = 65536,
        Usage = BufferUsageFlags.VertexBuffer | BufferUsageFlags.TransferDst
    };

    result = (Vulkan.Result)VMA.vmaCreateBuffer(allocatorPtr, ((IMarshalling)bufferCreateInfo).Handle, ref allocatorCreateInfo, out var pBuffer, out var pAllocation, IntPtr.Zero);


    if (result == Result.Success)
    {
        // Use the pBuffer ...

        VMA.vmaDestroyBuffer(allocatorPtr, pBuffer, pAllocation); // Dispose pBuffer and pAllocation
    }

    VMA.vmaDestroyAllocator(allocatorPtr);
}
```


## Contributors

This project was created by Caleb Cornett (TheSpydog) - https://github.com/TheSpydog

The development is continued by Andrej Benedik (AB4D) - https://github.com/ab4d

AB4D company works on advanced 3D visualizations for .Net. See https://www.ab4d.com for more info.
