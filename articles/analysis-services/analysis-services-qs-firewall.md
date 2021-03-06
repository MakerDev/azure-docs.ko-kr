---
title: 빠른 시작 - Azure Analysis Services 서버 방화벽 구성 | Microsoft Docs
description: 이 빠른 시작은 Azure Portal을 사용하여 Azure Analysis Services 서버에 대한 방화벽을 구성하도록 도와줍니다.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 48618815519fad31bff5d6a8d2d2edc82535f437
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697905"
---
# <a name="quickstart-configure-server-firewall---portal"></a>빠른 시작: 서버 방화벽 구성 - 포털

이 빠른 시작은 Azure Analysis Services 서버에 대한 방화벽을 구성하도록 도와줍니다. 서버에 액세스하는 해당 컴퓨터만을 위한 방화벽 활성화 및 IP 주소 범위 구성은 서버 및 데이터 보안의 중요한 부분입니다.

## <a name="prerequisites"></a>사전 요구 사항

- 구독의 Azure Analysis Services 서버 자세한 내용은 [빠른 시작: 서버 만들기 - 포털](analysis-services-create-server.md) 또는 [빠른 시작: 서버 만들기 - PowerShell](analysis-services-create-powershell.md)을 참조하세요.
- 클라이언트 컴퓨터에 대한 하나 이상의 IP 주소 범위(필요한 경우)
- 데이터 가져오기(새로 고침) 및 페이지가 매겨진 보고서를 비롯하여 Power BI 프리미엄이 Azure Analysis Services에 연결되는 일부 시나리오는 Power BI에서 액세스 허용이 설정된 경우에도 현재 지원되지 않습니다. Power BI 프리미엄에서 Live Connect를 사용하는 보다 일반적인 시나리오가 지원됩니다. 모든 Power BI Pro 시나리오가 지원됩니다.


## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인 

[포털에 로그인](https://portal.azure.com)

## <a name="configure-a-firewall"></a>방화벽 구성

1. 서버를 클릭하여 개요 페이지를 엽니다. 
2. **설정** > **방화벽** > **방화벽 설정**에서 **켜기**를 클릭합니다.
3. Power BI 서비스에서 DirectQuery 액세스를 허용하려면 **Power BI에서 액세스 허용**에서 **켜기**를 클릭합니다.  
4. (선택 사항) 하나 이상의 IP 주소 범위를 지정합니다. 각 범위에 대한 이름, 시작 및 끝 IP 주소를 입력합니다. 방화벽 규칙 이름은 128자로 제한되고 대문자, 소문자, 숫자, 밑줄 및 하이픈만 포함할 수 있습니다. 공백 및 기타 특수 문자는 허용되지 않습니다.
5. **저장**을 클릭합니다.

     ![방화벽 설정](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>리소스 정리

더 이상 필요하지 않은 경우 IP 주소 범위를 삭제하거나 방화벽을 비활성화합니다.

## <a name="next-steps"></a>다음 단계
이 빠른 시작에서는 서버에 대한 방화벽을 구성하는 방법을 알아보았습니다. 이제 서버가 있고 방화벽으로 보호했으므로 포털에서 기본 샘플 데이터 모델을 추가할 수 있습니다. 모델 데이터베이스 역할 구성 및 클라이언트 연결 테스트에 대해 알아보는 데 샘플 모델이 있으면 유용합니다. 자세히 알아보려면 샘플 모델 추가를 위한 자습서를 계속합니다.

> [!div class="nextstepaction"]
> [자습서: 서버에 샘플 모델 추가](analysis-services-create-sample-model.md)
