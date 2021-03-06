---
title: 관리 주소
description: App Service Environment를 제어하는 데 사용되는 관리 주소를 찾습니다. 경로 테이블에 구성된 관리 주소는 비대칭 라우팅 문제를 방지하는 데 필요합니다.
author: ccompy
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.topic: article
ms.date: 05/10/2020
ms.author: ccompy
ms.custom: seodec18, references_regions
ms.openlocfilehash: 4f1914b857aa6499ab4d4445cddf801feef73f43
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85831823"
---
# <a name="app-service-environment-management-addresses"></a>App Service Environment 관리 주소

ASE(App Service Environment)는 Azure VNet(Virtual Network)에서 실행되는 Azure App Service의 단일 테넌트 배포입니다.  ASE는 VNet에서 실행되지만 Azure App Service에서 서비스를 관리하는 데 사용되는 여러 전용 IP 주소에서도 ASE에 액세스할 수 있어야 합니다.  ASE의 경우 관리 트래픽은 사용자 제어 네트워크를 통과합니다. 이 트래픽이 차단되거나 잘못 라우팅된 경우 ASE는 일시 중단 됩니다. ASE 네트워킹 종속성에 대한 자세한 내용은 [네트워킹 고려 사항과 App Service Environment][networking]를 참조하세요. 시작하는 데 도움이 되는 ASE에 대한 일반적인 정보는 [App Service Environment 소개][intro]를 참조하세요.

모든 ASE에는 관리 트래픽이 전달되는 공개 VIP가 있습니다. 이러한 주소에서 들어오는 관리 트래픽은 ASE의 공개 VIP에서 포트 454 및 455로 전달됩니다. 이 문서에서는 ASE에 대한 관리 트래픽의 App Service 원본 주소를 나열합니다. 이러한 주소는 AppServiceManagement라는 IP 서비스 태그에도 있습니다.

관리 트래픽과의 비대칭 라우팅 문제를 방지하기 위해 경로 테이블에서 아래에 설명된 주소를 구성할 수 있습니다. 경로는 IP 수준의 트래픽에서 작동하며 트래픽 방향을 인식하지 못하거나 트래픽이 TCP 응답 메시지의 일부임을 인식하지 못합니다. TCP 요청에 대한 회신 주소가 요청을 보낸 주소와 다른 경우 비대칭 라우팅 문제가 발생합니다. ASE 관리 트래픽에서 비대칭 라우팅 문제를 방지하려면 트래픽을 보낸 주소에서 회신을 보내는지 확인해야 합니다. ASE를 아웃바운드 트래픽이 온-프레미스에서 전송되는 환경에서 작동하도록 구성하는 방법에 대한 내용은 [강제 터널링을 사용하여 ASE 구성][forcedtunnel]을 참조하세요.

## <a name="list-of-management-addresses"></a>관리 주소 목록 ##

| 지역 | 주소 |
|--------|-----------|
| 모든 공용 지역 | 13.66.140.0, 13.67.8.128, 13.69.64.128, 13.69.227.128, 13.70.73.128, 13.71.170.64, 13.71.194.129, 13.75.34.192, 13.75.127.117, 13.77.50.128, 13.78.109.0, 13.89.171.0, 13.94.141.115, 13.94.143.126, 13.94.149.179, 20.36.106.128, 20.36.114.64, 23.102.135.246, 23.102.188.65, 40.69.106.128, 40.70.146.128, 40.71.13.64, 40.74.100.64, 40.78.194.128, 40.79.130.64, 40.79.178.128, 40.83.120.64, 40.83.121.56, 40.83.125.161, 40.112.242.192, 51.140.146.64, 51.140.210.128, 52.151.25.45, 52.162.106.192, 52.165.152.214, 52.165.153.122, 52.165.154.193, 52.165.158.140, 52.174.22.21, 52.178.177.147, 52.178.184.149, 52.178.190.65, 52.178.195.197, 52.187.56.50, 52.187.59.251, 52.187.63.19, 52.187.63.37, 52.224.105.172, 52.225.177.153, 52.231.18.64, 52.231.146.128, 65.52.172.237, 70.37.57.58, 104.44.129.141, 104.44.129.243, 104.44.129.255, 104.44.134.255, 104.208.54.11, 104.211.81.64, 104.211.146.128, 157.55.208.185, 191.233.203.64, 191.236.154.88 |
| Microsoft Azure Government | 23.97.29.209, 13.72.53.37, 13.72.180.105 |

## <a name="configuring-a-network-security-group"></a>네트워크 보안 그룹 구성

네트워크 보안 그룹을 사용하면 개별 주소 또는 사용자 고유의 구성 유지 관리에 대해 걱정할 필요가 없습니다. 모든 주소를 사용하여 최신 상태로 유지되는 AppServiceManagement라는 IP 서비스 태그가 있습니다. NSG에서 이 IP 서비스 태그를 사용하려면 포털로 이동한 후 네트워크 보안 그룹 UI를 열고 인바운드 보안 규칙을 선택합니다. 인바운드 관리 트래픽에 대한 기존 규칙이 있으면 편집합니다. 이 NSG를 ASE과 함께 만들지 않았거나 모두 새 항목이면 **추가**를 선택합니다. 원본 드롭다운 목록에서 **서비스 태그**를 선택합니다.  원본 서비스 태그에서 **AppServiceManagement**를 선택합니다. 원본 포트 범위를 \*으로, 대상을 **모두**로, 대상 포트 범위를 **454-455**로, 프로토콜을 **TCP**로, 작업을 **허용**으로 설정합니다. 규칙을 만드는 경우 우선 순위를 설정해야 합니다. 

![서비스 태그를 사용하여 NSG 만들기][1]

## <a name="configuring-a-route-table"></a>경로 테이블 구성

관리 주소는 모든 인바운드 관리 트래픽이 동일한 경로로 다시 이동할 수 있도록 인터넷의 다음 홉을 사용하여 경로 테이블에 배치할 수 있습니다. 이러한 경로는 강제 터널링을 구성할 때 필요합니다. 경로 테이블을 만들려면 포털, PowerShell 또는 Azure CLI를 사용할 수 있습니다.  PowerShell 프롬프트에서 Azure CLI를 사용하여 경로 테이블을 만드는 명령은 다음과 같습니다. 

```azurepowershell-interactive
$rg = "resource group name"
$rt = "route table name"
$location = "azure location"
$managementAddresses = "13.66.140.0", "13.67.8.128", "13.69.64.128", "13.69.227.128", "13.70.73.128", "13.71.170.64", "13.71.194.129", "13.75.34.192", "13.75.127.117", "13.77.50.128", "13.78.109.0", "13.89.171.0", "13.94.141.115", "13.94.143.126", "13.94.149.179", "20.36.106.128", "20.36.114.64", "23.102.135.246", "23.102.188.65", "40.69.106.128", "40.70.146.128", "40.71.13.64", "40.74.100.64", "40.78.194.128", "40.79.130.64", "40.79.178.128", "40.83.120.64", "40.83.121.56", "40.83.125.161", "40.112.242.192", "51.140.146.64", "51.140.210.128", "52.151.25.45", "52.162.106.192", "52.165.152.214", "52.165.153.122", "52.165.154.193", "52.165.158.140", "52.174.22.21", "52.178.177.147", "52.178.184.149", "52.178.190.65", "52.178.195.197", "52.187.56.50", "52.187.59.251", "52.187.63.19", "52.187.63.37", "52.224.105.172", "52.225.177.153", "52.231.18.64", "52.231.146.128", "65.52.172.237", "70.37.57.58", "104.44.129.141", "104.44.129.243", "104.44.129.255", "104.44.134.255", "104.208.54.11", "104.211.81.64", "104.211.146.128", "157.55.208.185", "191.233.203.64", "191.236.154.88"

az network route-table create --name $rt --resource-group $rg --location $location
foreach ($ip in $managementAddresses) {
    az network route-table route create -g $rg --route-table-name $rt -n $ip --next-hop-type Internet --address-prefix ($ip + "/32")
}
```

경로 테이블을 만든 후에 ASE 서브넷에서 설정해야 합니다.  

## <a name="get-your-management-addresses-from-api"></a>API에서 관리 주소 가져오기 ##

다음 API 호출을 사용하여 ASE와 일치하는 관리 주소를 나열할 수 있습니다.

```http
get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01
```

이 API는 ASE에 대한 모든 인바운드 주소를 포함하는 JSON 문서를 반환합니다. 주소 목록에는 관리 주소, 즉, ASE 및 ASE 서브넷 주소 범위 자체에서 사용하는 VIP가 포함됩니다.  

[armclient](https://github.com/projectkudu/ARMClient)를 사용하여 API를 호출하려면 다음 명령을 사용하되 구독 ID, 리소스 그룹 및 ASE 이름을 바꿔야 합니다.  

```azurepowershell-interactive
armclient login
armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01
```

<!--IMAGES-->
[1]: ./media/management-addresses/managementaddr-nsg.png

<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
