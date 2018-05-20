```c
//构造ARP报文
//协议类型为0806所定义的类型值

eth_header.type = htons(0x0806); 
//ARP报文内容
arp_data.macType = htons(0x0001);
arp_data.ipType = htons(0x0800);
arp_data.macLongh = 0x6;
printf("请输入协议地址长度：");
scanf("%x",&arp_data.ipLongh);
arp_data.mode = htons(0x0001);

for(int j=0;j<6;j++){
	int k=0;
	u8_t mac=sour_mac[k]*16;
	mac +=sour_mac[k+1];
	arp_data.sourarp_address.addr[j] = mac;
	k+=2;
}

arp_data.destarp_ipaddress.ipaddr = inet_addr(destIP);

arp_data.destarp_address.addr[0] = 0xFF;
arp_data.destarp_address.addr[1] = 0xFF;
arp_data.destarp_address.addr[2] = 0xFF;
arp_data.destarp_address.addr[3] = 0xFF;
arp_data.destarp_address.addr[4] = 0xFF;
arp_data.destarp_address.addr[5] = 0xFF;

arp_data.sourarp_ipaddress.ipaddr = inet_addr(sourIP);

//申请发送缓冲区，用于存储待发送的数据包
packet_len = sizeof(struct eth_header) + sizeof(struct eth_arp); 
send_buff_len = sizeof(struct eth_arp) > 46 ? packet_len : sizeof(struct eth_header) + 46;
send_buff = (u8_t *)malloc(send_buff_len);  //malloc( )

memcpy(send_buff, &eth_header, sizeof(struct eth_header));
memcpy(send_buff + sizeof(struct eth_header), &arp_data, packet_len);

if (packet_len < send_buff_len) {
	memset(padding, 0, send_buff_len - packet_len);
	memcpy(send_buff + packet_len, padding, send_buff_len - packet_len);
}

```
