---
title: Azure Automation에서 변경 내용 추적 및 인벤토리 관리
description: 이 문서에서는 변경 내용 추적 및 인벤토리를 사용하여 사용자 환경에서 소프트웨어 및 Microsoft 서비스 변경 내용을 추적하는 방법을 설명합니다.
services: automation
ms.subservice: change-inventory-management
ms.date: 06/15/2020
ms.topic: conceptual
ms.openlocfilehash: eab509e389c074232526aa93fcebb72f3bc986c0
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185605"
---
# <a name="manage-change-tracking-and-inventory"></a>변경 내용 추적 및 인벤토리 관리

추적할 새 파일 또는 레지스트리 키를 추가 하면 Azure Automation [변경 내용 추적 및 인벤토리에](change-tracking.md)사용할 수 있습니다. 이 문서에서는 추적을 구성 하 고, 추적 결과를 검토 하 고, 변경 내용이 검색 되 면 경고를 처리 하는 방법을 설명 합니다.

이 문서의 절차를 사용 하기 전에 다음 방법 중 하나를 사용 하 여 Vm에 대 한 변경 내용 추적 및 인벤토리를 사용 하도록 설정 했는지 확인 합니다.

* [Automation 계정에서 변경 내용 추적 및 인벤토리 사용](automation-enable-changes-from-auto-acct.md)
* [Azure Portal를 탐색 하 여 변경 내용 추적 및 인벤토리를 사용 하도록 설정](automation-enable-changes-from-browse.md)
* [Runbook에서 변경 내용 추적 및 인벤토리 사용](automation-enable-changes-from-runbook.md)
* [Azure VM에서 변경 내용 추적 및 인벤토리 사용](automation-enable-changes-from-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>배포 범위 제한

변경 내용 추적 및 인벤토리는 작업 영역 내의 범위 구성을 사용하고 변경 내용을 수신할 컴퓨터를 대상으로 합니다. 자세한 내용은 [제한 변경 내용 추적 및 인벤토리 배포 범위](automation-scope-configurations-change-tracking.md)를 참조 하세요.

## <a name="track-files"></a>파일 추적

변경 내용 추적 및 인벤토리를 사용 하 여 파일 및 폴더/디렉터리에 대 한 변경 내용을 추적할 수 있습니다. 이 섹션에서는 Windows 및 Linux에서 파일 추적을 구성 하는 방법을 설명 합니다.

### <a name="configure-file-tracking-on-windows"></a>Windows에서 파일 추적 구성

다음 단계를 사용하여 Windows 컴퓨터에서 파일 추적을 구성합니다.

1. Automation 계정의 **구성 관리** 아래에서 **변경 내용 추적**을 선택합니다. 
2. **설정 편집**(기어 기호)을 클릭합니다.
3. 작업 영역 구성 페이지에서 **Windows 파일**을 선택한 다음 **+ 추가**를 클릭하여 추적할 새 파일을 추가합니다.
4. 변경 내용 추적에 대 한 Windows 파일 추가 창에서 추적할 파일이 나 폴더에 대 한 정보를 입력 하 고 **저장**을 클릭 합니다. 다음 표에는 정보에 사용할 수 있는 속성이 정의되어 있습니다.

    |속성  |설명  |
    |---------|---------|
    |사용     | 설정이 적용되면 True이고, 그렇지 않으면 False입니다.        |
    |Item Name     | 추적할 파일의 이름입니다.        |
    |그룹     | 논리적으로 파일을 그룹화하는 그룹 이름입니다.        |
    |경로 입력     | 파일을 확인할 경로입니다(예: **c:\temp\\\*.txt**). `%winDir%\System32\\\*.*`와 같은 환경 변수를 사용할 수도 있습니다.       |
    |경로 유형     | 경로의 유형입니다. 가능한 값은 파일 및 폴더입니다.        |    
    |재귀     | 추적할 항목을 찾을 때 재귀가 사용되면 True이고, 그렇지 않으면 False입니다.        |    
    |파일 콘텐츠 업로드 | 추적된 변경 내용에 대한 파일 콘텐츠를 업로드하려면 True이고, 그렇지 않으면 False입니다.|

5. **파일 콘텐츠 업로드**에 대해 True를 지정해야 합니다. 이 설정은 표시된 파일 경로에 대해 파일 콘텐츠 추적을 사용합니다.

### <a name="configure-file-tracking-on-linux"></a>Linux에서 파일 추적 구성

다음 단계를 사용하여 Linux 컴퓨터에서 추적할 파일을 구성합니다.

1. Automation 계정의 **구성 관리** 아래에서 **변경 내용 추적**을 선택합니다. 
2. **설정 편집**(기어 기호)을 클릭합니다.
3. 작업 영역 구성 페이지에서 **Linux 파일**을 선택한 다음 **+ 추가**를 클릭하여 추적할 새 파일을 추가합니다.
4. 변경 내용 추적에 대해 Linux 파일 추가 창에서 추적할 파일이나 디렉터리의 정보를 입력하고 **저장**을 클릭합니다. 다음 표에는 정보에 사용할 수 있는 속성이 정의되어 있습니다.

    |속성  |설명  |
    |---------|---------|
    |사용     | 설정이 적용되면 True이고, 그렇지 않으면 False입니다.        |
    |Item Name     | 추적할 파일의 이름입니다.        |
    |그룹     | 논리적으로 파일을 그룹화하는 그룹 이름입니다.        |
    |경로 입력     | 파일을 확인할 경로입니다(예: **/etc/*.conf**).       |
    |경로 유형     | 경로의 유형입니다. 가능한 값은 파일 및 디렉터리입니다.        |
    |재귀     | 추적할 항목을 찾을 때 재귀가 사용되면 True이고, 그렇지 않으면 False입니다.        |
    |sudo 사용     | 항목을 확인할 때 sudo를 사용하려면 True이고, 그렇지 않으면 False입니다.         |
    |링크     | 디렉터리를 트래버스할 때 바로 가기 링크를 처리하는 방법을 결정하는 설정입니다. 가능한 값은 다음과 같습니다.<br> 무시 - 바로 가기 링크가 무시되고 참조된 파일/디렉터리가 포함되지 않습니다.<br>실행 - 재귀 중에 바로 가기 링크를 따르고 참조된 파일/디렉터리도 포함합니다.<br>관리-기호화 된 링크를 따르고 반환 된 콘텐츠의 변경을 허용 합니다.<br>**참고:** 파일 콘텐츠 검색을 지원 하지 않으므로 관리 옵션을 권장 하지 않습니다.    |
    |파일 콘텐츠 업로드 | 추적된 변경 내용에 대한 파일 콘텐츠를 업로드하려면 True이고, 그렇지 않으면 False입니다. |

5. **파일 콘텐츠 업로드**에 대해 True를 지정해야 합니다. 이 설정은 표시된 파일 경로에 대해 파일 콘텐츠 추적을 사용합니다.

   ![Linux 파일 추가](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="track-file-contents"></a>파일 콘텐츠 추적

파일 콘텐츠 추적을 사용하면 추적된 변경 전후의 파일 콘텐츠를 볼 수 있습니다. 이 기능은 각 변경이 발생 한 후 [저장소 계정](../storage/common/storage-account-overview.md) 에 파일 내용을 저장 합니다. 다음은 파일 콘텐츠를 추적하기 위해 따라야 하는 몇 가지 규칙입니다.

* 파일 콘텐츠를 저장하려면 Resource Manager 배포 모델을 사용하는 표준 스토리지 계정이 필요합니다. 
* 프리미엄 및 클래식 배포 모델 스토리지 계정을 사용하지 마세요. [Azure Storage 계정 정보](../storage/common/storage-account-create.md)를 참조하세요.
* 저장소 계정을 하나의 Automation 계정에만 연결할 수 있습니다.
* Automation 계정에서 [변경 내용 추적 및 인벤토리](change-tracking.md) 를 사용 하도록 설정 해야 합니다.

### <a name="enable-tracking-for-file-content-changes"></a>파일 콘텐츠 변경 내용 추적 사용 설정

파일 콘텐츠 변경 내용 추적을 사용 하도록 설정 하려면 다음 단계를 사용 합니다.

1. Azure Portal에서 Automation 계정을 연 다음 **구성 관리** 아래에서 **변경 내용 추적**을 선택합니다.
2. **설정 편집**(기어 기호)을 클릭합니다.
3. **파일 콘텐츠**를 선택하고 **연결**을 클릭합니다. 그러면 변경 내용 추적을 위한 콘텐츠 위치 추가 창이 열립니다.

   ![콘텐츠 위치 추가](./media/change-tracking-file-contents/enable.png)

4. 파일 콘텐츠를 저장하는 데 사용할 구독 및 스토리지 계정을 선택합니다. 

5. 기존의 모든 추적된 파일에 대해 파일 콘텐츠 추적을 사용하려면 **모든 설정에 대한 파일 콘텐츠 업로드**에 대해 **켜기**를 선택합니다. 나중에 각 파일 경로에 대해 이 설정을 변경할 수 있습니다.

   ![스토리지 계정 설정](./media/change-tracking-file-contents/storage-account.png)

6. 변경 내용 추적 및 인벤토리는 파일 콘텐츠 변경 내용 추적을 사용하는 경우 스토리지 계정 및 SAS(공유 액세스 서명) URI를 표시합니다. 서명은 365일 후 만료되고 **다시 생성**을 클릭하여 다시 만들 수 있습니다.

   ![계정 키 나열](./media/change-tracking-file-contents/account-keys.png)

### <a name="view-the-contents-of-a-tracked-file"></a>추적된 파일의 콘텐츠 보기

변경 내용 추적 및 인벤토리가 추적된 파일에 대한 변경 내용을 감지하면 변경 내용 정보 창에서 파일 콘텐츠를 볼 수 있습니다.  

![변경 내용 나열](./media/change-tracking-file-contents/change-list.png)

1. Azure Portal에서 Automation 계정을 연 다음 **구성 관리** 아래에서 **변경 내용 추적**을 선택합니다.

2. 변경 내용 목록에서 파일을 선택하고 **파일 콘텐츠 변경 내용 보기**를 선택하여 파일 콘텐츠를 확인합니다. 변경 세부 정보 창에는 각 속성의 이전 및 이후 파일 정보에 대 한 표준 정보가 표시 됩니다.

   ![변경 세부 정보](./media/change-tracking-file-contents/change-details.png)

3. 파일 콘텐츠를 나란히 보기에서 볼 수 있습니다. **인라인**을 선택하여 변경 내용을 인라인 보기에 표시할 수도 있습니다.

## <a name="track-registry-keys"></a>레지스트리 키 추적

다음 단계를 사용하여 Windows 컴퓨터에서 추적할 레지스트리 키를 구성합니다.

1. Azure Portal에서 Automation 계정을 연 다음 **구성 관리** 아래에서 **변경 내용 추적**을 선택합니다. 
2. **설정 편집**(기어 기호)을 클릭합니다.
3. 작업 영역 구성 페이지에서 **Windows 레지스트리**를 선택합니다.
4. **+ 추가**를 클릭하여 추적할 새 레지스트리 키를 추가합니다.
5. 변경 내용 추적에 대해 Windows 레지스트리 추가 창에서 추적할 키에 대한 정보를 입력한 다음 **저장**을 클릭합니다. 다음 표에는 정보에 사용할 수 있는 속성이 정의되어 있습니다.

    |속성  |설명  |
    |---------|---------|
    |사용     | 설정이 적용되면 True이고, 그렇지 않으면 False입니다.        |
    |Item Name     | 추적할 레지스트리 키의 식별 이름입니다.        |
    |그룹     | 논리적으로 레지스트리 키를 그룹화하는 그룹 이름입니다.        |
    |Windows 레지스트리 키   | 경로가 있는 키 이름입니다 (예:) `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup` .      |

## <a name="search-logs-for-change-records"></a>변경 레코드에 대한 로그 검색

변경 레코드에 대한 Azure Monitor 로그에서 다양한 검색을 수행할 수 있습니다. 변경 내용 추적 페이지가 열려 있는 상태에서 **Log Analytics**를 클릭하여 로그 페이지를 엽니다. 다음 표에서는 변경 레코드에 대한 로그 검색 샘플을 제공합니다.

|쿼리  |Description  |
|---------|---------|
|`ConfigurationData`<br> &#124; `where ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"`<br> &#124; `where SvcState == "Stopped"`<br> &#124; `summarize arg_max(TimeGenerated, *) by SoftwareName, Computer`         | 자동으로 설정되었지만 중지됨으로 보고된 Microsoft 서비스에 대한 최근의 인벤토리 레코드를 표시합니다. 결과는 지정된 소프트웨어 이름 및 컴퓨터에 대한 최신 레코드로 제한됩니다.    |
|`ConfigurationChange`<br> &#124; `where ConfigChangeType == "Software" and ChangeCategory == "Removed"`<br> &#124; `order by TimeGenerated desc`|제거된 소프트웨어에 대한 변경 레코드를 표시합니다.|

## <a name="create-alerts-on-changes"></a>변경에 대한 경고 만들기

다음 예제에서는 **c:\windows\system32\drivers\etc\hosts** 파일이 컴퓨터에서 수정 된 것을 보여 줍니다. 이 파일은 Windows가 호스트 이름을 IP 주소로 확인하는 데 사용하기 때문에 중요합니다. 이 작업은 DNS보다 우선하므로 연결 문제가 발생할 수 있습니다. 악의적인 또는 위험한 웹 사이트로 트래픽 리디렉션이 발생할 수도 있습니다.

![호스트 파일 변경을 보여 주는 차트](./media/change-tracking-file-contents/changes.png)

이 예제를 사용하여 변경에 대한 경고를 만드는 단계를 설명하겠습니다.

1. Automation 계정의 **구성 관리** 아래에서 **변경 내용 추적**을 선택한 다음 **Log Analytics**를 선택합니다. 
2. 로그 검색에서 `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"` 쿼리를 사용하여 **hosts** 파일의 콘텐츠 변경 내용을 찾습니다. 이 쿼리는 단어를 포함 하는 정규화 된 경로 이름으로 파일에 대 한 콘텐츠 변경 내용을 찾습니다 `hosts` . 경로 부분을 정규화된 형식으로 변경하여(예: `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`를 사용) 특정 파일을 요청할 수도 있습니다.

3. 쿼리가 결과를 반환한 후 로그 검색에서 **새 경고 규칙** 을 클릭 하 여 경고 만들기 페이지를 엽니다. Azure Portal에서 **Azure Monitor**를 통해 이 페이지로 이동할 수도 있습니다. 

4. 쿼리를 다시 확인하고 경고 논리를 수정합니다. 여기서는 환경의 모든 컴퓨터에서 변경 내용이 여러 개 검색되면 경고를 트리거할 수 있습니다.

    ![hosts 파일 변경 내용을 추적하기 위한 쿼리의 변경](./media/change-tracking-file-contents/change-query.png)

5. 경고 논리를 설정한 후에는 경고의 트리거에 대 한 응답으로 작업을 수행 하는 작업 그룹을 할당 합니다. 이 경우 이메일을 보내도록 설정하고 ITSM(IT 서비스 관리) 티켓을 만듭니다. 

    ![변경 시 경고하도록 작업 그룹 구성](./media/change-tracking/action-groups.png)

## <a name="next-steps"></a>다음 단계

* 범위 구성에 대 한 자세한 내용은 [제한 변경 내용 추적 및 인벤토리 배포 범위](automation-scope-configurations-change-tracking.md)를 참조 하세요.
* Log Analytics 작업 영역에 저장된 로그를 검색해야 하는 경우 [Azure Monitor 로그에서 로그 검색](../azure-monitor/log-query/log-query-overview.md)을 참조하세요.
* 배포가 완료 되 면 [변경 내용 추적 및 인벤토리에 대해 Automation 계정에서 작업 영역 연결 해제](automation-unlink-workspace-change-tracking.md)를 참조 하세요.
* 변경 내용 추적 및 인벤토리에서 Vm을 삭제 하려면 [변경 내용 추적 및 인벤토리에서 Vm 제거](automation-remove-vms-from-change-tracking.md)를 참조 하세요.
* 기능 오류를 해결하려면 [변경 내용 추적 및 인벤토리 문제 해결](troubleshoot/change-tracking.md)을 참조하세요.
