---
title: 다중 테넌트 웹 애플리케이션 패턴 | Microsoft 문서
description: Azure에서 다중 테넌트 웹 애플리케이션을 구현하는 방법에 대해 설명하는 아키텍처 개요 및 디자인 패턴을 찾습니다.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: d1441ede9f448b3e6ffb0726c2ee92f192369e9a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81481846"
---
# <a name="multitenant-applications-in-azure"></a>Azure의 다중 테넌트 애플리케이션
다중 테 넌 트 응용 프로그램은 "별도의 테 넌 트에 있는 사용자"가 고유한 응용 프로그램을 볼 수 있도록 하는 공유 리소스입니다. 다중 테 넌 트 응용 프로그램에 사용 되는 일반적인 시나리오는 다른 테 넌 트에서 응용 프로그램의 모든 사용자가 사용자 환경을 사용자 지정 하려고 하지만 그렇지 않은 경우 동일한 기본 비즈니스 요구 사항이 있는 것입니다. 대규모 다중 테넌트 애플리케이션의 예로는 Office 365, Outlook.com 및 visualstudio.com이 있습니다.

애플리케이션 공급자의 관점에서 다중 테넌트 지원의 이점은 주로 운영 및 비용 효율성과 관련이 있습니다. 모니터링, 성능 조정, 소프트웨어 유지 관리, 데이터 백업과 같은 시스템 관리 작업을 통합할 수 있어, 한 버전의 애플리케이션으로 다수의 테넌트/고객의 요구 사항을 충족할 수 있습니다.

다음은 공급자의 관점에서 볼 때 가장 중요한 목표와 요구 사항의 목록입니다.

* **프로비전**: 애플리케이션의 새 테넌트를 프로비전할 수 있어야 합니다.  테넌트가 많은 다중 테넌트 애플리케이션의 경우 셀프 서비스 프로비전을 사용하여 이 프로세스를 자동화해야 하는 경우가 많습니다.
* **유지 관리**: 애플리케이션을 업그레이드하고 다중 테넌트에서 애플리케이션을 사용하는 동안 다른 유지 관리 작업을 수행할 수 있어야 합니다.
* **모니터링**: 문제를 식별하고 해결할 수 있도록 항상 애플리케이션을 모니터링할 수 있어야 합니다. 각 테넌트가 애플리케이션을 사용하는 방식도 모니터링합니다.

올바르게 구현된 다중 테넌트 애플리케이션을 통해 사용자가 얻는 이점은 다음과 같습니다.

* **격리**:개별 테넌트의 활동이 다른 테넌트의 애플리케이션 사용에 영향을 미치지 않습니다. 테넌트는 서로의 데이터에 액세스할 수 없습니다. 단일 테넌트의 입장에서 애플리케이션을 배타적으로 사용하는 것으로 보입니다.
* **가용성**: 개별 테넌트는 가능하면 SLA에 정의된 보증에 따라 애플리케이션을 지속적으로 사용할 수 있기를 바랍니다. 또한 다른 테넌트의 활동이 애플리케이션의 가용성에 영향을 미쳐서는 안 됩니다.
* **확장성**:개별 테넌트의 요구를 충족할 수 있도록 애플리케이션이 확장됩니다. 다른 테넌트의 존재와 동작이 애플리케이션의 성능에 영향을 미쳐서는 안 됩니다.
* **비용**: 다중 테넌트 지원은 리소스 공유를 가능하게 하므로 전용 단일 테넌트 애플리케이션을 실행하는 것보다 비용이 낮습니다.
* **사용자 지정 가능성**. 기능 추가 또는 제거, 색 및 로고 변경, 고유 코드 또는 스크립트 추가 등과 같은 다양한 방식으로 개별 테넌트에 맞게 애플리케이션을 사용자 지정하는 기능입니다.

간단히 말해서 확장성이 뛰어난 서비스를 제공 하기 위해 고려해 야 할 여러 가지 고려 사항이 있지만 많은 다중 테 넌 트 응용 프로그램에 공통 되는 다양 한 목표와 요구 사항도 있습니다. 일부는 특정 시나리오에서 적절하지 않을 수 있으며, 개별 목표와 요구 사항의 중요성은 시나리오별로 다릅니다. 다중 테 넌 트 응용 프로그램의 공급자는 테 넌 트의 목표 및 요구 사항 충족, 수익성, 청구, 여러 서비스 수준, 프로 비전, 유지 관리 모니터링 및 자동화와 같은 목표와 요구 사항도 있습니다.

다중 테넌트 애플리케이션 설계와 관련된 추가 고려 사항에 대한 자세한 내용은 [Azure에서 다중 테넌트 애플리케이션 호스트][Hosting a Multi-Tenant Application on Azure](영문)를 참조하십시오. 다중 테넌트 SaaS(software-as-a-service) 데이터베이스 애플리케이션의 일반적인 데이터 아키텍처 패턴에 대한 정보는 [Azure SQL Database를 사용한 다중 테넌트 SaaS 애플리케이션의 설계 패턴](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md)을 참조하세요. 

Azure에는 다중 테넌트 시스템을 설계할 때 발생하는 주요 문제를 해결할 수 있는 많은 기능이 있습니다.

**격리**

* TLS 통신을 사용 하거나 사용 하지 않고 호스트 헤더를 사용 하 여 웹 사이트 테 넌 트 분할
* 쿼리 매개 변수로 웹 사이트 테넌트 분할
* 작업자 역할의 웹 서비스
  * 일반적으로 응용 프로그램의 백 엔드에서 데이터를 처리 하는 작업자 역할입니다.
  * 웹 역할. 일반적으로 애플리케이션의 프런트 엔드 역할을 합니다.

**스토리지**

대량으로 구조화 되지 않은 데이터를 저장 하는 서비스를 제공 하는 Table service 같은 Azure SQL Database 또는 Azure Storage 서비스와 같은 데이터 관리 및 비디오, 오디오 및 이미지와 같은 이진 데이터 또는 많은 양의 구조화 되지 않은 텍스트를 저장 하는 서비스를 제공 하는 Blob service.

* 테 넌 트 당 SQL Database SQL Server 로그인에 다중 테 넌 트 데이터를 보호 합니다.
* 컨테이너 수준 액세스 정책을 지정 하 여 응용 프로그램 리소스에 Azure 테이블을 사용 하 여 공유 액세스 서명으로 보호 되는 리소스에 대 한 새 URL을 발급 하지 않고도 권한을 조정 하는 기능을 사용할 수 있습니다.
* 애플리케이션 리소스의 Azure 큐 - Azure 큐는 일반적으로 테넌트를 대신하여 처리를 구동하는 데 사용되지만 프로비전 또는 관리에 필요한 작업을 분산하는 데에도 사용됩니다.
* 애플리케이션 리소스의 Service Bus 큐 - 공유 서비스에 작업을 밀어 넣습니다. 각 전송 테넌트는 이 큐에 밀어 넣는 권한(ACS에서 발급한 클레임에서 파생)만 가지고 있고, 서비스로부터 수신하는 테넌트만 다중 테넌트에서 제공되는 데이터를 큐에서 끌어오는 권한을 가지는 단일 큐를 사용할 수 있습니다.

**연결 및 보안 서비스**

* 애플리케이션 사이에 배치되어 높은 확장성과 복원력을 위해 느슨한 결합 방식을 사용하여 메시지를 교환할 수 있게 하는 메시징 인프라인 Azure Service Bus가 있습니다.

**네트워킹 서비스**

Azure에서는 몇 가지 네트워킹 서비스를 통해 인증을 지원하고 호스트된 애플리케이션의 관리 효율성을 향상합니다. 이러한 서비스에는 다음 항목이 포함됩니다.

* Azure Virtual Network를 사용하여 Azure에서 VPN(가상 사설망)을 프로비전하고 관리하며 온-프레미스 IT 인프라와 안전하게 연결할 수 있습니다.
* Virtual Network Traffic Manager를 사용하면 같은 데이터 센터에서 또는 세계적으로 서로 다른 데이터 센터에서 실행 중인 들어오는 트래픽의 부하를 여러 Azure 호스티드 서비스에 분산할 수 있습니다.
* Azure AD(Azure Active Directory)는 클라우드 애플리케이션과 관련하여 ID를 관리하고 액세스를 제어할 수 있게 하는 최신 REST 기반 서비스입니다. 응용 프로그램 리소스에 Azure AD를 사용 하면 인증 및 권한 부여 기능이 코드에서 제외 될 수 있도록 하는 동시에 사용자를 인증 하 고 권한을 부여 하 여 웹 응용 프로그램 및 서비스에 액세스할 수 있는 쉬운 방법을 제공 합니다.
* Azure Service Bus는 분산 및 하이브리드 애플리케이션에 대해 복잡한 방화벽 및 보안 인프라 없이도 Azure 호스트된 애플리케이션과 온-프레미스 애플리케이션 및 서비스 사이의 통신과 같은 보안 메시징 및 데이터 흐름 기능을 제공합니다. 응용 프로그램 리소스에 대 한 Service Bus Relay를 사용 하 여 끝점으로 노출 되는 서비스에 액세스할 수 있습니다 (예: 온-프레미스와 같이 시스템 외부에서 호스트). 또는 테 넌 트 용으로 특별히 프로 비전 된 서비스 (중요 한 테 넌 트 별 데이터는 발생 하기 때문)입니다.

**리소스 프로비전**

Azure는 응용 프로그램에 대 한 새 테 넌 트를 프로 비전 하는 여러 가지 방법을 제공 합니다. 테넌트가 많은 다중 테넌트 애플리케이션의 경우 셀프 서비스 프로비전을 사용하여 이 프로세스를 자동화해야 하는 경우가 많습니다.

* 작업자 역할은 테넌트 리소스별로 프로비전하고 프로비전을 해제(예: 새 테넌트 등록 또는 취소 시)하며, 계량 용도로 메트릭을 수집하고, 특정 일정에 따르거나 핵심 성과 지표 임계값 초과에 대한 대응으로 확장을 관리할 수 있게 합니다. 이 역할은 업데이트 및 업그레이드를 솔루션에 밀어 넣는 데에도 사용할 수 있습니다.
* Azure Blob을 사용하여 새 테넌트용으로 컴퓨팅 또는 미리 초기화된 스토리지 리소스를 프로비전할 수 있으며 동시에 컴퓨팅 서비스 패키지, VHD 이미지, 기타 리소스를 보호하는 컨테이너 수준 액세스 정책을 제공합니다.
* 테넌트용으로 SQL Database 리소스를 프로비전하는 옵션에는 다음이 포함됩니다.
  
  * 스크립트의 DDL 또는 어셈블리 내 리소스로 포함
  * SQL Server 2008 R2 DAC 패키지를 프로그래밍 방식으로 배포
  * 마스터 참조 데이터베이스에서 복사
  * 데이터베이스 가져오기 및 내보내기를 사용하여 파일에서 새 데이터베이스 프로비전

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
