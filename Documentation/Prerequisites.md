## Prerequisites
* Disassemble your UT. Stop here if you do not want to void your UT warranty or if you are not willing to risk permanent damage.
    * [several](https://www.youtube.com/watch?v=omScudUro3s) [videos](https://youtu.be/iOmdQnIlnRo) [on](https://www.youtube.com/watch?v=yBnOS7V3oS4) [YouTube](https://youtu.be/-v7E7JIrW5Y) demonstrate the process.
* Read the contents of the eMMC chip. Make a backup!
    * The eMMC chip can be read in-circuit by connecting to the CMD, CLK and D0 test points. ![eMMC pinout](./img/emmc_testpoints.jpg)
    * The eMMC expects 1V8 logic levels!
    * Several tools exist to read eMMC chips, a good and cheap ($12) option is the [Low Voltage eMMC Adapter by the exploitee.rs](https://shop.exploitee.rs/shop/p/low-voltage-emmc-adapter).
* You will have to extract, patch and repackage the different boot stages to disable signature verification.
    * The eMMC layout is documented in the [U-Boot GPL release](https://github.com/SpaceExplorationTechnologies/u-boot/releases/tag/sx_2022_05_03) in the file `spacex_catson_boot.h`.
    * More information on extracting the firmware can be found in our [blog post](https://www.esat.kuleuven.be/cosic/blog/dumping-and-extracting-the-spacex-starlink-user-terminal-firmware/).
    * The early boot stages are all based on the [ARM Trusted Firmware-A project](https://trustedfirmware-a.readthedocs.io/en/latest/).
        * You can use TF fiptool to unpack the Firmware Image Packages into the distinct parts.
        * Patching these bootstages can be done using [Ghidra](https://github.com/NationalSecurityAgency/ghidra) after some basic static analysis (use the open source TF-A code to guide your efforts). 
        * Make sure to disable signature verification and to re-enable UART output.
        * Update the firmware hash in the certificates. We glitch signature verification, but the hash still has to be valid.
    * Similarly, you will have to make some changes to the U-Boot image to disable signature verification.
        * You can also increase the `bootdelay` parameter.
    * The Flattened uImage Tree can be unpacked using [dumpimage](https://github.com/u-boot/u-boot/blob/master/tools/dumpimage.c), provided as part of the U-Boot project. 
    	* Make the changes you want and repackage.
* Rewrite the patched firmware to the eMMC chip.