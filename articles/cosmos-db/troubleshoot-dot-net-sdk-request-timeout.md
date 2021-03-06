---
title: .NET SDK를 사용 하 여 HTTP 408 Azure Cosmos DB 또는 요청 시간 제한 문제 해결
description: .NET SDK 요청 시간 제한 예외를 진단 하 고 해결 하는 방법
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 09442e01fa160d3851169a51230fa4cbef7e0980
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88118572"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout"></a>.NET SDK 요청 시간 제한 Azure Cosmos DB 진단 및 문제 해결
HTTP 408 오류는 SDK가 시간 제한이 발생 하기 전에 요청을 완료할 수 없는 경우에 발생 합니다.

## <a name="customizing-the-timeout-on-the-azure-cosmos-net-sdk"></a>Azure Cosmos .NET SDK에서 시간 제한 사용자 지정

SDK에는 제한 시간을 제어 하는 두 가지 다른 대안이 있으며 각 항목은 서로 다른 범위를 사용 합니다.

### <a name="requesttimeout"></a>RequestTimeout

`CosmosClientOptions.RequestTimeout`(또는 `ConnectionPolicy.RequestTimeout` SDK V2) 구성을 사용 하면 각 개별 네트워크 요청에 영향을 주는 시간 제한을 설정할 수 있습니다.  사용자가 시작한 작업은 여러 네트워크 요청에 걸쳐 있을 수 있으며 (예: 제한이 있을 수 있음)이 구성은 다시 시도할 때 각 네트워크 요청에 적용 됩니다. 종단 간 작업 요청 시간이 초과 되지 않습니다.

### <a name="cancellationtoken"></a>CancellationToken

SDK의 모든 비동기 작업에는 선택적 CancellationToken 매개 변수가 있습니다. 이 [CancellationToken](https://docs.microsoft.com/dotnet/standard/threading/how-to-listen-for-cancellation-requests-by-polling) 는 모든 네트워크 요청에서 전체 작업 전체에 사용 됩니다. 네트워크 요청 사이에서 CancellationToken가 확인 되 고 관련 토큰이 만료 되 면 작업이 취소 될 수 있습니다. CancellationToken는 작업 범위에서 예상 되는 대략적인 시간 제한을 정의 하는 데 사용 해야 합니다.

> [!NOTE]
> CancellationToken는 [잘못 된 상태를 발생 시 키](https://devblogs.microsoft.com/premier-developer/recommended-patterns-for-cancellationtoken/)지 않을 때 라이브러리가 취소를 확인 하는 메커니즘입니다. 취소가 정의 된 시간이 되 면 작업은 취소 되지 않을 수 있지만 시간이 지난 후에는이 작업을 수행 하는 것이 안전 하다 면 취소 됩니다.

## <a name="troubleshooting-steps"></a>문제 해결 단계
다음 목록에는 요청 시간 제한 예외에 대 한 알려진 원인과 해결 방법이 나와 있습니다.

### <a name="1-high-cpu-utilization-most-common-case"></a>1. 높은 CPU 사용률 (가장 일반적인 경우)
최적 대기 시간을 위해 CPU 사용량을 약 40%로 지정 하는 것이 좋습니다. 최대 (평균) CPU 사용률을 모니터링 하는 간격으로 10 초를 사용 하는 것이 좋습니다. CPU 급증은 단일 쿼리에 대해 여러 연결을 수행할 수 있는 크로스 분할 쿼리를 사용 하는 것 보다 일반적입니다.

#### <a name="solution"></a>해결책:
SDK를 사용 하는 클라이언트 응용 프로그램을 확장/축소 해야 합니다.

### <a name="2-socket--port-availability-might-be-low"></a>2. 소켓/포트 가용성이 낮을 수 있습니다.
Azure에서 실행 하는 경우 .NET SDK를 사용 하는 클라이언트는 Azure SNAT (PAT) 포트 고갈에 도달할 수 있습니다.

#### <a name="solution-1"></a>해결 방법 1:
Azure Vm에서 실행 하는 경우 [SNAT 포트 소모 가이드](troubleshoot-dot-net-sdk.md#snat)를 따르세요.

#### <a name="solution-2"></a>해결 방법 2:
Azure App Service에서 실행 하는 경우 [연결 오류 문제 해결 가이드](../app-service/troubleshoot-intermittent-outbound-connection-errors.md#cause) 를 따르고 [App Service 진단을 사용](https://azure.github.io/AppService/2018/03/01/Deep-Dive-into-TCP-Connections-in-App-Service-Diagnostics.html)합니다.

#### <a name="solution-3"></a>해결 방법 3:
Azure Functions에서를 실행 하는 경우 관련 된 모든 서비스 (Cosmos DB 포함)에 대 한 단일/정적 클라이언트를 유지 관리 하는 [Azure Functions 권장 사항을](../azure-functions/manage-connections.md#static-clients) 따르고 함수 앱 호스트의 형식 및 크기에 따라 [서비스 제한을](../azure-functions/functions-scale.md#service-limits) 확인 합니다.

#### <a name="solution-4"></a>해결 방법 4:
HTTP 프록시를 사용하는 경우 SDK `ConnectionPolicy`에서 구성된 연결 수를 지원할 수 있는지 확인합니다.
그렇지 않으면 연결 문제가 발생할 수 있습니다.

### <a name="3-creating-multiple-client-instances"></a>3. 여러 클라이언트 인스턴스 만들기
여러 클라이언트 인스턴스를 만들면 연결 경합 및 시간 제한 문제가 발생할 수 있습니다.

#### <a name="solution"></a>해결책:
[성능 팁](performance-tips-dotnet-sdk-v3-sql.md#sdk-usage)을 따르고 전체 프로세스에서 단일 CosmosClient 인스턴스를 사용 합니다.

### <a name="4-hot-partition-key"></a>4. 핫 파티션 키
Azure Cosmos DB는 전체 프로 비전 된 처리량을 실제 파티션에 균등 하 게 분산 합니다. 핫 파티션이 있으면 실제 파티션에 있는 하나 이상의 논리 파티션 키가 모든 물리적 파티션의 r o s/s를 사용 하는 반면, 다른 실제 파티션은 사용 하지 않는 상태가 됩니다. 증상으로, 사용 된 총 r u/초는 데이터베이스 또는 컨테이너에서 전체 프로 비전 된 r u/초 보다 작으므로 핫 논리 파티션 키에 대 한 요청에 대 한 제한 (429s)이 계속 표시 됩니다. 정규화 된 작업 부하 [메트릭을](monitor-normalized-request-units.md) 사용 하 여 작업에 핫 파티션이 발생 하는지 확인 합니다. 

#### <a name="solution"></a>해결책:
요청 볼륨과 저장소를 균등 하 게 분산 하는 좋은 파티션 키를 선택 합니다. [파티션 키를 변경](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/)하는 방법에 대해 알아봅니다.

### <a name="5-high-degree-of-concurrency"></a>5. 높은 동시성 수준
응용 프로그램에서 동시성 수준이 높으면 채널 경합이 발생할 수 있습니다.

#### <a name="solution"></a>해결책:
SDK를 사용 하는 클라이언트 응용 프로그램을 확장/축소 해야 합니다.

### <a name="6-large-requests-andor-responses"></a>6. 대량 요청 및/또는 응답
대량 요청이 나 응답은 비교적 낮은 동시성을 사용 하는 경우에도 채널 및 악화 경합의 줄이 차단 될 수 있습니다.

#### <a name="solution"></a>해결책:
SDK를 사용 하는 클라이언트 응용 프로그램을 확장/축소 해야 합니다.

### <a name="7-failure-rate-is-within-cosmos-db-sla"></a>7. 실패율이 Cosmos DB SLA 내에 있습니다.
응용 프로그램은 일시적인 오류를 처리 하 고 필요한 경우 다시 시도할 수 있습니다. 408 만들기 경로에서는 서비스에서 항목을 만들었는지 또는 그렇지 않은 경우에는 알 수 없으므로 예외가 다시 시도 되지 않습니다. 생성을 위해 동일한 항목을 다시 보내면 충돌 예외가 발생 합니다. 사용자 응용 프로그램 비즈니스 논리에는 충돌을 처리 하는 사용자 지정 논리가 있을 수 있습니다 .이는 기존 항목의 모호성 및 만들기 재시도와 충돌 하지 않습니다.

### <a name="8-failure-rate-is-violating-the-cosmos-db-sla"></a>8. 실패율이 Cosmos DB SLA를 위반 합니다.
Azure 지원에 문의 하세요.

## <a name="next-steps"></a>다음 단계
* Azure Cosmos DB .NET SDK를 사용 하는 경우 문제 [진단 및 해결](troubleshoot-dot-net-sdk.md)
* [.Net V3](performance-tips-dotnet-sdk-v3-sql.md) 및 [.net V2](performance-tips.md) 에 대 한 성능 지침 알아보기
