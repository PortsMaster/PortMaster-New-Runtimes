# Godot/FRT 3.7-dev1 for PortMaster (experimental)

This is an experimental ARM64, non-X11 FRT runtime built from the Godot 3.7 development branch. Godot's official release archive identifies 3.7 as a development snapshot, so this runtime is versioned as a prerelease and is not a stable Godot 3.7 release.

The runtime contains no game data. A port must supply its own legally obtained game PCK and use the runtime key `godot_3.7-dev1` through the PortMaster runtime manager.

## Compatibility

- Architecture: AArch64 (`aarch64`), non-X11/FRT devices.
- ELF ABI target: highest direct glibc requirement is `GLIBC_2.25`; libstdc++ and libgcc are statically linked.
- SDL2 is dynamically provided by the device's PortMaster environment.
- On one dArkOSRE R36S (RK3326), the PortMaster squashfs mounted and reported `3.7.dev.custom_build.a117d512b`; the runtime started the lowercase Brotato 3.7 PCK without a pack-version or project-data load error. The 21-second smoke test also logged Brotato-side NativeScript and CrashReporter errors. This confirms startup on one device only, not a clean play session, broad hardware compatibility, or performance.

## Build provenance

- Godot source: commit `a117d512b00f1646db174e703e7e888519b64608` (`version.py` reports 3.7 development).
- FRT: Cebion's Godot 3 platform port, commit `400ec48a4fec049952e535c38d63f28100305ee2`.
- Target sysroot: Debian Buster ARM64 cross sysroot (glibc 2.28), with the matching GCC 8 C++ headers and static C++ runtime.
- The build disables compiler auto-vectorization because the target glibc's AArch64 math library does not provide the newer vector-math symbols emitted by the host compiler.
- The accompanying patch makes Godot's WebM/libvpx build include ARM CPU detection for the FRT ARM64 target.

This is a community proof-of-concept contribution. It has not been tested across PortMaster's full device set or accepted as an official runtime.
