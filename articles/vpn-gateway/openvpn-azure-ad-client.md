---
title: 'Gateway VPN: client VPN per le connessioni P2S del protocollo OpenVPN: autenticazione Azure AD'
description: È possibile usare la VPN P2S per connettersi alla VNet usando l'autenticazione Azure AD
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: alzam
ms.openlocfilehash: 3559a139ff89c949ee691310ae25af7d6950abdf
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2020
ms.locfileid: "79138961"
---
# <a name="configure-a-vpn-client-for-p2s-openvpn-protocol-connections-azure-ad-authentication"></a>Configurare un client VPN per le connessioni del protocollo OpenVPN P2S: autenticazione Azure AD

Questo articolo illustra come configurare un client VPN per connettersi a una rete virtuale usando la VPN da punto a sito e l'autenticazione Azure Active Directory. Prima di poter connettersi ed eseguire l'autenticazione con Azure AD, è necessario innanzitutto configurare il tenant di Azure AD. Per altre informazioni, vedere [configurare un tenant di Azure ad](openvpn-azure-ad-tenant.md).

> [!NOTE]
> L'autenticazione di Azure AD è supportata solo per le connessioni tramite il protocollo OpenVPN®.
>

## <a name="profile"></a>Utilizzo dei profili client

Per connettersi, è necessario scaricare il client VPN di Azure e configurare un profilo client VPN in ogni computer che desidera connettersi a VNet. È possibile creare un profilo client in un computer, esportarlo e quindi importarlo in altri computer.

### <a name="to-download-the-azure-vpn-client"></a>Per scaricare il client VPN di Azure

Usare questo [collegamento](https://go.microsoft.com/fwlink/?linkid=2117554) per scaricare il client VPN di Azure.

### <a name="cert"></a>Per creare un profilo client basato su certificati

Quando si utilizza un profilo basato su certificato, verificare che nel computer client siano installati i certificati appropriati. Per ulteriori informazioni sui certificati, vedere [Install Client Certificates](point-to-site-how-to-vpn-client-install-azure-cert.md).

  ![cert](./media/openvpn-azure-ad-client/create/create-cert1.jpg)

### <a name="radius"></a>Per creare un profilo client RADIUS

  ![radius](./media/openvpn-azure-ad-client/create/create-radius1.jpg)
  
> [!NOTE]
> Il segreto server può essere esportato nel profilo client VPN P2S.  Le istruzioni su come esportare un profilo client sono disponibili [qui](about-vpn-profile-download.md).
>

### <a name="export"></a>Per esportare e distribuire un profilo client

Quando si dispone di un profilo di lavoro ed è necessario distribuirlo ad altri utenti, è possibile esportarlo attenendosi alla procedura seguente:

1. Evidenziare il profilo client VPN che si vuole esportare, selezionare il **...** e quindi selezionare **Esporta**.

    ![esportare](./media/openvpn-azure-ad-client/export/export1.jpg)

2. Selezionare il percorso in cui si desidera salvare il profilo, lasciare il nome del file così com'è, quindi selezionare **Salva** per salvare il file XML.

    ![esportare](./media/openvpn-azure-ad-client/export/export2.jpg)

### <a name="import"></a>Per importare un profilo client

1. Nella pagina selezionare **Importa**.

    ![import](./media/openvpn-azure-ad-client/import/import1.jpg)

2. Individuare il file XML del profilo e selezionarlo. Con il file selezionato, selezionare **Apri**.

    ![import](./media/openvpn-azure-ad-client/import/import2.jpg)

3. Specificare il nome del profilo e selezionare **Salva**.

    ![import](./media/openvpn-azure-ad-client/import/import3.jpg)

4. Selezionare **Connetti** per connettersi alla VPN.

    ![import](./media/openvpn-azure-ad-client/import/import4.jpg)

5. Una volta stabilita la connessione, l'icona diventerà verde mostrerà lo stato **Connesso**.

    ![import](./media/openvpn-azure-ad-client/import/import5.jpg)

### <a name="delete"></a>Per eliminare un profilo client

1. Selezionare i puntini di sospensione accanto al profilo client che si desidera eliminare. Selezionare quindi **Rimuovi**.

    ![delete](./media/openvpn-azure-ad-client/delete/delete1.jpg)

2. Per procedere all'eliminazione, selezionare **Rimuovi**.

    ![delete](./media/openvpn-azure-ad-client/delete/delete2.jpg)

## <a name="connection"></a>Creare una connessione

1. Nella pagina selezionare **+** , quindi **+ Aggiungi**.

    ![connessione](./media/openvpn-azure-ad-client/create/create1.jpg)

2. Inserire le informazioni di connessione. Se non si è certi dei valori, contattare l'amministratore. Dopo aver compilato i valori, selezionare **Salva**.

    ![connessione](./media/openvpn-azure-ad-client/create/create2.jpg)

3. Selezionare **Connetti** per connettersi alla VPN.

    ![connessione](./media/openvpn-azure-ad-client/create/create3.jpg)

4. Selezionare le credenziali appropriate e quindi fare clic su **continua**.

    ![connessione](./media/openvpn-azure-ad-client/create/create4.jpg)

5. Una volta stabilita la connessione, l'icona diventerà verde e si **disconnetterà**.

    ![connessione](./media/openvpn-azure-ad-client/create/create5.jpg)

### <a name="autoconnect"></a>Per connettersi automaticamente

Questi passaggi consentono di configurare la connessione per la connessione automatica con always-on.

1. Nella home page per il client VPN selezionare **Impostazioni VPN**.

    ![auto](./media/openvpn-azure-ad-client/auto/auto1.jpg)

2. Selezionare **Sì** nella finestra di dialogo Cambia app.

    ![auto](./media/openvpn-azure-ad-client/auto/auto2.jpg)

3. Verificare che la connessione che si vuole impostare non sia già connessa, quindi evidenziare il profilo e selezionare la casella di controllo **Connetti automaticamente** .

    ![auto](./media/openvpn-azure-ad-client/auto/auto3.jpg)

4. Selezionare **Connetti** per avviare la connessione VPN.

    ![auto](./media/openvpn-azure-ad-client/auto/auto4.jpg)

## <a name="diagnose"></a>Diagnosticare i problemi di connessione

1. Per diagnosticare i problemi di connessione, è possibile usare lo strumento **Diagnosi**. Selezionare il **...** accanto alla connessione VPN che si desidera diagnosticare per visualizzare il menu. Selezionare quindi **Diagnosi**.

    ![diagnosi](./media/openvpn-azure-ad-client/diagnose/diagnose1.jpg)

2. Nella pagina **Proprietà connessione** selezionare **Esegui diagnosi**.

    ![diagnosi](./media/openvpn-azure-ad-client/diagnose/diagnose2.jpg)

3. Accedere con le credenziali personali.

    ![diagnosi](./media/openvpn-azure-ad-client/diagnose/diagnose3.jpg)

4. Visualizzare i risultati della diagnosi.

    ![diagnosi](./media/openvpn-azure-ad-client/diagnose/diagnose4.jpg)

## <a name="faq"></a>Domande frequenti

### <a name="how-do-i-add-dns-suffixes-to-the-vpn-client"></a>Ricerca per categorie aggiungere suffissi DNS al client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere il **\<dnssuffixes >\<dnssufix > \</dnssufix >\</dnssuffixes** > Tag

```
<azvpnprofile>
<clientconfig>

    <dnssuffixes>
          <dnssuffix>.mycorp.com</dnssuffix>
          <dnssuffix>.xyz.com</dnssuffix>
          <dnssuffix>.etc.net</dnssuffix>
    </dnssuffixes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-add-custom-dns-servers-to-the-vpn-client"></a>Ricerca per categorie aggiungere server DNS personalizzati al client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere il **\<dnsservers >\<dnsserver > \</dnsserver >\</dnsservers** > Tag

```
<azvpnprofile>
<clientconfig>

    <dnsservers>
        <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
    </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

> [!NOTE]
> Il client Azure AD OpenVPN usa le voci della tabella dei criteri di risoluzione dei nomi DNS, che significa che i server DNS non saranno elencati nell'output di `ipconfig /all`. Per confermare le impostazioni DNS in uso, vedere [Get-DnsClientNrptPolicy](https://docs.microsoft.com/powershell/module/dnsclient/get-dnsclientnrptpolicy?view=win10-ps) in PowerShell.
>

### <a name="how-do-i-add-custom-routes-to-the-vpn-client"></a>Ricerca per categorie aggiungere route personalizzate al client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere il **\<includeroutes >\<route >\<destinazione >\<mask > \</destination >\</Mask >\</route >\</includeroutes** > Tag

```
<azvpnprofile>
<clientconfig>

    <includeroutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </includeroutes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-block-exclude-routes-from-the-vpn-client"></a>Ricerca per categorie le route di blocco (escluse) dal client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere il **\<excluderoutes >\<route >\<destinazione >\<mask > \</destination >\</Mask >\</route >\</excluderoutes** > Tag

```
<azvpnprofile>
<clientconfig>

    <excluderoutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </excluderoutes>
    
</clientconfig>
</azvpnprofile>
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [creare un tenant di Azure Active Directory per le connessioni VPN aperte P2S che usano l'autenticazione Azure ad](openvpn-azure-ad-tenant.md).
