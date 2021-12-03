# jackal setup at vail 

- setup network connection, in `.bashrc` files set:
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

- to get jackal ready to move
    - turn on the robot motors
    - launch `base.launch`
    ```bash
    roslaunch jackal_base base.launch
    ```

- to control jackal using the joystick, launch the `teleop.launch` file on the workstation      

```bash
roslaunch jackal_control teleop.launch
```


