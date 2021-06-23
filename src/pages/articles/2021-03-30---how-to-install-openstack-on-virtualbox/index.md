---
title: VirtualBox를 이용해 OpenStack 설치하기 (네트워크 구성편)
date: '2021-03-30T00:00:00.000Z'
layout: post
draft: false
path: '/posts/how-to-install-openstack-on-virtualbox/'
category: 'OpenStack'
tags:
  - 'OpenStack'
description: 'VirtualBox를 이용한 OpenStack 설치 방법을 소개합니다.'
---

## VirtualBox 기본 설정

### VirtualBox 다운로드

![](./images/ca8259cb-e0be-4cce-be28-4c6dd20860ff.png)

https://www.virtualbox.org/wiki/Downloads 에서 PC의 OS에 맞는 VirtualBox 설치 파일을 다운로드 받습니다. (본 글은 Ubuntu 18.04 기준으로 설명하며, 설치 과정은 동일합니다!)

VirtualBox 6.1.18 platform packages > Linux distributions 링크를 클릭합니다.

![](./images/113e708e-7d65-42e5-a059-da2705e85773.png)

Ubuntu 18.04 / 18.10 / 19.04 링크를 클릭해 설치파일을 다운로드합니다.

![](./images/5989c30f-5cb1-49e4-9253-f20629797d28.png)

VirtualBox 6.1.18 Oracle VM VirtualBox Extension Pack > All supported platforms 링크를 클릭합니다.

![](./images/47812964-a9b0-40a3-9cc9-eb4875cc232f.png)

VirtualBox 설치 파일과 Extension Pack 파일이 정상적으로 다운로드 되었는지 확인합니다.

### VirtualBox 설치

![](./images/d1ab391f-1ffb-408f-91f6-000759486af3.png)

virtualbox-6.1_6.1.18-142142_Ubuntu_bionic_amd64.deb 파일을 설치합니다.

### VirtualBox Extension 설치

![](./images/e0bd769d-5376-4f11-a910-eb3f55ed1523.png)

VirtualBox 메인 화면에서 File > Preferences 를 클릭합니다.

![](./images/d2e73cf0-3a6b-4e6a-b45d-9148e88716c6.png)

Extensions 탭 클릭 후 Version 우측에 있는 [+] 모양의 아이콘을 클릭합니다.

![](./images/1a29c74a-6a59-496a-a961-69fd6f3ca3bb.png)

Oracle_VM_VirtualBox_Extension_Pack-6.1.18.vbox-extpack 파일을 선택합니다.

![](./images/db248878-7bc6-4d5c-9a1c-1b149bccbb86.png)

Install을 진행합니다.

![](./images/77a41578-8051-4243-8d64-f84735a28ebd.png)

![](./images/3b6fe844-7ae6-4306-b044-3aa70c887567.png)

성공적으로 Extension이 추가되었는지 확인합니다.

### VirtualBox VM 생성 (Ubuntu 18.04)

![](./images/f46f4ef9-4983-42cb-a311-78d62c7c5f28.png)

Machine > New 클릭합니다.

![](./images/c300e13d-11c2-4c7e-bb45-b65489679791.png)

```
Name: OpenStack (임의로 작성 가능)
Type: Linux
Version: Ubuntu (64-bit)
```

으로 설정하고 [Next] 를 클릭합니다.

![](./images/e575cd3d-3296-4de2-866d-2095cff901b0.png)

Memory size는 최소 2 GB 이상 설정을 권장합니다.
Memory size를 설정하고 [Next] 를 클릭합니다.

![](./images/98fb1d3d-1a3f-4857-8405-5230201b037a.png)

가상 하드 드라이브를 만듭니다.

![](./images/08a08f85-4668-4bd4-b32e-35eb01e3fd4a.png)

가상 하드 디스크 파일 타입을 VDI(VirtualBox Disk Image)로 설정하고 [Next]를 클릭합니다.

![](./images/a5fc149a-0cf0-4f82-b230-2e89390afd9e.png)

- Dynamically allocated (동적 할당)
- Fixed size (고정 할당)

두가지 방식으로 가상 하드 디스크를 만들 수 있습니다.

동적할당의 경우 실제 사용량 만큼만 파일이 커지기 때문에 호스트 컴퓨터 (VirtualBox가 설치된 컴퓨터) 용량을 낭비없이 효율적으로 사용할 수 있는 장점이 있지만, 고정 크기 방식에 비해 속도가 떨어지는 단점이 있습니다.

![](./images/478a745c-ecb2-437d-8edb-68a2b5f12912.png)

가상 하드 디스크의 크기를 입력하고 생성합니다. (최소 8 GB 이상)

![](./images/5404f9a9-7eb6-4458-b6e7-03175d8b8565.png)

Storage > Contoller: IDE
IDE Secondary Device 0: [Optical Drive] Empty 를 클릭합니다.

![](./images/02e75f50-b33c-42d8-bead-f55f49fdecff.png)

Ubuntu 18.04 Server 이미지를 선택합니다.

## VirtualBox 네트워크 설정

### VirtualBox Host Network 설정

![](./images/cd22166f-3fac-4d49-bc11-e01f208d72e2.png)

File > Host Network Manager를 클릭합니다.

![](./images/209b2272-ea97-4194-9c83-9dcf314047f6.png)

새로운 Host Network를 구성하기 위해 Create를 클릭합니다.

![](./images/dcefbcaf-ba8c-4489-b3a5-605c84cd10fc.png)

![](./images/6ffa95c2-f10a-4402-a653-9218c5fc42fc.png)

```
IPv4 Address: 192.168.56.1
IPv4 Network Mask: 255.255.255.0
```

으로 설정합니다.

> [참고]
>
> ![](./images/5a10306b-7660-4336-aee6-bdf4c5a52c6b.png)
>
> (VirtualBox가 설치되어 있는 호스트 컴퓨터에서 어댑터 정보를 확인해보면 VirtualBox Host-Only Network 어댑터로 IPv4가 192.168.56.1 로 설정되어 있는 것을 확인할 수 있습니다.)

### VirtualBox Adapter 설정

OpenStack은 내부 통신을 위한 Internal용 NIC, 외부 통신을 위한 External용 NIC, 최소 2개 이상의 물리 NIC을 필요로 합니다. VirtualBox에서 제공하는 Adapter 설정을 통해 VM에 여러개의 물리 NIC이 연결된 것처럼 구성할 수 있습니다.

![](./images/622f7b0a-7fac-419a-8150-217125a7eec4.png)

```
[VirtualBox Adapter]
Adapter1: 내부 통신용 NIC
Adapter2: 외부 통신용 NIC (Bridge)
Adapter3: NAT
```

```
[Ubuntu]
enp0s3: (host-only) Internal
(Adapter1이 Ubuntu VM에서 enp0s3로 설정됨)

enp0s8: (Bridge) External
(Adapter2가 Ubuntu VM에서 enp0s8로 설정됨)

enp0s9: (NAT)
(Adapter3가 Ubuntu VM에서 enp0s9로 설정됨)
```

![](./images/2c733e3f-aed1-4eca-9611-624b74176d80.png)

Name: Host Network Manager에서 만들었던 설정 이름으로 선택합니다.
Promiscuous Mode: Allow All로 설정합니다.

![](./images/78979128-6f4e-4fb5-8538-d0be171abc69.png)

Promiscuous Mode: Allow All로 설정합니다.

![](./images/0a5adc18-916f-477d-9a27-81a97d538595.png)
