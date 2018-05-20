```c
printf("请输入源的mac:\n");
for(int j=0;j<12;j++){
	scanf("%1x",&sour_mac[j]);	}
for(j=0;j<6;j++){
	u8_t mac=sour_mac[j*2]*16;
	mac +=sour_mac[j*2+1];
	eth_header.sour_address.addr[j] = mac;
}
printf("请输入目的mac:\n");
for( j=0;j<12;j++){
	scanf("%1x",&dest_mac[j]);
}
for(j=0;j<6;j++){
	u8_t mac=dest_mac[j*2]*16;
	mac += dest_mac[j*2+1];
	eth_header.dest_address.addr[j] = mac;
}
```
