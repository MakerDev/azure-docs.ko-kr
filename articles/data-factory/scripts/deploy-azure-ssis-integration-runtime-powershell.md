---
title: PowerShell을 사용 하 여 Azure SSIS 통합 런타임 배포
description: 이 PowerShell 스크립트는 클라우드에서 SSIS 패키지를 실행할 수 있는 Azure-SSIS 통합 런타임을 만듭니다.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.author: sawinark
author: swinarko
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 09/12/2017
ms.openlocfilehash: c6d9a9299fa25013a440ee6ac45f5eae407225b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74929815"
---
# <a name="powershell-script---deploy-azure-ssis-integration-runtime"></a>PowerShell 스크립트 ‑ Azure‑SSIS 통합 런타임 배포

이 샘플 PowerShell 스크립트는 Azure에서 SSIS 패키지를 실행할 수 있는 Azure-SSIS 통합 런타임을 만듭니다.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/data-factory/deploy-azure-ssis-integration-runtime/deploy-azure-ssis-integration-runtime.ps1 "Deploy Azure-SSIS Integration Runtime")]

## <a name="clean-up-deployment"></a>배포 정리

샘플 스크립트를 실행한 후에는 다음 명령을 사용하여 리소스 그룹 및 해당 그룹에 연결된 모든 리소스를 제거할 수 있습니다.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
리소스 그룹에서 Data Factory를 제거하려면 다음 명령을 실행합니다. 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다.

| 명령 | 메모 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | 데이터 팩터리를 만듭니다. |
| [AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) | 클라우드에서 SSIS 패키지를 실행할 수 있는 Azure-SSIS 통합 런타임을 만듭니다. |
| [AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) | Azure-SSIS 통합 런타임을 시작합니다. |
| [AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/get-Azdatafactoryv2integrationruntime) | Azure-SSIS 통합 런타임에 대한 정보를 가져옵니다. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |
|||

## <a name="next-steps"></a>다음 단계

Azure PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](https://docs.microsoft.com/powershell/)를 참조하세요.

추가 Azure Data Factory PowerShell 스크립트 샘플은 [Azure Data Factory PowerShell 샘플](../samples-powershell.md)에 있습니다.
