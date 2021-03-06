---
title: 모든 Azure Security Center 권장 사항에 대한 참조 테이블
description: 이 문서에는 리소스를 보호하는 데 도움이 되는 Azure Security Center의 보안 권장 사항이 나열되어 있습니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2020
ms.author: memildin
ms.openlocfilehash: 9df4ac8b9dc3385868a72d31da93bec00eeb57bf
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88042873"
---
# <a name="security-recommendations---a-reference-guide"></a>보안 권장 사항 - 참조 가이드

이 문서에는 Azure Security Center에서 볼 수 있는 권장 사항이 나열되어 있습니다. 환경에 표시되는 권장 사항은 보호하는 리소스 및 사용자 지정된 구성에 따라 다릅니다.

Security Center 권장 사항은 모범 사례를 기반으로 합니다. 일부는 일반적인 규정 준수 프레임 워크를 기반으로 하는 보안 및 규정 준수 모범 사례에 대 한 Microsoft에서 작성 한 Azure 관련 지침 인 **Azure 보안 벤치 마크**에 맞춰져 있습니다. [Azure 보안 벤치 마크에 대해 자세히 알아보세요](https://docs.microsoft.com/azure/security/benchmarks/introduction).

이러한 권장 사항에 대응하는 방법에 대한 자세한 내용은 [Azure Security Center의 권장 사항 해결](security-center-remediate-recommendations.md)을 참조하세요.

보안 점수는 완료한 Security Center 권장 사항의 수를 기준으로 계산됩니다. 먼저 해결할 권장 사항을 결정하려면 각 권장 사항의 심각도와 보안 점수에 미치는 잠재적 영향을 살펴보면 됩니다.

>[!TIP]
> 권장 사항의 설명에 "관련 정책 없음"이라고 표시되는 경우가 있는데, 이는 권장 사항이 다른 권장 사항 및 *해당* 정책에 따라 달라지기 때문입니다. 예를 들어 "엔드포인트 보호 상태 오류를 수정해야 합니다..."라는 내용의 권장 사항은 엔드포인트 보호 솔루션의 *설치* 여부를 확인하는 권장 사항("엔드포인트 보호 솔루션을 설치해야 합니다...")에 의존합니다. 기본 권장 사항에는 정책이 *있습니다*. 정책을 기본 권장 사항으로 제한하면 정책 관리가 간단해집니다.

## <a name="network-recommendations"></a><a name="recs-network"></a>네트워크 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**적응형 네트워크 강화 권장 사항은 인터넷에 연결된 가상 머신에 적용해야 합니다.**|적응형 네트워크 강화 기능이 허용 범위가 과도하게 큰 NSG 규칙을 발견하면 표준 가격 책정 계층의 고객에게 이 권장 사항이 표시됩니다.<br>(관련 정책: 인터넷 연결 가상 머신에 적응형 네트워크 강화 권장 사항을 적용해야 함)|높음|N|가상 머신|
|**VM에 연결된 NSG에서 모든 네트워크 포트를 제한해야 함**|기존 허용 규칙의 액세스를 제한하여 인터넷 연결 VM의 네트워크 보안 그룹을 강화합니다.<br>이 권장 사항은 포트가 *모든* 원본에 대해 열려 있을 때 트리거됩니다(22, 3389, 5985, 5986, 80 및 1443 포트는 제외).<br>(관련 정책: 인터넷 연결 엔드포인트를 통한 액세스를 제한)|높음|N|가상 머신|
|**DDoS Protection 표준을 사용하도록 설정해야 합니다.**|DDoS 차단 서비스 표준을 사용하도록 설정하여 공용 IP를 사용하는 애플리케이션이 포함된 가상 네트워크를 보호해야 합니다. DDoS 차단을 사용하여 네트워크 볼륨 및 프로토콜 공격을 완화할 수 있습니다.<br>(관련 정책: DDoS Protection 표준을 사용하도록 설정해야 함)|높음|N|가상 네트워크|
|**함수 앱은 HTTPS를 통해서만 액세스할 수 있어야 합니다.**|함수 앱에 "HTTPS 전용" 액세스를 사용합니다. HTTPS를 사용하여 서버/서비스 인증을 보장하고 전송 중인 데이터를 네트워크 계층 도청 공격으로부터 보호합니다.<br>(관련 정책: HTTPS를 통해서만 함수 앱에 액세스할 수 있어야 함)|중간|**예**|함수 앱|
|**인터넷 연결 가상 머신은 네트워크 보안 그룹과 함께 보호되어야 합니다.**|가상 머신의 네트워크 액세스를 제어하기 위해 네트워크 보안 그룹을 활성화합니다.<br>(관련 정책: 네트워크 보안 그룹을 사용하여 인터넷 연결 가상 머신을 보호해야 함)|높음/보통|N|가상 머신|
|**네트워크 보안 그룹을 사용하여 비인터넷 연결 가상 머신을 보호해야 함**|    NSG (네트워크 보안 그룹)로 액세스를 제한 하 여 잠재적인 위협 으로부터 인터넷에 연결 되지 않은 가상 컴퓨터를 보호 합니다.<br>NSGs에는 ACL (액세스 제어 목록)이 포함 되어 있으며 VM의 NIC 또는 서브넷에 할당할 수 있습니다. ACL 규칙은 할당 된 리소스에 대 한 네트워크 트래픽을 허용 하거나 거부 합니다.<br>(관련 정책: 인터넷에 연결 되지 않은 가상 머신은 네트워크 보안 그룹으로 보호 해야 함)|낮음|N|가상 머신|
|**가상 머신에서 IP 전달을 사용하지 않도록 설정해야 함**|IP 전달을 사용하지 않도록 설정합니다. 가상 머신의 NIC에서 IP 전달을 사용하도록 설정하면 머신이 다른 대상으로 주소가 지정된 트래픽을 수신할 수 있습니다. IP 전달이 필요한 상황은 매우 드물기 때문에(예: VM을 네트워크 가상 어플라이언스로 사용하는 경우) 네트워크 보안 팀에서 검토해야 합니다.<br>(관련 정책: [미리 보기]: 가상 머신에서 IP 전달을 사용하지 않도록 설정해야 함)|중간|N|가상 머신|
|**가상 머신의 관리 포트는 Just-In-Time 네트워크 액세스 제어로 보호해야 함**|JIT(Just-In-Time) VM(가상 머신) 액세스 제어를 적용하여 선택한 포트에 대한 액세스를 영구적으로 잠그고, 권한 있는 사용자가 제한된 시간 동안만 JIT를 통해 열 수 있게 합니다.<br>(관련 정책: 가상 머신의 관리 포트는 Just-In-Time 네트워크 액세스 제어로 보호해야 함)|높음|N|가상 머신|
|**가상 머신에서 관리 포트를 닫아야 합니다.**|관리 포트에 대한 액세스를 제한하도록 가상 머신의 네트워크 보안 그룹을 강화합니다.<br>(관련 정책: 가상 머신에서 관리 포트를 닫아야 함)|높음|N|가상 머신|
|**스토리지 계정에 보안 전송을 사용하도록 설정해야 함**|스토리지 계정에 보안 전송 사용 보안 전송은 사용자의 스토리지 계정이 보안 연결(HTTPS)에서 오는 요청만 수락하도록 강제 적용하는 옵션입니다. HTTPS를 사용하여 서버와 서비스 간 인증을 보장하고 전송 중인 데이터를 메시지 가로채기(man-in-the-middle), 도청 및 세션 하이재킹과 같은 네트워크 계층 공격으로부터 보호합니다.<br>(관련 정책: 스토리지 계정에 보안 전송을 사용하도록 설정해야 함)|높음|**예**|스토리지 계정|
|**서브넷을 네트워크 보안 그룹과 연결해야 합니다.**|서브넷에 배포된 리소스의 네트워크 액세스를 제어하도록 네트워크 보안 그룹을 사용합니다.<br>(관련 정책: 서브넷을 네트워크 보안 그룹과 연결해야 함.<br>이 정책은 기본적으로 사용하지 않도록 설정됨)|높음/보통|N|서브넷|
|**웹 애플리케이션은 HTTPS를 통해서만 액세스할 수 있어야 합니다.**|웹 애플리케이션에 "HTTPS 전용" 액세스를 사용합니다. HTTPS를 사용하여 서버/서비스 인증을 보장하고 전송 중인 데이터를 네트워크 계층 도청 공격으로부터 보호합니다.<br>(관련 정책: 웹 애플리케이션은 HTTPS를 통해서만 액세스할 수 있어야 함)|중간|**예**|웹 애플리케이션|
||||||


## <a name="container-recommendations"></a><a name="recs-containers"></a>컨테이너 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**Azure Kubernetes Service 클러스터에서 지능형 위협 방지를 사용하도록 설정해야 함**|Security Center는 실시간 위협 방지를 컨테이너화된 환경에 제공하고 의심스러운 활동에 대한 경고를 생성합니다. 이 정보를 사용하여 보안 문제를 신속하게 수정하고 컨테이너의 보안을 강화할 수 있습니다.<br>중요:이 권장 사항을 수정 AKS 클러스터를 보호 하는 데 비용이 부과 됩니다. 이 구독에 AKS 클러스터가 없으면 요금이 발생 하지 않습니다. 나중에이 구독에 AKS 클러스터를 만드는 경우 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [Azure Kubernetes Service 클러스터에서 Advanced threat protection을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a))|높음|**예**|Subscription|
|**권한 있는 IP 범위는 Kubernetes Services에 정의되어야 함**|특정 범위의 IP 주소에만 API 액세스 권한을 부여하여 Kubernetes 서비스 관리 API에 대한 액세스를 제한합니다. 허용된 네트워크의 애플리케이션만 클러스터에 액세스할 수 있도록 권한 있는 IP 범위를 구성하는 것이 좋습니다.<br>(관련 정책: [미리 보기]: Kubernetes Services에서 권한 있는 IP 범위를 정의해야 함)|높음|N|컴퓨팅 리소스(컨테이너)|
|**불필요한 애플리케이션 권한을 제거하여 공격 벡터를 줄이는 Pod 보안 정책을 정의해야 함(미리 보기)**|Pod 보안 정책을 정의하여 불필요한 애플리케이션 권한을 제거함으로써 공격 벡터를 줄일 수 있습니다. Pod가 액세스 권한이 부여된 리소스에만 액세스할 수 있도록 Pod 보안 정책을 구성하는 것이 좋습니다.<br>(관련 정책: [미리 보기]: Kubernetes 서비스에서 Pod 보안 정책을 정의해야 함)|중간|N|컴퓨팅 리소스(컨테이너)|
|**역할 기반 액세스 제어를 사용하여 Kubernetes Service 클러스터에 대한 액세스를 제한해야 함**|사용자가 수행할 수 있는 작업의 세부적인 필터링을 제공하려면 RBAC(역할 기반 액세스 제어)를 사용하여 Kubernetes Service 클러스터에서 권한을 관리하고 관련 권한 부여 정책을 구성하세요. 자세한 내용은 [Azure 역할 기반 액세스 제어](https://docs.microsoft.com/azure/aks/concepts-identity#role-based-access-controls-rbac)를 참조하세요.<br>(관련 정책: [미리 보기]: Kubernetes Services에서 RBAC(역할 기반 액세스 제어)를 사용해야 함)|중간|N|컴퓨팅 리소스(컨테이너)|
|**Kubernetes Service를 최신 Kubernetes 버전으로 업그레이드해야 함**|최신 취약점 패치를 활용할 수 있도록 Azure Kubernetes Service 클러스터를 최신 Kubernetes 버전으로 업그레이드합니다. Kubernetes 취약성에 대한 자세한 내용은 [Kubernetes CVE](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=kubernetes)를 참조하세요.<br>(관련 정책: [미리 보기]: Kubernetes Services를 취약하지 않은 Kubernetes 버전으로 업그레이드해야 함)|높음|N|컴퓨팅 리소스(컨테이너)|
|**Azure Container Registry 레지스트리에서 지능형 위협 방지를 사용하도록 설정해야 함**|안전한 컨테이너 화 된 워크 로드를 빌드하기 위해 기반으로 하는 이미지에 알려진 취약성이 없는지 확인 합니다. Security Center은 푸시 되는 각 컨테이너 이미지의 보안 취약점에 대 한 레지스트리를 검색 하 고 이미지 당 자세한 결과를 노출 합니다.<br>중요:이 권장 사항을 수정 ACR 레지스트리를 보호 하는 요금이 부과 됩니다. 이 구독에 ACR 레지스트리가 없으면 요금이 발생 하지 않습니다. 나중에이 구독에서 ACR 레지스트리를 만들면 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [Azure Container Registry 레지스트리에서 고급 위협 방지를 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4))|높음|**예**|Subscription|
|**Azure Container Registry 이미지의 취약성을 수정해야 함(Qualys 제공)**|컨테이너 이미지 취약성 평가는 레지스트리를 검사하여 푸시된 각 컨테이너 이미지의 보안 취약성을 확인하고 이미지마다 발견한 정보를 공개합니다. 취약성을 해결하면 컨테이너의 보안 상태가 크게 개선되며 공격으로부터 보호할 수 있습니다.<br>(관련 정책 없음)|높음|N|컴퓨팅 리소스(컨테이너)|
||||||


## <a name="app-service-recommendations"></a><a name="recs-appservice"></a>App Service 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**Azure App Service 계획에서 지능형 위협 방지를 사용하도록 설정해야 함**|Security Center는 클라우드의 규모와 Azure에서 클라우드 공급자로의 표시 유형을 활용 하 여 일반적인 웹 앱 공격을 모니터링 합니다.<br>중요:이 권장 사항을 수정 App Service 요금제를 보호 하는 요금이 부과 됩니다. 이 구독에 App Service 계획이 없으면 요금이 발생 하지 않습니다. 나중에이 구독에 대 한 App Service 계획을 만들면 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [Azure App Service 요금제에서 Advanced threat protection을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb))|높음|**예**|Subscription|
|**웹 애플리케이션은 HTTPS를 통해서만 액세스할 수 있어야 합니다.**|웹 애플리케이션에 "HTTPS 전용" 액세스를 사용합니다. HTTPS를 사용하여 서버/서비스 인증을 보장하고 전송 중인 데이터를 네트워크 계층 도청 공격으로부터 보호합니다.<br>(관련 정책: 웹 애플리케이션은 HTTPS를 통해서만 액세스할 수 있어야 함)|중간|**예**|App Service|
|**함수 앱은 HTTPS를 통해서만 액세스할 수 있어야 합니다.**|함수 앱에 "HTTPS 전용" 액세스를 사용합니다. HTTPS를 사용하여 서버/서비스 인증을 보장하고 전송 중인 데이터를 네트워크 계층 도청 공격으로부터 보호합니다.<br>(관련 정책: HTTPS를 통해서만 함수 앱에 액세스할 수 있어야 함)|중간|**예**|App Service|
|**API 앱은 HTTPS를 통해서만 액세스할 수 있어야 합니다.**|HTTPS를 통해서만 API Apps에 액세스할 수 있도록 제한합니다.<br>(관련 정책: API App은 HTTPS를 통해서만 액세스할 수 있어야 함)|중간|N|App Service|
|**웹 애플리케이션에 대해 원격 디버깅을 해제해야 합니다.**|더 이상 사용할 필요가 없으면 웹 애플리케이션에 대해 디버깅을 해제합니다. 원격으로 디버깅하려면 웹앱에서 인바운드 포트를 열어야 합니다.<br>(관련 정책: 웹 애플리케이션에 대한 원격 디버깅을 해제해야 함)|낮음|**예**|App Service|
|**함수 앱에 대한 원격 디버깅을 해제해야 함**|더 이상 사용할 필요가 없는 경우 함수 앱에 대한 디버깅을 해제합니다. 원격 디버깅에 함수 앱에서 열리는 인바운드 포트가 필요합니다.<br>(관련 정책: 함수 앱에 대한 원격 디버깅을 해제해야 함)|낮음|**예**|App Service|
|**API App에 대한 원격 디버깅을 해제해야 함**|더 이상 사용할 필요가 없으면 API App에 대한 디버깅을 해제합니다. 원격으로 디버깅하려면 API App에서 인바운드 포트를 열어야 합니다.<br>(관련 정책: API App에 대한 원격 디버깅을 해제해야 함)|낮음|**예**|App Service|
|**CORS에서 모든 리소스가 웹 애플리케이션에 액세스하도록 허용해서는 안 됩니다.**|필요한 도메인만 웹 애플리케이션과 상호 작용할 수 있도록 허용합니다. CORS(교차 원본 리소스 공유)는 웹 애플리케이션에 액세스하는 모든 도메인을 허용하지 않아야 합니다.<br>(관련 정책: CORS에서 모든 리소스가 웹 애플리케이션에 액세스하도록 허용하면 안 됨)|낮음|**예**|App Service|
|**CORS에서 모든 리소스가 함수 앱에 액세스하도록 허용하면 안 됨**|필요한 도메인만 함수 애플리케이션과 상호 작용할 수 있도록 허용합니다. CORS(교차 원본 리소스 공유)는 함수 애플리케이션에 액세스하는 모든 도메인을 허용하지 않아야 합니다.<br>(관련 정책: CORS에서 모든 리소스가 함수 앱에 액세스하도록 허용하면 안 됨)|낮음|**예**|App Service|
|**CORS에서 모든 리소스가 API 앱에 액세스하도록 허용해서는 안 됩니다.**|필요한 도메인만 API 애플리케이션과 상호 작용하도록 허용합니다. CORS(교차 원본 리소스 공유)에서 모든 도메인이 API 애플리케이션에 액세스하도록 허용하면 안 됩니다.<br>(관련 정책: CORS에서 모든 리소스가 API 앱에 액세스하도록 허용하면 안 됨)|낮음|**예**|App Service|
|**App Services의 진단 로그를 사용하도록 설정해야 합니다.**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: App Services에서 진단 로그를 사용하도록 설정해야 함)</span>|낮음|N|App Service|
||||||


## <a name="compute-and-app-recommendations"></a><a name="recs-computeapp"></a>컴퓨팅 및 앱 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**가상 머신에서 고급 위협 방지를 사용 하도록 설정 해야 합니다.**|Security Center는 가상 컴퓨터 작업에 대 한 실시간 위협 방지 기능을 제공 하 고 의심 스러운 활동에 대 한 경고 뿐만 아니라 강화 recommmendations 생성 합니다.<br>이 정보를 사용 하 여 보안 문제를 신속 하 게 수정 하 고 가상 머신의 보안을 향상 시킬 수 있습니다.<br>중요:이 권장 사항을 수정 가상 머신 보호에 대 한 요금이 부과 됩니다. 이 구독에 가상 컴퓨터가 없으면 요금이 발생 하지 않습니다. 나중에이 구독에서 가상 컴퓨터를 만드는 경우 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [가상 컴퓨터에서 Advanced threat protection을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d))|높음|**예**|Subscription|
|**Azure Stream Analytics의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Azure Stream Analytics에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Compute 리소스(스트림 분석)|
|**Batch 계정의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Batch 계정에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Compute 리소스(Batch)|
|**Event Hub의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Event Hub에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Compute 리소스(이벤트 허브)|
|**Logic Apps의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Logic Apps에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Compute 리소스(Logic Apps)|
|**Search Services의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Search 서비스에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Compute 리소스(검색)|
|**Service Bus의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Service Bus에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Compute 리소스(Service Bus)|
|**Service Fabric 클러스터는 클라이언트 인증에 대해서만 Azure Active Directory를 사용해야 함**|Service Fabric에서 Azure Active Directory를 통해서만 클라이언트 인증을 수행합니다.<br>(관련 정책: Service Fabric 클러스터가 클라이언트 인증에 Azure Active Directory만 사용해야 함)|높음|N|Compute 리소스(Service Fabric)|
|**Service Fabric 클러스터는 ClusterProtectionLevel 속성을 EncryptAndSign으로 설정해야 함**|Service Fabric은 기본 클러스터 인증서를 사용하여 노드 간 통신을 위한 3단계 보호(None, Sign 및 EncryptAndSign)를 제공합니다. 모든 노드 간 메시지가 암호화되고 디지털로 서명될 수 있게 보호 수준을 설정합니다.<br>(관련 정책: Service Fabric에서 ClusterProtectionLevel 속성을 EncryptAndSign으로 설정해야 함)|높음|N|Compute 리소스(Service Fabric)|
|**Service Bus 네임스페이스에서 RootManageSharedAccessKey를 제외한 모든 인증 규칙을 제거해야 함**|Service Bus 클라이언트는 네임스페이스의 모든 큐 및 토픽에 대한 액세스를 제공하는 네임스페이스 수준 액세스 정책을 사용하지 않아야 합니다. 최소 권한 보안 모델에 맞게 큐 및 토픽에 대한 엔터티 수준의 액세스 정책을 만들어 특정 엔터티에만 액세스를 제공해야 합니다.<br>(관련 정책: Service Bus 네임스페이스에서 RootManageSharedAccessKey를 제외한 모든 권한 부여 규칙을 제거해야 함)|낮음|N|Compute 리소스(Service Bus)|
|**RootManageSharedAccessKey를 제외한 모든 권한 부여 규칙을 이벤트 허브 네임스페이스에서 제거해야 함**|이벤트 허브 클라이언트는 네임스페이스의 모든 큐 및 토픽에 대한 액세스를 제공하는 네임스페이스 수준 액세스 정책을 사용하지 않아야 합니다. 최소 권한 보안 모델에 맞게 큐 및 토픽에 대한 엔터티 수준의 액세스 정책을 만들어 특정 엔터티에만 액세스를 제공해야 합니다.<br>(관련 정책: RootManageSharedAccessKey를 제외한 모든 권한 부여 규칙을 Event Hub 네임스페이스에서 제거해야 함)|낮음|N|Compute 리소스(이벤트 허브)|
|**Event Hub 엔터티의 권한 부여 규칙을 정의해야 함**|최소 액세스 권한을 부여하기 위해 이벤트 허브 엔터티에 대한 권한 부여 규칙을 감사합니다.<br>(관련 정책: Event Hub 엔터티의 권한 부여 규칙을 정의해야 함)|낮음|N|Compute 리소스(이벤트 허브)|
|**가상 머신에 모니터링 에이전트 설치**|모니터링 에이전트를 설치하여 각 머신에서 데이터 수집, 업데이트 검사, 기준 검사 및 엔드포인트 보호를 사용하도록 설정합니다.<br>(관련 정책 없음)|높음|**예**|컴퓨터|
|**Log Analytics 에이전트는 Windows 기반 Azure Arc 컴퓨터 (미리 보기)에 설치 되어야 합니다.**|Security Center는 [Log Analytics 에이전트](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent) (MMA 라고도 함)를 사용 하 여 Azure Arc 컴퓨터에서 보안 이벤트를 수집 합니다.<br>(관련 정책: [Preview]: Windows Azure Arc 컴퓨터에 Log Analytics 에이전트를 설치 해야 함)|높음|**예**|Azure Arc 컴퓨터|
|**Linux 기반 Azure Arc 컴퓨터 (미리 보기)에 Log Analytics 에이전트를 설치 해야 합니다.**|Security Center는 [Log Analytics 에이전트](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent) (MMA 라고도 함)를 사용 하 여 Azure Arc 컴퓨터에서 보안 이벤트를 수집 합니다.<br>(관련 정책: [Preview]: Linux Azure Arc 컴퓨터에 Log Analytics 에이전트를 설치 해야 함)|높음|**예**|Azure Arc 컴퓨터|
|**게스트 구성 확장은 Windows 가상 머신에 설치 해야 합니다 (미리 보기).**|게스트 구성 에이전트를 설치 하 여 컴퓨터 내의 운영 체제 구성, 응용 프로그램 구성 또는 현재 상태, 환경 설정 등의 감사 설정을 사용 하도록 설정 합니다. 설치 되 면 ' Windows Exploit guard를 사용 하도록 설정 해야 합니다. '와 같은 게스트 내 정책을 사용할 수 있습니다.<br>(관련 정책: Windows Vm에서 게스트 구성 정책을 사용 하기 위한 필수 구성 요소 감사)|높음|**예**|컴퓨터|
|**컴퓨터에 Windows Defender Exploit Guard를 사용 하도록 설정 해야 함 (미리 보기)**|Windows Defender Exploit Guard는 Azure Policy 게스트 구성 에이전트를 활용 합니다. Exploit Guard에는 기업에서 보안 위험 및 생산성 요구 사항을 균형 있게 조정할 수 있도록 하는 동시에 맬웨어 공격에 일반적으로 사용 되는 다양 한 공격 벡터 및 차단 동작에 대해 장치를 잠그기 위해 설계 된 4 가지 구성 요소가 있습니다 (Windows에만 해당).<br>(관련 정책: Windows Defender Exploit Guard를 사용 하지 않는 Windows Vm 감사)|중간|N|컴퓨터|
|**머신의 모니터링 에이전트 상태 문제를 해결해야 함**|완벽한 Security Center 보호를 위해 문제 해결 가이드의 지침에 따라 머신에서 모니터링 에이전트 문제를 해결합니다.<br>(관련 정책 없음 - "가상 머신에 모니터링 에이전트 설치"에 따라 다름)|중간|N|컴퓨터|
|**가상 머신에서 적응형 애플리케이션 제어를 사용하도록 설정해야 합니다.**|애플리케이션 제어를 사용하도록 설정하여 Azure에 있는 VM에서 실행할 수 있는 애플리케이션을 제어합니다. 이렇게 하면 VM을 강화하여 맬웨어로부터 보호할 수 있습니다. Security Center는 기계 학습을 통해 각 VM에서 실행 중인 애플리케이션을 분석하고 이러한 인텔리전스를 사용하여 허용 목록 규칙을 적용할 수 있습니다. 이 기능은 애플리케이션 허용 목록의 구성 및 유지 관리 프로세스를 간소화합니다.<br>(관련 정책: 가상 머신에서 적응형 애플리케이션 제어를 사용하도록 설정해야 함)|높음|N|컴퓨터|
|**머신에 엔드포인트 보호 솔루션 설치**|위협 및 취약성으로부터 보호할 수 있도록 Windows 및 Linux 머신에 엔드포인트 보호 솔루션을 설치합니다.<br>(관련 정책: Azure Security Center에서 누락된 Endpoint Protection 모니터링)|중간|N|컴퓨터|
|**가상 머신에 엔드포인트 보호 솔루션 설치**|가상 머신에 엔드포인트 보호 솔루션을 설치하여 위협 및 취약성으로부터 보호합니다.<br>(관련 정책 없음)|중간|N|컴퓨터|
|**클라우드 서비스 역할에 대한 OS 버전을 업데이트해야 함**|클라우드 서비스 역할의 OS(운영 체제) 버전을 OS 제품군의 사용 가능한 최신 버전으로 업데이트합니다.<br>(관련 정책 없음)|높음|N|컴퓨터|
|**시스템 업데이트를 머신에 설치해야 합니다.**|누락된 시스템 보안 및 중요 업데이트를 설치하여 Windows 및 Linux 가상 머신과 컴퓨터를 보호합니다.<br>(관련 정책: 머신에 시스템 업데이트를 설치해야 함)|높음|N|컴퓨터|
|**Linux 가상 머신에 네트워크 트래픽 데이터 수집 에이전트를 설치해야 함(미리 보기)**|Security Center에서는 Microsoft Monitoring Dependency Agent를 사용하여 Azure 가상 머신에서 네트워크 트래픽 데이터를 수집함으로써 네트워크 맵의 트래픽 시각화, 네트워크 강화 권장 사항 및 특정 네트워크 위협과 같은 고급 네트워크 보호 기능을 활성화합니다.<br>(관련 정책: [미리 보기]: Linux 가상 머신에 네트워크 트래픽 데이터 수집 에이전트를 설치해야 함)|중간|**예**|컴퓨터|
|**Windows 가상 머신에 네트워크 트래픽 데이터 컬렉션 에이전트를 설치해야 함(미리 보기)**|Security Center에서는 Microsoft Monitoring Dependency Agent를 사용하여 Azure 가상 머신에서 네트워크 트래픽 데이터를 수집함으로써 네트워크 맵의 트래픽 시각화, 네트워크 강화 권장 사항 및 특정 네트워크 위협과 같은 고급 네트워크 보호 기능을 활성화합니다.<br>(관련 정책: [미리 보기]: Windows 가상 머신에 네트워크 트래픽 데이터 수집 에이전트를 설치해야 함)|중간|**예**|컴퓨터|
|**가상 머신에서 기본 제공 취약성 평가 솔루션을 사용하도록 설정**|가상 머신에 Qualys 에이전트(기본 제공 Azure Security Center 표준 계층 제품)를 설치하여 동급 최고의 취약성 평가 솔루션을 사용하도록 설정합니다.<br>(관련 정책: [가상 컴퓨터에서 취약성 평가를 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F501541f7-f7e7-4cd6-868c-4190fdad3ac9))|중간|**예**|컴퓨터|
|**가상 머신에서 발견한 취약성 수정(Qualys 제공)**|Azure Security Center의 기본 제공 취약성 평가 솔루션(Qualys 제공)에서 발견한 가상 머신의 취약성 결과를 모니터링합니다.<br>(관련 정책: Virtual Machines)에서 취약성 평가를 사용 하도록 설정 해야 합니다.|낮음|N|컴퓨터|
|**시스템 업데이트를 적용하려면 머신을 다시 시작해야 함**|시스템 업데이트를 적용하고 취약성으로부터 머신을 보호하려면 머신을 다시 시작합니다.<br>(관련 정책 없음 - "머신에 시스템 업데이트를 설치해야 함"에 따라 다름)|중간|N|컴퓨터|
|**Automation 계정 변수를 암호화해야 함**|중요한 데이터를 저장할 때 Automation 계정 변수 자산의 암호화를 사용하도록 설정합니다.<br>(관련 정책: Automation 계정 변수에 암호화를 사용하도록 설정해야 함)|높음|N|Compute 리소스(Automation 계정)|
|**가상 머신에서 디스크 암호화를 적용해야 합니다.**|Windows 및 Linux 가상 머신 모두에 Azure Disk Encryption를 사용하여 가상 머신 디스크를 암호화합니다. ADE(Azure Disk Encryption)는 OS 및 데이터 암호화를 제공하는 Windows의 업계 표준 BitLocker 기능 및 Linux의 DM-Crypt 기능을 활용하여 데이터를 보호하고 고객 Azure Key Vault에서 조직 보안 및 규정 준수 약정을 충족하는 데 도움이 됩니다. 규정 준수 및 보안 요구 사항에서 임시(로컬로 임시 연결) 디스크 암호화를 포함하여 암호화 키를 사용한 데이터의 종단 간 암호화를 요구할 경우 Azure Disk Encryption을 사용합니다. 또는 기본적으로 Managed Disks는 Azure 스토리지 서비스 암호화를 사용하여 암호화되며, 암호화 키는 Azure의 Microsoft 관리형 키입니다. 이것이 규정 준수 및 보안 요구 사항을 충족한다면 기본 관리 디스크 암호화를 사용하여 요구 사항에 부합할 수 있습니다.<br>(관련 정책: 가상 머신에서 디스크 암호화를 적용해야 함)|높음|N|컴퓨터|
|**가상 머신을 새 Azure Resource Manager 리소스로 마이그레이션해야 함**|가상 머신에 Azure Resource Manager를 사용하여 더 강력한 액세스 제어(RBAC), 더욱 효율적인 감사, Resource Manager 기반 배포 및 관리, 관리 ID 액세스, 비밀용 Key Vault 액세스, Azure AD 기반 인증, 더욱 쉬운 보안 관리를 위한 태그 및 리소스 그룹 지원 등의 향상된 보안 기능을 제공합니다.<br>(관련 정책: 가상 머신을 새 Azure Resource Manager 리소스로 마이그레이션해야 함)|낮음|N|컴퓨터|
|**가상 머신에 취약성 평가 솔루션을 설치해야 함**|가상 머신에 취약성 평가 솔루션 설치<br>(관련 정책: 취약성 평가 솔루션으로 취약성을 수정해야 함)|중간|N|컴퓨터|
|**취약성 평가 솔루션으로 취약성을 수정해야 합니다.**|취약성 평가 타사 솔루션이 배포된 가상 머신이 애플리케이션 및 OS 취약성에 대해 연속적으로 평가됩니다. 이러한 취약성 항목이 발견될 때마다 이 내용을 자세한 권장 구성의 일부로 사용할 수 있습니다.<br>(관련 정책: 취약성 평가 솔루션으로 취약성을 수정해야 함)|높음|N|컴퓨터|
|**머신 보안 구성의 취약성을 수정해야 합니다.**|머신의 보안 구성에서 취약성을 수정하여 공격으로부터 보호합니다.<br>(관련 정책: 머신 보안 구성의 취약성을 수정해야 함)|낮음|N|컴퓨터|
|**컨테이너 보안 구성의 취약성을 수정해야 합니다.**|Docker가 설치된 머신의 보안 구성에서 취약성을 수정하여 공격으로부터 보호합니다.<br>(관련 정책: 컨테이너 보안 구성의 취약성을 수정해야 함)|높음|N|컴퓨터|
|**머신에서 엔드포인트 보호 상태 문제를 해결해야 함**|완벽한 Security Center 보호를 위해 문제 해결 가이드의 지침에 따라 머신에서 모니터링 에이전트 문제를 해결합니다.<br>(이 권장 사항은 "머신에 엔드포인트 보호 솔루션 설치" 및 해당 정책에 따라 다름)|중간|N|컴퓨터|
||||||


## <a name="virtual-machine-scale-set-recommendations"></a><a name="recs-vmscalesets"></a>가상 머신 확장 집합 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**Virtual Machine Scale Sets의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년간 보존합니다. 이 옵션을 사용하면 조사를 위해 작업 내역을 다시 만들 수 있습니다. 이 옵션은 보안 인시던트가 발생하거나 네트워크가 손상된 경우에 유용합니다.<br>(관련 정책: Virtual Machine Scale Sets에서 진단 로그를 사용하도록 설정해야 함)|낮음|N|가상 머신 크기 집합|
|**가상 머신 확장 집합에서 엔드포인트 보호 상태 오류를 해결해야 함**|가상 머신 확장 집합에 대한 엔드포인트 보호 상태 오류를 수정하여 위협 및 취약성으로부터 보호합니다.<br>(관련 정책 없음 - "가상 머신 확장 집합에 엔드포인트 보호 솔루션을 설치해야 함"에 따라 다름)|낮음|N|가상 머신 크기 집합|
|**가상 머신 확장 집합에 Endpoint Protection 솔루션을 설치해야 합니다.**|가상 머신 확장 집합에 엔드포인트 보호 솔루션을 설치하여 위협 및 취약성으로부터 보호합니다.<br>(관련 정책: 가상 머신 확장 집합에 엔드포인트 보호 솔루션을 설치해야 함)|높음|N|가상 머신 크기 집합|
|**가상 머신 확장 집합에 모니터링 에이전트를 설치해야 함**|Security Center는 MMA(Microsoft Monitoring Agent)를 사용하여 Azure 가상 머신 확장 집합에서 보안 이벤트를 수집합니다. Azure 가상 머신 확장 집합에 대한 MMA 자동 프로비저닝을 구성할 수 없습니다. 가상 머신 확장 집합(Azure Kubernetes Service 및 Azure Service Fabric 같은 Azure 관리형 서비스에서 사용하는 가상 머신 확장 집합 포함)에 MMA를 배포하려면 수정 단계의 절차를 따르세요.|높음|**예**|가상 머신 크기 집합|
|**가상 머신 확장 집합에 대한 시스템 업데이트를 설치해야 합니다.**|누락된 시스템 보안 및 중요 업데이트를 설치하여 Windows 및 Linux 가상 머신 확장 집합을 보호합니다.<br>(관련 정책: 가상 머신 확장 집합에 시스템 업데이트를 설치해야 함)|높음|N|가상 머신 크기 집합|
|**가상 머신 확장 집합에서 보안 구성의 취약성을 수정해야 합니다.**|가상 머신 확장 집합의 보안 구성에서 취약성을 수정하여 공격으로부터 보호합니다. <br>(관련 정책: 가상 머신 확장 집합에서 보안 구성의 취약성을 수정해야 함)|높음|N|가상 머신 크기 집합|
||||||


## <a name="data-and-storage-recommendations"></a><a name="recs-datastorage"></a>데이터 및 스토리지 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**Azure SQL Database 서버에서 Advanced Data Security를 사용하도록 설정해야 함**|고급 데이터 보안은 고급 SQL 보안 기능을 제공 하는 통합 패키지입니다. 여기에는 잠재적 데이터베이스 취약성을 표시 하 고 완화 하 고, 데이터베이스에 위협을 나타낼 수 있는 비정상적인 활동을 검색 하 고, 중요 한 데이터를 검색 및 분류 하기 위한 기능이 포함 되어 있습니다. <br>중요:이 권장 사항을 수정 Azure SQL Database 서버를 보호 하는 요금이 부과 됩니다. 이 구독에 Azure SQL Database 서버가 없는 경우 요금이 발생 하지 않습니다. 나중에이 구독에 Azure SQL Database 서버를 만들 경우 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [Azure SQL Database 서버에서 고급 데이터 보안을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2))|높음|**예**|Subscription|
|**머신의 SQL 서버에서 Advanced Data Security를 사용하도록 설정해야 함**|고급 데이터 보안은 고급 SQL 보안 기능을 제공 하는 통합 패키지입니다. 여기에는 잠재적 데이터베이스 취약성을 표시 하 고 완화 하 고, 데이터베이스에 위협을 나타낼 수 있는 비정상적인 활동을 검색 하 고, 중요 한 데이터를 검색 및 분류 하기 위한 기능이 포함 되어 있습니다. <br>중요:이 권장 사항을 수정 컴퓨터에서 SQL 서버를 보호 하는 요금이 부과 됩니다. 이 구독의 컴퓨터에 SQL server가 없는 경우에는 요금이 부과 되지 않습니다. 나중에이 구독에 대 한 컴퓨터에 SQL server를 만드는 경우 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [컴퓨터의 SQL server에서 고급 데이터 보안을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b))|높음|**예**|Subscription|
|**Azure Storage 계정에서 지능형 위협 방지를 사용하도록 설정해야 함**|저장소에 대 한 Advanced threat protection은 저장소 계정에 액세스 하거나 악용 하려는 비정상적이 고 잠재적으로 유해한 시도를 감지 합니다.<br>중요:이 권장 사항을 수정 Azure Storage 계정을 보호 하는 요금이 부과 됩니다. 이 구독에 Azure Storage 계정이 없으면 요금이 발생 하지 않습니다. 나중에이 구독에 Azure Storage 계정을 만드는 경우 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [Azure Storage 계정에서 Advanced threat protection을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa))|높음|**예**|Subscription|
|**방화벽 및 가상 네트워크 구성을 사용하여 스토리지 계정 액세스를 제한해야 함**|스토리지 계정 방화벽 설정에서 무제한 네트워크 액세스를 감사합니다. 또는, 허용되는 네트워크의 애플리케이션만 스토리지 계정에 액세스할 수 있도록 네트워크 규칙을 구성합니다. 특정 인터넷 또는 온-프레미스 클라이언트의 연결을 허용하려면 특정 Azure 가상 네트워크의 트래픽 또는 공용 인터넷 IP 주소 범위에 대한 액세스 권한을 부여하면 됩니다.<br>(관련 정책: 스토리지 계정에 대한 무제한 네트워크 액세스 감사)|낮음|N|스토리지 계정|
|**관리형 인스턴스에서 Advanced Data Security를 사용하도록 설정해야 함**|ADS(Advanced Data Security)는 고급 SQL 보안 기능을 제공하는 통합 패키지입니다. 중요한 데이터를 검색 및 분류하고, 잠재적 데이터베이스 취약성을 표시 및 완화하고, 데이터베이스에 위협이 될 수 있는 비정상적인 활동을 감지합니다. ADS는 관리 되는 인스턴스당 $15로 청구 됩니다.<br>(관련 정책: SQL Managed Instance에서 고급 데이터 보안을 사용 하도록 설정 해야 함)|높음|**예**|SQL|
|**SQL 서버에서 Advanced Data Security를 사용하도록 설정해야 합니다.**|ADS(Advanced Data Security)는 고급 SQL 보안 기능을 제공하는 통합 패키지입니다. 중요한 데이터를 검색 및 분류하고, 잠재적 데이터베이스 취약성을 표시 및 완화하고, 데이터베이스에 위협이 될 수 있는 비정상적인 활동을 감지합니다. ADS는 SQL 서버당 $15의 요금이 청구됩니다.<br>(관련 정책: SQL 서버에서 Advanced Data Security를 사용하도록 설정해야 함)|높음|**예**|SQL|
|**SQL Database에 대해 Azure Active Directory 관리자를 프로 비전 해야 합니다.**|Azure AD 인증을 사용 하도록 SQL Database에 대 한 Azure AD 관리자를 프로 비전 합니다. Azure AD 인증을 사용하면 데이터베이스 사용자 및 기타 Microsoft 서비스의 권한을 간편하게 관리하고 ID를 한 곳에서 집중적으로 관리할 수 있습니다.<br>(관련 정책: SQL 서버에 대한 Azure Active Directory 관리자 프로비저닝 감사)|높음|N|SQL|
|**SQL Database에 대 한 감사를 사용 하도록 설정 해야 합니다.**|SQL Database에 대 한 감사를 사용 하도록 설정 합니다. <br>(관련 정책: 서버에 대 한 고급 데이터 보안 설정에 대 한 SQL Database에 대해 감사를 사용 하도록 설정 해야 함)|낮음|**예**|SQL|
|**Azure Data Lake Store의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Azure Data Lake Store에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Data Lake Store|
|**Data Lake Analytics의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Data Lake Analytics에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Data Lake Analytics|
|**Redis Cache에 보안 연결만 사용하도록 설정해야 함**|Azure Cache for Redis에 SSL을 통한 연결만 사용하도록 설정합니다. 보안 연결을 사용하여 서버와 서비스 간 인증을 보장하고 전송 중인 데이터를 메시지 가로채기(man-in-the-middle), 도청 및 세션 하이재킹과 같은 네트워크 계층 공격으로부터 보호합니다.<br>(관련 정책: Redis Cache에 보안 연결만 사용하도록 설정해야 함)|높음|N|Redis|
|**스토리지 계정에 보안 전송을 사용하도록 설정해야 함**|보안 전송은 사용자의 스토리지 계정이 보안 연결(HTTPS)에서 오는 요청만 수락하도록 강제 적용하는 옵션입니다. HTTPS를 사용하면 서버와 서비스 간에 인증이 필요하며 전송 중인 데이터를 메시지 가로채기(man-in-the-middle), 도청 및 세션 하이재킹과 같은 네트워크 계층 공격으로부터 보호합니다.<br>(관련 정책: 스토리지 계정에 보안 전송을 사용하도록 설정해야 함)|높음|N|스토리지 계정|
|**SQL 데이터베이스의 중요한 데이터를 분류해야 함**|Azure SQL Database 데이터 검색 & 분류는 데이터베이스의 중요 한 데이터를 검색, 분류, 레이블 지정 및 보호 하는 기능을 제공 합니다. 데이터가 분류 되 면 감사 Azure SQL Database 사용 하 여 액세스를 감사 하 고 중요 한 데이터를 모니터링할 수 있습니다. 또한 Azure SQL Database는 중요 한 데이터에 대 한 액세스 패턴의 변경 사항을 기반으로 하는 지능형 경고를 만드는 고급 위협 방지 기능을 사용 하도록 설정 합니다.<br>(관련 정책: [미리 보기]: SQL 데이터베이스의 중요한 데이터를 분류해야 함)|높음|N|SQL|
|**스토리지 계정을 새 Azure Resource Manager 리소스로 마이그레이션해야 함**|스토리지 계정에 새 Azure Resource Manager를 사용하여 더 강력한 액세스 제어(RBAC), 더 나은 감사, Resource Manager 기반 배포 및 관리, 관리 ID 액세스, 비밀을 위해 키 자격 증명 모음에 액세스, Azure AD 기반 인증, 보다 쉬운 보안 관리를 위한 태그 및 리소스 그룹 지원과 같은 향상된 보안 기능을 제공합니다.<br>(관련 정책: 스토리지 계정을 새 Azure Resource Manager 리소스로 마이그레이션해야 함)|낮음|N|스토리지 계정|
|**SQL 데이터베이스에 투명한 데이터 암호화를 사용하도록 설정해야 합니다.**|투명한 데이터 암호화를 사용하도록 설정하여 미사용 데이터를 보호하고 규정 준수 요구 사항을 충족합니다.<br>(관련 정책: SQL 데이터베이스에 투명한 데이터 암호화를 사용하도록 설정해야 함)|낮음|**예**|SQL|
|**에서 취약성 평가를 사용 하도록 설정 해야 SQL Database**|취약성 평가는 잠재적 데이터베이스 취약성을 검색 및 추적할 수 있고 해결하는 데 도움이 될 수 있습니다.<br>(관련 정책: SQL 서버에서 취약성 평가를 사용하도록 설정해야 함)|높음|**예**|SQL|
|**SQL Managed Instance에서 취약성 평가를 사용하도록 설정해야 함**|취약성 평가는 잠재적 데이터베이스 취약성을 검색 및 추적할 수 있고 해결하는 데 도움이 될 수 있습니다.<br>(관련 정책: SQL Managed Instance)에서 취약성 평가를 사용 하도록 설정 해야 합니다.|높음|**예**|SQL|
|**컴퓨터의 SQL server에 대 한 취약성 평가 결과를 재구성 해야 함 (미리 보기)**|SQL 취약성 평가는 데이터베이스에서 보안 취약점을 검사 하 고 잘못 된 잘못 된 작업, 과도 한 권한, 보호 되지 않는 중요 데이터 등의 모범 사례에 대 한 모든 차이를 노출 합니다. 발견된 취약성을 해결하면 데이터베이스 보안 상태가 크게 향상될 수 있습니다.|높음|N|SQL|
|**SQL 데이터베이스에 대 한 취약성 평가 결과를 재구성 해야 합니다.**|SQL 취약성 평가는 데이터베이스에서 보안 취약점을 검사 하 고 잘못 된 오류, 과도 한 권한, 보호 되지 않는 중요 한 데이터 등의 모범 사례에 대 한 모든 편차를 제공 합니다. 발견된 취약성을 해결하면 데이터베이스 보안 상태가 크게 향상될 수 있습니다.<br>(관련 정책: SQL 데이터베이스의 취약성을 수정해야 함)|높음|N|SQL|
||||||


## <a name="identity-and-access-recommendations"></a><a name="recs-identity"></a>ID 및 액세스 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**구독에서 읽기 권한이 있는 계정에 MFA를 사용하도록 설정해야 합니다.**|계정 또는 리소스 위반을 방지하려면 읽기 권한이 있는 모든 구독 계정에 대해 MFA(Multi-factor Authentication)를 사용합니다.<br>(관련 정책: 구독에 대한 읽기 권한이 있는 계정에서 MFA를 사용하도록 설정해야 함)|높음|N|Subscription|
|**구독에 대한 쓰기 권한이 있는 계정에서 MFA를 사용하도록 설정해야 함**|계정 또는 리소스 위반을 방지하려면 쓰기 권한이 있는 모든 구독 계정에 대해 MFA(Multi-factor Authentication)를 사용합니다.<br>(관련 정책: 구독에 대한 쓰기 권한이 있는 계정에서 MFA를 사용하도록 설정해야 함)|높음|N|Subscription|
|**구독에서 소유자 권한이 있는 계정에 MFA를 사용하도록 설정해야 합니다.**|계정 또는 리소스 위반을 방지하려면 소유자 권한이 있는 모든 구독 계정에서 MFA(Multi-factor Authentication)를 사용하도록 설정해야 합니다.<br>(관련 정책: 구독에 대한 소유자 권한이 있는 계정에서 MFA를 사용하도록 설정해야 함)|높음|N|Subscription|
|**읽기 권한이 있는 외부 계정을 구독에서 제거해야 합니다.**|모니터링되지 않는 액세스를 방지하려면 읽기 권한이 있는 외부 계정을 구독에서 제거합니다.<br>(관련 정책: 읽기 권한이 있는 외부 계정을 구독에서 제거해야 함)|높음|N|Subscription|
|**쓰기 권한이 있는 외부 계정을 구독에서 제거해야 합니다.**|모니터링되지 않는 액세스를 방지하려면 쓰기 권한이 있는 외부 계정을 구독에서 제거합니다.<br>(관련 정책: 쓰기 권한이 있는 외부 계정을 구독에서 제거해야 함)|높음|N|Subscription|
|**소유자 권한이 있는 외부 계정은 구독에서 제거해야 합니다.**|모니터링되지 않는 액세스를 방지하려면 소유자 권한이 있는 외부 계정을 구독에서 제거합니다.<br>(관련 정책: 소유자 권한이 있는 외부 계정을 구독에서 제거해야 함)|높음|N|Subscription|
|**소유자 권한이 있는 사용되지 않는 계정은 구독에서 제거해야 합니다.**|구독에서 소유자 권한이 있는 사용되지 않는 계정을 제거합니다.<br>(관련 정책: 소유자 권한이 있는 사용되지 않는 계정을 구독에서 제거해야 함)|높음|N|Subscription|
|**더 이상 사용되지 않는 계정은 구독에서 제거해야 합니다.**|현재 사용자에 대해서만 액세스할 수 있게 구독에서 더 이상 사용되지 않는 계정을 제거합니다.<br>(관련 정책: 사용되지 않는 계정을 구독에서 제거해야 함)|높음|N|Subscription|
|**구독에 둘 이상의 소유자를 할당해야 합니다.**|관리자 액세스 중복성을 유지하려면 둘 이상의 구독 소유자를 지정합니다.<br>(관련 정책: 구독에 소유자를 2명 이상 할당해야 함)|높음|N|Subscription|
|**구독에 최대 3명의 소유자를 지정해야 합니다.**|보안이 침해된 소유자의 위반 가능성을 줄이려면 구독 소유자를 3명 미만으로 지정합니다.<br>(관련 정책: 구독 소유자를 3명 이하로 지정해야 함)|높음|N|Subscription|
|**Azure Key Vault 자격 증명 모음에서 지능형 위협 방지를 사용하도록 설정해야 함**|Azure Security Center에는 Azure Key Vault에 대한 Azure 네이티브 고급 위협 방지 기능이 포함되어 있으며, 추가 보안 인텔리전스를 계층에 제공합니다.<br>중요:이 권장 사항을 수정 AKV 자격 증명 모음을 보호 하는 요금이 부과 됩니다. 이 구독에 AKV 자격 증명 모음이 없으면 요금이 발생 하지 않습니다. 나중에이 구독에서 AKV 자격 증명 모음을 만드는 경우 자동으로 보호 되 고 해당 시간에 요금이 청구 됩니다.<br>(관련 정책: [Azure Key Vault 자격 증명 모음에서 Advanced threat protection을 사용 하도록 설정 해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047))|높음|**예**|Subscription|
|**Key Vault의 진단 로그를 사용하도록 설정해야 함**|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다.<br>(관련 정책: Key Vault에서 진단 로그를 사용하도록 설정해야 함)|낮음|**예**|Key Vault|
||||||


## <a name="deprecated-recommendations"></a>사용되지 않는 권장 사항

|권장|설명 및 관련 정책|심각도|빠른 수정이 사용되나요?[(자세한 내용](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#recommendations-with-quick-fix-remediation))|리소스 유형|
|----|----|----|----|----|
|**App Services에 대한 액세스를 제한해야 함**|너무 넓은 범위의 인바운드 트래픽을 거부하도록 네트워킹 구성을 변경하여 App Services 액세스를 제한합니다.<br>(관련 정책: [미리 보기]: App Services 액세스를 제한해야 함)|높음|N|App Service|
|**IaaS NSG의 웹 애플리케이션에 대한 규칙을 강화해야 함**|웹 애플리케이션을 실행하고, 웹 애플리케이션 포트와 관련하여 과도한 권한이 부여된 NSG 규칙을 사용하는 가상 머신의 NSG(네트워크 보안 그룹)를 강화합니다.<br>(관련 정책: IaaS에서 웹 애플리케이션에 대한 NSG 규칙을 강화해야 함)|높음|N|가상 머신|



## <a name="next-steps"></a>다음 단계
권장 사항에 대한 자세한 내용은 다음을 참조하세요.

* [Security Center에서 만든 권장 사항을 분석하는 방법에 대한 Microsoft Learn 모듈](https://docs.microsoft.com/learn/modules/identify-threats-with-azure-security-center/)
* [Azure Security Center의 보안 권장 사항](security-center-recommendations.md)
* [머신 및 애플리케이션 보호](security-center-virtual-machine-protection.md)
* [Azure Security Center에서 네트워크 보호](security-center-network-recommendations.md)