---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: ae431d27af4a1717a32e348ba852f9b2b9fe9331
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87451122"
---
| 리소스 | 표준 파일 공유 | 프리미엄 파일 공유 |
|----------|---------------|------------------------------------------|
| 파일 공유의 최소 크기 | 최솟값 없음, 종량제 | 100GiB, 프로비저닝됨 |
| 파일 공유의 최대 크기 | 100TiB*, 5TiB | 100TiB |
| 파일 공유의 최대 파일 크기 | 1TiB | 4TiB |
| 파일 공유의 최소 파일 수 | 제한 없음 | 제한 없음 |
| 공유당 최대 IOPS | 10,000IOPS*, 1,000IOPS | 100,000 IOPS |
| 파일 공유당 저장된 액세스 정책의 최대 수 | 5 | 5 |
| 단일 파일 공유에 대한 대상 처리량 | 최대 300MiB/초*, 최대 60MiB/초  | 프리미엄 파일 공유 수신 및 송신 값 참조|
| 단일 파일 공유에 대한 최대 송신 속도 | 표준 파일 공유 대상 처리량 참조 | 최대 6,204MiB/초 |
| 단일 파일 공유에 대한 최대 수신 속도 | 표준 파일 공유 대상 처리량 참조 | 최대 4,136MiB/초 |
| 파일 또는 디렉터리당 최대 열린 핸들 수 | 2,000개의 열린 핸들 | 2,000개의 열린 핸들 |
| 최대 공유 스냅샷 수 | 200개의 공유 스냅샷 | 200개의 공유 스냅샷 |
| 최대 개체(디렉터리 및 파일) 이름 길이 | 2,048자 | 2,048자 |
| 최대 경로 이름 구성 요소(경로 \A\B\C\D의 각 문자가 구성 요소) | 255자 | 255자 |

\* 표준 파일 공유의 기본값은 5TiB입니다. 표준 파일 공유 크기를 최대 100TiB까지 확장하는 방법에 대한 자세한 내용은 [대량 파일 공유 사용 및 만들기](../articles/storage/files/storage-files-how-to-create-large-file-share.md)를 참조하세요.
