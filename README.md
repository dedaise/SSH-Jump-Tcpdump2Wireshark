# SSH-Jump-Tcpdump2Wireshark
Understanding the SSH Command with -A, -J flags and Wireshark Integration 
 
   
# Use-Case: 

 This command is particularly useful for network diagnostics in environments where direct access to a device is restricted, but you can access it via an intermediate or "jump" host.  By forwarding the SSH agent, you leverage your local machine's credentials to authenticate to the target device. The immediate analysis in Wireshark allows you to visually inspect and troubleshoot the network traffic without having to save, transfer, and then open a pcap file. 
 
# Objective: 
 To explain the functionality and use-case of the command with Arista Switches: 
 
# Example: 

ssh -A -J root@10.1.110.200 admin@10.1.110.99 "bash sudo tcpdump -s 0 -Un -w - -i mirror0" | wireshark -k -i - 

![Example Image](https://github.com/dedaise/SSH-Jump-Tcpdump2Wireshark/blob/main/Screen%20Shot%202023-08-28%20at%209.56.28%20AM.png)

![Example Image]([https://github.com/dedaise/SSH-Jump-Tcpdump2Wireshark/blob/main/Screen%20Shot%202023-08-28%20at%209.56.28%20AM.png](https://github.com/dedaise/SSH-Jump-Tcpdump2Wireshark/blob/main/Screen%20Shot%202023-08-28%20at%209.56.54%20AM.png))

 
# Breakdown of the Command: 
  
ssh -A -J root@10.1.110.200 admin@10.1.110.99: This is an SSH command with two flags and two targeted addresses. 

 -A: Enables forwarding of the authentication agent connection. This is useful when you want to use SSH keys from the originating machine on a remote machine without copying them. 

 -J root@10.1.110.200: Specifies a "jump host" or an intermediate machine. The SSH connection first goes to `root@10.1.110.200`, then from there, it connects to the final destination `admin@10.1.110.99`. 
  
bash sudo tcpdump -s 0 -Un -w - -i mirror0: Once connected to the final destination (`admin@10.1.110.99`), this command is executed. 

tcpdump -s 0 -Un -w - -i mirror0: This is a command to capture packets.  
 -s 0: Capture the whole packet. 

 -U: Packet output is flushed immediately. 
 
 -n: Don't resolve hostnames (faster capture). 
 
 -w -: Write the output (packets) to standard output (`stdout`). 
 
 -i mirror0: Capture packets on the `mirror0` interface. 
 
 -s 0: Capture the whole packet. 
  
| wireshark -k -i -: This takes the output of the previous command (packet data) and provides it as input to Wireshark. 

 -k: Start capturing immediately 

 -i -: Read from standard input (`stdin`), which means it'll read the packet data being piped from the previous command. 
   
# Summary: 
  
This command captures network packets from a remote machine (`admin@10.1.110.99`) using `tcpdump`. However, to reach that machine, it first connects to another machine (`root@10.1.110.200`). The captured packets are then directly streamed to Wireshark on the local machine for real-time analysis. 

(https://github.com/dedaise/SSH-Jump-Tcpdump2Wireshark/blob/main/Screen%20Shot%202023-08-28%20at%209.56.28%20AM.png)

! [Wireshark] (https://github.com/dedaise/SSH-Jump-Tcpdump2Wireshark/blob/main/Screen%20Shot%202023-08-28%20at%209.56.54%20AM.png)
