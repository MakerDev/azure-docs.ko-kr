---
title: Azure HDInsight 클러스터용 Vm 다시 부팅
description: HDInsight 클러스터에 대해 응답 하지 않는 Vm을 다시 부팅 하는 방법을 알아봅니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: c0f0bd9eb423b3de6a602647dff93fd9fce6e13e
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077017"
---
# <a name="reboot-vms-for-hdinsight-cluster"></a>HDInsight 클러스터용 Vm 다시 부팅

HDInsight 클러스터는 클러스터 노드로 Vm 그룹을 포함 합니다. 장기 실행 클러스터의 경우 이러한 노드는 다양 한 이유로 응답 하지 않을 수 있습니다. 이 문서에서는 HDInsight 클러스터에서 응답 하지 않는 Vm을 다시 부팅 하는 방법을 설명 합니다.

## <a name="when-to-reboot"></a>다시 부팅 해야 하는 경우

> [!WARNING]  
> 클러스터에서 Vm을 다시 부팅 하면 노드의 가동 중지 시간이 발생 하 고 노드에서 서비스가 다시 시작 됩니다. 

노드가 다시 부팅 되는 동안 클러스터가 비정상 상태가 되 면 작업이 느려지거나 실패할 수 있습니다. 활성 헤드 노드를 다시 부팅 하는 경우 실행 중인 모든 작업이 중단 되 고 서비스가 실행 되 고 다시 실행 될 때까지 클러스터에 작업을 제출할 수 없습니다. 필요한 경우에만 Vm을 다시 부팅 하는 것을 고려해 야 합니다. 다음은 Vm 다시 부팅을 고려해 야 하는 경우에 대 한 몇 가지 지침입니다.

- 노드에는 SSH를 사용할 수 없지만 ping에 응답 합니다.
- Ambari UI의 하트 비트가 없으면 작업자 노드가 다운 됩니다.
- 임시 디스크가 노드에 꽉 찬 경우
- VM의 프로세스 테이블에는 프로세스가 완료 된 많은 항목이 있지만 "종료 된 상태"로 나열 됩니다.

> [!WARNING]  
> **HBase** 및 **kafka** Clustes에 대 한 vm을 다시 부팅 하는 경우 데이터가 손실 될 수 있기 때문에 더 주의 해야 합니다.

## <a name="use-powershell-to-reboot-vms"></a>PowerShell을 사용 하 여 Vm 다시 부팅

노드 재부팅 작업을 사용 하는 데 필요한 두 단계는 노드 나열 및 노드 다시 시작입니다.

1. 노드를 나열 합니다. [AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost)를 통해 클러스터 노드 목록을 가져올 수 있습니다. 

  ```
  Get-AzHDInsightHost -ClusterName myclustername
  ```

2. 호스트를 다시 시작 합니다. 다시 부팅 하려는 노드의 이름을 가져온 후 [AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)를 사용 하 여 노드를 다시 시작 합니다.

  ```
  Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
  ```

## <a name="use-rest-api-to-reboot-vms"></a>REST API를 사용 하 여 Vm 다시 부팅

API 문서에서 **사용해 보기** 기능을 사용 하 여 HDInsight에 요청을 보낼 수 있습니다. 노드 재부팅 작업을 사용 하는 데 필요한 두 단계는 노드 나열 및 노드 다시 시작입니다.

1. 노드를 나열 합니다. REST API 또는 Ambari에서 클러스터 노드 목록을 가져올 수 있습니다. 자세한 내용은 [HDInsight 목록 호스트 REST API 작업](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts)에서 찾을 수 있습니다.

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

2. 호스트를 다시 시작 합니다. 다시 부팅 하려는 노드의 이름을 가져온 후 노드 다시 시작을 사용 하 여 노드를 다시 부팅 REST API 합니다. 노드 이름은 **"NodeType (w)/hn/zk/gw)" + "x" + "first 6 characters of cluster name"** 패턴을 따릅니다. 자세한 내용은 [HDInsight 다시 시작 호스트 REST API 작업](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts)에서 찾을 수 있습니다.

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

다시 부팅 하려는 노드의 실제 이름은 요청 본문의 JSON 배열에 지정 됩니다.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>다음 단계

* [다시 시작-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [HDInsight 가상 컴퓨터 REST API](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [HDInsight REST API](https://docs.microsoft.com/rest/api/hdinsight/)
