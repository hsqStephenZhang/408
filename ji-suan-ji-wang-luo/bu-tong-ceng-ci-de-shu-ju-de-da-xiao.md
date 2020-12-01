# TCP报文的最大负载和报文的最小长度

以太网的最大数据帧长度为1518 Bytes

以太网的帧头部有14 Bytes：源主机的MAC地址6 Bytes，目的主机的MAC地址6 Bytes，Type域 2 Bytes

以太网的帧尾部有4 Bytes

所以数据域的长度最大为1500Bytes

所以TCP数据包大小为1500-20-20 （分别是IP头部信息20 Bytes，TCP头部信息20 Bytes） = 1460 Bytes

如果是UDP的话，就是 1500-20-8 = 1472 Bytes

TCP的最大负载为 65535 - 20 - 20 = 65595 Bytes

TCP报文段的最大负载为65595字节，

