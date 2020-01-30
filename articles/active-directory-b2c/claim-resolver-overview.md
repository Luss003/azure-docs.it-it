---
title: Resolver di attestazioni nei criteri personalizzati
titleSuffix: Azure AD B2C
description: Informazioni su come usare i resolver di attestazioni in un criterio personalizzato in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: bc8dbfd315702f666d6b811e855d6bcd99df938e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76836049"
---
# <a name="about-claim-resolvers-in-azure-active-directory-b2c-custom-policies"></a>Informazioni sui resolver di attestazioni nei criteri personalizzati in Azure Active Directory B2C

I resolver di attestazioni nei [criteri personalizzati](custom-policy-overview.md) Azure Active Directory B2C (Azure ad B2C) forniscono informazioni sul contesto di una richiesta di autorizzazione, ad esempio il nome del criterio, l'ID di correlazione della richiesta, la lingua dell'interfaccia utente e altro ancora.

Per usare un resolver di attestazioni in un'attestazione di input o output, si definisce un **ClaimType** di tipo stringa, nell'elemento [ClaimsSchema](claimsschema.md), quindi si imposta **DefaultValue** sul resolver di attestazioni nell'elemento attestazione di input o output. Azure AD B2C legge il valore del resolver di attestazioni e usa il valore nel profilo tecnico.

Nell'esempio seguente viene definito un tipo di attestazione denominato `correlationId` con **DataType**`string`.

```XML
<ClaimType Id="correlationId">
  <DisplayName>correlationId</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Request correlation Id</UserHelpText>
</ClaimType>
```

Nel profilo tecnico, mappare il resolver di attestazioni al tipo di attestazione. Azure AD B2C popola il valore del resolver di attestazioni `{Context:CorrelationId}` nell'attestazione `correlationId` e invia l'attestazione al profilo tecnico.

```XML
<InputClaim ClaimTypeReferenceId="correlationId" DefaultValue="{Context:CorrelationId}" />
```

## <a name="claim-resolver-types"></a>Tipi di resolver di attestazioni

Le sezioni seguenti elencano i resolver di attestazioni disponibili.

### <a name="culture"></a>Lingua

| Attestazione | Description | Esempio |
| ----- | ----------- | --------|
| {Culture:LanguageName} | Codice ISO di due lettere per la lingua. | en |
| {Culture:LCID}   | Identificatore LCID del codice della lingua. | 1033 |
| {Culture:RegionName} | Codice ISO di due lettere per la regione. | Stati Uniti |
| {Culture:RFC5646} | Codice RFC5646 della lingua. | it-IT |

### <a name="policy"></a>Criterio

| Attestazione | Description | Esempio |
| ----- | ----------- | --------|
| {Policy:PolicyId} | Nome dei criteri della relying party. | B2C_1A_signup_signin |
| {Policy:RelyingPartyTenantId} | ID del tenant dei criteri della relying party. | your-tenant.onmicrosoft.com |
| {Policy:TenantObjectId} | ID oggetto del tenant dei criteri della relying party. | 00000000-0000-0000-0000-000000000000 |
| {Policy:TrustFrameworkTenantId} | ID del tenant del framework attendibilità. | your-tenant.onmicrosoft.com |

### <a name="openid-connect"></a>OpenID Connect

| Attestazione | Description | Esempio |
| ----- | ----------- | --------|
| {OIDC:AuthenticationContextReferences} |Parametro di stringa di query `acr_values`. | N/D |
| {OIDC:ClientId} |Parametro di stringa di query `client_id`. | 00000000-0000-0000-0000-000000000000 |
| {OIDC:DomainHint} |Parametro di stringa di query `domain_hint`. | facebook.com |
| {OIDC:LoginHint} |  Parametro di stringa di query `login_hint`. | someone@contoso.com |
| {OIDC:MaxAge} | `max_age`. | N/D |
| {OIDC:Nonce} |Parametro di stringa di query `Nonce`. | defaultNonce |
| {OIDC:Prompt} | Parametro di stringa di query `prompt`. | login |
| {OIDC:Resource} |Parametro di stringa di query `resource`. | N/D |
| {OIDC:scope} |Parametro di stringa di query `scope`. | openid |

### <a name="context"></a>Context

| Attestazione | Description | Esempio |
| ----- | ----------- | --------|
| {Context:BuildNumber} | Versione del framework dell'esperienza di gestione delle identità (numero di build).  | 1.0.507.0 |
| {Context:CorrelationId} | L'ID di correlazione.  | 00000000-0000-0000-0000-000000000000 |
| {Context:DateTimeInUtc} |Data e ora in formato UTC.  | 10/10/2018 12:00:00 PM |
| {Context:DeploymentMode} |Modalità di distribuzione dei criteri.  | Produzione |
| {Context:IPAddress} | Indirizzo IP utente. | 11.111.111.11 |


### <a name="non-protocol-parameters"></a>Parametri non di protocollo

I nomi di parametro inclusi in una richiesta OIDC o OAuth2 possono essere mappati a un'attestazione nel percorso utente Ad esempio, la richiesta generata dall'applicazione può includere un parametro di stringa di query denominato `app_session`, `loyalty_number` o qualsiasi stringa di query personalizzata.

| Attestazione | Description | Esempio |
| ----- | ----------------------- | --------|
| {OAUTH-KV:campaignId} | Parametro di stringa di query. | hawaii |
| {OAUTH-KV:app_session} | Parametro di stringa di query. | A3C5R |
| {OAUTH-KV:loyalty_number} | Parametro di stringa di query. | 1234 |
| {OAUTH-KV:qualsiasi stringa di query personalizzata} | Parametro di stringa di query. | N/D |

### <a name="oauth2"></a>OAuth2

| Attestazione | Description | Esempio |
| ----- | ----------------------- | --------|
| {oauth2:access_token} | Token di accesso. | N/D |

## <a name="how-to-use-claim-resolvers"></a>Come usare i resolver di attestazioni

### <a name="restful-technical-profile"></a>Profilo tecnico RESTful

In un profilo tecnico [RESTful](restful-technical-profile.md) può essere utile inviare la lingua dell'utente, il nome dei criteri, l'ambito e l'ID client. Sulla base di queste attestazioni, l'API REST può eseguire logica di business personalizzata e, se necessario, generare un messaggio di errore localizzato.

Nell'esempio seguente viene illustrato un profilo tecnico RESTful:

```XML
<TechnicalProfile Id="REST">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" />
    <InputClaim ClaimTypeReferenceId="policyName" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="{OIDC:scope}" />
    <InputClaim ClaimTypeReferenceId="clientId" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="direct-sign-in"></a>Accesso diretto

Con i resolver di attestazioni è possibile precompilare il nome di accesso o l'accesso diretto a un provider di identità di social networking specifico, ad esempio Facebook, LinkedIn o un account Microsoft. Per altre informazioni, vedere [Configurare l'accesso diretto tramite Active Directory B2C](direct-signin.md).

### <a name="dynamic-ui-customization"></a>Personalizzazione dell'interfaccia utente dinamica

Azure AD B2C consente di passare parametri della stringa di query agli endpoint della definizione del contenuto HTML, in modo da poter eseguire il rendering dinamico del contenuto della pagina. È ad esempio possibile modificare l'immagine di sfondo nella pagina di accesso o di iscrizione ad Azure AD B2C in base a un parametro personalizzato passato dall'applicazione Web o per dispositivi mobili. Per altre informazioni, vedere [Azure Active Directory B2C: Configurare l'interfaccia utente con contenuto dinamico usando criteri personalizzati](custom-policy-ui-customization-dynamic.md). È anche possibile localizzare la pagina HTML in base a un parametro di lingua oppure è possibile modificare il contenuto in base all'ID client.

L'esempio seguente passa nella stringa di query un parametro denominato **campaignId** con il valore `hawaii`, un codice **language**`en-US` e **app** che rappresenta l'ID client:

```XML
<UserJourneyBehaviors>
  <ContentDefinitionParameters>
    <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
    <Parameter Name="language">{Culture:RFC5646}</Parameter>
    <Parameter Name="app">{OIDC:ClientId}</Parameter>
  </ContentDefinitionParameters>
</UserJourneyBehaviors>
```

Azure AD B2C invia quindi i parametri di cui sopra alla pagina di contenuto HTML:

```
/selfAsserted.aspx?campaignId=hawaii&language=en-US&app=0239a9cc-309c-4d41-87f1-31288feb2e82
```

### <a name="application-insights-technical-profile"></a>Profilo tecnico di Application Insights

Con Azure Application Insights e i resolver di attestazioni è possibile ottenere informazioni dettagliate sul comportamento degli utenti. Nel profilo tecnico di Application Insights, si inviano le attestazioni di input che vengono salvate in Azure Application Insights. Per altre informazioni, vedere [Tenere traccia del comportamento degli utenti nei percorsi di Azure AD B2C usando Application Insights](analytics-with-application-insights.md). L'esempio seguente invia l'ID dei criteri, l'ID di correlazione, la lingua e l'ID client ad Azure Application Insights.

```XML
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Alternate Email</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
    <InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
    <InputClaim ClaimTypeReferenceId="AppId" PartnerClaimType="{property:App}" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
</TechnicalProfile>
```
