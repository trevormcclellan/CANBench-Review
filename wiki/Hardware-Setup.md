All the hardware used in the CANBench testbed is commercial off-the-shelf hardware, designed to be as easy to reproduce as possible. To simulate ECUs (nodes in the automotive CAN network), the Longan Labs CANBed RP2040 was selected due to its low cost and easy setup. In the development of the CANBench testbed, 9 CANBed RP2040 boards were used. This number can be scaled up or down as needed. A complete list of components used is provided below:
# Hardware used
* (1x) CANable adapter ([Amazon](https://a.co/d/2MDXfTZ)) ([openlightlabs](https://openlightlabs.com/))
* (9x) Longan Labs RP2040 Development Boards ([Mouser](https://www.mouser.com/ProductDetail/Seeed-Studio/102991596?qs=By6Nw2ByBD2uf0clu8HOVA%3D%3D)) ([DigiKey](https://www.digikey.com/en/products/detail/seeed-technology-co-ltd/102991596/15976345)) ([Seeed Studio](https://www.seeedstudio.com/CANBed-RP2040-CAN-Bus-development-board-p-5262.html))
* (9x) Micro USB Cables ([Amazon](https://a.co/d/fpgBKCo))
* (1x) 10 Port Powered USB Hub ([Amazon](https://a.co/d/9VR5Ynk))
* CAN Bus Wire
## Notes
The USB cables and hub that were used in the development of this testbed are linked above. It is not required to use these exact products, however, there are a few things to consider when selecting these products:
1. Make sure to use Micro USB cables that support data transfer and are not "charging only" cables. The testbed uses the USB cables both to power the boards and to communicate with them over serial.
2. Make sure to use a USB hub that is powered and has an external power supply. The USB port on a computer may not be able to power all 10 boards at the same time. A USB hub with individual power switches is not required but can be useful when flashing or connecting boards to the dashboard.
3. CAN Bus generally uses twisted pair wires for the CAN High and CAN Low Wires. Dedicated CAN wire can be purchased. In the development of the testbed, a twisted pair from a CAT6 ethernet cable was used. This may not be the ideal wire to use, but it is easy to find and works.
4. The CANable hardware and firmware is open-source and can be purchased from many sources. It does not need to be the exact board that was used here.
5. There are two main firmware options written for CANable hardware, `slcan` and `candleLight`. This project uses the `candleLight` firmware.

# Preparing the Boards
The CANBed Development boards come with several components that can be soldered on, including a D-Sub Connector, a 4-pin screw terminal, headers for the GPIO and SPI pins, connectors for the I2C and UART pins, and a switch for the 120Ω terminal resistor. The only component that is required for all 9 boards is the 4-pin screw terminal. At least one of the boards will need the switch for the 120Ω terminal resistor (this will be the last board in the loop). If you would like to use the GPIO pins to connect to external sensors or components, you may want to attach the GPIO headers.
## Attaching the 4-pin Screw Terminal
1. Slide the screw terminal into the 4 holes on the edge of the board as shown. 
2. Solder all 4 pins to the board. Do this for all 9 boards. 
> Note: These will be used to connect all the boards to the CAN bus.

<img src=https://github.com/user-attachments/assets/0286f878-da9f-4744-9c43-889a1431ba04 width="500">

## Attaching the 120Ω Terminal Resistor Switch
> You only need to do this on one board.
1. Slide the switch into the 6 holes in the corner of the board as shown. 
2. Solder all 6 pins to the board. 
3. Slide the switch toward the edge of the board to enable the 120Ω terminal resistor. 
> Note: CAN bus uses a 120Ω terminal resistor at each end to ensure signal integrity and proper operation of the network.

<img src=https://github.com/user-attachments/assets/76344d4e-d9bb-4742-bb46-79aa616eeea0 width="500">

# Connecting the Boards

1. Open the screw terminals marked CANH and CANL on each board. These are the CAN High and CAN Low connections. 
2. Connect all 9 boards in a chain, ensuring that the board with the switch is the last in the chain. 
> Note: It is best to use two different colors of wire for the CAN High and CAN Low connections to ensure that they are not mixed up. The standard is to use yellow for CAN High and green for CAN Low. 
3. Make sure all connections are tight.
4. Connect a Micro USB cable from each board to the USB hub
5. Plug in the USB Hub's power adapter
6. Connect the USB Hub's USB cable to your computer
<img src=https://github.com/user-attachments/assets/f0e497c2-523d-4288-9981-c91a5bf66bf0 width="700">

# Connecting the CANable Adapter
1. Open the screw terminals marked CANH and CANL on the CANable adapter and the first board in the chain.
2. Connect the CANable CANH and CANL terminals to the board's CANH and CANL terminals.
3. Make sure the connection is tight.
4. Plug the CANable adapter into the USB hub
5. Push the "R120" switch down to enable the 120Ω resistor
> Note: It might be more convenient to use a USB extension cable to connect the CANable adapter to the USB hub
<img src=https://github.com/user-attachments/assets/370deb77-69d2-47f3-89ad-0fc5d1245b95 width="500">


# (Optional) Board Holder
You can create a holder to neatly arrange all the boards. A 9-board holder was designed and 3D Printed for the development of the testbed. This holder can be downloaded [here](../CANBench_holder.stl).