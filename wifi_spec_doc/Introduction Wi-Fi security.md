[toc]

# Introduction Wi-Fi security

## 1. WLAN Security Overview

​		根据目前802.11-2020 spec针对802.11网络采用两种类型的加密算法。

-  RSNA algorithms
- Pre-RSNA algorithms

​		在当前的spec中并没有规定是否可以同时运行上述两种算法，其中Spec有明确规定WEP算法是被废弃的，详情见802.11-2020 12.2.1章节，另外TKIP算法也是不被联盟推荐的。

### 1.1 802.11 Security Basics

​		当对802.11 网络进行加密的过程中，有5个元素是必要的

1.  Data privacy
2. Authentication，authorization and accounting（AAA)
3. Segmentation
4. Monitoring
5. Policy

​		首先在无线网络中，数据的传输都是在空气中，因此为了保证数据的安全性，需要对数据进行相应的加密。其次在802.3以太网中也需要对端口进行认证，802.11也借用此机制，保证使用者加入802.11无线网络都是通过认证的，由于无线网络的不可靠性，因此在进行认证后还需要更多的认证机制来保证加入者的安全性。经历上述步骤后，802.11无线网络机制可能还是被攻击，因此还需要提供监控机制随时保证网络的安全。当然上述所有的加密步骤都要在合适的加密策略执行，从而保证整个网络的安全。

### 1.2 802.11 Security History

​		802.11 Security历史表如下：

| 802.11 Standard | Wi-Fi Alicance Certification       | Authentication                 | Encryption Method                   | Cipher                            | Key Generation |
| --------------- | ---------------------------------- | ------------------------------ | ----------------------------------- | --------------------------------- | -------------- |
| 802.11 legacy   |                                    | Open system or Shared Key      | WEP                                 | RC4                               | Static         |
|                 | WPA-Personal                       | WPA PSK and WPA Pre-Shared Key | TKIP                                | RC2                               | Dynamic        |
|                 | WPA-Enterprise                     | 802.1X/EAP                     | TKIP                                | RC4                               | Dynamic        |
| 802.11-2007     | WPA2-Personal                      | WPA2 Pass-phrase               | CCMP(mandatory)                     | AES(mandatory)                    | Dynamic        |
| 802.11-2007     | WPA2-Enterprise                    | 802.1X/EAP                     | CCMP(mandatory)<br />TKIP(optional) | AES(mandatory)<br />RC4(Optional) | Dynamic        |
| 802.11-2018     | WPA3-Personal<br />WPA3-Enterprise | SAE/802.1x/EAP                 | GCMP-256                            | AES                               | Dynamic        |





## 2. Legacy 802.11 Security

​		802.11无线加密技术从1997年发布至今，已经有很多重大的改变，根据802.11-2020 spec规定，传统加密技术或者pre-RSNA(Open System authorization，Shared Key authorization and WEP encryption) 依然有被定义到，即使Spec中也明确提出要避免使用Lengacy 802.11 Security，它们仍然被集成到大多数(如果不是所)802.11设备中，以提供与现有设备的向后兼容性。理解这些安全方法、理解为什么开放系统身份验证仍然有效以及为什么应该避免共享密钥身份验证和WEP加密是很重要的。

### 2.1 Authentication

​		802.3的设备需要与其他设备进行连接交互，只需要两者通过网线插口连接好即可，而802.11无线网络的设备端与接入端的认证，只是他们连接的第一步。在802.11-2007中有提到两种认证方式：Open Sytem authentication 和Shared Key authentication。

#### 2.1.1 Open System Authentication

​	open system authentication是在当前spec中802.11-2020中没有被废弃的加密机制。它也被称为Null authenticated algorithm ，因为它的交互方式简单，在常见的使用场景中也不需要加密方式去验证对比以排查问题，开发系统认证它的工作流程如下图所示：

<img src="Introduction Wi-Fi security.assets/image-20221209225555280.png" alt="image-20221209225555280" style="zoom:50%;" />

​		如上述图所示 open system authentication client与AP之间只有4帧的交互，其中STA可以通过passive 或者active scan 发现AP。通过sniffer来抓取他们的具体交互信息

![img](Introduction Wi-Fi security.assets/cwsp-wep-03.png)

在上述1246行帧中

![CWSP-WEP-04](Introduction Wi-Fi security.assets/cwsp-wep-04.png)

*(wlan.fc.type == 0)&&(wlan.fc.type_subtype == 0x0b)* 通过wireshark 可以看到 authentication Algorithm是open system，序列号是1代表auth request，然后AP回复authentication response，Authenticaion SEQ变为2

![CWSP-WEP-05](Introduction Wi-Fi security.assets/cwsp-wep-05.png)

然后Cline发起assoc request后，AP回复assoction response并返回连线结果，如下图所示：

![CWSP-WEP-07](Introduction Wi-Fi security.assets/cwsp-wep-07.png)



#### 2.1.2 Shared Key Authentication

​		Shared Key authentication是采用WEP的方式去加密设备，在连线的过程中要保证Client和AP端的静态密码是一致的，如果两端静态密码不匹配，则两台设备交互则会失败，client与AP之间采用shared key的方式交互，处理过程与open system类似，但有一点不同的是shared key会在auth的过程中去检查其中的key是否匹配。下面是它的工作流程图：

<img src="Introduction Wi-Fi security.assets/image-20221210112033968.png" alt="image-20221210112033968" style="zoom:50%;" />

​		下面是Shared Key的抓包示例：

他们整体交互过程如下，整体交互过程中有4个auth frame

![img](Introduction Wi-Fi security.assets/cwsp-wep-09-1670642787415-3.png)

第一个是发出authentication request给AP，SEQ No 是1，authentication algorithm是shared key，如下图所示：

<img src="Introduction Wi-Fi security.assets/cwsp-wep-10.png" alt="img" style="zoom: 75%;" />

然后AP发送一个明文挑战给Client，次数SEQ NO是2，如下图所示：

<img src="Introduction Wi-Fi security.assets/cwsp-wep-11.png" alt="img" style="zoom:75%;" />

由于后面两次auth数据采用WEP的加密方式去处理，因此需要wireshark去进行解密，sniffer结果如下：

<img src="Introduction Wi-Fi security.assets/cwsp-wep-12.png" alt="img" style="zoom:75%;" />

client明文挑战后以authentication request的方式在发送给AP，SEQ NO是3解密后sniffer结果如下：

<img src="Introduction Wi-Fi security.assets/cwsp-wep-13.png" alt="img" style="zoom:75%;" />

AP收到Client的WEP加密数据后，进行解密后进行比较是否匹配AP的明文挑战，匹配后发送第四包auth frame给STA，并返回验证结果，sniffer结果如下：

<img src="Introduction Wi-Fi security.assets/cwsp-wep-14.png" alt="img" style="zoom:75%;" />

### 2.2 Wire Equivalent Privacy(WEP) Encryption

​		WEP主要是针对ISO mode L2的加密方法，在当前的Spec 802.11-2020有两种称呼，WEP-40以及WEP-104，主要是由于他们的static key的长度不同，如下图所示：

<img src="Introduction Wi-Fi security.assets/image-20221210142308590.png" alt="image-20221210142308590" style="zoom:50%;" />

由于MSDU包含Layers3~7的数据以及LLC相关的数据，在目前针对L2的加密算法都是针对MSDU 进行的，下图WEP具体的帧格式的大小设定，从图中看出WEP将对MSDU和ICV部分进行加密，对IV信息是不加密的，其中IV信息中包含ID详细信息，接收设备可以在其中设别哪个密钥已用于加密。

<img src="Introduction Wi-Fi security.assets/image-20221210151221756.png" alt="image-20221210151221756" style="zoom:50%;" />

WEP加密过程步骤如下：

1. 24bit的明文IV(Iniialization Vector)随机生成并与静态密钥结合
2. 密钥长度在64位WEP中位40位，在128位WEP中密钥长度为104位
3. IV & Key用ARC4伪随机算法生成密钥流
4. 在明文数据上加上CRC并作为32位ICV(Integrity Check Value) 附加到明文的数据的末尾
5. 然后使用XOR过程，将生成的密钥流和明文数据位组合
6. 最终生成结果是WEP密文

WEP添加8字节（4-IV、4-ICV）加密开销，导致最大MSDU从2304字节增加到2312字节。加密过程整体如下图所示：

<img src="Introduction Wi-Fi security.assets/image-20221210141407143.png" alt="image-20221210141407143" style="zoom: 50%;" />

WEP加密步骤如下：

1. 收到对端发过来的加密明文，将其中IV信息与本地的WEP key 静态结合
2. IV & Key通过ARC4 伪随机算法生成密钥流
3. 生成的密钥流与加密的明文 使用XOR 解析
4. 一方面解析出明文，另外一方面检查ICV是否与预期相等。

整个过程如下图所示：

<img src="Introduction Wi-Fi security.assets/image-20221210141455150.png" alt="image-20221210141455150" style="zoom: 50%;" />

随机WEP的使用，在实际的使用过程中，WEP相关的缺点也暴露出来，因此802.11Spec中已经明确废弃此加密算法，为了解决以下的问题，进而提出TKIP加密算法。

1. IV冲突攻击 --- 24位IV可能发生IV冲突，攻击这可以轻松恢复WEP密钥
2. 弱密钥攻击
3. 再注入攻击 --- 存在实施数据包注入工具，以加速再流量较小的网络上收集弱IV
4. 位翻转攻击 --- ICV数据完整性检查安全性较差，它不是真正完整校验，真正需要hash

## 3. Encryption Ciphers and Methods

### 3.1 Encryption Basics

​		加密算法的本质，首先是为了对数据进行保密并防止篡改，其次更具有身份验证的功能，根据密钥类型的不同，加密算法分为对称和非对称两种

**对称算法**：采用单钥密码系统的加密方法，同一个密钥用来加密和解密，常见的对称加密算法有 DES，3DES，AES，RC2，RC4，RC5等。由于此方法算法公开，计算量小，加密效率高，在无线网络中广泛使用。效果如下图所示：

![img](Introduction Wi-Fi security.assets/SouthEast.jpeg)

**非对称算法**：采取加密钥匙和解密是采用不同的方式。可以表现如下图所示：

<img src="Introduction Wi-Fi security.assets/image-20221210155213830.png" alt="image-20221210155213830" style="zoom:50%;" />

常见加密算法的对比如下：

| 名称 | 密钥长度      | 运算速度    | 安全性               | 资源消耗 | 应用场合                 |
| ---- | ------------- | ----------- | -------------------- | -------- | ------------------------ |
| DES  | 56位          | 一般        | 低                   | 中       | 适用与硬件实现           |
| RC4  | 256位         | 比DES快10倍 | 中                   | 低       | 算法简单，易于编程实现   |
| AES  | 128、192、256 | 快          | 目前最安全的加密算法 | 低       | 适用与无线网络的安全保障 |

AES:

AES算法是Advanced Encryption Standard 的缩写，是美国联邦政府采用的一种区块加密标准。这个标准用来替代原先的DES，已经被多方分析且广为全世界所使用。它的工作流程如下图所示：

<img src="Introduction Wi-Fi security.assets/SouthEast-1670660128498-19.jpeg" alt="img" style="zoom:50%;" />



### 3.2 WLAN Encryption Methods

​		802.11-2007标准定义了三种工作在OSI模型L2的加密方法:WEP、TKIP和CCMP。这些第2层加密方法所保护的信息是在第3 -7层中找到的数据。第二层加密方法用于为802.11数据帧提供数据隐私。

​		802.11数据帧的技术名称是MAC协议数据单元(MPDU)。如图所示，

<img src="Introduction Wi-Fi security.assets/image-20221210162335427.png" alt="image-20221210162335427" style="zoom: 50%;" />

802.11数据帧包含一个二层MAC报头、一个帧体和一个拖尾，这是一个32位的CRC，称为帧检查序列(frame check sequence, FCS)。第二层报头包含MAC地址和持续时间值。封装在802.11数据帧的帧体内部的是称为MAC服务的上层有效负载数据单元(MSDU)。MSDU包含LLC (Logical Link Control)和layer 3 -7的数据。MSDU的一个简单定义是，它是包含IP包和一些LLC数据的数据有效负载。

​		802.11-2007标准规定，MSDU有效负载可以是0到2304字节的任何地方。由于加密开销，帧体实际上可能更大。采用WEP、TKIP、CCMP等专有的二层加密方法对802.11数据帧的MSDU有效载荷进行加密。因此，信息即是被保护的是上面的3 -7层，也就是通常所说的IP数据包。

#### 3.2.1 TKIP

<img src="Introduction Wi-Fi security.assets/image-20221210162843326.png" alt="image-20221210162843326" style="zoom: 50%;" />



<img src="Introduction Wi-Fi security.assets/image-20221210163304752.png" alt="image-20221210163304752" style="zoom:50%;" />

<img src="Introduction Wi-Fi security.assets/image-20221210163331577.png" alt="image-20221210163331577" style="zoom:50%;" />

#### 3.2.2 CCMP

<img src="Introduction Wi-Fi security.assets/image-20221210163006619.png" alt="image-20221210163006619" style="zoom: 50%;" />

<img src="Introduction Wi-Fi security.assets/image-20221210163516079.png" alt="image-20221210163516079" style="zoom:50%;" />

#### 3.2.3 BIP

#### 3.2.4 GCMP

<img src="Introduction Wi-Fi security.assets/image-20221210163758877.png" alt="image-20221210163758877" style="zoom:50%;" />

<img src="Introduction Wi-Fi security.assets/image-20221210163817614.png" alt="image-20221210163817614" style="zoom:50%;" />

### 3.3 WPA/WPA2

## 4. WPA3 Introduction

### 4.1 Management Frame Protection(PMF)

### 4.2 WPA3-Personal

#### 4.2.1 WAP3-SAE

#### 4.2.2 WPA3  SAE Transition Mode

### 4.3  WPA3-Enterprise

### 4.4 Enhanced Open

### 4.5 WPA3 Code Flow

#### 4.5.1 WPA3 In wpa_supplicant

#### 4.5.2 WPA3 In driver

## 5. SOHO 802.11 Security

### 5.1  WPA/WPA2-Personal

### 5.2 Wi-Fi Protected Setup(WPS)

## 6. 802.1X Authentication Methods

### 6.1 802.1X Introduction Architecture

### 6.2 802.1X EAPOL & EAPOR

### 6.3 802.1 Authentication Flow

### 6.4 802.1X MTK Code Flow

### 6.5 802.1X 环境搭建及测试

## 7. 802.11 Fast Secure Roaming (option)

### 7.1 802.11 Roaming Basics

### 7.2 802.11r Key Hierarchy

### 7.3 802.11r Over-the-Air-FT

### 7.4 802.11r Over-the-DS-FT

### 7.5 802.11r FT Association

### 7.6 802.11k AP Assisted Roaming

## reference

1. https://blog.csdn.net/u014294681/article/details/86690241
2. https://blog.csdn.net/wangbaochu/article/details/44199089
3. https://blog.csdn.net/rachel_4869/article/details/80128703









