---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/22/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e7750049ea7c2e504aec325c39b2760e93e3c9dd
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87351800"
---
## <a name="azure-security-benchmark"></a>Azure 보안 벤치마크

[Azure Security Benchmark](../../../../articles/security/benchmarks/overview.md)는 Azure에서 클라우드 솔루션을 보호하는 방법에 대한 권장 사항을 제공합니다. 이 서비스가 Azure Security Benchmark에 완전히 매핑되는 방법을 보려면 [Azure Security Benchmark 매핑 파일](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)을 참조하세요.

모든 Azure 서비스에 사용 가능한 Azure Policy 기본 제공이 이 규정 준수 표준에 매핑되는 방식을 검토하려면 [Azure Policy 규정 준수 - Azure Security Benchmark](../../../../articles/governance/policy/samples/azure-security-benchmark.md)를 참조하세요.

|도메인 |컨트롤 ID |컨트롤 제목 |정책<br /><sub>(Azure Portal)</sub> |정책 버전<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|네트워크 보안 |1.1 |Virtual Network에서 네트워크 보안 그룹 또는 Azure Firewall을 사용하여 리소스 보호 |[권한 있는 IP 범위는 Kubernetes Services에 정의되어야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e246bcf-5f6f-4f87-bc6f-775d4712c7ea) |[1.0.1 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableIpRanges_KubernetesService_Audit.json) |
|데이터 보호 |4.6 |Azure RBAC를 사용하여 리소스에 대한 액세스 제어 |[Kubernetes Services에서 RBAC(역할 기반 액세스 제어)를 사용해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.1 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |
|취약성 관리 |5.3 |자동화된 타사 소프트웨어 패치 관리 솔루션 배포 |[Kubernetes Services를 취약하지 않은 Kubernetes 버전으로 업그레이드해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb893a29-21bb-418c-a157-e99480ec364c) |[1.0.1 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UpgradeVersion_KubernetesService_Audit.json) |
|보안 구성 |7.3 |보안 Azure 리소스 구성 유지 관리 |[[미리 보기]: Pod 보안 정책은 Kubernetes Services에 정의되어야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3abeb944-26af-43ee-b83d-32aaf060fb94) |[1.0.0 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnablePSP_KubernetesService_Audit.json) |
|보안 구성 |7.9 |Azure 서비스에 대한 자동화된 구성 모니터링 구현 |[[미리 보기]: Pod 보안 정책은 Kubernetes Services에 정의되어야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3abeb944-26af-43ee-b83d-32aaf060fb94) |[1.0.0 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnablePSP_KubernetesService_Audit.json) |

## <a name="cis-microsoft-azure-foundations-benchmark"></a>CIS Microsoft Azure Foundations 벤치마크

모든 Azure 서비스에 사용 가능한 Azure Policy 기본 제공이 이 규정 준수 표준에 매핑되는 방식을 검토하려면 [Azure Policy 규정 준수 - CIS Microsoft Azure Foundations Benchmark 1.1.0](../../../../articles/governance/policy/samples/cis-azure-1-1-0.md)을 참조하세요.
이러한 규정 준수 표준에 대한 자세한 내용은 [CIS Microsoft Azure Foundations 벤치마크](https://www.cisecurity.org/benchmark/azure/)를 참조하세요.

|도메인 |컨트롤 ID |컨트롤 제목 |정책<br /><sub>(Azure Portal)</sub> |정책 버전<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|기타 보안 고려 사항 |8.5 |Azure Kubernetes Services 내에서 RBAC(역할 기반 액세스 제어) 사용 |[Kubernetes Services에서 RBAC(역할 기반 액세스 제어)를 사용해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.1 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |

## <a name="nist-sp-800-171-r2"></a>NIST SP 800-171 R2

모든 Azure 서비스에 사용 가능한 Azure Policy 기본 제공이 이 규정 준수 표준에 매핑되는 방식을 검토하려면 [Azure Policy 규정 준수 - NIST SP 800-171 R2](../../../../articles/governance/policy/samples/nist-sp-800-171-r2.md)를 참조하세요.
이 규정 준수 표준에 대한 자세한 내용은 [NIST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final)를 참조하세요.

|도메인 |컨트롤 ID |컨트롤 제목 |정책<br /><sub>(Azure Portal)</sub> |정책 버전<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|시스템 및 정보 무결성 |3.14.1 |적시에 시스템 결함을 식별, 보고 및 수정합니다. |[Kubernetes Services를 취약하지 않은 Kubernetes 버전으로 업그레이드해야 함](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb893a29-21bb-418c-a157-e99480ec364c) |[1.0.1 - 미리 보기](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UpgradeVersion_KubernetesService_Audit.json) |

