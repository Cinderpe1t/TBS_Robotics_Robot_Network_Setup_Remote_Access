# TBS Robotics Remote Access to Robot
TBS Robotics Remote Access to Robot
## Fix remote host's IP address
- Usually a computer joins network through wireless and DHCP (Dynamic Host Configuration Protocol) from wireless access point and router.
- While mostly fixed, the IP address of the remote computer can change by dynamic IP address assignment
- Need to fix IP address of the remote computer to have reliable network access to the robot
- Wireless access point and router allows fixed or static IP address assignment as an exception
### Figure out remote computer's hardware ID
- First need to know WiFi MAC (Media Access Control) address of the robot controller
- MAC address is assigned by vendor with vendor ID and unique serial number
- Need locally connected monitor, keyboard, and mouse
- After login, run `ifconfig`
```
(base) Apex-penguin:~ akim$ ifconfig
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
	options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
	inet 127.0.0.1 netmask 0xff000000 
	inet6 ::1 prefixlen 128 
	inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1 
	nd6 options=201<PERFORMNUD,DAD>

...

en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=6463<RXCSUM,TXCSUM,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	ether 3c:22:fb:ab:76:a5 
	inet6 fe80::cc:2243:8473:927%en0 prefixlen 64 secured scopeid 0x7 
	inet 192.168.1.6 netmask 0xffffff00 broadcast 192.168.1.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
```
- Six group of two-digit hexadecimal numbers are MAC address
- From `ether 3c:22:fb:ab:76:a5`, `3c:22:fb:ab:76:a5` is MAC address
### Set up wireless access point
- Log in to router's set up web page
- Browse to xxx page
- Enter MAC address and desired IP address
- Router needs reboot for the static IP address to be effective
- Remote computer needs reboot to rejoin the wireless network
## Graphic User Interface (GUI) remote access
- Virtual Network Computing (VNC) allows remote access through a windows
- Similar to Windows Remote Desktop or Apple Remote Desktop
- It mirrors display at the remote computer
- The remote computer has only one screen. The local monitor and remote screen show the exact same screen.
- If another remote session is requested, the existing remote session can be disconnected.
- Disconnecting does not affect remote computer's power. The remote computer is still running. Only the screen connection is terminated.
### Enable VNC server
- NVIDIA documentation https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup
- Raspberry Pi documentation
  - https://raspberrypi-guide.github.io/networking/connecting-via-VNC
  - https://learn.sparkfun.com/tutorials/how-to-use-remote-desktop-on-the-raspberry-pi-with-vnc/enable-vnc
  - https://magpi.raspberrypi.com/articles/vnc-raspberry-pi
### Download VNC viewer
- RealVNC viewer - https://www.realvnc.com/en/connect/download/viewer/
### Connect to VNC server
- Type the IP address at VNC viewer
## Text or X-Window-based remote accress
- Utilizes `telnet` or `ssh` (Secure Shell)
- `ssh` is preferred for security. `telnet` communication is plainly viewable from other observing computers
- `ssh -X` supports X-Windows forwarding from remote computer
- It allows graphical applications running at remote shows up as if local application
- It is a very convenient feature, but need to be aware of the applications and the running hosts to avoid confusion. For example the same application might be running at host and remote, and they look very similar by looks.
- `ssh` logs into remote as a session. Multiple sessions can be open. 
- Each session has independent window. If you lose a session by closing it, you lose it permanently. Repeated log in will give another session.
### Log in to remote with `ssh`
- Need to know the remote host's IP address in advance
- Need to specify user ID to login
```
ssh -X userID@remote_host_IP_address
```
- After password input, the terminal windows become remote host's terminal until `exit` or close the window
- Try to run a GUI app
```
gedit &
```
- `&` allows multiple applications run in the background 
- `ps` will show the background process running, including `ps` itself and shell
### File transfer with `ftp` or `sftp`
- File Transfer Protocol (FTP) allows file exchange with remote host
```
sftp userID@remote_host_IP_address
```
- Login
- Try `get`, `sget`, `put`, `sput` to exchange files
