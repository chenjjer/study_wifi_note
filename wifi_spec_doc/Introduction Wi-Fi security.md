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

| 802.11 Standard | Wi-Fi Alicance Certification       | Authentication                 | Encryption Method                   | Cipher                            | Key Generation |
| --------------- | ---------------------------------- | ------------------------------ | ----------------------------------- | --------------------------------- | -------------- |
| 802.11 legacy   |                                    | Open system or Shared Key      | WEP                                 | RC4                               | Static         |
|                 | WPA-Personal                       | WPA PSK and WPA Pre-Shared Key | TKIP                                | RC2                               | Dynamic        |
|                 | WPA-Enterprise                     | 802.1X/EAP                     | TKIP                                | RC4                               | Dynamic        |
| 802.11-2007     | WPA2-Personal                      | WPA2 Pass-phrase               | CCMP(mandatory)                     | AES(mandatory)                    | Dynamic        |
| 802.11-2007     | WPA2-Enterprise                    | 802.1X/EAP                     | CCMP(mandatory)<br />TKIP(optional) | AES(mandatory)<br />RC4(Optional) | Dynamic        |
| 802.11-2018     | WPA3-Personal<br />WPA3-Enterprise |                                | GCMP-256                            | AES                               | SAE handshake  |





## 2. Legacy 802.11 Security

### 2.1 Authentication

#### 2.1.1 Open System Authentication

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













