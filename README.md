# networkBasicStructs
basic structs for network


======================== 1 ==================================
struct ifconf {
    int ifc_len;                    /* 缓冲区的总长度（以字节为单位） */
    union {
        char *ifcu_buf;             /* 指向缓冲区的指针 */
        struct ifreq *ifcu_req;     /* 指向 ifreq 数组的指针 */
    } ifc_ifcu;
};
为了简化访问，通常会使用以下宏定义来访问 ifconf 结构体的字段：
ifc_buf：指向 ifc_ifcu.ifcu_buf，表示存储网络接口信息的缓冲区。
ifc_req：指向 ifc_ifcu.ifcu_req，表示存储 ifreq 结构体数组的指针。
因此，struct ifconf 可以用来传递一个缓冲区或一个 ifreq 结构体数组，用于存储多个网络接口的配置信息。

======================== 1 ==================================
struct ifreq {
    union {
        char ifrn_name[IFNAMSIZ];   /* Interface name, e.g., "eth0" */
    } ifr_ifrn;

    union {
        struct sockaddr ifru_addr;
        struct sockaddr ifru_dstaddr;
        struct sockaddr ifru_broadaddr;
        struct sockaddr ifru_netmask;
        struct sockaddr ifru_hwaddr;
        short int ifru_flags;
        int ifru_ivalue;
        int ifru_mtu;
        struct ifmap ifru_map;
        char ifru_slave[IFNAMSIZ];
        char ifru_newname[IFNAMSIZ];
        __u32 ifru_data;
    } ifr_ifru;
};
为了简化访问，通常会使用以下宏定义来访问 ifreq 结构体的字段：
ifr_name：指向 ifr_ifrn.ifrn_name，表示网络接口的名称（如 "eth0"）。
ifr_addr：指向 ifr_ifru.ifru_addr，表示网络接口的地址（如IP地址、MAC地址等）。
ifr_flags：指向 ifr_ifru.ifru_flags，表示网络接口的标志（如是否启用、是否处于混杂模式等）。
ifr_ifindex：指向 ifr_ifru.ifru_ivalue，表示网络接口的索引
因此，struct ifreq 可以用来传递网络接口的名称，并通过 ioctl 系统调用获取或设置该接口的各种属性。

======================== 1 ==================================
struct sockaddr_ll {	// ll 表示 "link layer"（链路层）
    unsigned short sll_family;   /* 地址族，通常是 AF_PACKET */
    unsigned short sll_protocol; /* 协议类型（如 ETH_P_ALL, ETH_P_IP 等） */
    int            sll_ifindex;  /* 接口索引（通过 if_nametoindex 获取） */
    unsigned short sll_hatype;   /* 硬件类型（通常为 ARPHRD_ETHER 表示以太网） */
    unsigned char  sll_pkttype;  /* 包类型（如 PACKET_HOST, PACKET_BROADCAST 等） */
    unsigned char  sll_halen;    /* 硬件地址长度 */
    unsigned char  sll_addr[8];  /* 硬件地址（通常是MAC地址） */
};
sll_family：	地址族，通常设置为 AF_PACKET，表示这是一个链路层套接字。
sll_protocol：	协议类型，指定要捕获或发送的协议（如 ETH_P_ALL、ETH_P_IP 等）。
sll_ifindex：	接口索引，标识要使用的网络接口。
sll_hatype：	硬件类型，表示底层网络接口的类型（如 ARPHRD_ETHER 表示以太网）。
sll_pkttype：	包类型，表示接收到的数据包的类型（如 PACKET_HOST、PACKET_BROADCAST 等）。
sll_halen：		硬件地址的长度，通常为 6 字节（对于以太网MAC地址）。
sll_addr：		硬件地址（通常是MAC地址），最多 8 字节。

struct sockaddr {
    sa_family_t sa_family;  /* 地址族（如 AF_INET, AF_INET6, AF_UNIX 等） */
    char        sa_data[14]; /* 依赖于地址族的具体地址数据 */
};
