ROS Installation Guide (Ubuntu 20.04)
=====================================

First Steps
-----------

If you are using Conda, make sure to deactivate all Conda environments (including the base environment) by running the following command in every new terminal::

    conda deactivate

Alternatively, you can modify your ``~/.bashrc`` file to include these lines::

    # Deactivate Conda's base environment
    conda deactivate

Required Repositories
---------------------

Clone the following repositories and check out the specified branches:

- **Vicon Nodelet:**  
  Repository: `https://github.com/utiasDSL/extras/tree/estimator-integration <https://github.com/utiasDSL/extras/tree/estimator-integration>`_

- **Firmware:**  
  Repository: `https://github.com/utiasDSL/crazyflie-firmware-import/tree/attitude-control-interface <https://github.com/utiasDSL/crazyflie-firmware-import/tree/attitude-control-interface>`_

- **Crazyswarm:**  
  Repository: `https://github.com/utiasDSL/crazyswarm-import/tree/attitude-control-identified-xz <https://github.com/utiasDSL/crazyswarm-import/tree/attitude-control-identified-xz>`_  
  Commit: ``ad2f7ea987f458a504248a1754b124ba39fc2f21``

Additionally, ensure you can install the standard Crazyswarm package by following its installation guide:  
`https://crazyswarm.readthedocs.io/en/latest/installation.html <https://crazyswarm.readthedocs.io/en/latest/installation.html>`_

Export your repository folder before proceeding::

    cd ~/{your repositories folder}
    pwd 
    export REPO_FOLDER=<pwd result from last step>

Crazyswarm Import
-----------------

Clone the repository and set up the environment::

    cd $REPO_FOLDER
    git clone -b dsl-sim2real git@github.com:utiasDSL/crazyswarm-import.git
    cd crazyswarm-import
    git checkout attitude-control-identified-xz
    git submodule update --init --recursive
    export CSW_PYTHON=python3
    sudo apt install -y ros-noetic-tf ros-noetic-tf-conversions ros-noetic-joy
    sudo apt install -y libpcl-dev libusb-1.0-0-dev
    sudo apt install -y swig lib${CSW_PYTHON}-dev ${CSW_PYTHON}-pip
    ${CSW_PYTHON} -m pip install pytest numpy PyYAML scipy
    ${CSW_PYTHON} -m pip install matplotlib empy rospkg pandas

Modify `vicon.cpp` to avoid errors when rigid bodies are lost. Fix missing binary operators by editing `/usr/include/boost/thread/pthread/thread_data.hpp`. Add::

    #undef PTHREAD_STACK_MIN
    #define PTHREAD_STACK_MIN 16384

If ``catkin_tool`` cannot be found, source the installed ROS version and try again::

    source /opt/ros/noetic/setup.bash

Finally, source the Crazyswarm workspace::

    source ros_ws/devel/setup.bash
