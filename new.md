```c
//构造TCP报文

eth_header.type = htons(0x0800);
//IP头
ip_data.ipType = 0x45;
ip_data.severType = 0x00;
ip_data.ipLongth = htons(0x38);
ip_data.ipTge = 0x0000;
ip_data.ipTgeset = 0x0000;
ip_data.lifeTime = 0xff;
ip_data.higeType = 0x06;
ip_data.checkSum = 0x0000;
ip_data.destip_ipaddress.ipaddr = inet_addr(destIP);
ip_data.sourip_ipaddress.ipaddr = inet_addr(sourIP);

struct eth_IP *sendip_buff = &ip_data;
ip_data.checkSum = checksum((USHORT*)sendip_buff, 20);

//TCP首部
printf("请输入源端口：\n");
scanf("%x",&tcp_data.sourPort);
tcp_data.sourPort = htons(tcp_data.sourPort);
printf("请输入目的端口：\n");
scanf("%x",&tcp_data.destPort);
tcp_data.destPort = htons(tcp_data.destPort);
tcp_data.sequNum = htonl(0xa6877d25);
tcp_data.acknowledgeNum = htonl(0x00000000);
printf("请输入数据偏移，保留，和标志位：\n");
scanf("%x",&tcp_data.headerLenAndFlag);
tcp_data.headerLenAndFlag = htons(tcp_data.headerLenAndFlag);
tcp_data.windowSize = htons(0xffff);
tcp_data.checkSum = htons(0x0000);
tcp_data.urgentPointer = htons(0x0000);


//TCP伪首部
tcp_psd_header_data.saddr = inet_addr(sourIP);
tcp_psd_header_data.daddr = inet_addr(destIP);
tcp_psd_header_data.mbz = 0;
tcp_psd_header_data.ptcl = 0x06;
tcp_psd_header_data.tcpl = htons(0x0024);

//准备计算TCP校验和
int strlenth = strlen((char *)payload);
u8_t ling = 0x00;


int sendtcp_buffer_len = sizeof(struct tcp_psd_header) + sizeof(struct eth_tcp) + strlenth;
u8_t *sendtcp_buffer = (u8_t *)malloc(sendtcp_buffer_len);


memcpy(sendtcp_buffer, &tcp_psd_header_data, sizeof(struct tcp_psd_header)); 
memcpy(sendtcp_buffer + sizeof(struct tcp_psd_header), &tcp_data, sizeof(struct eth_tcp));
memcpy(sendtcp_buffer + sizeof(struct tcp_psd_header)+sizeof(struct eth_tcp), payload, strlen((char *)payload));
u8_t *c=sendtcp_buffer;

tcp_data.checkSum = checksum((USHORT *)sendtcp_buffer,sendtcp_buffer_len);
printf("%x\n",tcp_data.checkSum);

//申请发送缓冲区，用于存储待发送的数据包
packet_len = sizeof(struct eth_header) + sizeof(struct eth_IP) + sizeof(struct eth_tcp) + strlen((char *)payload);
send_buff_len = packet_len > 46 ? packet_len : 60;
send_buff = (u8_t *)malloc(send_buff_len);

memcpy(send_buff, &eth_header, sizeof(struct eth_header));
memcpy(send_buff + sizeof(struct eth_header), &ip_data, sizeof(struct eth_IP));
memcpy(send_buff + sizeof(struct eth_header) + sizeof(struct eth_IP), &tcp_data, sizeof(struct eth_tcp));
memcpy(send_buff + sizeof(struct eth_header) + sizeof(struct eth_IP) + sizeof(struct eth_tcp), payload, strlen((char *)payload));

if (packet_len < send_buff_len) {
memset(padding, 0, send_buff_len - packet_len);
memcpy(send_buff + packet_len, padding, send_buff_len - packet_len);
}
```
