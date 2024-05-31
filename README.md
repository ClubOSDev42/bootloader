# bootloader
Simple bootloader for ARM (Raspberry Pi) Architecture.

![banner](./banner.jpeg)

## Boot Process Overview

### POST (Power-On Self-Test)
- **Description**: POST is a diagnostic testing sequence run by the BIOS (Basic Input/Output System) firmware when the computer is powered on. Its primary purpose is to verify that the hardware components are functioning correctly before bootstrapping the operating system.
- **Details**:
	- Tests system hardware such as memory, keyboard, and other peripherals.
	- If a problem is detected, it typically results in a series of beep codes or error messages.

### MBR (Master Boot Record)
- **Description**: The MBR is a special type of boot sector at the very beginning of partitioned storage devices like hard drives. It is crucial for the booting process on systems that use the BIOS.
- **Components**:
	- **Bootloader code**: Usually the first 446 bytes. This code is responsible for loading the rest of the bootloader or directly loading the OS.
	- **Partition table**: The next 64 bytes contain the partition table, which defines how the drive is divided into partitions.
	- **Magic number**: The last 2 bytes are a boot signature (0x55AA), indicating the end of the MBR and that the sector is bootable.

### Kernel Initialization
- **Description**: Once the bootloader has loaded the kernel into memory, it transfers control to the kernel, which then initializes the operating system.
- **Details**:
	- **Kernel Initialization Tasks**:
	    - Setting up memory management (e.g., page tables).
	    - Initializing hardware components and drivers.
	    - Starting system services and processes.

### Notes and Clarifications

#### Bootloader runs from ROM (Read-Only Memory)
- The bootloader itself typically runs from RAM (Random Access Memory), but the initial bootstrapping code (such as the BIOS or UEFI firmware) that loads the bootloader resides in ROM or flash memory.
- **Details**: 
	- The BIOS/UEFI firmware, stored in ROM/flash, is responsible for loading the bootloader from the storage device into RAM.

##### Bootloader is completely independent from OS
- **Clarification**: The bootloader is designed to be independent of any specific operating system, but it must be compatible with the OS it intends to load.
- **Details**:
	- The bootloader's primary function is to load and transfer control to the OS kernel.
	- Some bootloaders (like GRUB) support multiple operating systems and provide a user interface to choose between them at startup.

#### Ultimate goal of a bootloader is to boot (start) the OS
- The bootloader's ultimate goal is to prepare the system and load the operating system kernel into memory, then transfer control to it.

### Additional Information

#### Types of Bootloaders
- **Stage 1 Bootloader**: Typically fits within the 512-byte MBR limit. Its main job is to load the Stage 2 bootloader.
- **Stage 2 Bootloader**: More complex, can reside in a separate area of the disk. It often includes a user interface (probably not in case of our project) and can load the OS kernel.

#### Modern Bootloader Examples
- **GRUB (GRand Unified Bootloader)**:
	- Supports a variety of file systems.
	- Can boot multiple operating systems.
	- Provides a command-line interface and configuration files for customization.
- **LILO (Linux Loader)**:
	- An older Linux bootloader.
	- Directly loads Linux kernels.
	- Has largely been replaced by GRUB.

#### UEFI Boot Process
- **Description**: Modern systems use UEFI (Unified Extensible Firmware Interface) instead of BIOS.
- **Differences from BIOS**:
	- Can boot from large disks (over 2TB) with GPT (GUID Partition Table).
	- Supports secure boot, which ensures only trusted OS bootloaders are executed.
	- Provides a more flexible and powerful pre-boot environment.

#### Secure Boot
- **Description**: A feature of UEFI that helps prevent unauthorized code from running during the boot process.
- **Details**:
	- Ensures that only signed bootloaders and kernels from trusted sources are executed.
	- Helps protect against rootkits and other malware that might try to load during boot.

#### Tools and Debugging
- **QEMU**: An open-source emulator and virtualizer that can be used to test and debug bootloaders.
- **GDB**: The GNU Debugger, which can be used in conjunction with QEMU for low-level debugging of bootloader code.