# ESE5190_LAB2_SETUP_JUILEE-KOTNIS
This is a setup guide that will provide assistance to anyone who is trying to execute C code on the RP2040 microcontroller board. There are many ways in which this can be achieved. I have chosen to use Windows Subsystem for Linux and Visual Studio Code. I would first like to describe the machine I am using before I get into the details of the setup.

Laptop: Legion 5 
Operating System: Windows 11
System type: 64-bit operating system
Processor speed: 2.50 GHz
RAM: 8 GB

Now I would like to describe the setup that I have chosen. The Windows Subsystem for Linux basically helps us run a Linux environment with all its functionalities directly on a Windows machine thus saving us from the hassle of operating it via a virtual machine or having a dual boot setup. However, the Windows Subsystem for Linux only acts as a terminal. In order to be able to edit the code that you plan to run on your RP2040 microcontroller board you would need a code editor. For this I chose Visual Studio Code. In order to display the output, I chose Putty to be my serial terminal. I have been using Putty in another course that I am taking this semester which is Digital Integrated Circuits and hence chose to use it due to familiarity. 
Now that I am done explaining my setup, I would like to guide the readers of my document on how to setup everything before moving onto the task. The following steps will guide you in the easy installation of Windows Subsystem for Linux, Visual Studio Code and Putty.
1)	Open the Windows PowerShell and run it as an administrator. 
    •	Type the command wsl --install
    
![image](https://user-images.githubusercontent.com/114092868/195962635-586fe41d-e063-4a24-aec2-4be6188722cc.png)

    •	This will start installing Windows Subsystem for Linux and Ubuntu.
    •	After installation, a command asking you to reboot your system will flash on the PowerShell terminal.
    •	Restart your system.
    •	After restarting your system, the Ubuntu terminal will open automatically on your system.
    •	The Ubuntu terminal will prompt you to set a Linux username and password. 
    •	After this you are done setting up Windows Subsystem for Linux and your terminal will look like this.
    
![image](https://user-images.githubusercontent.com/114092868/195962694-af0d6ccd-5797-4ee4-b419-f2ab1fe5ab0d.png)

2)	Download VS Code from https://code.visualstudio.com/download

    •	After installing VS Code, you need to install certain extensions inside VS Code.
    •	You will an icon in the menu in the left side called Extensions. 
    •	Click on it and search for WSL and C/C++ for Visual Studio Code.
    •	Install both the extensions.
    •	After this you are done with the setup for your Visual Studio Code.
    
3)	Download Putty from https://www.putty.org/ 

    •	Just install Putty and keep as we will get back to its settings later when we are at the final stage.
    •	After this you are done with the setup for Putty.
    
Now that we are done with the installation of everything that we require for our setup, we can now move on to the task. Our task is to install Raspberry Pi Pico Software Development Kit on our machine which provides all the header files, libraries and build system which is essential to write programs for our RP2040 based boards and then compile and flash the output of the hello_usb.c example on our board. An important point to be remembered here is that we are using RP2040 QtPy board whereas the reference we will be using to carry out our task belong to the RP2040 Pico board. Hence, it is vital to keep in mind the differences between the two boards and accordingly implement our code in order to provide the correct output. The reference being used is https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf.

Now let us begin with the task. Follow the steps given below and refer to the images attached in order to keep track of whether you are heading in the right direction.

1)	Open Ubuntu terminal. As we will be using USB while connecting our RP2040 QtPy board to our machine we will need to install certain drivers as Windows Subsystem for Linux will not support USB devices. Download the driver files required by selecting the .msi file from https://github.com/dorssel/usbipd-win/releases. While doing this your system might give you a warning if you would want to proceed with the installation of this file, confirm it and proceed with it.

2)	Now you need to install certain Linux tools. In order to do so type the following commands.

    sudo apt install linux-tools-5.4.0-77-generic hwdata
    sudo update-alternatives --install /usr/local/bin/usbip /usr/lib/linux-tools/5.4.0-77-generic/usbip 20
    
![image](https://user-images.githubusercontent.com/114092868/195962825-1f6a349e-ee28-423f-b9f6-282975098609.png)

3)	Now we will start with the installation of SDK on RP2040 QtPy board. The following commands need to be typed in the Ubuntu terminal. We start by creating a pico directory.

    $ cd ~/
    $ mkdir pico
    $ cd pico
    
![image](https://user-images.githubusercontent.com/114092868/195962839-fae2947e-335b-4f17-b49e-5d1a2631f10a.png)

4)	Now we need to clone the pico-sdk and pico-examples git repositories. The pico-examples repository is basically a set of example applications that can be written onto the RP2040 QtPy board using pico-sdk.

    $ git clone -b master https://github.com/raspberrypi/pico-sdk.git 
    $ cd pico-sdk 
    $ git submodule update --init 
    $ cd .. 
    $ git clone -b master https://github.com/raspberrypi/pico-examples.git
    
![image](https://user-images.githubusercontent.com/114092868/195962891-6085d234-6786-4131-9dd8-11c42c293fbf.png)

5)	In order to build applications in pico-examples we will require some extra tools which are CMake and GNU Embedded Toolchain for Arm. CMake is an open-source, cross-platform tool that uses compiler and platform independent configuration files to generate native build tool files specific to any compiler and platform that you could be using. The GNU Arm Embedded Toolchain is a ready-to-use, open-source suite of tools for C, C++ and assembly programming. This can be done from the Ubuntu terminal itself by using the following commands.

    $ sudo apt update 
    $ sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential libstdc++-arm-none-eabi-newlib
    
![image](https://user-images.githubusercontent.com/114092868/195962904-efe8f2db-99af-46d7-a675-ba6081ea6dc5.png)

6)	This step is to be followed only when a new version of SDK is released in order to update your copy of SDK.

    $ cd pico-sdk 
    $ git pull 
    $ git submodule update
    
7)	Now we can focus on compiling and running our RP2040 code by executing the following commands. After cloning we are basically setting the path PICO_SDK_PATH=~/pico/pico-sdk and prepared cmake file by running the command cmake ..

    $ cd ~/
    $ cd pico/pico-examples
    $ mkdir build 
    $ cd build
    $ export PICO_SDK_PATH=~/pico/pico-sdk
    $ cmake ..
    
![image](https://user-images.githubusercontent.com/114092868/195962944-121411f7-6d56-4ab1-9dd7-0079ab21dc5a.png)

8)	In order to edit the code of the example application we will be using from the pico-examples folder we execute the following commands. This will start building the target hello_usb and might take a few minutes.

    $ cd ~/
    $ cd pico/pico-examples/build/hello_world
    $ make -j4
    
![image](https://user-images.githubusercontent.com/114092868/195962975-ec2e053f-31f0-4781-96b8-ef776b680d0e.png)

![image](https://user-images.githubusercontent.com/114092868/195962981-30520e98-609b-492f-9de4-8a22e74a3753.png)

![image](https://user-images.githubusercontent.com/114092868/195962988-56a3a7c5-0a76-4031-b933-9b24e4a07bd6.png)

9)	After the target has been built it produces a hello_usb.uf2 file which now is stored inside the build folder of pico-examples of Ubuntu folder. We need to retrieve it from there and get it in the downloads folder of our machine. Hence, we execute the following commands.

    $ cd ~/
    $ cd pico/pico-examples/build/hello_world/usb
    $ cp hello_usb.uf2 /mnt/c/Users/Juilee/Downloads
    
![image](https://user-images.githubusercontent.com/114092868/195963006-78f9f19c-a6f4-4b3e-9d07-e4946e903962.png)

![image](https://user-images.githubusercontent.com/114092868/195963014-3503ed8b-ee82-46da-823c-9854b1f127f8.png)

Now, that we are done creating the file that we are going to run our RP2040 QtPy board, we now come to the final stage of execution of our output. In order to do this the following steps need to be executed.

1)	As we had been using the RP2040 QtPy board in the other lab sessions, it has been configured to CIRCUITPY. Hence, when we would drop the hello_usb.uf2 file in it, it will not be able to read from this file. In order to change that we will reset the board. This can be done by following the steps given below:

    •	Plug in the board using USB. 
    •	Hold the BOOT and RST buttons on the board for a few seconds which will reset it.
    •	When you will open your file explorer you will see a new drive by the name RPI-RP2.
    
![image](https://user-images.githubusercontent.com/114092868/195963094-9c9d03ef-ce9a-49af-8571-ce8dc0f12574.png)

2)	Now we need to get back to Putty which we are going to use to display our output. In order to do that we need to execute the following steps:

    •	Open the device manager on your machine to check the COM port assigned to the USB. It will appear as shown in the image.
    
![image](https://user-images.githubusercontent.com/114092868/195963115-a6ec9a96-93d3-4bb6-8e1c-632df17bc512.png)

    •	Now open Putty. Click on Serial and enter the COM Port Number and the baud rate as 115200.
    •	Type a name for your session and save it. I will be using the name Embedded Systems Lab2 Setup.
    
![image](https://user-images.githubusercontent.com/114092868/195963127-b1d013d2-37e0-4c76-a233-0ee0090c92dd.png)

    •	Drag and drop the hello_usb.uf2 file in the RPI-RP2 drive.
    •	Click on the session name you just saved and click on load and open it.
    •	A serial terminal would open flashing the output of your code.
    
![image](https://user-images.githubusercontent.com/114092868/195963140-41e9a475-1046-4090-b9fe-1e7fb29be9c2.png)













    







