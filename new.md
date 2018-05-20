```c
struct eth_addr {
	u8_t addr[6];                       //48位以太网地址
};
//32位ip地址
struct eth_ipaddr {
	u32_t ipaddr;                       
};

//以太网数据帧头结构
struct eth_header {
  struct eth_addr dest_address;    //目的MAC地址
  struct eth_addr sour_address;    //源MAC地址 
  u16_t type;                      //协议类型或数据长度 
};
//ARP结构 0806
struct eth_arp {
	u16_t macType;                            //硬件类型
	u16_t ipType;                             //协议类型
	u8_t macLongh;                            //硬件地址长度
	u8_t ipLongh;                             //协议地址长度
	u16_t mode;                               //操作码
	struct eth_addr sourarp_address;		  //源MAC地址
	struct eth_ipaddr sourarp_ipaddress;      //源IP
	struct eth_addr destarp_address;          //目的MAC地址
	struct eth_ipaddr destarp_ipaddress;      //目的IP地址
};
//IP结构
struct eth_IP {
	u8_t  ipType;                        //版本号（4位）首部长度（4位）
	u8_t  severType;                     //服务类型（8位）0
	u16_t ipLongth;                      //总长度（16位）
	u16_t ipTge;                         //标识（16位）
	u16_t ipTgeset;                      //标志（3位）偏移量（13位）
	u8_t  lifeTime;                      //生存时间（8位）	
	u8_t  higeType;                      //高层协议类型（8位）	
	u16_t checkSum;                      //首部检验和（16位）
	struct eth_ipaddr sourip_ipaddress;  //源IP地址（32位）
	struct eth_ipaddr destip_ipaddress;  //目的IP地址（32位）
};
//TCP结构
struct eth_tcp
{
	u16_t sourPort;                      // 源端口号16bit
	u16_t destPort;                      // 目的端口号16bit
	u32_t sequNum;                       // 序列号32bit
	u32_t acknowledgeNum;                // 确认号32bit
	u16_t headerLenAndFlag;              // 前4位：TCP头长度；中6位：保留；后6位：标志位
	u16_t windowSize;                    // 窗口大小16bit
	u16_t checkSum;                      // 检验和16bit
	u16_t urgentPointer;                 // 紧急数据偏移量16bit
};
//TCP伪首部
struct tcp_psd_header
{
	u32_t saddr;                          // 源地址
	u32_t daddr;                          // 目的地址
	u8_t mbz;                             // 置空
	u8_t ptcl;                            // 协议类型
	u16_t tcpl;                           //TCP 长度
};
//定义ICMP首部
struct eth_icmp {
	u8_t i_type; //8位类型
	u8_t i_code; //8位代码
	u16_t i_cksum; //16位校验和, 从TYPE开始,直到最后一位用户数据,如果为字节数为奇数则补充一位
	u16_t i_id; //识别号（一般用进程号作为识别号）, 用于匹配ECHO和ECHO REPLY包
	u16_t i_seq; //报文序列号, 用于标记ECHO报文顺序
	u32_t timestamp; //时间戳
};
```c
