# Steps to Force Stop or Kill a Stuck Docker Container:
Identify the Stuck Container: First, list all the running Docker containers to identify which one is stuck:

```sh
docker ps
```
This will show a list of running containers along with their container IDs and names. Take note of the ID or name of the stuck container.

Try to Stop the Stuck Container: Attempt to stop the container gracefully:

```sh
docker stop <container_id_or_name>
```
Replace <container_id_or_name> with the actual container ID or name.

If this doesn't work, proceed to force kill the container.
Force Kill the Stuck Container: If the container doesn’t respond to the stop command, you can forcefully kill it:

```sh
docker kill <container_id_or_name>
```
Check If the Container Is Killed: After killing the container, verify that it has been stopped by listing all running containers again:

```sh
docker ps
```
The container should no longer appear in the list.

Stop the Docker Service: Now that the stuck container has been killed, you can try stopping the entire Docker service again:

```sh
/etc/rc.d/rc.docker stop
```
Check System Resources (Optional): If you notice that containers keep getting stuck, it might be a system resource issue. You can check the current CPU, memory, and disk I/O usage with:

```sh
top
```
This will show you a real-time view of system resource usage and may help you identify if your system is under heavy load.



# Steps to Force a Reboot in Unraid via Terminal:
Access the Terminal:

You can either SSH into your Unraid server or use the local console (keyboard and monitor connected to the Unraid server).
Stop Critical Services (Optional):

If possible, stop services like Docker and VMs before forcing a reboot to avoid potential data corruption. Use these commands:
Stop Docker:

```sh
/etc/rc.d/rc.docker stop
```
Stop Virtual Machines:

```sh
/etc/rc.d/rc.libvirt stop
```
Stop the array (optional):

```sh
/root/mdcmd stop
```
If these services won't stop and the system is too unresponsive, you can skip this step.

Force Reboot: To force a reboot immediately, use the following command:

```sh
reboot
```
This will trigger a system-wide reboot, and Unraid will attempt to safely stop processes. However, if the system is very unresponsive, you may need to issue a more immediate shutdown.

Immediate Forced Reboot: If the above reboot command doesn't work, you can use this command to force a more immediate reboot:

```sh
reboot -f
```
This bypasses the usual shutdown process and forces the system to reboot immediately, similar to pressing the reset button on a physical machine.

Check Status After Reboot: After the reboot, Unraid will likely perform a parity check upon restart, especially if the array wasn’t properly stopped before rebooting. This is a normal safety measure to ensure the integrity of your data.

If Reboot Doesn’t Work:
If even the reboot -f command doesn't work, you can try a more direct hardware-level reboot with:

```sh
echo 1 > /proc/sys/kernel/sysrq
echo b > /proc/sysrq-trigger
```
This will immediately reboot the system without attempting a clean shutdown. Note that this is highly aggressive and should be a last resort.



# Getting RAM Information in Unraid
To install the Nerd Pack plugin:

Go to the Unraid web UI.
Navigate to the "Plugins" tab.
Install the Nerd Pack plugin (if not already installed).
Once installed, go to Nerd Pack settings and ensure that dmidecode is enabled.
Check RAM Information: Once you have access to dmidecode, you can check your RAM frequency and timings using the following commands:

To check memory frequency:

```sh
dmidecode -t memory | grep -i "speed"
```
This will show the current and maximum supported memory speeds for each memory module.

To check detailed memory timings:

```sh
dmidecode --type 17
```
This command will provide detailed information about each memory stick, including size, speed, and other parameters like CAS Latency, tRCD, tRP, and tRAS.

Analyze the output: You will see details for each memory module installed in your system, including important timing information such as CAS Latency (CL), tRCD, tRP, and tRAS, as well as the memory speed (frequency).

Example Output:
You may see something like this:

```sh
Speed: 3200 MHz
Configured Clock Speed: 3200 MHz
CAS Latency (CL): 16
RAS to CAS Delay (tRCD): 18
RAS Precharge (tRP): 18
Cycle Time (tRAS): 36
```
This will give you the information about your RAM's frequency and timings.

Alternative: Use lshw
If dmidecode isn't available, you could try using the lshw tool (which may also be available via Nerd Pack):

```sh
lshw -short -C memory
```
This will provide a high-level overview of your memory configuration.

By following these steps, you should be able to gather information on your RAM's frequency and timings in Unraid.
