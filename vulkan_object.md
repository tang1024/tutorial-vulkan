# **VkInstance**
* create by
  * vkCreateInstance
  * vkDestroyInstance

# **VkDebugUtilsMessengerEXT**
* create with
  * VkInstance
* create by
  * vkCreateDebugUtilsMessengerEXT
  * vkDestroyDebugUtilsMessengerEXT

# VkPhysicalDevice (**VkInstance**)
* get with
  * VkInstance
* get by
  * vkEnumeratePhysicalDevices

* vkGetPhysicalDeviceProperties
* vkGetPhysicalDeviceFeatures

# VkQueueFamilyProperties (QueueFamily) (**?**)
* get with
  * VkPhysicalDevice
* get by
  * vkGetPhysicalDeviceQueueFamilyProperties

# **VkDevice**
* create with
  * VkPhysicalDevice
* create by
  * vkCreateDevice
  * vkDestroyDevice

# VkQueue (**VkDevice**)
* get with
  * VkDevice
  * QueueFamily index
* get by 
  * vkGetDeviceQueue

# **VkSurfaceKHR**
* create with
  * VkInstance
  * window handle
* create by
  * vkCreateWin32SurfaceKHR
  * vkDestroySurfaceKHR

* vkGetPhysicalDeviceSurfaceSupportKHR
	* VkPhysicalDevice
	* VkSurfaceKHR

# **VkSwapchainKHR**
* create with
  * VkDevice
* create by
  * vkCreateSwapchainKHR
  * vkDestroySwapchainKHR

vkEnumerateDeviceExtensionProperties (VkPhysicalDevice)
* VK_KHR_SWAPCHAIN_EXTENSION_NAME: VK_KHR_swapchain 

* VkSurfaceCapabilitiesKHR
  * vkGetPhysicalDeviceSurfaceCapabilitiesKHR
  * VkPhysicalDevice
  * VkSurfaceKHR
  * -> VkExtent2D
  
* VkSurfaceFormatKHR
  * vkGetPhysicalDeviceSurfaceFormatsKHR
  * VkPhysicalDevice
  * VkSurfaceKHR
  
* VkPresentModeKHR
  * vkGetPhysicalDeviceSurfacePresentModesKHR
  * VkPhysicalDevice
  * VkSurfaceKHR

# VkImage (**VkSwapchainKHR**)
* get with
  * VkDevice
  * VkSwapchainKHR
* get by 
  * vkGetSwapchainImagesKHR

# **VkImageView**
* create with
  * VkDevice
* create by
  * vkCreateImageView
  * vkDestroyImageView

# **VkShaderModule**
* create with
  * VkDevice
* create by
  * vkCreateShaderModule
  * vkDestroyShaderModule

# **VkPipelineLayout**
* create with
  * VkDevice
* create by
  * vkCreatePipelineLayout
  * vkDestroyPipelineLayout

# **VkRenderPass**
* create with
  * VkDevice
* create by
  * vkCreateRenderPass
  * vkDestroyRenderPass

* VkAttachmentDescription
* VkAttachmentReference
* VkSubpassDescription

# Fixed functions
* VkPipelineShaderStageCreateInfo
* VkPipelineVertexInputStateCreateInfo
* VkPipelineInputAssemblyStateCreateInfo
* VkPipelineViewportStateCreateInfo
* VkPipelineRasterizationStateCreateInfo
* VkPipelineMultisampleStateCreateInfo
* VkPipelineDepthStencilStateCreateInfo
* VkPipelineColorBlendStateCreateInfo
  * VkPipelineColorBlendAttachmentState
* VkPipelineDynamicStateCreateInfo

# **VkPipeline**
* create with
  * VkDevice
  * VkPipelineCache
* create by
  * vkCreateGraphicsPipelines
  * vkDestroyPipeline

* VkGraphicsPipelineCreateInfo
  * pStages
  * 
  * pVertexInputState
  * pInputAssemblyState
  * pViewportState
  * pRasterizationState
  * pMultisampleState
  * pDepthStencilState
  * pColorBlendState
  * pDynamicState
  * 
  * layout
  * 
  * renderPass
  * subpass

# **VkFramebuffer**
The attachments specified during render pass creation are bound by wrapping them into a VkFramebuffer object. 

That means that we have to create a framebuffer for all of the images in the swap chain and use the one that corresponds to the retrieved image at drawing time.

 We first need to specify with which renderPass the framebuffer needs to be compatible. 

* create with
  * VkDevice
* create by
  * vkCreateFramebuffer
  * vkDestroyFramebuffer

* relies on
  * VkRenderPass
  * VkImageView (pAttachments)
	VkFramebuffer <-> VkImageView
	one on one

# **VkCommandPool**
* create with
  * VkDevice
* create by
  * vkCreateCommandPool
  * vkDestroyCommandPool

queuefamily index

# VkCommandBuffer
VkCommandBuffer <-> VkImage

from the pool

## Command buffer allocation
vkAllocateCommandBuffers

VkDevice

vkFreeCommandBuffers

## Starting command buffer recording
vkBeginCommandBuffer

VkCommandBuffer

## Starting a render pass
vkCmdBeginRenderPass

VkCommandBuffer

## Basic drawing commands
vkCmdBindPipeline

vkCmdDraw

## Finishing up
vkCmdEndRenderPass

vkEndCommandBuffer


# Synchronization
The drawFrame function will perform the following operations:

Acquire an image from the swap chain
Execute the command buffer with that image as attachment in the framebuffer
Return the image to the swap chain for presentation

The difference is that the state of fences can be accessed from your program using calls like `vkWaitForFences` and semaphores cannot be. 

# **VkSemaphore**
* create with
  * VkDevice
* create by
  * vkCreateSemaphore
  * vkDestroySemaphore	

Acquire an image from the swap chain
	vkAcquireNextImageKHR
		VkDevice
		VkSwapchainKHR

Execute the command buffer with that image as attachment in the framebuffer
	vkQueueSubmit
		graphicsQueue
		VkSubmitInfo
			VkCommandBuffer

Return the image to the swap chain for presentation
	vkQueuePresentKHR
		presentQueue
		VkPresentInfoKHR 

vkDeviceWaitIdle(device);

VkFence
* vkWaitForFences
* vkResetFences