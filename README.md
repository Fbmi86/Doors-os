# Doors-os
Doors SDK Development Note and OS Introduction
Development Note for Doors SDK (doors_sdk.h)
Purpose
The doors_sdk.h header defines the core interfaces and structures for the Doors Operating System's Software Development Kit (SDK). It provides a standardized framework for developing dynamically loadable kernel modules, referred to as "Dro modules," which support hardware drivers, system services, and other extensible components. The SDK ensures modularity, ease of integration, and robust lifecycle management for modules within the Doors kernel.
Key Components

Core Structures:

dro_module_t: Defines the module interface, including metadata (magic number, version, name) and callbacks for lifecycle management (init, release), device binding (probe, attach, detach), and system events (on_power_failure, on_resume). This structure allows modules to interact with the kernel and device manager in a standardized way.
loaded_module_t: Tracks loaded modules in the kernel, storing metadata such as the module's file path, base address, size, and a pointer to its dro_module_t structure. It supports a linked list for managing multiple loaded modules.


Dro Loader Subsystem API:

Functions like dro_load_module and dro_unload_module handle the loading and unloading of Dro modules, including ELF parsing, memory allocation, and relocation. The kmain entry point in each module is responsible for registering the module via register_dro_module.
Utility functions (dro_find_loaded_module, dro_find_by_info) enable module lookup by name or dro_module_t pointer.
Event callback functions (dro_call_on_power_failure, dro_call_on_resume) propagate system events to relevant modules.


Module Registration:

The register_dro_module function allows a module to register itself with the kernel, ensuring proper integration and lifecycle management.



Design Considerations

Modularity: The SDK emphasizes a modular architecture, enabling developers to create independent, reusable components that can be loaded or unloaded without rebooting the system.
Extensibility: The callback-based design allows modules to support various device types (e.g., PCI, USB) and system events, making the SDK adaptable to diverse hardware and use cases.
Safety and Robustness: The use of a magic number in dro_module_t ensures structure integrity, while boolean return values for critical operations (init, probe, attach) enable error handling.
Memory Efficiency: Constants like SDKK_PAGE_SIZE align with the kernel's memory management, and SDKK_MAX_PATH ensures reasonable limits for file paths.

Future Enhancements

Add support for more system event callbacks (e.g., ioctl, read, write) to handle device-specific operations.
Introduce versioning checks to prevent compatibility issues between modules and the kernel.
Expand dro_module_t to include fields for device-specific IDs (e.g., PCI or USB IDs) to streamline device probing.

Introduction to the Doors Operating System
Overview
The Doors Operating System is a modern, modular, and lightweight kernel designed for flexibility and extensibility. It is built to support a wide range of hardware and software environments through a dynamic module system, enabling developers to extend kernel functionality without modifying its core. The Doors OS prioritizes performance, security, and ease of development, making it suitable for embedded systems, desktops, and specialized applications.
Module System
At the heart of Doors OS is its Dro module system, which allows drivers and system services to be loaded and unloaded dynamically. Dro modules are implemented as ELF binaries with a standardized interface defined in doors_sdk.h. Each module provides a dro_module_t structure that specifies its capabilities and lifecycle callbacks. The kernel's Dro loader subsystem manages these modules, handling tasks such as loading from disk, memory allocation, and integration with the device manager.
Key Features

Dynamic Loading: Modules can be loaded at runtime from specified paths (e.g., /System/Drivers/Dro/ahci.dro), enabling hot-plugging of drivers and services.
Device Management: The SDK's probe, attach, and detach callbacks integrate with the kernel's device manager, supporting a device tree model for hardware discovery and binding.
Event Handling: System-wide events, such as power failures or system resume, are propagated to modules, ensuring robust system behavior.
Scalability: The linked list of loaded_module_t structures supports efficient management of multiple modules, suitable for systems with varying complexity.

Use Cases
The Doors OS and its SDK are ideal for:

Developing drivers for diverse hardware (e.g., AHCI controllers, RTL8139 network cards).
Creating system services that respond to kernel events.
Building extensible platforms where new functionality can be added without kernel recompilation.

This SDK and the Doors OS together provide a powerful foundation for creating scalable, modular, and maintainable system software.
