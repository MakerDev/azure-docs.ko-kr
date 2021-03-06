---
title: 지역화-Azure Active Directory B2C
description: Azure Active Directory B2C에서 사용자 지정 정책의 Localization 요소를 지정하는 방법을 설명합니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 84850b7d44033a2759c51c5c6b9c53d1c945a99d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005381"
---
# <a name="localization-element"></a>Localization 요소

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Localization** 요소를 사용하면 사용자 경험용 정책에서 여러 로캘이나 언어를 지원할 수 있습니다. 정책의 지역화 지원을 사용하여 다음을 수행할 수 있습니다.

- 정책에서 지원되는 언어의 명시적 목록을 설정하고 기본 언어를 선택합니다.
- 언어별 문자열 및 컬렉션을 제공합니다.

```xml
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

**Localization** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| 사용 | 아니요 | 가능한 값은 `true` 또는 `false`입니다. |

**Localization** 요소에는 다음 XML 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1:n | 지원되는 언어 목록입니다. |
| LocalizedResources | 0:n | 지역화된 리소스 목록입니다. |

## <a name="supportedlanguages"></a>SupportedLanguages

**SupportedLanguages** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| DefaultLanguage | 예 | 지역화된 리소스의 기본값으로 사용할 언어입니다. |
| MergeBehavior | 아니요 | 동일한 식별자를 가진, 부모 정책에 있는 모든 ClaimType과 함께 병합되는 값의 열거형 값입니다. 기본 정책에 지정된 클레임을 덮어쓰는 경우 이 특성을 사용합니다. 가능한 값은 `Append`, `Prepend` 또는 `ReplaceAll`입니다. `Append` 값은 부모 정책에 지정된 컬렉션의 끝에 데이터 컬렉션을 추가하도록 지정합니다. `Prepend` 값은 부모 정책에 지정된 컬렉션 앞에 데이터 컬렉션을 추가하도록 지정합니다. `ReplaceAll` 값은 부모 정책에 정의된 데이터 컬렉션을 무시하고 현재 정책에 정의된 데이터를 대신 사용하도록 지정합니다. |

### <a name="supportedlanguages"></a>SupportedLanguages

**SupportedLanguages** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1:n | RFC 5646 - 언어 식별 태그를 기준으로 언어 태그를 준수하는 콘텐츠를 표시합니다. |

## <a name="localizedresources"></a>LocalizedResources

**LocalizedResources** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| Id | 예 | 지역화된 리소스를 고유하게 식별하는 데 사용되는 식별자입니다. |

**LocalizedResources** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0:n | 다양한 문화권에서 전체 컬렉션을 정의합니다. 컬렉션은 문화권마다 여러 항목과 다른 문자열을 포함할 수 있습니다. 컬렉션의 예로는 클레임 유형에 표시되는 열거형이 있습니다. 예를 들어 국가/지역 목록이 드롭다운 목록을 통해 사용자에게 표시됩니다. |
| LocalizedStrings | 0:n | 다양한 문화권에서 컬렉션에 표시되는 문자열을 제외한 모든 문자열을 정의합니다. |

### <a name="localizedcollections"></a>LocalizedCollections

**LocalizedCollections** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1:n | 지원되는 언어 목록입니다. |

#### <a name="localizedcollection"></a>LocalizedCollection

**LocalizedCollection** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| ElementType | 예 | 정책 파일의 ClaimType 요소 또는 사용자 인터페이스 요소를 참조합니다. |
| ElementId | 예 | **ElementType**이 ClaimType으로 설정된 경우 사용하는 ClaimsSchema 섹션에 이미 정의된 클레임 유형에 대한 참조를 포함하는 문자열입니다. |
| TargetCollection | 예 | 대상 컬렉션입니다. |

**LocalizedCollection** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| 항목 | 0:n | 드롭다운의 값과 같이 사용자 인터페이스에서 사용자가 클레임에 대해 선택할 수 있는 옵션을 정의합니다. |

**Item** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| 텍스트 | 예 | 사용자 인터페이스에서 이 옵션에 대해 사용자에게 표시되어야 하는 사용자에게 친숙한 표시 문자열입니다. |
| 값 | 예 | 이 옵션 선택과 연결된 문자열 클레임 값입니다. |
| SelectByDefault | 아니요 | UI에서 이 옵션을 기본적으로 선택해야 하는지 여부를 나타냅니다. 가능한 값은 True 또는 False입니다. |

다음 예제는 **LocalizedCollections** 요소의 사용을 보여 줍니다. 각각 영어와 스페인어로 된 두 개의 **LocalizedCollection** 요소를 포함합니다. 둘 다 영어 및 스페인어 항목 목록을 사용하여 `Gender` 클레임의 **Restriction** 컬렉션을 설정합니다.

```xml
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>
```

### <a name="localizedstrings"></a>LocalizedStrings

**LocalizedStrings** 요소에는 다음 요소가 포함됩니다.

| 요소 | 발생 수 | 설명 |
| ------- | ----------- | ----------- |
| LocalizedString | 1:n | 지역화된 문자열입니다. |

**LocalizedString** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| ElementType | 예 | 가능한 값: [ClaimsProvider](#claimsprovider), [ClaimType](#claimtype), [ErrorMessage](#errormessage), [GetLocalizedStringsTransformationClaimType](#getlocalizedstringstransformationclaimtype), [Predicate](#predicate), [inputvalidation](#inputvalidation)또는 [UxElement](#uxelement).   | 
| ElementId | 예 | **ElementType** 을, 또는로 설정 하면 `ClaimType` `Predicate` `InputValidation` 이 요소는 ClaimsSchema 섹션에 이미 정의 된 클레임 형식에 대 한 참조를 포함 합니다. |
| StringId | 예 | **ElementType**이 `ClaimType`으로 설정된 경우 이 요소는 클레임 유형의 특성에 대한 참조를 포함합니다. 가능한 값은 `DisplayName`, `AdminHelpText` 또는 `PatternHelpText`입니다. `DisplayName` 값은 클레임 표시 이름을 설정하는 데 사용됩니다. `AdminHelpText` 값은 클레임 사용자의 도움말 텍스트 이름을 설정하는 데 사용됩니다. `PatternHelpText` 값은 클레임 패턴 도움말 텍스트를 설정하는 데 사용됩니다. **ElementType**이 `UxElement`로 설정된 경우 이 요소는 사용자 인터페이스 요소의 특성에 대한 참조를 포함합니다. **ElementType**이 `ErrorMessage`로 설정된 경우 이 요소는 오류 메시지의 식별자를 지정합니다. `UxElement` 식별자의 전체 목록은 [지역화 문자열 ID](localization-string-ids.md)를 참조하세요.|

## <a name="elementtype"></a>ElementType

지역화할 정책의 클레임 형식, 클레임 변환 또는 사용자 인터페이스 요소에 대 한 ElementType 참조입니다.

| 지역화할 요소 | ElementType | ElementId |StringId |
| --------- | -------- | ----------- |----------- |
| Id 공급자 이름 |`ClaimsProvider`| | Claim이상 변경 요소의 ID입니다.|
| 클레임 유형 특성|`ClaimType`|클레임 유형 이름| 지역화할 클레임의 특성입니다. 가능한 값은 `AdminHelpText` , `DisplayName` , `PatternHelpText` 및 `UserHelpText` 입니다.|
|오류 메시지|`ErrorMessage`||오류 메시지의 ID입니다. |
|지역화 된 문자열을 클레임에 복사 합니다.|`GetLocalizedStringsTra nsformationClaimType`||출력 클레임의 이름입니다.|
|조건자 사용자 메시지|`Predicate`|조건자 이름| 지역화할 조건자의 특성입니다. 가능한 값은 `HelpText` 입니다.|
|조건자 그룹 사용자 메시지|`InputValidation`|PredicateValidation 요소의 ID입니다.|PredicateGroup 요소의 ID입니다. 조건자 그룹은 ElementId에 정의 된 대로 조건자 유효성 검사 요소의 자식 이어야 합니다.|
|사용자 인터페이스 요소 |`UxElement` | | 지역화할 사용자 인터페이스 요소의 ID입니다.|

## <a name="examples"></a>예

### <a name="claimsprovider"></a>ClaimsProvider

ClaimsProvider 값은 클레임 공급자 표시 이름 중 하나를 지역화 하는 데 사용 됩니다. 

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
  </ClaimsExchanges>
</OrchestrationStep>

```

다음 예제에서는 클레임 공급자의 표시 이름을 지역화 하는 방법을 보여 줍니다.

```xml
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">Facebook</LocalizedString>
<LocalizedString ElementType="ClaimsProvider" StringId="GoogleExchange">Google</LocalizedString>
<LocalizedString ElementType="ClaimsProvider" StringId="LinkedInExchange">LinkedIn</LocalizedString>
```

### <a name="claimtype"></a>ClaimType

ClaimType 값은 클레임 특성 중 하나를 지역화 하는 데 사용 됩니다. 

```xml
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

다음 예제에서는 전자 메일 클레임 형식의 DisplayName, UserHelpText 및 PatternHelpText 특성을 지역화 하는 방법을 보여 줍니다.

```xml
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
```

### <a name="errormessage"></a>ErrorMessage

ErrorMessage 값은 시스템 오류 메시지 중 하나를 지역화 하는 데 사용 됩니다. 

```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

다음 예제에서는 UserMessageIfClaimsPrincipalAlreadyExists 오류 메시지를 지역화 하는 방법을 보여 줍니다.


```xml
<LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
```

### <a name="getlocalizedstringstransformationclaimtype"></a>GetLocalizedStringsTransformationClaimType

GetLocalizedStringsTransformationClaimType 값은 지역화 된 문자열을 클레임에 복사 하는 데 사용 됩니다. 자세한 내용은 [GetLocalizedStringsTransformation 클레임 변환](string-transformations.md#getlocalizedstringstransformation) 을 참조 하세요.


```xml
<ClaimsTransformation Id="GetLocalizedStringsForEmail" TransformationMethod="GetLocalizedStringsTransformation">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="subject" TransformationClaimType="email_subject" />
    <OutputClaim ClaimTypeReferenceId="message" TransformationClaimType="email_message" />
    <OutputClaim ClaimTypeReferenceId="codeIntro" TransformationClaimType="email_code" />
    <OutputClaim ClaimTypeReferenceId="signature" TransformationClaimType="email_signature" />
   </OutputClaims>
</ClaimsTransformation>
```

다음 예제에서는 GetLocalizedStringsTransformation 클레임 변환의 출력 클레임을 지역화 하는 방법을 보여 줍니다.

```xml
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_subject">Contoso account email verification code</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_message">Thanks for verifying your account!</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_code">Your code is</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_signature">Sincerely</LocalizedString>
```

### <a name="predicate"></a>Predicate

조건자 값은 [조건자](predicates.md) 오류 메시지 중 하나를 지역화 하는 데 사용 됩니다. 

```xml
<Predicates>
  <Predicate Id="LengthRange" Method="IsLengthRange"  HelpText="The password must be between 6 and 64 characters.">
    <Parameters>
      <Parameter Id="Minimum">6</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>
  <Predicate Id="Lowercase" Method="IncludesCharacters" HelpText="a lowercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>
  <Predicate Id="Uppercase" Method="IncludesCharacters" HelpText="an uppercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

다음 예제에서는 조건자 도움말 텍스트를 지역화 하는 방법을 보여 줍니다.

```xml
<LocalizedString ElementType="Predicate" ElementId="LengthRange" StringId="HelpText">The password must be between 6 and 64 characters.</LocalizedString>
<LocalizedString ElementType="Predicate" ElementId="Lowercase" StringId="HelpText">a lowercase letter</LocalizedString>
<LocalizedString ElementType="Predicate" ElementId="Uppercase" StringId="HelpText">an uppercase letter</LocalizedString>
```

### <a name="inputvalidation"></a>InputValidation

InputValidation 값은 [PredicateValidation](predicates.md) group 오류 메시지 중 하나를 지역화 하는 데 사용 됩니다. 

```xml
<PredicateValidations>
  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences MatchAtLeast="1">
          <PredicateReference Id="LengthRange" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

다음 예에서는 조건자 유효성 검사 그룹 도움말 텍스트를 지역화 하는 방법을 보여 줍니다.

```xml
<LocalizedString ElementType="InputValidation" ElementId="CustomPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>
```

### <a name="uxelement"></a>UxElement

UxElement 값은 사용자 인터페이스 요소 중 하나를 지역화 하는 데 사용 됩니다. 다음 예제에서는 계속 및 취소 단추를 지역화 하는 방법을 보여 줍니다.

```xml
<LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
<LocalizedString ElementType="UxElement" StringId="button_cancel">Cancel</LocalizedString>
```

## <a name="next-steps"></a>다음 단계

지역화 예제는 다음 문서를 참조 하세요.

- [Azure Active Directory B2C에서 사용자 지정 정책을 사용 하 여 언어 사용자 지정](custom-policy-localization.md)
- [Azure Active Directory B2C에서 사용자 흐름을 사용 하 여 언어 사용자 지정](user-flow-language-customization.md)
