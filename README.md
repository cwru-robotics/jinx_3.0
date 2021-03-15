# Jinx 3.0
jinx repositories in ubuntu 20.04 and ros Noetic

# installation 
Refer to https://github.com/cwru-robotics/jinx_3.0/blob/master/DOC/Installation_instructions.md

# Starting Jinx up

Currently there is an issue with the serial liberary. we have to make the serial package every time we log in. To do that
```
cd ~/serial 
make
make install
```
or use the alias:
```
serial_make
```
Then startup jinx launch file:
```
roslaunch launchers start_jinx.launch
```

