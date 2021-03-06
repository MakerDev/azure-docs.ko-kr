---
title: TLS 구성-Azure Portal-Azure Database for MySQL
description: Azure Database for MySQL에 대 한 Azure Portal를 사용 하 여 TLS 구성을 설정 하는 방법을 알아봅니다.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 46eaa6a3b97967da9c4743d0cf1f6edc8f90b1ce
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119787"
---
# <a name="configuring-tls-settings-in-azure-database-for-mysql-using-azure-portal"></a>Azure Portal를 사용 하 여 Azure Database for MySQL에서 TLS 설정 구성

이 문서에서는 연결에 대해 허용 되는 최소 TLS 버전을 적용 하 고 최소 tls 버전의 모든 연결을 거부 하 여 네트워크 보안을 향상 시키기 위해 Azure Database for MySQL 서버를 구성할 수 있는 방법을 설명 합니다.

Azure Database for MySQL에 연결 하는 데 TLS 버전을 적용할 수 있습니다. 이제 고객은 데이터베이스 서버에 대 한 최소 TLS 버전을 설정 하도록 선택할 수 있습니다. 예를 들어이 최소 TLS 버전을 1.0로 설정 하면 TLS 1.0, 1.1 및 1.2를 사용 하 여 연결 하는 클라이언트를 허용 하는 것입니다. 또는이를 1.2로 설정 하면 TLS 1.2 +를 사용 하 여 연결 하는 클라이언트만 허용 하 고 TLS 1.0 및 TLS 1.1을 사용 하는 모든 들어오는 연결은 거부 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법 가이드를 완료하려면 다음이 필요합니다.

* [Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mysql"></a>Azure Database for MySQL에 대 한 TLS 구성 설정

MySQL server 최소 TLS 버전을 설정 하려면 다음 단계를 수행 합니다.

1. [Azure Portal](https://portal.azure.com/)에서 기존 Azure Database for MySQL 서버를 선택 합니다.

1. MySQL server 페이지의 **설정**에서 **연결 보안** 을 클릭 하 여 연결 보안 구성 페이지를 엽니다.

1. **최소 tls 버전**에서 MySQL 서버용 tls 1.2 보다 작은 tls 버전으로 연결을 거부 하려면 **1.2** 을 선택 합니다.

    ![Azure Database for MySQL TLS 구성](./media/howto-tls-configurations/setting-tls-value.png)

1. **저장**을 클릭하여 변경 내용을 저장합니다.

1. 연결 보안 설정이 성공적으로 설정 되었는지 확인 하는 알림이 나타납니다.

    ![Azure Database for MySQL TLS 구성 성공](./media/howto-tls-configurations/setting-tls-value-success.png)

## <a name="next-steps"></a>다음 단계

- [메트릭에 대 한 경고를 만드는 방법](howto-alert-on-metric.md) 에 대해 알아봅니다.