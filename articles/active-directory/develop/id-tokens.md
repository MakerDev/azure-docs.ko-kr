---
title: Microsoft id 플랫폼 ID 토큰 | Microsoft
titleSuffix: Microsoft identity platform
description: Azure AD v1.0 및 v2.0 (Microsoft identity platform) 끝점에서 내보낸 id_tokens를 사용 하는 방법에 대해 알아봅니다.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 07/29/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms:custom: fasttrack-edit
ms.openlocfilehash: e242e6ce59c715cf3a9ca95523a9a9eda274407a
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87418919"
---
# <a name="microsoft-identity-platform-id-tokens"></a>Microsoft id 플랫폼 ID 토큰

`id_tokens`는 oidc ( [Openid connect Connect](v2-protocols-oidc.md) ) 흐름의 일부로 클라이언트 응용 프로그램으로 전송 됩니다. 액세스 토큰과 함께 또는 액세스 토큰 대신 보낼 수 있고 클라이언트가 사용자를 인증하는 데 사용됩니다.

## <a name="using-the-id_token"></a>id_token 사용

ID 토큰은 사용자가 누구 인지를 확인 하 고, 사용자에 대 한 추가 유용한 정보를 얻는 데 사용 되어야 합니다. [액세스 토큰](access-tokens.md)대신 권한 부여에 사용 하면 안 됩니다. 제공 되는 클레임은 응용 프로그램 내의 UX, [데이터베이스의 키](#using-claims-to-reliably-identify-a-user-subject-and-object-id)로, 클라이언트 응용 프로그램에 대 한 액세스를 제공 하는 데 사용할 수 있습니다.  

## <a name="claims-in-an-id_token"></a>Id_token의 클레임

`id_tokens`는 [Jwts](https://tools.ietf.org/html/rfc7519) (JSON 웹 토큰)입니다. 즉, 헤더, 페이로드 및 서명 부분으로 구성 됩니다. 헤더와 서명을 사용하여 토큰의 신뢰성을 확인할 수 있으며, 페이로드에는 클라이언트가 요청한 사용자에 관한 정보가 포함됩니다. 명시 된 경우를 제외 하 고 여기에 나열 된 모든 JWT 클레임은 v 1.0 및 v2.0 토큰에 표시 됩니다.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

[jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q)에서 이 v1.0 샘플 토큰을 확인합니다.

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw
```

[jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw)에서 이 v2.0 샘플 토큰을 확인합니다.

### <a name="header-claims"></a>헤더 클레임

|클레임 | 형식 | 설명 |
|-----|--------|-------------|
|`typ` | 문자열 - 항상 "JWT" | 토큰이 JWT 토큰 임을 나타냅니다.|
|`alg` | String | 토큰을 서명하는 데 사용된 알고리즘을 나타냅니다. 예: "RS256" |
|`kid` | String | 이 토큰에 서명하는 데 사용되는 공개 키의 지문입니다. v1.0 및 v2.0 액세스 `id_tokens`로 내보냅니다. |
|`x5t` | String | `kid`와 동일합니다(사용 및 값). 그러나 호환성을 위해 v1.0 `id_tokens`로만 내보내는 레거시 클레임입니다. |

### <a name="payload-claims"></a>페이로드 클레임

이 목록에는 기본적으로 가장 id_tokens 된 JWT 클레임이 표시 됩니다 (명시 된 경우 제외).  그러나 앱은 [선택적 클레임](active-directory-optional-claims.md) 을 사용 하 여 id_token에서 추가 JWT 클레임을 요청할 수 있습니다.  이러한 범위는 클레임에서 `groups` 사용자의 이름에 대 한 정보와 같은 범위에 있을 수 있습니다.

|클레임 | 형식 | Description |
|-----|--------|-------------|
|`aud` |  문자열, 앱 ID URI | 토큰의 의도한 수신자를 식별합니다. `id_tokens`에서 액세스 토큰에서 대상은 Azure Portal에서 앱에 할당된 앱의 애플리케이션 ID입니다. 앱은 이 값의 유효성을 검사하고 값이 일치하지 않을 경우 토큰을 거부해야 합니다. |
|`iss` |  문자열, STS URI | 토큰을 생성하고 반환하는 STS(보안 토큰 서비스) 및 사용자가 인증된 Azure AD 테넌트를 식별합니다. v2.0 엔드포인트에서 토큰을 발급한 경우 URI는 `/v2.0`에서 종료됩니다.  사용자가 Microsoft 계정의 소비자 사용자임을 나타내는 GUID는 `9188040d-6c67-4c5b-b112-36a304b66dad`입니다. 앱은 클레임의 GUID 부분을 사용하여 앱에 로그인할 수 있는 테넌트 집합을 제한할 수 있습니다(해당되는 경우). |
|`iat` |  int, UNIX 타임스탬프 | "발급 시간"은 이 토큰에 대한 인증이 발생한 시간을 나타냅니다.  |
|`idp`|문자열, 대개 STS URI입니다. | 토큰의 주체를 인증한 ID 공급자를 기록합니다. 이 값은 사용자 계정이 발행자(예: 게스트)와 동일한 테넌트에 속하지 않는 경우 발행자 클레임의 값과 동일합니다. 클레임이 없는 경우 `iss` 값을 대신 사용할 수 있음을 의미합니다.  조직 컨텍스트(예: Azure AD 테넌트에 초대된 개인 계정)에서 사용되는 개인 계정의 경우, `idp` 클레임은 ‘live.com’ 또는 Microsoft 계정 테넌트 `9188040d-6c67-4c5b-b112-36a304b66dad`가 포함된 STS URI일 수 있습니다. |
|`nbf` |  int, UNIX 타임스탬프 | "nbf"(not before) 클레임은 JWT가 그 이전에는 처리를 허용하지 않아야 하는 시간을 식별합니다.|
|`exp` |  int, UNIX 타임스탬프 | "exp"(만료 시간) 클레임은 JWT가 그 이후에는 처리를 허용하지 않아야 하는 만료 시간을 식별합니다.  예를 들어 인증이 필요 하거나 토큰 해지가 검색 된 경우와 같이 리소스는이 시간 전에 토큰을 거부할 수 있습니다. |
| `c_hash`| String |코드 해시는 OAuth 2.0 권한 부여 코드와 함께 ID 토큰이 발급된 경우에만 ID 토큰에 포함됩니다. 이 코드 해시는 인증 코드의 신뢰성이 유효한지 검사하는 데 사용할 수 있습니다. 이 유효성 검사를 수행하는 방법에 대한 자세한 내용은 [OpenID Connect 사양](https://openid.net/specs/openid-connect-core-1_0.html)을 참조하세요. |
|`at_hash`| String |액세스 토큰 해시는 `/authorize` OAuth 2.0 액세스 토큰을 사용 하 여 끝점에서 id 토큰을 발급 하는 경우에만 id 토큰에 포함 됩니다. 액세스 토큰의 신뢰성이 유효한지 검사하는 데 사용할 수 있습니다. 이 유효성 검사를 수행하는 방법에 대한 자세한 내용은 [OpenID Connect 사양](https://openid.net/specs/openid-connect-core-1_0.html#HybridIDToken)을 참조하세요. 이는 끝점의 ID 토큰에서 반환 되지 않습니다 `/token` . |
|`aio` | 불투명 문자열 | Azure AD에서 토큰을 다시 사용하기 위해 데이터를 기록하는 데 사용하는 내부 클레임입니다. 무시됩니다.|
|`preferred_username` | String | 사용자를 나타내는 기본 사용자 이름입니다. 메일 주소, 전화 번호 또는 지정된 형식이 없는 일반 사용자 이름일 수 있습니다. 해당 값은 변경 가능하며 시간이 지남에 따라 변경될 수 있습니다. 해당 값은 변경 가능하므로 권한 부여 결정을 내리는 데 사용되지 않아야 합니다. `profile`이 클레임을 받으려면 범위가 필요 합니다.|
|`email` | String | `email` 클레임은 이메일 주소가 있는 게스트 계정에 대해 기본적으로 제공됩니다.  사용자 앱은 `email` [선택적 클레임](active-directory-optional-claims.md)을 사용하여 관리되는 사용자(리소스와 동일한 테넌트의 사용자)에 대한 이메일 클레임을 요청할 수 있습니다.  v2.0 엔드포인트에서 사용자 앱은 `email` OpenID Connect 범위를 요청할 수도 있지만, 클레임을 가져오기 위해 선택적 클레임 및 범위를 모두 요청할 필요는 없습니다.  이메일 클레임은 사용자의 프로필 정보에서 주소 지정이 가능한 메일만 지원합니다. |
|`name` | String | `name` 클레임은 토큰의 주체를 식별하는, 사람이 읽을 수 있는 값을 제공합니다. 값은 고유 하지 않을 수 있으며, 변경 가능 하 고, 표시 용도로만 사용 하도록 설계 되었습니다. `profile`이 클레임을 받으려면 범위가 필요 합니다. |
|`nonce`| String | Nonce는 원본에 포함된 매개 변수와 일치하며 IDP에 대한 요청을 인증합니다. 일치하지 않는 경우 애플리케이션이 토큰을 거부해야 합니다. |
|`oid` | 문자열, GUID | 이 경우에 Microsoft ID 시스템에 있는 개체의 변경할 수 없는 식별자는 사용자 계정입니다. 이 ID는 애플리케이션에서 사용자를 고유하게 식별합니다. 동일한 사용자가 로그인한 두 개의 다른 애플리케이션은 `oid` 클레임에서 동일한 값을 받습니다. Microsoft Graph는 이 ID를 지정된 사용자 계정에 대한 `id` 속성으로 반환합니다. 에서는 `oid` 여러 앱에서 사용자의 상관 관계를 지정할 수 있으므로 `profile` 이 클레임을 받으려면 범위가 필요 합니다. 단일 사용자가 여러 테 넌 트에 있는 경우 사용자는 각 테 넌 트에 다른 개체 ID를 포함 합니다. 사용자가 동일한 자격 증명을 사용 하는 각 계정에 로그인 하더라도 다른 계정으로 간주 됩니다. `oid`클레임은 GUID 이며 다시 사용할 수 없습니다. |
|`roles`| 문자열 배열 | 로그인 하는 사용자에 게 할당 된 역할 집합입니다. |
|`rh` | 불투명 문자열 |Azure에서 토큰의 유효성을 다시 검사하기 위해 사용하는 내부 클레임입니다. 무시됩니다. |
|`sub` | 문자열, GUID | 앱 사용자 등 토큰에서 정보를 어설션하는 보안 주체입니다. 이 값은 변경할 수 없으며 재할당 또는 재사용할 수 없습니다. 주체는 쌍으로 된 식별자이며 특정 애플리케이션 ID에 고유합니다. 단일 사용자가 두 개의 다른 클라이언트 Id를 사용 하 여 두 개의 다른 앱에 로그인 하는 경우 해당 앱은 주체 클레임에 대해 두 개의 다른 값을 받게 됩니다. 이는 아키텍처 및 개인 정보 요구 사항에 따라 필요 하거나 필요 하지 않을 수 있습니다. |
|`tid` | 문자열, GUID | 사용자가 속해 있는 Azure AD 테넌트를 나타내는 GUID입니다. 회사 및 학교 계정의 경우 GUID는 사용자가 속해 있는 조직의 변경이 불가능한 테넌트 ID입니다. 개인 계정의 경우 이 값은 `9188040d-6c67-4c5b-b112-36a304b66dad`입니다. `profile`이 클레임을 받으려면 범위가 필요 합니다. |
|`unique_name` | String | 토큰의 주체를 식별하는, 사람이 인식할 수 있는 값을 제공합니다. 이 값은 지정 된 시점에 고유 하지만 메일 및 기타 식별자를 다시 사용할 수 있으므로이 값은 다른 계정에 다시 나타날 수 있으므로 표시 목적 으로만 사용 해야 합니다. v1.0 `id_tokens`로만 발급됩니다. |
|`uti` | 불투명 문자열 | Azure에서 토큰의 유효성을 다시 검사하기 위해 사용하는 내부 클레임입니다. 무시됩니다. |
|`ver` | 문자열, 1.0 또는 2.0 | id_token의 버전을 나타냅니다. |

> [!NOTE]
> V1.0 및 v2.0 id_token 위의 예제에서 볼 수 있는 것 처럼 전달 되는 정보의 양에 차이가 있습니다. 이 버전은 요청 된 끝점을 기준으로 합니다. 기존 응용 프로그램에서 Azure AD 끝점을 사용 하는 경우 새 응용 프로그램은 v2.0 "Microsoft id 플랫폼" 끝점을 사용 해야 합니다.
>
> - v1.0: Azure AD 끝점:`https://login.microsoftonline.com/common/oauth2/authorize`
> - v2.0: Microsoft id 플랫폼 끝점:`https://login.microsoftonline.com/common/oauth2/v2.0/authorize`

### <a name="using-claims-to-reliably-identify-a-user-subject-and-object-id"></a>클레임을 사용 하 여 사용자를 안정적으로 식별 (주체 및 개체 ID)

사용자를 식별 하는 경우 (예를 들어 데이터베이스에서 조회 하거나 해당 사용자가 보유 한 권한을 결정 하는 경우)에는 일정 시간에 걸쳐 일정 하 게 유지 되는 정보를 사용 하는 것이 중요 합니다.  레거시 응용 프로그램은 전자 메일 주소, 전화 번호 또는 UPN과 같은 필드를 사용 하는 경우도 있습니다.  이러한 모든 항목은 시간이 지남에 따라 변경 될 수 있으며 시간이 지남에 따라 다시 사용 될 수도 있습니다. 직원의 이름을 변경 하거나 직원에 게 이전에는 직원을 더 이상 표시 하지 않는 전자 메일 주소를 지정 합니다. 따라서 사용자가 읽을 수 있는 사용자를 식별 하기 위해 응용 프로그램에서 사람이 읽을 수 있는 데이터를 사용 하지 않는 것이 **중요** 합니다.  대신 OIDC 표준에서 제공 하는 클레임 또는 Microsoft에서 제공 하는 확장 클레임 (및 클레임)을 사용 `sub` `oid` 합니다.

사용자 단위 정보를 정확 하 게 저장 하려면 `sub` `oid` `tid` 필요한 경우 라우팅 또는 분할에 사용 되는 또는 단독 (guid가 고유 함)을 사용 합니다.  서비스 간에 데이터를 공유 해야 하는 경우 `oid` + `tid` 모든 앱이 `oid` 지정 된 사용자에 대해 동일 하 고 클레임을 얻게 되는 것이 가장 좋습니다 `tid` .  `sub`Microsoft id 플랫폼의 클레임은 "페어링" 이며 토큰 수신자, 테 넌 트 및 사용자의 조합에 따라 고유 합니다.  따라서 지정 된 사용자에 대 한 ID 토큰을 요청 하는 두 개의 앱은 다른 `sub` 클레임을 수신 하지만 `oid` 해당 사용자에 대 한 클레임은 동일 합니다.

>[!NOTE]
> 사용자에 `idp` 대 한 정보를 테 넌 트 간에 상관 관계를 저장 하기 위해 클레임을 사용 하지 마세요.  `oid` `sub` 응용 프로그램이 테 넌 트 간에 사용자를 추적할 수 없도록 설계를 통해 테 넌 트 간에 사용자 변경에 대 한 및 클레임이 변경 되기 때문에 작동 하지 않습니다.  
>
> 사용자가 한 테 넌 트에 있고 다른 사용자에서 인증 하는 게스트 시나리오는 사용자가 서비스에 대 한 새 사용자 인 것 처럼 사용자를 처리 해야 합니다.  Contoso 테 넌 트의 문서와 권한은 Fabrikam 테 넌 트에서 적용 되지 않습니다. 이는 테 넌 트 간 실수로 인 한 데이터 누출을 방지 하는 데 중요 합니다.

## <a name="validating-an-id_token"></a>id_token을 유효성 검사하는 중

의 유효성을 검사 `id_token` 하는 것은 [액세스 토큰의 유효성을 검사](access-tokens.md#validating-tokens) 하는 첫 번째 단계와 유사 합니다. 클라이언트는 올바른 발급자가 토큰을 다시 보내고 변조 되지 않았는지 확인할 수 있습니다. `id_tokens`는 항상 JWT 토큰이 기 때문에 이러한 토큰의 유효성을 검사 하는 많은 라이브러리가 있습니다. 이러한 토큰을 직접 사용 하는 대신이 중 하나를 사용 하는 것이 좋습니다.  기밀 클라이언트만 (암호 포함) ID 토큰의 유효성을 검사 해야 합니다.  악의적인 사용자가 토큰의 유효성 검사에 사용 되는 키를 가로채 고 편집할 수 있으므로 공용 응용 프로그램 (사용자가 제어 하지 않는 장치 또는 네트워크에서 완전히 실행 되는 코드 (예를 들어, 사용자의 브라우저 또는 홈 네트워크)에서 ID 토큰의 유효성을 검사 하는 것은 효율적이 지 않습니다.

수동으로 토큰 유효성을 검사하려면 [액세스 토큰의 유효성 검사](access-tokens.md#validating-tokens)에서 자세한 단계를 참조하세요. 토큰에서 서명의 유효성을 검사 한 후에는 id_token에서 다음 JWT 클레임의 유효성을 검사 해야 합니다 (토큰 유효성 검사 라이브러리에 의해 수행 될 수도 있음).

* 타임스탬프: `iat`, `nbf` 및 `exp` 타임스탬프는 모두 적절히 현재 시간 이전이거나 이후여야 합니다.
* 대상: `aud` 클레임은 애플리케이션의 앱 ID와 일치해야 합니다.
* Nonce: 페이로드의 `nonce` 클레임은 최초 요청 중에 /authorize 엔드포인트로 전달된 nonce 매개 변수와 일치해야 합니다.

## <a name="next-steps"></a>다음 단계

* [액세스 토큰](access-tokens.md) 에 대 한 자세한 정보
* [선택적 클레임](active-directory-optional-claims.md)을 사용 하 여 id_token JWT 클레임을 사용자 지정 합니다.
