# EPICS IOCs
IOCs are the server component of a typical EPICS-based control system. To create an IOC, we will use the `makeBaseApp.pl` script which is part of EPICS base installation. `makeBaseApp.pl` is a perl script that creates EPICS application of many types:
* `caClient`: Example CA client program in C.
* `caPerl`: Example CA client program in Perl.
* `example`: Basic IOC with some example records.
* `ioc`: Basic IOC with no databases.
* support: Standard IOC support module.

These are passed to the `-t` option to the script. To begin, choose a `top` directory, which is where the IOC will be located, and execute the following:
```bash
<base>/bin/<arch>/makeBaseApp.pl -t ioc example
<base>/bin/<arch>/makeBaseApp.pl -i -t ioc example
```
Where:
* `<base>`: EPICS base location
* `<arch>`: Your host's architecture.

## Inspecting the IOC
We will go through various parts of the IOC, both before and after the build.

### EPICS Directory Tree
* `configure/`: Various configuration files used by the EPICS build system.
* `configure/RELEASE`: All needed support modules are defined here.
* `exampleApp/`: IOC databases and sources. Separate folder per IOC.
* `exampleApp/Db/`: This where databases are added.
* `exampleApp/src/`: Additional source codes.
* `iocBoot/iocexample/st.cmd`: IOC startup file, this will run the IOC. Separate folder per IOC.

### EPICS Build Output
To build an IOC, run `make` in the top directory. Once finished, the following tree is created depending on the application type from `makeBaseApp.pl`:
* `bin/<arch>/`: Contains the IOC executable.
* `db/`: Containes coompiled databases during `make`
* `dbd/`: Database definitions, files containing declaration to what IOC component are available during runtime.
* `lib/<arch>/`: Shared libraries when creating a support module.
* `include/`: Header files generated from compiling support modules.
