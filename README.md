# My Kernel

## _**Prerequisites**_

- A text editor such as VS Code.
- Docker for creating our build-environment.
- Qemu for emulating our operating system

## _**Build**_

Docker Image : https://hub.docker.com/r/randomdude/gcc-cross-x86_64-elf

Enter build environment:
- Linux or MacOS: `docker run --rm -it -v "$(pwd)":/root/env mykernel-buildenv`
- Windows (CMD): `docker run --rm -it -v "%cd%":/root/env mykernel-buildenv`
- Windows (PowerShell): `docker run --rm -it -v "${pwd}:/root/env" mykernel-buildenv`
  sometimes pwd doesnt work , try PWD in that case

## _**Build for x86:**_
```bash
make build-x86_64
```
While using Qemu, closing it prevents usual errors.
To leave the build environment, type exit.

## **Emulation in QEMU**

To run the kernel we need to type:
```bash
qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso
```

## _**Cleanup**_

Remove the build-evironment image:

```bash
docker rmi mykernel-buildenv -f
```


## _**PageTable structure**_
![image](https://github.com/user-attachments/assets/b97ade92-eb1b-42b6-8db9-9e4fe08fee6d)


## _**Explanation of Key Directories and Files:**_

* **`build/`**: Contains object files generated during the build process.
    * `kernel/main.o`: Object file for the kernel's main functionality.
    * `x86_64/boot/`: Object files related to the bootloader.
* **`buildenv/`**: Contains the docker file
    * `Dockerfile`: Configuration for building a Docker image.
* **`dist/x86_64/`**: Contains the final distributable kernel files.
    * `kernel.bin`: The raw binary of the kernel.
    * `kernel.iso`: An ISO image containing the kernel and bootloader.
* **`src/impl/kernel/`**: Contains the source code for the kernel.
    * `main.c`: The main entry point and core logic of the kernel.
    * `x86_64/boot/`: Assembly files for early boot stages.
        * `header.asm`: Kernel header ( for multiboot compatibility).
        * `main.asm`: Assembly code executed early in the boot process.
        * `main64.asm`: 64-bit assembly code (if applicable).
    * `print.c`: Source code for printing/output functions.
* **`intf/print.h`**: Header file for the printing functions.
* **`targets/x86_64/`**: (Target-specific configurations).
* **`iso/boot/grub/`**: Files related to the GRUB bootloader for creating the ISO image.
    * `grub.cfg`: GRUB configuration file.
* **`.gitignore`**: Specifies intentionally untracked files that Git should ignore.
* **`linked.ld`**: Linker script used to define how the kernel binary is created in memory.
* **`Makefile`**: Contains build rules for compiling the kernel and creating the ISO image.

### For better understanding: File Structure
![image](https://github.com/user-attachments/assets/4a608a51-5f2d-4c27-94ac-74ad6f7cdc97)

