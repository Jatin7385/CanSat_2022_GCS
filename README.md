# Team Sammard's Official GCS Software for CanSat 2022  

<p align="center">
  <a href="https://teamsammard.com/#/" target="_blank" rel="noopener noreferrer">
    <img width="500" alt="Team Sammard Icon" src="https://res.cloudinary.com/dmukzswrc/image/upload/v1643113435/maxresdefault_kfcyty.jpg">
  </a>
</p> 

## Introduction to CanSat
The CanSat competition is a design-build-fly competition that provides teams with an
opportunity to experience the design life-cycle of an aerospace system. The CanSat
competition is designed to reflect a typical aerospace program on a small scale and includes
all aspects of an aerospace program from the preliminary design review to post mission
review. The mission and its requirements are designed to reflect various aspects of real world
missions including telemetry requirements, communications, and autonomous operations.
Each team is scored throughout the competition on real-world deliverables such as
schedules, design review presentations, and demonstration flights.

<p align="center">
  <a href="https://www.cansatcompetition.com/" target="_blank" rel="noopener noreferrer">
    <img width="300" alt="CanSat Icon" src="https://res.cloudinary.com/dmukzswrc/image/upload/v1656154216/CanSat_Logo_-_Color_w1y2ph.png">
  </a>
</p> 

## Mission Overview
Design a Cansat that shall consist of a container and a payload. The payload shall be
attached to the container by a 10 meter long tether. The Cansat shall be launched to an
altitude ranging from 670 meters to 725 meters above the launch site and deployed near
apogee (peak altitude). Orientation of deployment is not controlled and is very violent with
large shock forces. The Cansat must survive the forces incurred at launch and deployment.
Once the Cansat is deployed from the rocket, the Cansat shall descend using a parachute at
a rate of 15 m/s. At 400 meters, the Cansat shall deploy a larger parachute to reduce the
descent rate to 5 m/s. At 300 meters, the Cansat shall release a tethered payload to a
distance of 10 meters in 20 seconds. During that time, the payload shall maintain the
orientation of a video camera pointing in the south direction. The video camera shall be
pointed 45 degrees downward to assure terrain is in the video.
Bonus: As the container is releasing the payload, the container shall contain a video camera
and start recording to show the descent of the payload. All videos are to be recorded and
recovered when the Cansat is recovered from the field.

## CANSAT GCS Software
The main objectives of CANSAT GUI were :- 
- Receive real time telemetry from the onboard flight computer
- Plot and display the telemetry on the GUI
- Generate Csv files that contained the telemetry data packets segregated into 2 .csv files Flight_<TEAM_ID>_C.csv and Flight_<TEAM_ID>_T.csv
- Send commands from GCS Software to the Flight Computer to perform certain tasks during the flight such as Start Telemetry, Stop Telemetry, Start Simulation Mode, Stop Simulation Mode, Set UTC Time and Calibrate Sensors.

### <ins> GCS Overview </ins>
![GCS_OVERVIEW](https://user-images.githubusercontent.com/73430464/175769037-256f1248-f7df-481f-b907-011ded05b519.jpg)

![GCS_DESIGN](https://user-images.githubusercontent.com/73430464/175769040-61bd332c-d745-4d4a-abc0-c5ee64fc25f9.jpg)

### <ins> Telemetry Packet </ins>
1) #### The Container telemetry packet format to be transmitted at a rate of one (1) Hz is as follows:
TEAM_ID, MISSION_TIME, PACKET_COUNT, PACKET_TYPE, MODE, TP_RELEASED, ALTITUDE, TEMP, VOLTAGE, GPS_TIME, GPS_LATITUDE,GPS_LONGITUDE, GPS_ALTITUDE, GPS_SATS, SOFTWARE_STATE, CMD_ECHO

2) #### The Tethered Payload relay telemetry packet format to be transmitted at a rate of four (4) Hz is defined as follows:
TEAM_ID, MISSION_TIME, PACKET_COUNT, PACKET_TYPE, TP_ALTITUDE, TP_TEMP, TP_VOLTAGE, GYRO_R, GYRO_P, GYRO_Y, ACCEL_P, ACCEL_P, ACCEL_Y, MAG_R, MAG_P, MAG_Y, POINTING_ERROR, TP_SOFTWARE_STATE

### <ins> GCS Software Design </ins>
1) #### Commercial off the shelf (COTS) software packages used​
  - ##### Software packages
    - Development environment - Visual Studio Code, QT Designer.
    - Programming Suite - PyQt5(framework), uses Python.
  - ##### Tools
    - Matplotlib(library) for Graphs.
    - OpenStreetView Map was used using pyqtlet(library) for displaying map in real time.
    - csv(library) used for .csv file generation
    - time(library) used for delays for keeping track of the frequency provided in the mission guide
    - pyserial(library) used for for the GUI to receive telemetry and send commands using the serial port
    - paho-mqtt(library) used for MQTT Protocol integration
    - datetime(library) used for UTC time setting

2) #### Real-time plotting software design and media presentation
  - The GCS contains 2 tabs, “Container” and “Payload”, each containing two pages, “Charts” and “Tables” respectively.
  - The “Charts” page, for container as well as payload, allows us to view the graphs together in real time.
  - The “Tables” page, for container as well as payload, allows us to view the data packets that is being received in real time.
  - Real time plotting is being conducted using QThreads(Multithreading).
  - Once the data packet is received, it is processed and then saved in the csv file. 
  - Then it is read from the CSV file and plotted on the graphs. Then the packet is displayed in the table.

3) #### Command software and interface​
- ##### Calibrate :- 
  - To command the container to calibrate sensors. The GCS will send a calibration command to the container, after which the container flight software will calibrate it’s sensors. The verification of the calibration will happen once the first data packet arrives.
- ##### Set Time :- 
  - Set UTC time in container .
- ##### Start/Stop Telemetry :- 
  - To turn container transmission on and off. Start telemetry also begins sending data packets to the server using MQTT
- ##### Simulation mode buttons
  - ###### Enable - Enables simulation mode.
  - ###### Activate - Activates simulation mode.
  - ###### Disable - Disables and Deactivates simulation mode.

4) #### CSV file creation
- Telemetry data is plotted and displayed in real time in engineering units.
- ##### CSV file creation :-  
  - The two .csv files(Flight_1007_C.csv & Flight_1007_T.csv) are generated, after receiving all the data packets. A function is written, which writes the data into the two CSV file.

5) #### Simulation mode description
- Once the ‘Enable’ and ‘Activate’ buttons are clicked, the GCS reads barometric pressure data from a .csv file that simulates the mission profile and transmit the values via SIMP commands to the container at a rate of 1Hz. 
- To stop the Simulation mode, the user needs to click on the ‘Disable’ button to disable and deactivate the simulation mode

### <ins> GCS Software Prototypes </ins>
1) #### Container Charts Page
   ![Container_Charts](https://user-images.githubusercontent.com/73430464/175768847-a03a6573-5bc6-4027-87d7-66df24da6de4.jpeg)
2) #### Container Tables Page
   ![Container_Tables](https://user-images.githubusercontent.com/73430464/175768867-39f8c868-d431-4953-883c-4536e68021e8.jpeg)
3) #### Payload Charts Page
   ![Payload_Charts](https://user-images.githubusercontent.com/73430464/175768884-93d71ab0-322d-4ea2-ae1f-fe0926cbeb98.jpeg)
4) #### Payload Tables Page
   ![Payload_Tables](https://user-images.githubusercontent.com/73430464/175768910-f6fed1f5-72e0-4ecd-bf3b-7d3552ebd32a.jpeg)


### <ins> MQTT Protocol Integration </ins>
#### Submission of telemetry packets Using MQTT 
  - The GUI will collect the telemetry and save into two .csv files namely Flight_1007_C.csv & Flight_1007_T.csv. Both the .csv files will be read in real-time by a python script and the data will be sent to the given MQTT server.
  - ##### Language Used : 
    - Python
  - ##### Libraries Used : 
    - PAHO-MQTT
  - ##### Working
    - ![MQTT_Working](https://user-images.githubusercontent.com/73430464/175769239-280ba032-4298-43a5-aa1d-0f01dc9b599a.jpg)
  - MQTT Protocol Testing done at [http://cansat.info/plot.html](http://cansat.info/plot.html)
  
  
## Testing of the software
An entire testing procedure was created, which was implemented thoroughly. From isolated Software testing, to testing with individual hardware components, to integrated tests with hardware and other subsystems was performed.

### Testing procedure 
  This testing procedure, gives an overview of the types of tests performed on the Software. Many more tests were performed on the software, but mentioning each of the tests performed is not feasible.
  - Testing the software with predefined .csv files containing data and flight profile, for the software to extract this data at the required frequency and performing the required functions.
  - .CSV File generation tests
  - Testing the software with an Arduino Script that simulates data transmission from the Flight Software of the container and performing the required functions. This script can be found in the Host Connect Disconnect Directory.
  - Testing the software with just XBEE's
  - Testing the software with XBEE's + Sensors on breadboard.
  - Testing the software with PCB

## Installation Guide
- Clone the repository in a particular directory
- Run the following command in the particular directory : pip install -r requirements.txt
- Run cansat_show.py file
