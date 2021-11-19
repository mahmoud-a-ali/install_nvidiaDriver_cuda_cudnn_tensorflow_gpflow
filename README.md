# Install nvidia driver 

- Check the nvidia driver version that is compatable with your GPU: [nvidia-drivers][] 
- Install the driver using the `Install Nvidia Beta Drivers via PPA Repository` section in the [phoenixnap tutorial][]: 

    - Step 1: Add PPA GPU Drivers Repository to the System
    ```bash
    $ sudo add-apt-repository ppa:graphics-drivers/ppa
    ```

    - Step 2: Identify GPU Model and Available Drivers
    ```bash
    $ ubuntu-drivers devices
    ```
    - Step 3: Install Nvidia Driver, select a version  based on  either `the recomended driver from the previous command` or `the one  compatible with the cuda/tensorflow version you want to install`. For example, [tensorflow compatibility table][] with cuda nad cudnn, and [the minimum Required Driver Version for CUDA][]
    ```bash
    sudo apt install [driver_name: e.g. nvidia-driver-460]
    ```
    - Step 4: Restart the System
    ```bash
    $ sudo reboot
    ```
    - Step 5:check which version is installed
    ```bash
    $ nvidia-smi
    ```
    
# Install cuda
- Select which version you want to install based on the [tensorflow compatibility table][] with cuda nad cudnn, and [the minimum Required Driver Version for CUDA][], then download the selected one from [cuda-archive website][]
- On the [cuda-archive website][], select: Operating System `Linux`, Architecture `x86-64`, Distribution `ubuntu`, Version `18.04`, Installer Type `runfile (local)`

- After downloading the `runfile`, follow the base installer commands (here considering `cuda version: 11.2.2`), Note that the driver is already installed on the previous section, so when a message appears to tell you ` "a driver found ... " ` choose `continue` then `accept` the license agreement, then `uncheck` the driver item from the list by pressing `enter`, and finally choose `install` 
```bash
$ wget https://developer.download.nvidia.com/compute/cuda/11.2.2/local_installers/cuda_11.2.2_460.32.03_linux.run
$ sudo sh cuda_11.2.2_460.32.03_linux.run 
```
- After installed, check cuda version:
```bash
$ nvcc –version 
$ whereis cuda 
```
- Add cuda to environment variables, open the ~/.bashrc file and add the following lines
```bash
export CUDA_HOME=/usr/local/cuda
export PATH=/usr/local/cuda/bin:$PATH
export CPATH=/usr/local/cuda/include:$CPATH
export LIBRARY_PATH=/usr/local/cuda/lib64:$LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH
export PATH
```
- To test every thing is working well, compile the cuda examples in the directry `NVIDIA_CUDA-11.2_Samples` 
```bash
$ cd NVIDIA_CUDA-11.2_Samples/
$ make
$ cd 1_Utilities/deviceQuery
$ ./deviceQuery
```

# Install cudnn
- Check which version is compaible with your nvidia-driver/cuda/tensorflow/gpflow on the [developer-nvidia website][] (you have you rregister/login in to be able to download cudnn)
- After downloading the `cudnn*.tgz` file , here we consider `cudnn 8.1` which compatible with `cuda 11.2` , follow the following commands
```bash
$ sudo tar -xzvf cudnn-11.2-linux-x64-v8.1.1.33.tgz
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

# Install TensorFlow TF and TensorFlow Probability TFP 
- Check [tensorflow-install guide][] for dependencies, tensorflow requires python3.7 or higher, so if you are using ubuntu 18.04 with ros-melodic and python2/python3.6, you need to install python3.7 or higher and then create a virtual environment for python3.7 or higher using `python3-venv` or `conda` or any other tool for creating virtual environments. you can check [upgrade python tutorial][] to change the python3 default version.
- check TensorFlow Probability TFP version compatiblitiy: TFP 0.12 requires TF>=2.4, TFP 0.11 requires TF>=2.3, and TFP 0.10 requires TF>=2.2.
- Install python3.7 or higher (ubuntu20 have already python3.8)
- Create virtual environment using python-venv or any other tool 
- Install tensorflow using `pip`, check [tensorflow-install guide][]
```bash
pip install tensorflow  #currently it will install TF==2.7
```
- Install tensorflow probability using `pip`, check [tensorflow-probability install guide][]
```bash
pip install tensorflow-probability  #currently it will install TFP==0.12
```

# Install gpflow 
- For current version gpflow 2.1, the dependencies are: TensorFlow (TF, version ≥ 2.2), TensorFlow Probability (TFP, version ≥ 0.10.1), and Python ≥ 3.6.
- As all dependencies are met, install gpflow following the install section on [gpflow github repository][]
```bash
pip install gpflow  #currently it will install GPflow==2.1
```



[nvidia-drivers]: https://www.nvidia.com/download/index.aspx?lang=en-us
[phoenixnap tutorial]: https://phoenixnap.com/kb/install-nvidia-drivers-ubuntu
[tensorflow compatibility table]: https://www.tensorflow.org/install/source#gpu 
[the minimum Required Driver Version for CUDA]: https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html
[cuda-archive website]: https://developer.nvidia.com/cuda-toolkit-archive
[developer-nvidia website]: https://developer.nvidia.com/rdp/cudnn-archive
[tensorflow-install guide]: https://www.tensorflow.org/install/pip
[tensorflow-probability install guide]: https://www.tensorflow.org/probability/install
[upgrade python tutorial]: https://medium.com/@jeethu.samsani/upgrade-python-3-5-to-3-7-in-ubuntu-a1d4347b6a3
[gpflow github repository]: https://github.com/GPflow/GPflow
