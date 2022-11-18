# A quick guide on IAR BX
Welcome to this quick guide on how to start using the most recent versions of the IAR Build Tools on Linux.

## Installation
- Ubuntu, Debian, etc.
  - `sudo apt install /path/to/bx<arch>-<version>.deb`
- RHEL, Fedora, etc.
  - `sudo dnf install /path/to/bx<arch>-<version>.rpm`

## Usability
There are at least 2 ways to avoid typing the full path to the tools. Choose the preferred way and append it to `~/.bashrc`.
### Bash `alias`
```bash
alias iccarm=/opt/iarsystems/bxarm/arm/bin/iccarm
alias iasmarm=/opt/iarsystems/bxarm/arm/bin/iasmarm
alias ilinkarm=/opt/iarsystems/bxarm/arm/bin/ilinkarm
alias iarbuild=/opt/iarsystems/bxarm/common/bin/iarbuild
alias lightlicensemanager=/opt/iarsystems/bxarm/common/bin/lightlicensemanager
```
>:bulb: If needed, replace `arm` by the architecture target tag in use.

### `PATH` environment variable
```bash
ARCHBX=(rx rl78 rh850 riscv arm rxfs rl78fs rh850fs riscvfs armfs)
for a in ${ARCHBX[@]}; do
  if [ -d /opt/iarsystems/bx${a} ]; then
    export PATH=/opt/iarsystems/bx${a}/${a}/bin:$PATH
    export PATH=/opt/iarsystems/bx${a}/common/bin:$PATH
  fi
done
```
>:bulb: Aliases take precedence over `$PATH`.


## License setup
- Setup client to use the license server, then test if the compiler passes the license checkpoint
```
lightlicensemanager setup -s 10.0.0.0
iccarm --version
```

## Building
- Create a simple "hello-world.c" using `echo`
```bash
echo -e "#include <stdio.h>\nvoid main() { printf(\"Hello BX world.\"); }" > hello-world.c
```

- Compile and link it
```bash
iccarm hello-world.c -o hello-world.o
ilinkarm hello-world.o --semihosting -o hello-world.elf 
```

## Knowing your compiler
### Shortcut
- Embedded Workbench users: the IDE's "Build Log" window can display "All" the command lines used to build any project (via its context menu).

## Further reading on building...
- with "CMake"
  - https://github.com/iarsystems/cmake-tutorial
- with "CMake on VSCode"
  - https://github.com/felipe-iar/iar-vscode-cmake-on-fedora
