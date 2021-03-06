---
title: Azure Blockchain Workbench 문제 해결
description: Azure Blockchain 워크 벤치 미리 보기 응용 프로그램 문제를 해결 하는 방법입니다.
ms.date: 10/14/2019
ms.topic: troubleshooting
ms.reviewer: brendal
ms.openlocfilehash: 20c0f9bdd6f820a73b1ba6660de805268c0d8714
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212856"
---
# <a name="azure-blockchain-workbench-preview-troubleshooting"></a>Azure Blockchain 워크 벤치 미리 보기 문제 해결

개발자 디버깅 또는 지원을 돕는 데 PowerShell 스크립트를 사용할 수 있습니다. 스크립트는 요약을 생성하고 문제 해결을 위한 자세한 로그를 수집합니다. 수집된 로그에는 다음이 포함됩니다.

* Ethereum과 같은 블록체인 네트워크
* Blockchain Workbench 마이크로 서비스
* Application Insights
* Azure 모니터링 (Azure Monitor 로그)

다음 단계를 확인하고 문제의 근본 원인을 확인하는 데 정보를 사용할 수 있습니다.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="troubleshooting-script"></a>문제 해결 스크립트

GitHub에서 PowerShell 문제 해결 스크립트를 사용할 수 있습니다. GitHub에서 [zip 파일을 다운로드](https://github.com/Azure-Samples/blockchain/archive/master.zip)하거나 샘플을 복제합니다.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>스크립트 실행
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

`collectBlockchainWorkbenchTroubleshooting.ps1` 스크립트를 실행하여 로그를 수집하고 문제 해결 정보의 폴더가 포함된 ZIP 파일을 만듭니다. 예를 들어:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
스크립트는 다음 매개 변수를 수락합니다.

| 매개 변수  | 설명 | 필요한 공간 |
|---------|---------|----|
| 구독 ID | 모든 리소스를 만들거나 찾는 SubscriptionID입니다. | 예 |
| ResourceGroupName | Blockchain Workbench가 배포된 Azure 리소스 그룹의 이름입니다. | 예 |
| OutputDirectory | 출력 ZIP 파일을 생성하는 경로입니다. 지정하지 않으면 기본값은 현재 디렉터리입니다. | 아니요 |
| LookbackHours | 원격 분석을 끌어올 때 사용할 시간 수입니다. 기본값은 24시간입니다. 최댓값은 90시간입니다. | 아니요 |
| OmsSubscriptionId | Azure Monitor 로그가 배포 되는 구독 ID입니다. Blockchain 네트워크에 대 한 Azure Monitor 로그가 Blockchain 워크 벤치의 리소스 그룹 외부에 배포 되는 경우에만이 매개 변수를 전달 합니다.| 아니요 |
| OmsResourceGroup |Azure Monitor 로그가 배포 되는 리소스 그룹입니다. Blockchain 네트워크에 대 한 Azure Monitor 로그가 Blockchain 워크 벤치의 리소스 그룹 외부에 배포 되는 경우에만이 매개 변수를 전달 합니다.| 아니요 |
| OmsWorkspaceName | Log Analytics 작업 영역 이름입니다. Blockchain 네트워크에 대 한 Azure Monitor 로그가 Blockchain 워크 벤치의 리소스 그룹 외부에 배포 되는 경우에만이 매개 변수를 전달 합니다. | 아니요 |

## <a name="what-is-collected"></a>수집되는 항목은?

출력 ZIP 파일에는 다음 폴더 구조가 포함됩니다.

| 폴더 또는 파일 | 설명  |
|---------|---------|
| \Summary.txt | 시스템의 요약 |
| \Metrics\blockchain | 블록체인에 대한 메트릭 |
| \Metrics\Workbench | 워크벤치에 대한 메트릭 |
| \Details\Blockchain | 블록체인에 대한 자세한 로그 |
| \Details\Workbench | 블록체인에 대한 자세한 로그 |

요약 파일에는 애플리케이션의 전반적인 상태와 애플리케이션의 상태에 대한 스냅샷을 제공합니다. 요약에서는 실행하는 서비스에 대한 권장되는 작업, 강조 표시된 최상위 오류 및 메타데이터를 제공합니다.

**메트릭** 폴더는 시간에 따라 다양한 시스템 구성 요소의 메트릭을 포함합니다. 예를 들어 출력 파일 `\Details\Workbench\apiMetrics.txt`는 다양한 응답 코드 및 컬렉션 기간 동안 응답 시간의 요약을 포함합니다. **세부 정보** 폴더는 워크벤치 또는 기본 블록체인 네트워크의 특정 문제 해결을 위한 자세한 로그를 포함합니다. 예를 들어 `\Details\Workbench\Exceptions.csv`는 시스템에서 발생한 가장 최근 예외의 목록을 포함합니다. 이는 스마트 계약의 오류를 해결하거나 블록체인과 상호 작용하는 데 유용합니다. 

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench Application Insights 문제 해결 가이드](https://aka.ms/workbenchtroubleshooting)
