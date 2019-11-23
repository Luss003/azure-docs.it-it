---
title: Troubleshooting hybrid Azure Active Directory joined devices
description: Risoluzione dei problemi relativi a dispositivi Windows 10 e Windows Server 2016 aggiunti all'identità ibrida di Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 932540c830940ec18c439352d54f671db7387b94
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74379154"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-devices"></a>Troubleshooting hybrid Azure Active Directory joined devices 

The content of this article is applicable to devices running Windows 10 or Windows Server 2016.

For other Windows clients, see the article [Troubleshooting hybrid Azure Active Directory joined down-level devices](troubleshoot-hybrid-join-windows-legacy.md).

Questo articolo presuppone che siano stati [configurati dispositivi aggiunti all'identità ibrida di Azure Active Directory](hybrid-azuread-join-plan.md) per supportare gli scenari seguenti:

- Device-based Conditional Access
- [Roaming aziendale delle impostazioni](../active-directory-windows-enterprise-state-roaming-overview.md)
- [Windows Hello for Business](../active-directory-azureadjoin-passport-deployment.md) (Configurare Windows Hello for Business)

This document provides troubleshooting guidance to resolve potential issues. 

Per Windows 10 e Windows Server 2016, l'aggiunta all'identità ibrida di Azure Active Directory supporta l'aggiornamento di Windows del 10 novembre 2015 e versioni successive.

## <a name="troubleshoot-join-failures"></a>Troubleshoot join failures

### <a name="step-1-retrieve-the-join-status"></a>Passaggio 1: Recuperare lo stato delle aggiunte 

**Per recuperare lo stato delle aggiunte:**

1. Open a command prompt as an administrator
2. Digitare `dsregcmd /status`.

```
+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+

    AzureAdJoined: YES
 EnterpriseJoined: NO
         DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7
       Thumbprint: B753A6679CE720451921302CA873794D94C6204A
   KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9
      KeyProvider: Microsoft Platform Crypto Provider
     TpmProtected: YES
     KeySignTest: : MUST Run elevated to test.
              Idp: login.windows.net
         TenantId: 72b988bf-xxxx-xxxx-xxxx-2d7cd011xxxx
       TenantName: Contoso
      AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize
   AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token
           MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc
        MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx
  dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance
      SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ==
   JoinSrvVersion: 1.0
       JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/
        JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net
    KeySrvVersion: 1.0
        KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/
         KeySrvId: urn:ms-drs:enterpriseregistration.windows.net
     DomainJoined: YES
       DomainName: CONTOSO

+----------------------------------------------------------------------+
| User State                                                           |
+----------------------------------------------------------------------+

             NgcSet: YES
           NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
    WorkplaceJoined: NO
      WamDefaultSet: YES
WamDefaultAuthority: organizations
       WamDefaultId: https://login.microsoft.com
     WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)
         AzureAdPrt: YES
```

### <a name="step-2-evaluate-the-join-status"></a>Passaggio 2: Valutare lo stato delle aggiunte 

Esaminare i campi seguenti e assicurarsi che siano presenti i valori previsti:

#### <a name="domainjoined--yes"></a>DomainJoined: YES  

Questo campo mostra se il dispositivo è aggiunto a un dominio Active Directory locale. Se il valore è **NO**, il dispositivo non riesce a eseguire un'aggiunta all'identità ibrida di Azure AD.  

#### <a name="workplacejoined--no"></a>WorkplaceJoined: NO  

Questo campo mostra se il dispositivo è registrato con Azure AD come dispositivo personale, contrassegnato come *aggiunto correttamente* all'area di lavoro. Questo valore deve essere **NO** per un computer aggiunto al dominio che è anche aggiunto all'identità ibrida di Azure AD. Se il valore è **YES**, è stato aggiunto un account aziendale o dell'istituto di istruzione prima del completamento dell'aggiunta all'identità ibrida di Azure AD. In questo caso l'account viene ignorato quando si usa la versione anniversario dell'aggiornamento di Windows 10 (1607).

#### <a name="azureadjoined--yes"></a>AzureAdJoined: YES  

Questo campo mostra se il dispositivo è aggiunto ad Azure AD. Se il valore è **NO**, l'aggiunta ad Azure AD non è ancora completata. 

Proceed to next steps for further troubleshooting.

### <a name="step-3-find-the-phase-in-which-join-failed-and-the-errorcode"></a>Step 3: Find the phase in which join failed and the errorcode

#### <a name="windows-10-1803-and-above"></a>Windows 10 1803 and above

Look for 'Previous Registration' subsection in the 'Diagnostic Data' section of the join status output. This section is displayed only if the device is domain joined and is unable to hybrid Azure AD join.
The 'Error Phase' field denotes the phase of the join failure while 'Client ErrorCode' denotes the error code of the Join operation.

```
+----------------------------------------------------------------------+
     Previous Registration : 2019-01-31 09:16:43.000 UTC
         Registration Type : sync
               Error Phase : join
          Client ErrorCode : 0x801c03f2
          Server ErrorCode : DirectoryError
            Server Message : The device object by the given id (e92325d0-xxxx-xxxx-xxxx-94ae875d5245) is not found.
              Https Status : 400
                Request Id : 6bff0bd9-820b-484b-ab20-2a4f7b76c58e
+----------------------------------------------------------------------+
```

#### <a name="older-windows-10-versions"></a>Older Windows 10 versions

Use Event Viewer logs to locate the phase and error code for the join failures.

1. Open the **User Device Registration** event logs in event viewer. Located under **Applications and Services Log** > **Microsoft** > **Windows** > **User Device Registration**
2. Look for events with the following eventIDs 304, 305, 307.

![Failure Log Event](./media/troubleshoot-hybrid-join-windows-current/1.png)

![Failure Log Event](./media/troubleshoot-hybrid-join-windows-current/2.png)

### <a name="step-4-check-for-possible-causes-and-resolutions-from-the-lists-below"></a>Step 4: Check for possible causes and resolutions from the lists below

#### <a name="pre-check-phase"></a>Pre-check phase

Possible reasons for failure:

- Device has no line of sight to the Domain controller.
   - The device must be on the organization’s internal network or on VPN with network line of sight to an on-premises Active Directory (AD) domain controller.

#### <a name="discover-phase"></a>Discover phase

Possible reasons for failure:

- Service Connection Point (SCP) object misconfigured/unable to read SCP object from DC.
   - A valid SCP object is required in the AD forest, to which the device belongs, that points to a verified domain name in Azure AD.
   - Details can be found in the section [Configure a Service Connection Point](hybrid-azuread-join-federated-domains.md#configure-hybrid-azure-ad-join).
- Failure to connect and fetch the discovery metadata from the discovery endpoint.
   - The device should be able to access `https://enterpriseregistration.windows.net`, in the SYSTEM context, to discover the registration and authorization endpoints. 
   - If the on-premises environment requires an outbound proxy, the IT admin must ensure that the computer account of the device is able to discover and silently authenticate to the outbound proxy.
- Failure to connect to user realm endpoint and perform realm discovery. (Windows 10 version 1809 and later only)
   - The device should be able to access `https://login.microsoftonline.com`, in the SYSTEM context, to perform realm discovery for the verified domain and determine the domain type (managed/federated).
   - If the on-premises environment requires an outbound proxy, the IT admin must ensure that the SYSTEM context on the device is able to discover and silently authenticate to the outbound proxy.

**Common error codes:**

- **DSREG_AUTOJOIN_ADCONFIG_READ_FAILED** (0x801c001d/-2145648611)
   - Reason: Unable to read the SCP object and get the Azure AD tenant information.
   - Resolution: Refer to the section [Configure a Service Connection Point](hybrid-azuread-join-federated-domains.md#configure-hybrid-azure-ad-join).
- **DSREG_AUTOJOIN_DISC_FAILED** (0x801c0021/-2145648607)
   - Reason: Generic Discovery failure. Failed to get the discovery metadata from DRS.
   - Resolution: Find the suberror below to investigate further.
- **DSREG_AUTOJOIN_DISC_WAIT_TIMEOUT**  (0x801c001f/-2145648609)
   - Reason: Operation timed out while performing Discovery.
   - Resolution: Ensure that `https://enterpriseregistration.windows.net` is accessible in the SYSTEM context. For more information, see the section [Network connectivity requirements](hybrid-azuread-join-managed-domains.md#prerequisites).
- **DSREG_AUTOJOIN_USERREALM_DISCOVERY_FAILED** (0x801c0021/-2145648611)
   - Reason: Generic Realm Discovery failure. Failed to determine domain type (managed/federated) from STS. 
   - Resolution: Find the suberror below to investigate further.

**Common suberror codes:**

To find the suberror code for the discovery error code, use one of the following methods.

##### <a name="windows-10-1803-and-above"></a>Windows 10 1803 and above

Look for 'DRS Discovery Test' in the 'Diagnostic Data' section of the join status output. This section is displayed only if the device is domain joined and is unable to hybrid Azure AD join.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : UN-ELEVATED User
               Client Time : 2019-06-05 08:25:29.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : FAIL [0x801c0021/0x80072ee2]
     DRS Connectivity Test : SKIPPED
    Token acquisition Test : SKIPPED
     Fallback to Sync-Join : ENABLED

+----------------------------------------------------------------------+
```

##### <a name="older-windows-10-versions"></a>Older Windows 10 versions

Use Event Viewer logs to locate the phase and errorcode for the join failures.

1. Open the **User Device Registration** event logs in event viewer. Located under **Applications and Services Log** > **Microsoft** > **Windows** > **User Device Registration**
2. Look for events with the following eventIDs 201

![Failure Log Event](./media/troubleshoot-hybrid-join-windows-current/5.png)

###### <a name="network-errors"></a>Network errors

- **WININET_E_CANNOT_CONNECT** (0x80072efd/-2147012867)
   - Reason: Connection with the server could not be established
   - Resolution: Ensure network connectivity to the required Microsoft resources. For more information, see [Network connectivity requirements](hybrid-azuread-join-managed-domains.md#prerequisites).
- **WININET_E_TIMEOUT** (0x80072ee2/-2147012894)
   - Reason: General network timeout.
   - Resolution: Ensure network connectivity to the required Microsoft resources. For more information, see [Network connectivity requirements](hybrid-azuread-join-managed-domains.md#prerequisites).
- **WININET_E_DECODING_FAILED** (0x80072f8f/-2147012721)
   - Reason: Network stack was unable to decode the response from the server.
   - Resolution: Ensure that network proxy is not interfering and modifying the server response.

###### <a name="http-errors"></a>HTTP errors

- **DSREG_DISCOVERY_TENANT_NOT_FOUND** (0x801c003a/-2145648582)
   - Reason: SCP object configured with wrong tenant ID. Or no active subscriptions were found in the tenant.
   - Resolution: Ensure SCP object is configured with the correct Azure AD tenant ID and active subscriptions or present in the tenant.
- **DSREG_SERVER_BUSY** (0x801c0025/-2145648603)
   - Reason: HTTP 503 from DRS server.
   - Resolution: Server is currently unavailable. future join attempts will likely succeed once server is back online.

###### <a name="other-errors"></a>Other errors

- **E_INVALIDDATA** (0x8007000d/-2147024883)
   - Reason: Server response JSON couldn't be parsed. Likely due to proxy returning HTTP 200 with an HTML auth page.
   - Resolution: If the on-premises environment requires an outbound proxy, the IT admin must ensure that the SYSTEM context on the device is able to discover and silently authenticate to the outbound proxy.

#### <a name="authentication-phase"></a>Authentication phase

Applicable only for federated domain accounts.

Reasons for failure:

- Unable to get an Access token silently for DRS resource.
   - Windows 10 devices acquire auth token from the federation service using Integrated Windows Authentication to an active WS-Trust endpoint. Details: [Federation Service Configuration](hybrid-azuread-join-manual.md##set-up-issuance-of-claims)

**Common error codes:**

Use Event Viewer logs to locate the error code, suberror code, server error code, and server error message.

1. Open the **User Device Registration** event logs in event viewer. Located under **Applications and Services Log** > **Microsoft** > **Windows** > **User Device Registration**
2. Look for events with the following eventID 305

![Failure Log Event](./media/troubleshoot-hybrid-join-windows-current/3.png)

##### <a name="configuration-errors"></a>Errori di configurazione

- **ERROR_ADAL_PROTOCOL_NOT_SUPPORTED** (0xcaa90017/-894894057)
   - Reason: Authentication protocol is not WS-Trust.
   - Resolution: The on-premises identity provider must support WS-Trust 
- **ERROR_ADAL_FAILED_TO_PARSE_XML** (0xcaa9002c/-894894036)
   - Reason: On-premises federation service did not return an XML response.
   - Resolution: Ensure MEX endpoint is returning a valid XML. Ensure proxy is not interfering and returning non-xml responses.
- **ERROR_ADAL_COULDNOT_DISCOVER_USERNAME_PASSWORD_ENDPOINT** (0xcaa90023/-894894045)
   - Reason: Could not discover endpoint for username/password authentication.
   - Resolution: Check the on-premises identity provider settings. Ensure that the WS-Trust endpoints are enabled and ensure the MEX response contains these correct endpoints.

##### <a name="network-errors"></a>Network errors

- **ERROR_ADAL_INTERNET_TIMEOUT** (0xcaa82ee2/-894947614)
   - Reason: General network timeout.
   - Resolution: Ensure that `https://login.microsoftonline.com` is accessible in the SYSTEM context. Ensure the on-premises identity provider is accessible in the SYSTEM context. For more information, see [Network connectivity requirements](hybrid-azuread-join-managed-domains.md#prerequisites).
- **ERROR_ADAL_INTERNET_CONNECTION_ABORTED** (0xcaa82efe/-894947586)
   - Reason: Connection with the auth endpoint was aborted.
   - Resolution: Retry after sometime or try joining from an alternate stable network location.
- **ERROR_ADAL_INTERNET_SECURE_FAILURE** (0xcaa82f8f/-894947441)
   - Reason: The Secure Sockets Layer (SSL) certificate sent by the server could not be validated.
   - Resolution: Check the client time skew. Retry after sometime or try joining from an alternate stable network location. 
- **ERROR_ADAL_INTERNET_CANNOT_CONNECT** (0xcaa82efd/-894947587)
   - Reason: The attempt to connect to `https://login.microsoftonline.com` failed.
   - Resolution: Check network connection to `https://login.microsoftonline.com`.

##### <a name="other-errors"></a>Other errors

- **ERROR_ADAL_SERVER_ERROR_INVALID_GRANT** (0xcaa20003/-895352829)
   - Reason: SAML token from the on-premises identity provider was not accepted by Azure AD.
   - Resolution: Check the federation server settings. Look for the server error code in the authentication logs.
- **ERROR_ADAL_WSTRUST_REQUEST_SECURITYTOKEN_FAILED** (0xcaa90014/-894894060)
   - Reason: Server WS-Trust response reported fault exception and it failed to get assertion
   - Resolution: Check the federation server settings. Look for the server error code in the authentication logs.
- **ERROR_ADAL_WSTRUST_TOKEN_REQUEST_FAIL** (0xcaa90006/-894894074)
   - Reason: Received an error when trying to get access token from the token endpoint.
   - Resolution: Look for the underlying error in the ADAL log. 
- **ERROR_ADAL_OPERATION_PENDING** (0xcaa1002d/-895418323)
   - Reason: General ADAL failure
   - Resolution: Look for the suberror code or server error code from the authentication logs.
    
#### <a name="join-phase"></a>Join Phase

Reasons for failure:

Find the registration type and look for the error code from the list below.

#### <a name="windows-10-1803-and-above"></a>Windows 10 1803 and above

Look for 'Previous Registration' subsection in the 'Diagnostic Data' section of the join status output. This section is displayed only if the device is domain joined and is unable to hybrid Azure AD join.
'Registration Type' field denotes the type of join performed.

```
+----------------------------------------------------------------------+
     Previous Registration : 2019-01-31 09:16:43.000 UTC
         Registration Type : sync
               Error Phase : join
          Client ErrorCode : 0x801c03f2
          Server ErrorCode : DirectoryError
            Server Message : The device object by the given id (e92325d0-7ac4-4714-88a1-94ae875d5245) is not found.
              Https Status : 400
                Request Id : 6bff0bd9-820b-484b-ab20-2a4f7b76c58e
+----------------------------------------------------------------------+
```

#### <a name="older-windows-10-versions"></a>Older Windows 10 versions

Use Event Viewer logs to locate the phase and errorcode for the join failures.

1. Open the **User Device Registration** event logs in event viewer. Located under **Applications and Services Log** > **Microsoft** > **Windows** > **User Device Registration**
2. Look for events with the following eventIDs 204

![Failure Log Event](./media/troubleshoot-hybrid-join-windows-current/4.png)

##### <a name="http-errors-returned-from-drs-server"></a>HTTP errors returned from DRS server

- **DSREG_E_DIRECTORY_FAILURE** (0x801c03f2/-2145647630)
   - Reason: Received an error response from DRS with ErrorCode: "DirectoryError"
   - Resolution: Refer to the server error code for possible reasons and resolutions.
- **DSREG_E_DEVICE_AUTHENTICATION_ERROR** (0x801c0002/-2145648638)
   - Reason: Received an error response from DRS with ErrorCode: "AuthenticationError" and ErrorSubCode is NOT "DeviceNotFound". 
   - Resolution: Refer to the server error code for possible reasons and resolutions.
- **DSREG_E_DEVICE_INTERNALSERVICE_ERROR** (0x801c0006/-2145648634)
   - Reason: Received an error response from DRS with ErrorCode: "DirectoryError"
   - Resolution: Refer to the server error code for possible reasons and resolutions.

##### <a name="tpm-errors"></a>TPM errors

- **NTE_BAD_KEYSET** (0x80090016/-2146893802)
   - Reason: TPM operation failed or was invalid
   - Resolution: Likely due to a bad sysprep image. Ensure the machine from which the sysprep image was created is not Azure AD joined, hybrid Azure AD joined, or Azure AD registered.
- **TPM_E_PCP_INTERNAL_ERROR** (0x80290407/-2144795641)
   - Reason: Generic TPM error. 
   - Resolution: Disable TPM on devices with this error. Windows 10 version 1809 and higher automatically detects TPM failures and completes hybrid Azure AD join without using the TPM.
- **TPM_E_NOTFIPS** (0x80280036/-2144862154)
   - Reason: TPM in FIPS mode not currently supported.
   - Resolution: Disable TPM on devices with this error. Windows 1809 automatically detects TPM failures and completes hybrid Azure AD join without using the TPM.
- **NTE_AUTHENTICATION_IGNORED** (0x80090031/-2146893775)
   - Reason: TPM locked out.
   - Resolution: Transient error. Wait for the cooldown period. Join attempt after some time should succeed. More Information can be found in the article [TPM fundamentals](https://docs.microsoft.com/windows/security/information-protection/tpm/tpm-fundamentals#anti-hammering)

##### <a name="network-errors"></a>Network Errors

- **WININET_E_TIMEOUT** (0x80072ee2/-2147012894)
   - Reason: General network time out trying to register the device at DRS
   - Resolution: Check network connectivity to `https://enterpriseregistration.windows.net`.
- **WININET_E_NAME_NOT_RESOLVED** (0x80072ee7/-2147012889)
   - Reason: The server name or address could not be resolved.
   - Resolution: Check network connectivity to `https://enterpriseregistration.windows.net`. Ensure DNS resolution for the hostname is accurate in the n/w and on the device.
- **WININET_E_CONNECTION_ABORTED** (0x80072efe/-2147012866)
   - Reason: The connection with the server was terminated abnormally.
   - Resolution: Retry after sometime or try joining from an alternate stable network location.

##### <a name="federated-join-server-errors"></a>Federated join server Errors

| Server error code | Server error message | Possible reasons | Risoluzione |
| --- | --- | --- | --- |
| DirectoryError | Your request is throttled temporarily. Please try after 300 seconds. | Expected error. Possibly due to making multiple registration requests in quick succession. | Retry join after the cooldown period |

##### <a name="sync-join-server-errors"></a>Sync join server Errors

| Server error code | Server error message | Possible reasons | Risoluzione |
| --- | --- | --- | --- |
| DirectoryError | AADSTS90002: Tenant <UUID> not found. This error may happen if there are no active subscriptions for the tenant. Check with your subscription administrator. | Tenant ID in SCP object is incorrect | Ensure SCP object is configured with the correct Azure AD tenant ID and active subscriptions and present in the tenant. |
| DirectoryError | The device object by the given ID is not found. | Expected error for sync join. The device object has not synced from AD to Azure AD | Wait for the Azure AD Connect sync to complete and the next join attempt after sync completion will resolve the issue |
| AuthenticationError | The verification of the target computer's SID | The certificate on the Azure AD device doesn't match the certificate used to sign the blob during the sync join. This error typically means sync hasn’t completed yet. |  Wait for the Azure AD Connect sync to complete and the next join attempt after sync completion will resolve the issue |

### <a name="step-5-collect-logs-and-contact-microsoft-support"></a>Step 5: Collect logs and contact Microsoft Support

Get public scripts here: [https://1drv.ms/u/s!AkyTjQ17vtfagYkZ6VJzPg78e3o7PQ]( https://1drv.ms/u/s!AkyTjQ17vtfagYkZ6VJzPg78e3o7PQ)

1. Open an admin command prompt and run `start_ngc_tracing_public.cmd`.
2. Perform the steps to reproduce the issue.
3. Stop running the logging script by executing `stop_ngc_tracing_public.cmd`.
4. Zip and send the logs under `%SYSTEMDRIVE%\TraceDJPP\*` for analysis.

## <a name="troubleshoot-post-join-issues"></a>Troubleshoot Post-Join issues

### <a name="retrieve-the-join-status"></a>Recuperare lo stato delle aggiunte 

#### <a name="wamdefaultset-yes-and-azureadprt-yes"></a>WamDefaultSet: YES and AzureADPrt: YES
  
Questi campi indicano se l'utente è autenticato correttamente in Azure AD durante l'accesso al dispositivo. Se i valori sono **NO**, il motivo potrebbe essere:

- Bad storage key in the TPM associated with the device upon registration (check the KeySignTest while running elevated).
- ID di accesso alternativo
- Proxy HTTP non trovato

## <a name="known-issues"></a>Problemi noti
- Under Settings -> Accounts -> Access Work or School, Hybrid Azure AD joined devices may show two different accounts, one for Azure AD and one for on-premises AD, when connected to mobile hotspots or external WiFi networks. This is only a UI issue and does not have any impact on functionality. 
 
## <a name="next-steps"></a>Passaggi successivi

Continue [troubleshooting devices using the dsregcmd command](troubleshoot-device-dsregcmd.md)

Per altre informazioni, vedere le [domande frequenti sulla gestione dei dispositivi](faq.md)
