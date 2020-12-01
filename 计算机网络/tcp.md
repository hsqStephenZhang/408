# TCP 



## 特点



1. 面向连接
2. 全双工服务
3. 点对点连接
4. 可靠有序的字节流
5. 流量控制
6. 拥塞控制
7. 流水线





### 超时时间



显然超时时间应该大于 RTT（往返时延），但是具体应该设置为多少，这需要设置为一个动态变化和调整的量

而具体如何动态变化，决定于当前链路的状态



首先引入2个概念：

样本RTT(SampleRTT): 对报文段被发出到收到该报文段的确认之间的时间进行测量

偏差RTT(DevRTT): 考虑RTT的波动，估计EstimatedRTT与SampleRTT的偏差



EstimatedRTT = (1-  $ \alpha $  )*EstimatedRTT + $\alpha$ SampleRTT

DevRTT = (1- $\beta $ )*DevRTT + $ \beta $ *|SampleRTT-EstimatedRTT|



由上面的两个公式，就可以得出 超时时间间隔为：

TimeoutInterval = EstimatedRTT + 4*DevRTT





### 快速重传



```c
while (true){
    
    switch (事件):
    
    case 从上层接收应用数据:
    {
        生成具有nextSeqnum的报文段;
        if (定时器没有运行){
            启动定时器;
        }
        向网络层传递报文段
        nextSqenum=nextSeqnum + len(data);
        break;
    }
    
    case 超时:
    {
        重传具有最小的序号的未ack报文;
        启动定时器;
        break;
    }
    
    // 快速重传
    // 一个每一个base都对应着当前已经发送，没有ack的分组的定时器
    case 收到ack，序号值为y:
    {
        if (y>sendBase){
            sendBase=y;
        }
        if (当前窗口无任何应答报文段){
            启动定时器;
        }else{
            对y的报文的冗余ack加一;
            if (y的报文的冗余ack==3){
                重新发送y对应的报文;
            }
        }
        break;
    }
    
    
}
```







### 流量控制









#### Theno和Reno算法



主要包括了三个部分：

1. 慢启动
2. 拥塞避免
3. 快速恢复





##### 慢启动



##### 拥塞避免



##### 快速恢复















#### 吞吐量（速率）



一个高度简化的模型就是，当速率增长到 W/RTT 的时候，网络丢弃该分组，然后进入快速重传阶段，每过一轮，速率增加 MSS/RTT ，直到速率重新增长到  W/RTT ，又重新发生了冗余，快速重传，这一个过程不断重复，TCP的吞吐量就在 W/RTT 和 W/2*RTT 之间线性增长，平均速率为 3\*W / 4\* RTT ,也就是 0.75 * W/RTT 



高带宽下的吞吐量公式计算：



假设上面发生丢失的时候，分组个数为R，则，在一个周期内，发送的分组个数为：

R/2 + (R/2 + 1 ) + ... + R =  $ \frac{3}{8} * R^{2} + \frac{3}{4} * R $ 

这时候发生了丢包，所以丢包率 L = $ \frac{1}{ \frac{3}{8} * R^{2} + \frac{3}{4} * R}$

而当R很大的时候，L= $ \frac{1}{ \frac{3}{8} * R^{2} }$

也就是说 R= $ \sqrt{\frac{8}{3*L}}$ 



所以平均传输速率 = $\frac{R*MSS}{RTT}$ * $\frac{3}{4}$  ，带入 R= $ \sqrt{\frac{8}{3*L}}$  ，得到了最后的结果 = $\frac{1.22R*MSS}{RTT*\sqrt{L}}$ 







 













