Once you have completed the hardware and software setup, you can start using the frontend to manage the simulation!

# Connecting Boards to the Dashboard
1. Naviagte to http://localhost:5173 (or the IP address of the computer the dashboard is running on if you would like to control it from a different device).
> Note: You may need to change the port if you already have another application using port 5173
2. Click on the "Connect Device" link
3. Enter a name for the first board
> Note: The names are for your reference only. You can name the boards whatever you like.
4. Connect or power on the first board
> Note: Again, it is easiest to connect one board at a time
5. Click the "Add Device" button

6. A window will pop up asking which serial port you would like to connect. The port will most likely be named "PicoArduino". If you only have one board connected, there will only be one option with this name. Select the port of the board and click "Connect".

![ports](https://github.com/user-attachments/assets/8da7a0cd-84d6-4e01-856f-a2ac5ea711d3)

7. The board is now added. 
> Note: You can click the "Identify" button for any of the connected ports. This will blink the LED 6 times to indicate which board corresponds to that port.

8. Connect and add the remaining boards one at a time. You do not have to disconnect or power off boards once added. Previously added ports will show up as "Paired". When you go to add the next board, select the port that does not say "Paired". Give each board a unique name.

![second](https://github.com/user-attachments/assets/6834f8f7-a007-463c-8f1f-18c3e43fa236)

9. Make sure the CANable adapter is plugged into the computer the dashboard is running on. The adapter only needs to be powered on, it does not need to be added like the CANBed RP2040 boards do.

10. Once you have connected all the boards, navigate to the "Home" tab

11. If you have successfully connected all the boards, you should see a serial console for each on the home page

![consoles](https://github.com/user-attachments/assets/61bb9816-4b9d-4a3a-b50e-990e8f518c1c)


# Uploading CAN Traffic
You can upload a log in the standard [can-utils](https://github.com/linux-can/can-utils) candump format. This will be the CAN traffic that will be played on the testbed. 
1. Click the "Choose File" button and select the file that contains your CAN traffic.
2. The file name will be shown next to the "Choose File" button when the file is uploaded.

![upload_file](https://github.com/user-attachments/assets/b5b564d1-3343-48ab-95f0-c8a259394c0d)

# Setting CAN ID Filters
Each CANBed RP2040 board has 6 hardware filters. This means they can be configured to filter up to 6 individual CAN IDs. You can select the IDs that you want each board to receive. If you do not select any IDs for a board, it will receive all messages. If you know what IDs correspond to a certain component, you can have one of the boards represent that component and only process messages with those IDs. For example, if you knew that the id `0x5B0` corresponded to the Instrument Cluster, you could name one of your boards "Cluster" and set the filter to `0x5B0`.
1. Use the dropdown menu under each console to select IDs to filter on that board. The dropdown will show each ID that was found in the CAN dump
2. Once you have selected the IDs for each board that you want to filter, click the "Upload IDs to ECUs" button. This will set up the hardware filters on each board. It will also display all the messages that each board should receive. When the simulation is started, these messages will be checked off as they are received. 

> Note: You can click "Show Console" to view the serial console for debugging

![filters](https://github.com/user-attachments/assets/cf61dbb2-efb8-49a8-9cf1-048b2c830a4f)

# Run the Simulation
Once you have uploaded the CAN dump to the Sender and uploaded ID filters to the ECUs, you are ready to run the simulation.
1. Click the "Start Simulation" button at the top of the page.
2. After the simulation is complete, a checkmark (✔️) will appear next to each message from the log file that was received. If a message is not received, it will not have a checkmark. If an unexpected message was received (one that was not in the CAN dump or one that didn't match the ID filters), it will have an exclamation (❗) next to it. This can be useful for testing attacks because it allows you to see if the attack successfully injected a message or prevented a message from being received. Also, next to each received message is a time in milliseconds. This is the amount of time from the beginning of the simulation that the message was reported as received by the board.

![received](https://github.com/user-attachments/assets/70226509-ba6d-4b74-bd44-d249fc77538c)

# Ignoring IDs
If you are connecting a real component to the testbed, it will likely transmit CAN messages of its own. This can cause many "unexpected" messages to be displayed as they will not be in the log file that you are replaying. If a component sends messages with certain IDs, you may want to ignore them by following the steps below.
1. Click on the "Ignore IDs" button. Enter the IDs you would like to ignore in hexadecimal format (eg. 5B0)
2. You may want to save presets of IDs to ignore for different components. Enter a name for the set (eg. Instrument Cluster) and save. This set can now be recalled in the future by clicking on the "Instrument Cluster" button.
3. Click "Upload". These IDs will now be ignored by all the boards.

# Testing an Attack (Message Injection)
1. Choose one of the boards to be the "attacker"
2. Click the "Configure Injection" button under that board
3. Set the message injection parameters. These are the message to inject, the number of times to repeat that message, the interval between retransmissions, and the start time. The message is in ID#DATA format, like a message is represented in the candump log. Repeat is the number of times that message will be replayed. The interval is the amount of time in between replays if the repeat is greater than 1. The start time is the amount of time to wait after the first CAN message is received before injecting the message(s).
4. Click the "Configure" button. If you click the "Show Console" button, you should see some output in that board's console confirming the configuration.

![replay_conf](https://github.com/user-attachments/assets/f95402ac-7511-4f0c-86bf-6d3e55aba302)
![replay_console](https://github.com/user-attachments/assets/894e9c90-45ab-42c2-81b0-166fcbac44ae)

6. Restart the simulation and look for the injected messages. They should have exclamations since they are unexpected messages.
![replay_msgs](https://github.com/user-attachments/assets/136764a7-a7cb-4e51-935a-71fbc8c4b889)

# Writing Firmware for Custom Mitigations
The Arduino code can be modified to test different mitigations. The `CANBed.ino` file has a placeholder function called transformData that can be used to transform data before it is processed.
```c
void transformData(unsigned char *transformedData, unsigned char *data, int dataLength)
{
  // Add code here to transform the data before processing
  for (int i = 0; i < dataLength; i++)
  {
    transformedData[i] = data[i];
  }
}
```
As an example, encryption and decryption using the `ChaCha20` algorithm was implemented in the `sender_encryption.ino` and `CANBed_encryption.ino` files in the `rp2040` directory of this repository. You can flash these files using the methods found in the [Software Setup](./Software-Setup.md#step-2-flash-the-boards-with-the-canbench-firmware) portion of the Wiki to try them out.
For example, here is a snippet from the `CANBed_encryption.ino` file showing how the `transformData` function is used to decrypt data before processing it:
```c
void decryptData(unsigned char *decryptedData, unsigned char *data, int dataLength)
{
    chacha.setKey(key, 32);
    chacha.setIV(nonce, 12);
    chacha.decrypt(decryptedData, data, dataLength);
}

void transformData(unsigned char *transformedData, unsigned char *data, int dataLength)
{
    // Add code here to transform the data before processing
    decryptData(transformedData, data, dataLength);
}
```
The `backend.py` file can be modified to transform data before sending. There is a similar `transformData` function. Here is an example from the `backend_encryption.py` file showing how the `transformData` function is used to encrypt data before it is sent:
```python
def transform_data(data):
    """
    Encrypt the data using ChaCha20.
    """
    cipher = ChaCha20.new(key=key, nonce=nonce)
    encrypted_data = cipher.encrypt(data)
    return encrypted_data
```
