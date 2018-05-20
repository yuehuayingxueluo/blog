```c
//Retrieve the device list
if (pcap_findalldevs(&alldevs, errbuf) == -1)   //当前主机的网卡列表，devs中存列表
{
	fprintf(stderr, "Error in pcap_findalldevs: %s\n", errbuf);
	return -1;
}

// Print the device list 打印网卡列表每一项的描述信息
for (d = alldevs,i=0; d; d = d->next)
{
	printf("%d. %s", ++i, d->name);
	if (d->description)
		printf(" (%s)\n", d->description);
	else
		printf(" (No description available)\n");
}

if (i == 0)      //没有获取到网卡
{
	printf("\nNo interfaces found! Make sure WinPcap is installed.\n");
	return -2;
}

printf("Enter the interface number (1-%d):", i);
scanf("%d", &inum);

if (inum < 1 || inum > i)
{
	printf("\nInterface number out of range.\n");
	// Free the device list 释放获取的网卡列表
	pcap_freealldevs(alldevs);
	return -3;
}

//Jump to the selected adapter 找到用户选择的网卡，最后由d指向选择的网卡
for (d = alldevs, i = 0; i < inum - 1; d = d->next, i++);
// 获得网络地址和掩码地址
pcap_lookupnet(d->name, &net_ip, &net_mask, errbuf);


if ((fp = pcap_open_live(d->name,
	65536,
	1,
	1000,
	errbuf
)) == NULL)
{
	fprintf(stderr,
		"\nUnable to open the adapter. %s is not supported by WinPcap\n",
		d->name);
	return -4;
}
```
