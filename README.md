Panda3D CI Build Environments
=============================

This repository contains automation files (for Vagrant, Docker, and Ansible)
to stand up the build images used by Panda3D's official CI system quickly and
automatically.

While the main purpose of this repository is to track our build environment
configuration, users/developers of Panda3D may find this useful as these
environments represent our "known good" baseline configuration (thus helping to
solve the "works on my machine" problem) and the files in this repository
automate the setup of those environments, allowing users to get up and running
quickly with a ready-to-go build/test environment.

Getting started
---------------

You will need the following installed:
- VirtualBox
- Vagrant
- Ansible (at least v2.5.1; latest recommended)

If you are building the Windows environments, you will additionally need:
- `winrm` and `winrm-elevated` Ruby gems
- pywinrm Python module (`pip install --user pywinrm`)

You can start ALL virtual machines with `vagrant up` - however, unless you are
on a system with high available RAM (at least 32GB) this is not recommended.
Instead, just start up the machine(s) you need with
`vagrant up MACHINENAME`. You can then use the machine with
`vagrant ssh MACHINENAME`, power it off with `vagrant halt MACHINENAME`, and
destroy it with `vagrant destroy MACHINENAME`.

Defined machines
----------------

| Name      | Operating System   | Notes                                   |
|-----------|--------------------|-----------------------------------------|
| alpine    | Alpine Linux 3.8   | Linux images will be migrated to Docker |
| fedora    | Fedora Linux 29    | Linux images will be migrated to Docker |
| trusty    | Ubuntu Trusty 64   | Linux images will be migrated to Docker |
| freebsd11 | FreeBSD 11.x       |                                         |
| freebsd12 | FreeBSD 12.x       |                                         |
| win7      | Windows 7 64-bit   | TODO: Upstream image is missing?        |
| winxp     | Windows XP 32-bit  | TODO                                    |
| win10     | Windows 10 64-bit  | TODO                                    |
| macos7    | macOS 10.7 64-bit  | TODO; also, will only work on Apple HW  |
| macos14   | macOS 10.14 64-bit | TODO; also, will only work on Apple HW  |

User accounts
-------------

You can log in with:

| Username | Password | Notes                                               |
|----------|----------|-----------------------------------------------------|
| vagrant  | vagrant  | Administrator account; use this for troubleshooting |
| builder  | builder  | Unprivileged builder account; used by the CI tasks  |

What's installed
================

FreeBSD
-------

The FreeBSD environments include the recommended set of third-party software
for building Panda3D, installed via `pkg` (FreeBSD's package manager). The base
image has a few packages installed as well; overall, this should include:

- Toolchain (Clang compiler, BSD make, etc.)
- CMake 3
- Git
- Python 2.7 & 3.6 (with pytest installed)
- Panda3D third-party dependencies:
  - zlib (part of FreeBSD)
  - OpenSSL (part of FreeBSD)
  - flex (part of FreeBSD)
  - bison
  - libpng
  - libjpeg-turbo
  - libtiff
  - FreeType 2
  - HarfBuzz
  - Eigen 3
  - Squish
  - OpenAL
  - opusfile
  - libvorbis
  - libX11
  - mesa-libs (i.e. OpenGL, GLES 1/2, GLX, ...)
  - ODE
  - Bullet
  - Assimp
  - OpenEXR

Linux
-----

The Linux environments vary, but all of them include:

- Toolchain (sometimes GCC, always Clang, GNU Make, etc.)
- CMake 3
- Git
- Python 2 and 3 (with pytest installed)
- Panda3D core third-party dependencies:
  - zlib
  - OpenSSL (LibreSSL on Alpine)
  - libX11 (and libxrandr, libxxf86dga, libxcursor)
  - Mesa (i.e. OpenGL, ...)
  - bison & flex
- Various optional Panda3D dependencies, but usually:
  - Eigen 3
  - FreeType
  - HarfBuzz
  - OpenAL
  - FFmpeg
  - libvorbis
  - opusfile

Windows
-------

The Windows environments include not just the dependencies for Panda3D, but
also the dependencies for building several of Panda3D's thirdparty packages:

- Windows SDK 8.1
- Cygwin and a few of its packages:
  - OpenSSH
  - Git
  - make
  - patch
  - wget
  - unzip
- CMake (for native Windows; not Cygwin)
- ActivePerl (available at `C:\Perl64\bin\perl.exe`)
- .NET Framework 4.6.1
- Visual Studio 2015 Build Tools (e.g. MSBuild)
- LLVM Clang (driver for Visual Studio - i.e. compiler mimics cl.exe)
- NASM
- GitLab CI Runner
