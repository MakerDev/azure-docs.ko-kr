---
title: 파일 포함
description: 포함 파일
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: dd2dd84cbcd50fba48011e1836cdc64a6ad5855d
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86040754"
---
'Get-AzureVNetConnection' cmdlet을 사용하여 연결이 성공했는지 확인할 수 있습니다.

1. 일치하는 값을 구성하는 데 다음 cmdlet 예제를 사용합니다. 공백이 포함된 가상 네트워크의 이름은 따옴표로 묶어야 합니다.

   ```azurepowershell
   Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
   ```
2. cmdlet이 완료되면 값을 봅니다. 아래 예제에서는 [연결 상태]가 '연결됨'으로 표시되고 송/수신 바이트를 볼 수 있습니다.

```output
ConnectivityState         : Connected
EgressBytesTransferred    : 181664
IngressBytesTransferred   : 182080
LastConnectionEstablished : 1/7/2016 12:40:54 AM
LastEventID               : 24401
LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                            Connected.
LastEventTimeStamp        : 1/7/2016 12:40:54 AM
LocalNetworkSiteName      : RMVNetLocal
```
