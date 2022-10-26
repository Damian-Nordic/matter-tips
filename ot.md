# OpenThread tips and tricks

## Running OpenThread Border Router in the host network...

...to turn a PC into a backbone router.

1. Set up an RCP device as described in [nRF Connect documentation](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/ug_thread_tools.html#id7)
2. `sudo modprobe ip6table_filter`
3. `docker run -it --rm --privileged --name otbr --network host --volume /dev/ttyACM0:/dev/radio -e DNS64=0 -e NAT64=0 openthread/otbr --radio-url spinel+hdlc+uart:///dev/radio?uart-baudrate=1000000 -B eth0`
   - `/dev/ttyACM0` -> path to your RCP device node name
   - `eth0` -> network interface you use to connect to your backbone (e.g. Wi-Fi) network