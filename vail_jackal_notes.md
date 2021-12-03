# jackal setup at vail 

## Setup network connection between jackal and workstation PCs, in `.bashrc` files set:
- `ROS_MASTER_URI` should be the same in both jackal and workstation PCs, should represent jackal IP
```bash
export ROS_MASTER_URI=http://jackal_IP_ADDRESS:11311 
```
- either `ROS_HOSTNAME` or `ROS_IP`
```bash
export ROS_HOSTNAME=IP_DDRESS_OF_THE_MACHINE_ITSELF  # workstation_ip for the workstation PC
export ROS_IP=IP_DDRESS_OF_THE_MACHINE_ITSELF        # and jackal_ip for the jackal PC
``` 
- test the network, run `roscore` in jackal PC and try to list and echo topics in workstation PC
- if you can list topics but you can't echo them on workstation then try:
    - disable firewalls
    ```bash
    sudo ufw disable
    ```
    - check host file in `/etc/hosts`
    ```bash
    sudo gedit /etc/hosts 
    ```

## To get jackal ready to move
- turn on the robot motors
- launch `base.launch`
```bash
roslaunch jackal_base base.launch #you can comment the teleop node on this one if you want connect the joystick to jackal PC
```
- if it shows `cant open /dev/ttyACM0 or even /dev/jackal/` which represent the jackal controller board:
    - first list the `/dev/input/` or `/dev/tty*` to check if PC can see the controller
    ```bash
    ls /dev/tty* #check if there is ttyACM0, you can check /dev/input as well
    ```
    - if you can't see ttyACM0, then check the usb connection between jackal PC and controller board
    - if you can see it, give it full access 
    ```bash 
    sudo chmod 777 (= +x) /dev/ttyACM0
    ```

## To control jackal using the joystick, launch the `teleop.launch` file on the workstation      
```bash
roslaunch jackal_control teleop.launch #be sure that you can echo topics in both PCs 
```

## To get the velodyne VLP16 working:
- turn it on 
- run bash scripts that connect jackal PC to velodyne `conn_velodyne.bash`
```bash 
sudo bash conn_velodyne.bash 
```
- run the `loam.launch` file from the `slam` package
```bash
roslaunch slam loam.launch
```
- if you still see `failed to pull device`, try run the bash command like
```bash
sudo bash # which will get you in the root mode 
./conn_velodyne.bash 
```


