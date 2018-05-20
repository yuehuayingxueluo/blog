```c
//构造ICMP报文

eth_header.type = htons(0x0800);
                   //IP报文内容
ip_data.ipType = 0x45;
ip_data.severType = 0x00;
ip_data.ipLongth = htons(0x001c);
ip_data.ipTge = 0x0000;
ip_data.ipTgeset = 0x0000;
ip_data.lifeTime = 0xff;
ip_data.higeType = 0x01;
ip_data.checkSum = 0x0000;
ip_data.destip_ipaddress.ipaddr = inet_addr(destIP);
ip_data.sourip_ipaddress.ipaddr = inet_addr(sourIP);
struct eth_IP *sendip_buff = &ip_data;
ip_data.checkSum = checksum((USHORT*)sendip_buff, 20);//计算校验和
//ICMP首部
printf("请输入ICMP类型：8请求，0回送,13时间戳请求，14时间戳应答\n");
scanf("%x",&icmp_data.i_type);
icmp_data.i_code = 0x00;
icmp_data.i_cksum = 0x0000;
icmp_data.i_id = htons(0x4000);
icmp_data.i_seq = htons(0x4000);
icmp_data.timestamp = 0;

struct eth_icmp *sendicmp_buff = &icmp_data;
icmp_data.i_cksum = checksum((USHORT*)sendicmp_buff, 12);//计算校验和

//申请发送缓冲区，用于存储待发送的数据包
packet_len = sizeof(struct eth_header) + sizeof(struct eth_IP) + sizeof(struct eth_icmp);
send_buff_len = packet_len > 46 ? packet_len : 60;  
send_buff = (u8_t *)malloc(send_buff_len);

memcpy(send_buff, &eth_header, sizeof(struct eth_header));
memcpy(send_buff + sizeof(struct eth_header), &ip_data, sizeof(struct eth_IP));
memcpy(send_buff + sizeof(struct eth_header) + sizeof(struct eth_IP), &icmp_data, sizeof(struct eth_icmp));

if (packet_len < send_buff_len) {
  memset(padding, 0, send_buff_len - packet_len);
  memcpy(send_buff + packet_len, padding, send_buff_len - packet_len);
}

```
