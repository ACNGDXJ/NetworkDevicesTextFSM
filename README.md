# TextFSM
Network device textfsm 
# 文档使用规范
---------------------------
编辑时间：2023/8/15
---------------------------
## 文档结构
采用二层文件见架构，第一层是设备厂商，第二层是操作textfsm

会对每个不同的厂商添加不同的操作textfsm解析模板

例如：Huawei的一台交换机设备，需要添加查询端口简要信息的操作#display interface brief

那么他应该在pidnetwork/JD_TextFSM/Huawei文件夹下面建立，建立一个huawei_vrp_display_interface_brief文件夹

huawei是设备厂商，vrp是系统名称，display_interface_brief是具体查询操作

在/pidnetwork/JD_TextFSM/Huawei/huawei_vrp_display_interface_brief这个文件夹下面建立三个文件

#### 1.huawei_vrp_display_interface_brief.textfsm  //该命令的解析模板

##### Value Required INTERFACE_NAME (\S+)
##### Value INTERFACE_PHYSICAL_STATE (down|[\*\^]down|up|up\(\w+\))
##### Value INTERFACE_PROTOCOL_STATE (down|[\*\^]down|up|up\(\w+\))
##### Value INUTI (\d*\.?\d*%|\-\-)
##### Value OUTUTI (\d*\.?\d*%|\-\-)
##### Value INERRORS (\d+)
##### Value OUTERRORS (\d+)
##### 
##### Start
 ##### ^Interface\s+PHY\s+Protocol\s+InUti\s+OutUti\s+inErrors\s+outErrors -> Interface


##### Interface
  ##### ^\s*${INTERFACE_NAME}\s+${INTERFACE_PHYSICAL_STATE}\s+${INTERFACE_PROTOCOL_STATE}\s+${INUTI}\s+${OUTUTI}\s+${INERRORS}\s+${OUTERRORS} -> Record


#### 2.huawei_vrp_display_interface_brief.raw  //该命令的设备命令测试查询显示文档（可有多个，与yml对应，如raw1对应的是yml1）

##### PHY: Physical
##### *down: administratively down
##### ^down: standby
##### (l): loopback
##### (s): spoofing
##### (b): BFD down
##### (e): ETHOAM down
##### (d): Dampening Suppressed
##### (p): port alarm down
##### (dl): DLDP down
##### (c): CFM down
##### InUti/OutUti: input utility rate/output utility rate
##### Interface                  PHY      Protocol  InUti OutUti   inErrors  outErrors
##### 100GE1/0/8                 down     down         0%     0%          0          0
##### 100GE1/0/9                 down     down         0%     0%          0          0
##### 100GE1/0/10                down     down         0%     0%          0          0
##### 100GE1/0/11                down     down         0%     0%          0          0
##### 100GE1/0/12                down     down         0%     0%          0          0


#### 3.huawei_vrp_display_interface_brief.yml  //该命令的解析结果（可有多个，与yml对应，如yml1对应的是raw1）

##### [
##### {
  ##### "INTERFACE_NAME": "100GE1/0/8",
  ##### "INTERFACE_PHYSICAL_STATE": "down",
  ##### "INTERFACE_PROTOCOL_STATE": "down",
  ##### "INUTI": "0%",
  ##### "OUTUTI": "0%",
  ##### "INERRORS": "0",
  ##### "OUTERRORS": "0"
##### },
##### {
  ##### "INTERFACE_NAME": "100GE1/0/9",
  ##### "INTERFACE_PHYSICAL_STATE": "down",
  ##### "INTERFACE_PROTOCOL_STATE": "down",
  ##### "INUTI": "0%",
  ##### "OUTUTI": "0%",
  ##### "INERRORS": "0",
  ##### "OUTERRORS": "0"
##### },
##### {
  ##### "INTERFACE_NAME": "100GE1/0/10",
  ##### "INTERFACE_PHYSICAL_STATE": "down",
  ##### "INTERFACE_PROTOCOL_STATE": "down",
  ##### "INUTI": "0%",
  ##### "OUTUTI": "0%",
  ##### "INERRORS": "0",
  ##### "OUTERRORS": "0"
##### },
##### {
  ##### "INTERFACE_NAME": "100GE1/0/11",
  ##### "INTERFACE_PHYSICAL_STATE": "down",
  ##### "INTERFACE_PROTOCOL_STATE": "down",
  ##### "INUTI": "0%",
  ##### "OUTUTI": "0%",
  ##### "INERRORS": "0",
  ##### "OUTERRORS": "0"
##### },
##### {
  ##### "INTERFACE_NAME": "100GE1/0/12",
  ##### "INTERFACE_PHYSICAL_STATE": "down",
  ##### "INTERFACE_PROTOCOL_STATE": "down",
  ##### "INUTI": "0%",
  ##### "OUTUTI": "0%",
  ##### "INERRORS": "0",
  ##### "OUTERRORS": "0"
##### }]


## 注意事项

### 文件建立中注意文件末尾的文件格式需要写清

### yml和raw可以定义多个对应多组测试用例，尽可能多组测试
