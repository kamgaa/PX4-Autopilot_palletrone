# Building the bundled-source snapshot

This repository tracks third-party sources as ordinary directories, rather than
Git submodules. The historical `.gitmodules` file does not register those
directories as submodules. `cmake/px4_git.cmake` recognizes committed source
directories and preserves their build targets without requiring a nested `.git`.
Actual Git submodules still use the existing initialization and validation path.
Without independent NuttX or MAVLink Git metadata, the version header reports
their sources as `bundled` and their hashes as zero. The NuttX upstream version is
reported as unknown. The parent PX4 commit still identifies this snapshot; no
upstream dependency commit is fabricated.

Build Pixhawk 6C with the PX4 build prerequisites installed:

```sh
make px4_fmu-v6c_default
```

The resulting firmware is
`build/px4_fmu-v6c_default/px4_fmu-v6c_default.px4`.
The Micro XRCE-DDS build downloads Micro-CDR v2.0.1 on its first build, so network
access is needed for that dependency.

The following required files were omitted by inherited ignore rules when the
third-party source trees were imported. They have been restored without changing
the surrounding libraries:

| Restored files | Upstream version | Commit |
| --- | --- | --- |
| `src/modules/mavlink/mavlink/pymavlink/generator/C/include_v2.0/*.h` | [pymavlink v2.4.41](https://github.com/ArduPilot/pymavlink/tree/v2.4.41/generator/C/include_v2.0) | `4d8c4ff274d41b9bc8da1a411cb172d39786e46b` |
| `src/drivers/uavcan/libuavcan/libuavcan/dsdl_compiler/pyuavcan/dronecan/dsdl/*.py` | [pydronecan 1.0.12](https://github.com/dronecan/pydronecan/tree/1.0.12/dronecan/dsdl) | `21eab6f065a7fc61672e6324b65a95d6127bb103` |
| NuttX `libs/libc/bin/Makefile`, `libs/libnx/bin/Makefile`, `mm/bin/Makefile` | [PX4 NuttX snapshot](https://github.com/PX4/NuttX/tree/e1db3983427c983962a5896dae538accd800dddf) | `e1db3983427c983962a5896dae538accd800dddf` |
| Micro XRCE-DDS `src/c/core/log/log.c`, `log_internal.h` | [PX4 Micro XRCE-DDS snapshot](https://github.com/PX4/Micro-XRCE-DDS-Client/tree/2884794316f494d4c001b301e28db01f23288e56) | `2884794316f494d4c001b301e28db01f23288e56` |

The versions match the version declarations in the bundled Python packages.
The upstream NuttX `libs/libc/Makefile` and `mm/Makefile`, and Micro XRCE-DDS
`CMakeLists.txt` and `src/c/core/session/session.c`, match the bundled files.
The files retain their upstream copyright and license notices. The ignore rules
allow these files to be committed so subsequent clones include them.
