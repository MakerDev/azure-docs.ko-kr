---
title: '자습서: 장애 조치 (failover) 그룹에 데이터베이스 추가'
description: Azure Portal, PowerShell 또는 Azure CLI를 사용 하 여 Azure SQL Database의 데이터베이스를 autofailover 조치 (failover) 그룹에 추가 합니다.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1, devx-track-azurecli
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 4caad36d21d3facb97dc358fdfee61e89c420213
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496341"
---
# <a name="tutorial-add-an-azure-sql-database-to-an-autofailover-group"></a>자습서: autofailover 그룹에 Azure SQL Database 추가
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[장애 조치 (failover) 그룹](auto-failover-group-overview.md) 은 여러 지역에서 복제 된 데이터베이스를 그룹화 하는 데 사용할 수 있는 선언적 추상화 계층입니다. Azure SQL Database에 대해 장애 조치 (failover) 그룹을 구성 하 고 Azure Portal, PowerShell 또는 Azure CLI를 사용 하 여 장애 조치 (failover)를 테스트 하는 방법을 알아봅니다.  이 자습서에서 학습할 방법은 다음과 같습니다.

> [!div class="checklist"]
>
> - Azure SQL Database에서 데이터베이스 만들기
> - 두 서버 간에 데이터베이스의 장애 조치 (failover) 그룹을 만듭니다.
> - 테스트 장애 조치 (failover)

## <a name="prerequisites"></a>필수 구성 요소

# <a name="the-portal"></a>[포털](#tab/azure-portal)

이 자습서를 완료하려면 다음이 설치되어 있어야 합니다.

- Azure 구독 아직 없는 경우 [무료 계정을 만듭니다](https://azure.microsoft.com/free/) .

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

자습서를 완료 하려면 다음 항목이 있는지 확인 합니다.

- Azure 구독 아직 없는 경우 [무료 계정을 만듭니다](https://azure.microsoft.com/free/) .
- [Azure PowerShell](/powershell/azure/)

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

자습서를 완료 하려면 다음 항목이 있는지 확인 합니다.

- Azure 구독 아직 없는 경우 [무료 계정을 만듭니다](https://azure.microsoft.com/free/) .
- 최신 버전의 [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)입니다.

---

## <a name="1---create-a-database"></a>1-데이터베이스 만들기

[!INCLUDE [sql-database-create-single-database](../includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2-장애 조치 (failover) 그룹 만들기

이 단계에서는 기존 서버와 다른 지역의 새 서버 간에 [장애 조치 (failover) 그룹](auto-failover-group-overview.md) 을 만듭니다. 그런 다음 장애 조치(failover) 그룹에 샘플 데이터베이스를 추가합니다.

# <a name="the-portal"></a>[포털](#tab/azure-portal)

장애 조치 (failover) 그룹을 만들고 Azure Portal를 사용 하 여 데이터베이스에 데이터베이스를 추가 합니다.

1. [Azure Portal](https://portal.azure.com)의 왼쪽 메뉴에서 **Azure SQL** 을 선택 합니다. **AZURE sql** 이 목록에 없는 경우 **모든 서비스**를 선택한 다음 검색 상자에 azure sql을 입력 합니다. (선택 사항) **Azure SQL** 옆의 별표를 선택하여 즐겨찾기로 선택하고 왼쪽 탐색에 항목으로 추가합니다.
1. 섹션 1에서 만든 데이터베이스 (예:)를 선택 `mySampleDatabase` 합니다.
1. 서버 수준에서 장애 조치 (Failover) 그룹을 구성할 수 있습니다. 서버 **이름** 아래에서 서버 이름을 선택 하 여 서버에 대 한 설정을 엽니다.

   ![데이터베이스에 대 한 서버 열기](./media/failover-group-add-single-database-tutorial/open-sql-db-server.png)

1. **설정** 창에서 **장애 조치 (failover) 그룹** 을 선택 하 고 **그룹 추가** 를 선택 하 여 새 장애 조치 그룹을 만듭니다.

   ![새 장애 조치 (failover) 그룹 추가](./media/failover-group-add-single-database-tutorial/sqldb-add-new-failover-group.png)

1. **장애 조치 (Failover) 그룹** 페이지에서 다음 값을 입력 하거나 선택한 다음 **만들기**를 선택 합니다.

   - **장애 조치 (Failover) 그룹 이름**: 고유한 장애 조치 (failover) 그룹 이름 (예:)을 입력 `failovergrouptutorial` 합니다.
   - **보조 서버**: *필수 설정을 구성* 하는 옵션을 선택한 다음 **새 서버를 만들도록**선택 합니다. 또는 기존 서버를 보조 서버로 선택할 수도 있습니다. 다음 값을 입력 한 후 **선택**을 선택 합니다.
      - **서버 이름**: 보조 서버에 대 한 고유한 이름 (예:)을 입력 `mysqlsecondary` 합니다.
      - **서버 관리자 로그인**: 유형`azureuser`
      - **암호**: 암호 요구 사항을 충족 하는 복잡 한 암호를 입력 합니다.
      - **위치**: 드롭다운 (예:)에서 위치를 선택 `East US` 합니다. 이 위치는 주 서버와 동일한 위치가 될 수 없습니다.

     > [!NOTE]
     > 서버 로그인 및 방화벽 설정은 주 서버의 이름과 일치 해야 합니다.

     ![장애 조치 (failover) 그룹에 대 한 보조 서버 만들기](./media/failover-group-add-single-database-tutorial/create-secondary-failover-server.png)

   - **그룹 내의 데이터베이스**: 보조 서버를 선택 하면이 옵션의 잠금이 해제 됩니다. 이를 선택 하 여 **추가할 데이터베이스를 선택한** 다음 섹션 1에서 만든 데이터베이스를 선택 합니다. 장애 조치 (failover) 그룹에 데이터베이스를 추가 하면 지역에서 복제 프로세스가 자동으로 시작 됩니다.

   ![장애 조치 (failover) 그룹에 SQL Database 추가](./media/failover-group-add-single-database-tutorial/add-sqldb-to-failover-group.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell을 사용 하 여 장애 조치 (failover) 그룹을 만들고 데이터베이스에 데이터베이스를 추가 합니다.

   > [!NOTE]
   > 서버 로그인 및 방화벽 설정은 주 서버의 이름과 일치 해야 합니다.

   ```powershell-interactive
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "West US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $databaseName = "mySampleDatabase"
   $drLocation = "East US"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # The ip address range that you want to allow to access your server
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName
   Write-host "Failover group name is" $failoverGroupName

   # Create a secondary server in the failover region
   Write-host "Creating a secondary server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule

   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup

   # Add the database to the failover group
   Write-host "Adding the database to the failover group..."
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..."
   ```

자습서의이 부분에서는 다음 PowerShell cmdlet을 사용 합니다.

| 명령 | 메모 |
|---|---|
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | 단일 데이터베이스 및 탄력적 풀을 호스트 하는 Azure SQL Database에 서버를 만듭니다. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Azure SQL Database 서버에 대 한 방화벽 규칙을 만듭니다. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Azure SQL Database에서 새 단일 데이터베이스를 만듭니다. |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Azure SQL Database에서 새 장애 조치 (failover) 그룹을 만듭니다. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Azure SQL Database에서 하나 이상의 데이터베이스를 가져옵니다. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Azure SQL Database의 장애 조치 (failover) 그룹에 하나 이상의 데이터베이스를 추가 합니다. |

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

장애 조치 (failover) 그룹을 만들고 Azure CLI를 사용 하 여 데이터베이스에 데이터베이스를 추가 합니다.

   > [!NOTE]
   > 서버 로그인 및 방화벽 설정은 주 서버의 이름과 일치 해야 합니다.

   ```azurecli-interactive
   #!/bin/bash
   # set variables
   $failoverLocation = "West US"
   $failoverServer = "failoverServer-$randomIdentifier"
   $failoverGroup = "failoverGroup-$randomIdentifier"

   echo "Creating a secondary server in the DR region..."
   az sql server create --name $failoverServer --resource-group $resourceGroup --location $failoverLocation --admin-user $login --admin-password $password

   echo "Creating a failover group between the two servers..."
   az sql failover-group create --name $failoverGroup --partner-server $failoverServer --resource-group $resourceGroup --server $server --add-db $database --failover-policy Automatic
   ```

자습서의이 부분에서는 다음 Azure CLI cmdlet을 사용 합니다.

| 명령 | 메모 |
|---|---|
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | 데이터베이스 및 탄력적 풀을 호스트하는 서버를 만듭니다. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | 서버의 방화벽 규칙을 만듭니다. |
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | 장애 조치(failover) 그룹을 만듭니다. |

---

## <a name="3---test-failover"></a>3-장애 조치 (failover) 테스트

이 단계에서는 장애 조치 (failover) 그룹을 보조 서버로 장애 조치 (failover) 한 다음 Azure Portal를 사용 하 여 장애 복구 (failback) 합니다.

# <a name="the-portal"></a>[포털](#tab/azure-portal)

Azure Portal를 사용 하 여 테스트 장애 조치 (failover)

1. [Azure Portal](https://portal.azure.com)의 왼쪽 메뉴에서 **Azure SQL** 을 선택 합니다. **AZURE sql** 이 목록에 없는 경우 **모든 서비스**를 선택한 다음 검색 상자에 azure sql을 입력 합니다. (선택 사항) **Azure SQL** 옆의 별표를 선택하여 즐겨찾기로 선택하고 왼쪽 탐색에 항목으로 추가합니다.
1. 섹션 2에서 만든 데이터베이스 (예:)를 선택 `mySampleDatbase` 합니다.
1. 서버 **이름** 아래에서 서버 이름을 선택 하 여 서버에 대 한 설정을 엽니다.

   ![데이터베이스에 대 한 서버 열기](./media/failover-group-add-single-database-tutorial/open-sql-db-server.png)

1. **설정** 창에서 **장애 조치 (failover) 그룹** 을 선택 하 고 섹션 2에서 만든 장애 조치 (failover) 그룹을 선택 합니다.
  
   ![포털에서 장애 조치 (failover) 그룹을 선택 합니다.](./media/failover-group-add-single-database-tutorial/select-failover-group.png)

1. 주 서버 및 보조 서버를 검토 합니다.
1. 작업 창에서 **장애 조치 (Failover)** 를 선택 하 여 예제 데이터베이스가 포함 된 장애 조치 (failover) 그룹을 장애 조치 합니다.
1. TDS 세션의 연결을 끊을 것을 알리는 경고에서 **예** 를 선택 합니다.

   ![데이터베이스를 포함 하는 장애 조치 (failover) 그룹 장애 조치 (failover)](./media/failover-group-add-single-database-tutorial/failover-sql-db.png)

1. 현재 주 서버와 보조 서버가 있는 서버를 검토 합니다. 장애 조치 (failover)에 성공 하면 두 서버에 교환 된 역할이 있어야 합니다.
1. **장애 조치 (Failover)** 를 다시 선택 하 여 서버가 원래 역할로 다시 장애 조치 (Failover) 합니다.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell을 사용 하 여 테스트 장애 조치 (failover)

보조 복제본의 역할을 확인 합니다.

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...."
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

보조 서버로 장애 조치 (failover):

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to" $drServerName
   ```

장애 조치(failover) 그룹을 주 서버로 되돌립니다.

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully back to" $serverName
   ```

자습서의이 부분에서는 다음 PowerShell cmdlet을 사용 합니다.

| 명령 | 메모 |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Database 장애 조치(failover) 그룹을 가져오거나 나열합니다. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Database 장애 조치(failover) 그룹의 장애 조치(failover)를 실행합니다. |

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI를 사용 하 여 테스트 장애 조치 (failover)

보조 서버를 확인 합니다.

   ```azurecli-interactive
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list --server $server --resource-group $resourceGroup
   ```

보조 서버로 장애 조치 (failover):

   ```azurecli-interactive
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $failoverServer
   echo "Successfully failed failover group over to" $failoverServer
   ```

장애 조치(failover) 그룹을 주 서버로 되돌립니다.

   ```azurecli-interactive
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $server
   echo "Successfully failed failover group back to" $server
   ```

자습서의이 부분에서는 다음 Azure CLI cmdlet을 사용 합니다.

| 명령 | 메모 |
|---|---|
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | 서버에 있는 장애 조치(failover) 그룹을 나열합니다. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | 현재 주 서버의 모든 데이터베이스를 장애 조치(failover)하여 주 장애 조치(failover) 그룹을 설정합니다. |

---

## <a name="clean-up-resources"></a>리소스 정리

리소스 그룹을 삭제 하 여 리소스를 정리 합니다.

# <a name="the-portal"></a>[포털](#tab/azure-portal)

Azure Portal를 사용 하 여 리소스 그룹을 삭제 합니다.

1. [Azure Portal](https://portal.azure.com)에서 리소스 그룹으로 이동합니다.
1. 리소스 그룹 자체 뿐만 아니라 그룹의 모든 리소스를 삭제 하려면 **리소스 그룹 삭제** 를 선택 합니다.
1. 텍스트 상자에 리소스 그룹의 이름을 입력 하 `myResourceGroup` 고 **삭제** 를 선택 하 여 리소스 그룹을 삭제 합니다.  

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell을 사용 하 여 리소스 그룹을 삭제 합니다.

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

자습서의이 부분에서는 다음 PowerShell cmdlet을 사용 합니다.

| 명령 | 메모 |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 리소스 그룹을 제거합니다. |

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI를 사용 하 여 리소스 그룹을 삭제 합니다.

   ```azurecli-interactive
   echo "Cleaning up resources by removing the resource group..."
   az group delete --name $resourceGroup
   echo "Successfully removed resource group" $resourceGroup
   ```

자습서의이 부분에서는 다음 Azure CLI cmdlet을 사용 합니다.

| 명령 | 메모 |
|---|---|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

---

> [!IMPORTANT]
> 리소스 그룹을 유지 하지만 보조 데이터베이스를 삭제 하려는 경우에는 장애 조치 (failover) 그룹을 삭제 하기 전에 해당 그룹에서 제거 합니다. 장애 조치 (failover) 그룹에서 보조 데이터베이스를 제거 하기 전에 삭제 하면 예기치 않은 동작이 발생할 수 있습니다.

## <a name="full-scripts"></a>전체 스크립트

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-ps.ps1 "Add database to a failover group")]

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Azure SQL Database에서 단일 데이터베이스 및 탄력적 풀을 호스트 하는 서버를 만듭니다. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Azure SQL Database 서버에 대 한 방화벽 규칙을 만듭니다. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Azure SQL Database에서 새 데이터베이스를 만듭니다. |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Azure SQL Database에서 새 장애 조치 (failover) 그룹을 만듭니다. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Azure SQL Database에서 하나 이상의 데이터베이스를 가져옵니다. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Azure SQL Database의 장애 조치 (failover) 그룹에 하나 이상의 데이터베이스를 추가 합니다. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Database에서 장애 조치 (failover) 그룹을 가져오거나 나열 합니다. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Database 장애 조치 (failover) 그룹의 장애 조치 (failover)를 실행 합니다. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Azure SQL Database에서 리소스 그룹을 제거 합니다.|

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add database to a failover group")]

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) | 현재 활성 구독이 되도록 구독을 설정합니다. |
| [az group create](/cli/azure/group#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Azure SQL Database에서 단일 데이터베이스 및 탄력적 풀을 호스트 하는 서버를 만듭니다. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Azure SQL Database에서 서버 수준 IP 방화벽 규칙을 만듭니다. |
| [az sql db create](/cli/azure/sql/db?view=azure-cli-latest) | Azure SQL Database에서 데이터베이스를 만듭니다. |
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Azure SQL Database에서 장애 조치 (failover) 그룹을 만듭니다. |
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Azure SQL Database 서버의 장애 조치 (failover) 그룹을 나열 합니다. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | 현재 주 서버의 모든 데이터베이스를 장애 조치(failover)하여 주 장애 조치(failover) 그룹을 설정합니다. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

# <a name="the-portal"></a>[포털](#tab/azure-portal)

Azure Portal 사용할 수 있는 스크립트가 없습니다.

---

[Azure PowerShell](powershell-script-content-guide.md) 및 [Azure CLI](az-cli-script-samples-content-guide.md)에서 다른 Azure SQL Database 스크립트를 찾을 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 장애 조치 (failover) 그룹에 Azure SQL Database 데이터베이스를 추가 하 고 장애 조치 (failover)를 테스트 했습니다. 구체적으로 다음 작업 방법을 알아보았습니다.

> [!div class="checklist"]
>
> - Azure SQL Database에서 데이터베이스 만들기
> - 두 서버 간에 데이터베이스의 장애 조치 (failover) 그룹을 만듭니다.
> - 테스트 장애 조치 (failover)

탄력적 풀을 장애 조치 (failover) 그룹에 추가 하는 방법에 대 한 다음 자습서로 이동 합니다.

> [!div class="nextstepaction"]
> [자습서: 장애 조치 (failover) 그룹에 Azure SQL Database 탄력적 풀 추가](failover-group-add-elastic-pool-tutorial.md)
