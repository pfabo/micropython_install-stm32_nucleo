
# How to install MicroPython on Nucleo32/64 with STM32L4 MCU

## Sources

### MicroPython

    https://micropython.org/
    https://github.com/micropython/micropython
    http://docs.micropython.org/en/latest/
    
### Programmers

    https://github.com/texane/stlink.git
    https://www.st.com/en/development-tools/stm32cubeprog.html
    
### Terminal emulator

    https://en.wikipedia.org/wiki/Minicom
    https://github.com/npat-efault/picocom
    https://gitlab.com/cutecom/cutecom/


## Preparation (Linux)

Install gcc-arm compiler and libraries

    apt-get install build-essential
    apt-get install git cmake unzip
    sudo apt-get install gcc-arm-none-eabi
    sudo apt-get install libstdc++-arm-none-eabi-newlib
    sudo apt-get install libusb-1.0-0-dev

Install stlink programmmer (or use original STM32CubeProgrammer from STM)

    git clone https://github.com/texane/stlink.git
    cd stlink
    make release
    cd build/Release
    sudo make install
    sudo ldconfig
    
Connect Nucleo board to computer and test programmer installation

    st-info --probe
    
Install some terminal emulator (minicom, CuteCom) or use simple picocom 

    sudo apt-get install picocom


## MicroPython compilation

Clone MicroPython source code

    git clone https://github.com/micropython/micropython
    
Compile Python cross compiler

    cd mpy-cross
    make

Go to directory

    cd ../ports/stm32
    
and change line in Makefile from

    GIT_SUBMODULES = lib/lwip lib/mbedtls lib/mynewt-nimble lib/stm32lib

to

    GIT_SUBMODULES = lib/mbedtls lib/mynewt-nimble lib/stm32lib

and run

    make submodules
    
Finally, compile MicroPython for your board. Look in directory ./ports/stm32/boards and find appropriate library, use library name as parameter for make a.e.

    make BOARD=NUCLEO_L476RG
    
Upload firmware to board with st-flash

    cd ./build-NUCLEO_L476RG
    st-flash write firmware.bin 0x8000000 
     

## Usage

Run terminal emulator

    picocom -b 115200 /dev/ttyACM0
    
wait a few seconds, reset your board (Buttom B2) and you get Python prompt

    >>>

look on on-board help

    >>> help()
 
and enjoy Python on STM Nucleo board :-)

    >>> import pyb
    >>> d = pyb.LED(1)
    >>> d.on()
    >>> d.off()
    
Detailed documentation is in directory ./docs


