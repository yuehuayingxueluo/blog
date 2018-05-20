```c
// Send down the packet 
for (i = 0; i < 10; i++)
  if (pcap_sendpacket(fp,
    send_buff,
    send_buff_len
  ) != 0)
  {
    fprintf(stderr, "\nError sending the packet: %s\n", pcap_geterr(fp));
    return -5;
  }
```
