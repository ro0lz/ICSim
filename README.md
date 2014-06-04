Instrument Cluster Simulator for SocketCAN
------------------------------------------

By: OpenGarages <agent.craig@gmail.com>

Compiling
---------
You will need SDL2 and SDL2_Image
you will also need can-utils https://git.gitorious.org/linux-can/can-utils.git
This is mainly just used for the canplayer app to play background can messages

Testing on a virtual CAN interface
----------------------------------
You can run the following commands to setup a virtual can interface

  sudo modprobe can
  sudo modprobe vcan
  sudo ip link add dev vcan0 type vcan
  sudo ip link set up vcan0

If you type ifconfig vcan0 you should see a vcan0 interface. A setup_vcan.sh file has also been provided with this
repo.

Usage
-----
Default operations:

Start the Instrument Cluster (IC) simulator:

  ./icsim vcan0

Then startup the controls

  ./controls vcan0

The hard coded defaults should be in sync and the controls should control the IC.  Ideally use a controller similar to
an XBox controller to interact with the controls interface.  The controls app will generate corrosponding CAN packets
based on the buttons you press.  The IC Sim sniffs the CAN and looks for relevant CAN packets that would change the
display.

CAN Hacking Training Usage
--------------------------
To *safely* train on CAN hacking you can play back a sample recording included in this repo of generic CAN traffic.  This will
create something similar to normal CAN "noise".  Then start the IC Sim with the -r (randomize) switch.

  ./icsim -r vcan0
  Using CAN interface vcan0
  Seed: 1401717026

Now copy the seed number and paste it as the -s (seed) option for the controls.

  ./controls -s 1401717026 vcan0

This will randomize what CAN packets the IC needs and by passing the seed to the controls they will sync.  Randomizing
changes the arbitration IDs as well as the byte position of the packets used.  This will give you experience in hunting down
different types of CAN packets on the CAN Bus.

For the most realistic training you can change the difficulty levels.  Set the difficulty to 2 with the controls:

  ./controls -s 1401717026 -l 2 vcan0

This will add additional randomization to the target packets, simulating other data stored in the same arbitration id.
