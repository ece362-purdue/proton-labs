# Lab 8
## An Introduction to RISC-V Assembly

### Table of Contents
<br>

| Step | Description                           | Points |
|------|---------------------------------------|--------|
| 0    | Setting up Docker and VScode          |        |
| 1    | The flow from C to ASM                |        |
| 2    | Arithmetic Instructions               |        |
| 3    | Bitwise/logical Instructions          |        |
| 4    | Memory Instructions                   |        |
| &nbsp; | Total: | 100 |
<br>

> ![NOTE]
> The RISC-V labs will not have checkoffs.  The only requirement for your lab is that you submit your work to Gradescope in the code repository you opened from Piazza.

### Instructional Objectives

- To understand the basics of RISC-V assembly language and its instruction set.
- To learn how to translate C code into RISC-V assembly code.
- To gain hands-on experience with basic non-branching RISC-V assembly programming.

### Step 0.1: Setting up Docker and VScode

For the assembly labs, we need to have compilers that can produce assembly and machine code specific to the RISC-V architecture.  This is fairly easy to obtain while you are on Linux or macOS (with some work on the latter), but to ensure that everyone has a consistent environment, we're going to have you use a Docker container to run the tools.

Docker is essentially a very stripped down version of a virtual machine, an application that allows you to run different operating systems within the active operating system.  Examples of such virtual machines are Windows Subsystem For Linux (WSL) which is how you run a Linux distribution within Windows, or third-party programs like VMware, Virtualbox, or Parallels for Mac.

In Spring 2026, we switched to Docker, away from the `eceprog` computing cluster, for the following reasons:
- Docker allows you to run the compilers and other tools needed directly on your own machine.  This eliminates the need to rely on a remote server that could become inaccessible.  
- The way we implemented the Docker infrastructure was done through VScode using a feature called Dev Containers.  This allows you to do the assembly labs in two ways:
    - You can clone the lab repository and open it in a Dev Container on your own machine.  This requires that you install Docker alongside VScode, but do not fret - you do not have to do much configuration beyond the initial install.  This is the preferred method for you to do your labs.
    - You can start a GitHub Codespace for your repository that lets you run VScode entirely within your browser, with the tools running on a cloud VM on GitHub servers.  While this offers the convenience of a one-click solution, it is prone to the same problems of latency and potential disconnection that using eceprog faces.



<!-- Read https://github.com/riscv-non-isa/riscv-asm-manual/releases/download/v0.0.1/riscv-asm.pdf -->