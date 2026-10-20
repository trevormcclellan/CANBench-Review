# Software Structure
Two main pieces of software are needed to run the CANBench testbed: the code that runs on the CANBed RP2040 development boards, and the user interface which is a web dashboard. The CANBed RP2040 development boards are programmed through the Arduino IDE. The following instructions are for Debian-Based Linux distributions.

Instructions for automated (easiest) and manual setup are provided below.

# Software Setup
## Step 1: Clone the Repository
Before proceeding with either setup method, clone the project repository:
```bash
git clone XXXXX
cd CAN-Testbed
```

## Step 2: Install the Arduino IDE and Libraries
### Recommended: Automatic Setup (Arduino CLI)
For most users, the easiest way to install dependencies is to use the provided setup script. The `install_deps.sh` is located in the `rp2040` directory. This script will install `arduino-cli` to the user's home directory if it is not found, then install all required libraries, including the custom CANBed RP2040 library from Longan Labs. If you have the Arduino IDE installed, this script should add all of the dependencies there as well, if you would like to use it later. Run the script using the following command:
```bash
./rp2040/install_deps.sh
```

### Alternative: Manual Setup (Arduino IDE)
If you prefer to configure everything manually or are not using a Debian-based system, follow the steps below.

1. Follow the instructions from the CANBed RP2040 Documentation in the "Getting Started with Arduino IDE" section [here](https://docs.longan-labs.cc/1030018/#getting-started-with-arduino-ide) up to and including the "Download the install the library" section.
2. Find the `CANBed.ino` file from the `rp2040` directory of this repository and open it in the Arduino IDE.
3. Click Tools -> Manage Libraries in the top bar of the Arduino IDE.
4. Search for and install the `ArduinoJson` library by Benoit Blanchon.

![arduinojson](https://github.com/user-attachments/assets/399ca040-3419-4641-9d89-6b38b07e533e)

5. Search for and install the `ArduinoQueue` library by Einar Arnason

![arduinoqueue](https://github.com/user-attachments/assets/916d474e-07bc-4ccc-81da-85e482aa9ed5)

6. Search for the `mcp_canbus` library by Longan Labs. You should have installed this library from a zip file in number 1 of this step. If the library is missing, refer to the instructions [here](https://docs.longan-labs.cc/1030018/#download-the-install-the-library).
7. Select Tools -> Board -> Longan RP2040 Boards -> CANBed 2040. If you do not see this option, ensure that you have done number 1 of this step.

![select_board](https://github.com/user-attachments/assets/87b478f0-3e0e-4154-b9dd-7e7c7eea77eb)

## Step 2: Flash the Boards with the CANBench Firmware
### Recommended: Automatic Firmware Flashing (Arduino CLI)
The easiest way to flash all the boards is to use the `flash_boards.sh` script in the `rp2040` directory of this repository. This script will compile the provided sketch and flash all connected CANBed RP2040 boards. To install the base CANBench firmware, use the following command:
```bash
./rp2040/flash_boards.sh ./rp2040/CANBed/CANBed.ino
```
The script will instruct you to turn on all boards you want to flash and then hit Enter to continue.
> Note: If you want to update the firmware later, make sure no other applications (including the web dashboard) are using the serial ports before running the script. If you already have the web dashboard running, you will need to close the tab.

### Alternative: Manual Firmware Flashing (Arduino IDE)
1. Make sure the board is set to CANBed 2040 (See Step 2: Install the Arduino IDE and Libraries, instruction #6)
2. Select the port of the board you are flashing
> Note: It may be helpful to connect one board at a time to ensure that you have selected the correct port.

> Note: Make sure no other program is using the Serial Port (including the web dashboard) or the upload will fail. If you are using the web dashboard, close the tab before reflashing.

![select_port](https://github.com/user-attachments/assets/73c5d4a7-4bb9-4e5f-8d3e-9409418527ed)


4. Click the "Upload" arrow in the top bar
> Note: It will take a few seconds for the code to compile and upload. You should see the blue LED on the CANBed board flash 6 times when it has booted.

![upload](https://github.com/user-attachments/assets/92196fb1-fa42-4d33-adcd-3131a043d8a6)

5. Disconnect or power off the board and connect the next board
6. Change the port to the next board's port
7. Repeat instructions 4-6 for the rest of the boards

## Step 3: Set up the Web Backend
1. Install python3 if you don't already have it installed
2. Open a terminal and change directory to the cloned `CAN-Testbed` repository
3. Change directory to `Backend` (`cd Backend`)
4. Run `pip install -r requirements.txt` to install the required packages
5. Run `sudo python3 backend.py`
> Note: It is necessary to run the backend with `sudo` to give the application permission to configure the CANable adapter.

![image](https://github.com/user-attachments/assets/cba67b0b-a691-47a8-8f51-6860d462cc08)

## Step 4: Set up the Web Dashboard
1. Install Node.js and npm if you don't already have them installed. Follow the instructions [here](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).
2. Open a terminal and change directory to the cloned `CAN-Testbed` repository
3. Change directory to `CAN-Frontend` (`cd CAN-Frontend`)
4. Run `npm install` to install all the required packages
5. Run `npm run dev` to start the web dashboard
6. The dashboard can now be accessed at http://localhost:5173/

![frontend](https://github.com/user-attachments/assets/82c779c6-74f3-4780-beba-a2de7b91721b)

