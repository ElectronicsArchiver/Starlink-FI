## Mounting the modchip on the Starlink UT PCB

* Make sure that the modchip you assembled works before you try to solder it into place.
* Remove the decoupling capacitors within the red squares, these are normally used to stabilise the SoC core voltage supply.
![Decoupling capacitors](./img/decoupling.jpg)

* Align the assembled modchip on the UT PCB and solder it into place using the castellated holes. Make sure to not make any shorts between the core voltage supply and ground.
   * The rightmost castellated holes are not in the correct position. You can redesign the PCB or simply mask the exposed pads on the UT PCB and connect the castellated holes to a nearby ground pad (see picture).

* Connect the test point marked `UT RST` to the enable pin of the the core voltage regulator.
* Connect the test point marked `12V` to a nearby 12V source on the UT PCB.
* Connect the rightmost jumper pad (below the button) to the eMMC D0 test point.
    * We had to do this because of a firmware update that disabled the UART output.
* Connect the test point marked `1V8` to a nearby 1.8V source on the UT PCB (there is a decoupling capacitor next to the eMMC that is connected to 1V8).

You should be all set now to start glitching, good luck! 
The [Python folder](./src/) contains an example that demonstrates how you can start using the modchip for experimentation.


![Modchip mounted to the UT.](./img/installed_modchip.jpg)
