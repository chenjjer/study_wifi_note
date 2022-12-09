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
| 802.11-2018     | WPA3-Personal<br />WPA3-Enterprise | SAE                            | GCMP-256                            | AES                               | SAE handshake  |





## 2. Legacy 802.11 Security

​		802.11无线加密技术从1997年发布至今，已经有很多重大的改变，根据802.11-2020 spec规定，传统加密技术或者pre-RSNA(Open System authorization，Shared Key authorization and WEP encryption) 依然有被定义到，即使Spec中也明确提出要避免使用Lengacy 802.11 Security，它们仍然被集成到大多数(如果不是所)802.11设备中，以提供与现有设备的向后兼容性。理解这些安全方法、理解为什么开放系统身份验证仍然有效以及为什么应该避免共享密钥身份验证和WEP加密是很重要的。

### 2.1 Authentication

​		802.3的设备需要与其他设备进行连接交互，只需要两者通过网线插口连接好即可，而802.11无线网络的设备端与接入端的认证，只是他们连接的第一步。在802.11-2007中有提到两种认证方式：Open Sytem authentication 和Shared Key authentication。

#### 2.1.1 Open System Authentication

 		open system authentication是在当前spec中802.11-2020中没有被废弃的加密机制。因为它的交互方式简单，在常见的使用场景中也不需要加密方式去验证对比以排查问题，开发系统认证它的工作流程如下图所示：

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

### 2.2 Wire Equivalent Privacy(WEP) Encryption

### 2.3 MAC Filters

### 2.4 SSID Cloaking

## 3. Encryption Ciphers and Methods

### 3.1 Encryption Basics

### 3.2 WLAN Encryption Methods

#### 3.2.1 TKIP

#### 3.2.2 CCMP

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













