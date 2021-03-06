---
title: Azure 가상 머신 확장 집합에 대한 네트워킹
description: Azure 가상 머신 확장 집합에 대 한 고급 네트워킹 속성 중 일부를 구성 하는 방법입니다.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: networking
ms.date: 06/25/2020
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 6113ee61d4949649b65607c0f1bd606be4edb2ac
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837162"
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Azure 가상 머신 확장 집합에 대한 네트워킹

포털을 통해 Azure 가상 머신 확장 집합을 배포하면 인바운드 NAT 규칙이 있는 Azure Load Balancer와 같은 특정 네트워크 속성이 기본값으로 설정됩니다. 이 문서에서는 확장 집합을 사용하여 구성할 수 있는 고급 네트워킹 기능 중 일부를 사용하는 방법에 대해 설명합니다.

이 문서에서 다루는 모든 기능은 Azure Resource Manager 템플릿을 사용하여 구성할 수 있습니다. Azure CLI 및 PowerShell 예제도 선택한 기능에 포함되어 있습니다.

## <a name="accelerated-networking"></a>가속 네트워킹
Azure 가속 네트워킹은 가상 머신에서 SR-IOV(단일 루트 I/O 가상화)를 사용하도록 설정하여 네트워킹 성능을 향상시킵니다. 가속 네트워킹 사용에 대한 자세한 내용은 [Windows](../virtual-network/create-vm-accelerated-networking-powershell.md) 또는 [Linux](../virtual-network/create-vm-accelerated-networking-cli.md) 가상 머신에 대한 가속 네트워킹을 참조하세요. 확장 집합에서 가속 네트워킹을 사용하려면 확장 집합의 networkInterfaceConfigurations 설정에서 enableAcceleratedNetworking을 **true**로 설정합니다. 예를 들어:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="azure-virtual-machine-scale-sets-with-azure-load-balancer"></a>Azure Load Balancer를 사용 하는 Azure 가상 머신 확장 집합

가상 머신 확장 집합 및 부하 분산 장치를 사용 하는 경우 다음 항목을 고려해 야 합니다.

* **여러 가상 머신 확장 집합은 동일한 부하 분산 장치를 사용할 수 없습니다**.
* **포트 전달 및 인바운드 NAT 규칙**:
  * 각 가상 머신 확장 집합에는 인바운드 NAT 규칙이 있어야 합니다.
  * 확장 집합을 만든 후에는 부하 분산 장치의 상태 프로브에서 사용 하는 부하 분산 규칙에 대해 백 엔드 포트를 수정할 수 없습니다. 포트를 변경 하려면 Azure 가상 머신 확장 집합을 업데이트 하 고, 포트를 업데이트 한 후 상태 프로브를 다시 구성 하 여 상태 프로브를 제거할 수 있습니다.
  * 부하 분산 장치의 백 엔드 풀에 있는 가상 머신 확장 집합을 사용 하는 경우 기본 인바운드 NAT 규칙이 자동으로 만들어집니다.
* **인바운드 NAT 풀**:
  * 인바운드 NAT 풀은 인바운드 NAT 규칙의 컬렉션입니다. 하나의 인바운드 NAT 풀에서 여러 가상 머신 확장 집합을 지원할 수 없습니다.
* **부하 분산 규칙**:
  * 부하 분산 장치의 백 엔드 풀에 있는 가상 머신 확장 집합을 사용 하는 경우 기본 부하 분산 규칙이 자동으로 만들어집니다.
* **아웃 바운드 규칙**:
  *  부하 분산 규칙에서 이미 참조 하는 백 엔드 풀에 대 한 아웃 바운드 규칙을 만들려면 인바운드 부하 분산 규칙을 만들 때 먼저 포털에서 **"암시적 아웃 바운드 규칙 만들기"** 를 **아니요** 로 표시 해야 합니다.

  :::image type="content" source="./media/vmsslb.png" alt-text="부하 분산 규칙 만들기" border="true":::

다음 메서드는 기존 Azure 부하 분산 장치를 사용 하 여 가상 머신 확장 집합을 배포 하는 데 사용할 수 있습니다.

* [Azure Portal를 사용 하 여 기존 Azure Load Balancer를 사용 하 여 가상 머신 확장 집합을 구성](../load-balancer/configure-vm-scale-set-portal.md)합니다.
* [Azure PowerShell를 사용 하 여 기존 Azure Load Balancer를 사용 하 여 가상 머신 확장 집합을 구성](../load-balancer/configure-vm-scale-set-powershell.md)합니다.
* [Azure CLI를 사용 하 여 기존 Azure Load Balancer를 사용 하 여 가상 머신 확장 집합을 구성](../load-balancer/configure-vm-scale-set-cli.md)합니다.

## <a name="create-a-scale-set-that-references-an-application-gateway"></a>Application Gateway를 참조하는 확장 집합 만들기
애플리케이션 게이트웨이를 사용하는 확장 집합을 만들려면 이 ARM 템플릿 구성과 같이 설정된 확장의 ipConfigurations 섹션에서 애플리케이션 게이트웨이의 백 엔드 주소 풀을 참조합니다.

```json
"ipConfigurations": [{
  "name": "{config-name}",
  "properties": {
  "subnet": {
    "id": "{subnet-id}"
  },
  "ApplicationGatewayBackendAddressPools": [{
    "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/applicationGateways/{gateway-name}/backendAddressPools/{pool-name}"
  }]
}]
```

>[!NOTE]
> 애플리케이션 게이트웨이는 확장 집합과 동일한 가상 네트워크에 있어야 하지만 확장 집합과 다른 서브넷에 있어야 합니다.


## <a name="configurable-dns-settings"></a>구성 가능한 DNS 설정
기본적으로 확장 집합은 생성한 VNET 및 서브넷의 특정 DNS 설정을 사용하지만, 확장 집합에 대한 DNS 설정을 직접 구성할 수 있습니다.

### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>구성 가능한 DNS 서버가 포함된 확장 집합 만들기
Azure CLI를 사용하여 사용자 지정 DNS 구성이 포함된 확장 집합을 만들려면 **vmss create** 명령에 **--dns-servers** 인수를 추가한 다음 공백으로 구분된 서버 IP 주소를 추가합니다. 예를 들어:

```bash
--dns-servers 10.0.0.6 10.0.0.5
```

Azure 템플릿에서 사용자 지정 DNS 서버를 구성하려면 networkInterfaceConfigurations 확장 집합 섹션에 dnsSettings 속성을 추가합니다. 예를 들어:

```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>구성 가능한 가상 머신 도메인 이름이 포함된 확장 집합 만들기
CLI를 사용하여 가상 머신에 대한 사용자 지정 DNS 이름이 포함된 확장 집합을 만들려면 **virtual machine scale set create** 명령에 **--vm-domain-name** 인수를 추가한 다음, 도메인 이름을 나타내는 문자열을 추가합니다.

Azure 템플릿에서 도메인 이름을 설정하려면 **networkInterfaceConfigurations** 확장 집합 섹션에 **dnsSettings** 속성을 추가합니다. 예를 들어:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": true,
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

개별 가상 컴퓨터 DNS 이름의 출력 형식은 다음과 같습니다.

```output
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a>가상 머신당 공용 IPv4
일반적으로 Azure 확장 집합 가상 머신에는 자체의 공용 IP 주소가 필요하지 않습니다. 대부분의 시나리오에서는 공용 IP 주소를 부하 분산 장치 또는 개별 가상 머신 (jumpbox 라고도 함)에 연결 하 고, 필요한 경우 (예: 인바운드 NAT 규칙을 통해) 확장 집합 가상 머신으로 들어오는 연결을 라우팅하는 것이 더 경제적 이며 안전 합니다.

그러나 일부 시나리오의 경우 확장 집합 가상 머신에는 자체의 공용 IP 주소가 필요합니다. 게임 물리 처리를 수행하는 클라우드 가상 머신에 콘솔을 직접 연결해야 하는 게임이 그 예입니다. 또 다른 예로 가상 머신이 분산된 데이터베이스의 여러 지역에서 서로를 외부 연결해야 하는 경우가 있습니다.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>가상 머신별 공용 IP가 포함된 확장 집합 만들기
CLI를 사용하여 각 가상 머신에 공용 IP 주소를 할당하는 확장 집합을 만들려면 **vmss create** 명령에 **--public-ip-per-vm** 매개 변수를 추가합니다. 

Azure 템플릿을 사용하여 확장 집합을 만들려면 Microsoft.Compute/virtualMachineScaleSets 리소스의 API 버전이 적어도 **2017-03-30**인지 확인하고, ipConfigurations 확장 집합 섹션에 **publicIpAddressConfiguration** JSON 속성을 추가합니다. 예를 들어:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```

템플릿 예제: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-the-public-ip-addresses-of-the-virtual-machines-in-a-scale-set"></a>확장 집합에 있는 가상 머신의 공용 IP 주소 쿼리
CLI를 사용하여 확장 집합 가상 머신에 할당된 공용 IP 주소를 나열하려면 **az vmss list-instance-public-ips** 명령을 사용합니다.

PowerShell을 사용하여 확장 집합 공용 IP 주소를 나열하려면 _Get-AzPublicIpAddress_ 명령을 사용합니다. 예를 들어:

```powershell
Get-AzPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

공용 IP 주소 구성의 리소스 ID를 직접 참조하여 공용 IP 주소를 쿼리할 수도 있습니다. 예를 들어:

```powershell
Get-AzPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

확장 집합 가상 머신에 할당된 공용 IP 주소를 쿼리하려면 [Azure Resource Explorer](https://resources.azure.com) 또는 **2017-03-30** 버전 이상의 Azure REST API를 사용할 수 있습니다.

[Azure Resource Explorer](https://resources.azure.com)를 쿼리하려면

1. 웹 브라우저에서 [Azure Resource Explorer](https://resources.azure.com)를 엽니다.
1. 옆의 *+* 를 클릭하여 왼쪽에 *구독*을 확장합니다. *구독* 아래 항목이 하나뿐이면 이미 확장되어 있습니다.
1. 구독을 확장합니다.
1. 리소스 그룹을 확장합니다.
1. *공급자*를 확장합니다.
1. *Microsoft.Compute*를 확장합니다.
1. *virtualMachineScaleSets*를 확장합니다.
1. 확장 집합을 확장합니다.
1. *publicipaddresses*를 클릭합니다.

Azure REST API를 쿼리하려면

```bash
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

[Azure Resource Explorer](https://resources.azure.com) 예제 출력 및 Azure REST API:

```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>NIC당 여러 IP 주소
확장 집합의 VM에 연결된 모든 NIC에는 하나 이상의 IP 구성이 연결되어 있습니다. 각 구성에는 하나의 개인 IP 주소가 할당됩니다. 각 구성에는 연결된 하나의 공용 IP 주소 리소스가 있을 수도 있습니다. NIC에 할당할 수 있는 IP 주소 수와 Azure 구독에서 사용할 수 있는 공용 IP 주소 수를 이해하려면 [Azure 제한](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)을 참조하세요.

## <a name="multiple-nics-per-virtual-machine"></a>가상 컴퓨터당 여러 NIC
컴퓨터 크기에 따라 가상 머신당 최대 8개 NIC를 포함할 수 있습니다. 컴퓨터당 최대 NIC 수는 [VM 크기 문서](../virtual-machines/sizes.md)에서 확인할 수 있습니다. VM 인스턴스에 연결된 모든 NIC는 동일한 가상 네트워크에 연결해야 합니다. NIC는 다른 서브넷에 연결할 수 있지만, 모든 서브넷은 동일한 가상 네트워크에 속해야 합니다.

다음 예제는 가상 머신마다 여러 개의 NIC 항목과 여러 개의 공용 IP를 보여 주는 확장 집합 네트워크 프로필입니다.

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": true,
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": false,
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg--asgs-per-scale-set"></a>확장 집합당 NSG 및 ASG
[네트워크 보안 그룹](../virtual-network/security-overview.md)을 사용하면 보안 규칙을 사용하여 Azure 가상 네트워크에서 Azure 리소스와 주고 받는 트래픽을 필터링할 수 있습니다. [애플리케이션 보안 그룹](../virtual-network/security-overview.md#application-security-groups)을 사용하면 Azure 리소스의 네트워크 보안을 처리하고 애플리케이션 구조의 확장으로 그룹화할 수 있습니다.

네트워크 보안 그룹은 확장 집합 가상 머신 속성의 네트워크 인터페이스 구성 섹션에 참조를 추가하여 확장 집합에 직접 적용할 수 있습니다.

애플리케이션 보안 그룹도 확장 집합 가상 머신 속성의 네트워크 인터페이스 구성 섹션에 참조를 추가하여 확장 집합에 직접 지정할 수 있습니다.

예를 들어:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": true,
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            },
                            "applicationSecurityGroups": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/applicationSecurityGroups/', variables('asgName'))]"
                                }
                            ],
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

확장 집합과 네트워크 보안 그룹이 연결되었는지 확인하려면 `az vmss show` 명령을 사용합니다. 아래 예제에서는 `--query`를 사용하여 결과를 필터링하고 출력의 관련 섹션만 표시합니다.

```azurecli
az vmss show \
    -g myResourceGroup \
    -n myScaleSet \
    --query virtualMachineProfile.networkProfile.networkInterfaceConfigurations[].networkSecurityGroup

[
  {
    "id": "/subscriptions/.../resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/nsgName",
    "resourceGroup": "myResourceGroup"
  }
]
```

애플리케이션 보안 그룹이 확장 집합과 연결되었는지 확인하려면 `az vmss show` 명령을 사용합니다. 아래 예제에서는 `--query`를 사용하여 결과를 필터링하고 출력의 관련 섹션만 표시합니다.

```azurecli
az vmss show \
    -g myResourceGroup \
    -n myScaleSet \
    --query virtualMachineProfile.networkProfile.networkInterfaceConfigurations[].ipConfigurations[].applicationSecurityGroups

[
  [
    {
      "id": "/subscriptions/.../resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationSecurityGroups/asgName",
      "resourceGroup": "myResourceGroup"
    }
  ]
]
```



## <a name="next-steps"></a>다음 단계
Azure 가상 네트워크에 대한 자세한 내용은 [Azure 가상 네트워크 개요](../virtual-network/virtual-networks-overview.md)를 참조하세요.