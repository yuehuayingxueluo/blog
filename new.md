```c
//校验和算法
u16_t checksum(u16_t* buffer, int size)
{
	unsigned long cksum = 0;
	while (size>1)
	{
		cksum += *buffer;
		//printf("%x\n", *buffer);
		buffer++;
		size -= sizeof(u16_t);
		if(cksum>(0xffff)){
			cksum = (cksum >> 16) + (cksum & 0xffff);   // 将高 16bit 与低 16bit 相加
			cksum += (cksum >> 16);              // 将进位到高位的 16bit 与低 16bit 再相加
		}
		//printf("%x\n", cksum);
	}
	if (size)
	{
		cksum += *(u8_t*)buffer;
	}

	return (u16_t)(~cksum);
}
```c
