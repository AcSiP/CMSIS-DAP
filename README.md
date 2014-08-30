CMSIS-DAP Refactoring Efforts
============================
**Goals are:**

- merge in mbed build system tools
- automate testing for validating port contributions and patches
- use mbed exporters for creating project files to debug when needed
- share common source USB, RTOS, IO, etc. 
- Add hooks where custom handlers are needed but generally not break buids when adding new targets and features

Current Status
-------------
**Nothing on this branch is guaranteed to work. Current efforts are with the bootloader and then the interface project**

[List of progress](https://docs.google.com/spreadsheets/d/1zZUFL7tFEOW9WSvjQx4NOZjAowWSAgiHxeV6c4Dt2kw/edit#gid=0) - not all work is incorporated on this branch yet

Bootloader:

* K20DX128: working / work in progress

Interface:

* Not working. Changes to the directory structure and hooks broke these

ToDo
------

* mbed_ser driver installer to not require hardware be connected
* Change USB to expose HID without needing the CDC driver
* Implement XON/XOFF flowcontrol
* Incorporate mbed build system
* Automate basic cross OS tests
* Change application offset address to 0x8000 for all targets with a bootloader
* Only erase sectors that code fits in (when needed)
* Use media eject for MSD
* Verify semi-hosting on HID connection
* RAM allocation for virtual file-system hidden files and folders
* version.txt file for offline, non html-access to the bootloader and CMSIS-DAP version
* Add hex file support (interface only I think). Exists on master but needs verification and testing
* Add srec file support (interface only I think)
* etc...

Tests
------
**A baseline for testing the dap firmware should cover:**

1. program hello world echo application onto target MCU
 * copy using cmd line method
 * copy using drag n drop method
 * Top 3 browsers for each OS
2. flash with pyOCD
 * change demo app with package to be hello world echo

Note: Test should be completed when echo is received so CDC support is inclusive to all tests.

Note: To automate bootloader testing it may be worth putting a sequence in the dap firmware to execute the bootloader (ie: erase vector table at offset and reset)

Directory
--------

* **bootloader** - source files that are only used by the bootloader
* **dap**- source files that are only used by the CMSIS-DAP application
* **flash** - Source files to build position independant flash algorithms. [Adding new flash algorithms](http://keil.com/support/man/docs/ulink2/ulink2_su_newalgorithms.htm)
* **shared** - A software component that is used by __both__ bootloader and interface
* **tools** - python files for building and testing the projects as well as exporting project files for debugging

<pre>
+---source
    +---bootloader
        +---shared
        +---hal
            +---freescale
            +---nxp
    +---openlink
        +---shared
        +---hal
            +---freescale
            +---nxp
    +---common
        +---cmsis_core
            +---freescale
            +---nxp
        +---cmsis_dap
            +---shared
            +---hal
                +---freescale
                +---nxp
        +---flash_algorithms
            +---shared
            +---hal
                +---freescale
                +---nxp
        +---shared
            +---freescale
            +---nxp
        +---rtos
        +---usb
+---tools
    +---build
    +---exporters
    +---test
</pre>

<!---

**Contribution is welcomed on this branch**

Documentation
-------------
* [Porting the FW to new boards](http://mbed.org/handbook/cmsis-dap-interface-firmware)

Community
---------
For discussing the development of the CMSIS-DAP Interface Firmware please join our [mbed-devel mailing list](https://groups.google.com/forum/?fromgroups#!forum/mbed-devel).
-->

Dependencies for tools and exporters
----------------------
* Python 2.7
 * [pyYAML](https://github.com/yaml/pyyaml)
 * [Setuptools](https://pypi.python.org/pypi/distribute)
 * [Jinja2](https://pypi.python.org/pypi/Jinja2)
 
Notes
-----
**All scripts should be run from the projects tools directory**
<pre>
# Create project files from yaml descriptions
tools>python exporters/export.py -f exporters/records/projects.yaml
# Build all projects that were generated
tools>python build.py
</pre>