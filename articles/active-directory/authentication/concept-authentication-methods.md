---
title: Metodi di autenticazione-Azure Active Directory
description: Metodi di autenticazione disponibili in Azure AD per multi-factor authentication e SSPR
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b0c91357e5ab15b88c92b04fd0896b989e83953
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051439"
---
# <a name="what-are-authentication-methods"></a>Cosa si intende per metodi di autenticazione?

Gli amministratori che scelgono i metodi di autenticazione per Azure Multi-Factor Authentication e la reimpostazione della password self-service (SSPR) si consiglia di richiedere agli utenti di registrare più metodi di autenticazione. Quando un metodo di autenticazione non è disponibile per un utente, può scegliere di eseguire l'autenticazione con un altro metodo.

Nei criteri, gli amministratori possono definire quali metodi di autenticazione sono disponibili agli utenti di SSPR e autenticazione a più fattori. Alcuni metodi di autenticazione potrebbero non essere disponibili per tutte le funzionalità. Per ulteriori informazioni sulla configurazione dei criteri, vedere gli articoli [come implementare correttamente la reimpostazione della password self-service](howto-sspr-deployment.md) e la [pianificazione di un multi-factor authentication Azure basato sul cloud](howto-mfa-getstarted.md)

Si consiglia agli amministratori di consentire agli utenti la selezione di un numero maggiore di metodi di autenticazione rispetto al limite minimo nel caso in cui non abbiano accesso a uno.

|Metodo di autenticazione|Utilizzo|
| --- | --- |
| Password | Autenticazione a più fattori e SSPR |
| Domande di sicurezza | Solo SSPR |
| Indirizzo di posta elettronica | Solo SSPR |
| App Microsoft Authenticator | Autenticazione a più fattori e SSPR |
| Token hardware OATH | Anteprima pubblica per autenticazione a più fattori e SSPR |
| sms | Autenticazione a più fattori e SSPR |
| Chiamata vocale | Autenticazione a più fattori e SSPR |
| Password dell'app | Autenticazione a più fattori solo in alcuni casi |

![Metodi di autenticazione in uso nella schermata di accesso](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| I token hardware per multi-factor authentication e SSPR sono funzionalità di anteprima pubblica di Azure Active Directory. Per altre informazioni sulle funzioni in anteprima, vedere [Condizioni per l'utilizzo supplementari per le anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="password"></a>Password

La password di Azure AD viene considerata un metodo di autenticazione. È l'unico metodo che **non può essere disabilitato**.

## <a name="security-questions"></a>Domande di sicurezza

Le domande di sicurezza sono disponibili **solo per la reimpostazione della password di Azure AD in modalità self-service**  agli account senza privilegi di amministratore.

Se vengono usate le domande di sicurezza, è consigliabile combinare questo metodo con un altro. Le domande di sicurezza possono risultare meno sicure rispetto ad altri metodi perché alcuni utenti potrebbero conoscere le risposte alle domande di un altro utente.

> [!NOTE]
> Le domande di sicurezza vengono archiviate privatamente e in modo sicuro in un oggetto utente nella directory e gli utenti possono rispondere a tali domande solo durante la registrazione. All'amministratore non è consentito leggere o modificare le domande e le risposte di un utente.
>

### <a name="predefined-questions"></a>Domande predefinite

* In quale città hai incontrato tuo marito o il tuo partner?
* In quale città si sono incontrati i tuoi genitori?
* In quale città vive tuo fratello/sorella più vicino/a?
* In quale città è nato tuo padre?
* In quale città hai avuto il tuo primo lavoro?
* In quale città è nata tua madre?
* In quale città hai trascorso il Capodanno del 2000?
* Qual è il cognome del tuo insegnante preferito delle scuole superiori?
* Qual è il nome di una delle università a cui hai inviato una richiesta di iscrizione ma che non hai frequentato?
* In quale luogo si è tenuto il tuo primo ricevimento di matrimonio?
* Qual è il secondo nome di tuo padre?
* Qual è il tuo piatto preferito?
* Qual è il nome e il cognome della nonna materna?
* Qual è il secondo nome di tua madre?
* Qual è l'anno e il mese di nascita di tuo/a fratello/sorella maggiore? Ad esempio, novembre 1985
* Qual è il secondo nome di tuo/a fratello/sorella maggiore?
* Qual è il nome e il cognome del nonno paterno?
* Qual è il secondo nome di tuo/a fratello/sorella minore?
* Quale scuola hai frequentato in prima media?
* Qual è il nome e il cognome del tuo/a migliore amico/a di infanzia?
* Qual è il nome e il cognome del primo/a fidanzato/a?
* Qual è il nome del tuo insegnante preferito delle scuole elementari?
* Qual è la marca e il modello della tua prima auto o moto?
* Qual è il nome della prima scuola che hai frequentato?
* Qual è il nome dell'ospedale in cui sei nato?
* Qual è il nome della via della tua prima casa di infanzia?
* Qual è il nome del tuo supereroe preferito dell'infanzia?
* Qual è il nome del tuo animale di peluche preferito?
* Qual è il nome del primo animale domestico?
* Qual era il tuo soprannome da bambino?
* Qual è il tuo sport preferito alle scuole superiori?
* Qual è stato il tuo primo lavoro?
* Quali erano le ultime quattro cifre del tuo numero di telefono quando eri bambino/a?
* Cosa volevi fare da grande quando eri piccolo?
* Qual è la persona più famosa che hai mai incontrato?

Tutte le domande di sicurezza predefinite sono tradotte e localizzate nel set completo delle lingue di Office 365 in base alle impostazioni locali del browser dell'utente.

### <a name="custom-security-questions"></a>Domande di sicurezza personalizzate

Le domande di sicurezza personalizzate non sono localizzate. Tutte le domande personalizzate vengono visualizzate nella lingua in cui vengono immesse nell'interfaccia utente di amministrazione, anche se le impostazioni locali del browser dell'utente sono diverse. Se si desidera disporre domande localizzate, è consigliabile usare le domande predefinite.

La lunghezza massima di una domanda di sicurezza personalizzata è di 200 caratteri.

### <a name="security-question-requirements"></a>Requisiti della domanda di sicurezza

* Il limite minimo di caratteri della risposta è 3 caratteri.
* Il limite massimo di caratteri della risposta è 40 caratteri.
* Gli utenti non possono rispondere alla stessa domanda due volte.
* Gli utenti non possono dare la stessa risposta a due diverse domande.
* È possibile usare qualsiasi set di caratteri per definire domande e risposte, compresi i caratteri Unicode.
* Il numero di domande definite deve essere maggiore o uguale al numero di domande necessarie per la registrazione.

## <a name="email-address"></a>Indirizzo di posta elettronica

L'indirizzo di posta elettronica è disponibile **solo per la reimpostazione della password di Azure AD in modalità self-service**.

Microsoft consiglia l'uso di un account di posta elettronica che non richiede la password di Azure AD dell'utente per accedere.

## <a name="microsoft-authenticator-app"></a>App Microsoft Authenticator

L'app Microsoft Authenticator offre un livello di sicurezza aggiuntivo a un account aziendale o dell'istituto di istruzione.

L'app Microsoft Authenticator è disponibile per [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) e [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071).

> [!NOTE]
> Gli utenti non avranno la possibilità di registrare l'app per dispositivi mobili quando ci si registra per la reimpostazione della password in modalità self-service. Al contrario, gli utenti possono registrare l'app per dispositivi mobili al link [https://aka.ms/mfasetup](https://aka.ms/mfasetup) o nell'anteprima di registrazione delle informazioni di sicurezza alla pagina [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo).
>

### <a name="notification-through-mobile-app"></a>Notifica tramite l'app per dispositivi mobili

L'app Microsoft Authenticator consente di impedire l'accesso non autorizzato agli account e di arrestare le transazioni illecite eseguendo il push di una notifica allo smartphone o al tablet dell'utente. Gli utenti visualizzano la notifica e, se legittima, selezionano Verifica. In caso contrario, è possibile selezionare Nega.

> [!WARNING]
> Allo scopo di reimpostare la password in modalità self-service quando viene richiesto un solo metodo di ripristino, il codice di verifica rappresenta l'unica opzione disponibile per gli utenti **per garantire il livello di sicurezza più elevato**.
>
> Quando vengono richiesti due metodi, gli utenti sono in grado di eseguire la reimpostazione usando **O** la notifica **O** il codice di verifica, oltre a qualsiasi altro metodo abilitato.
>

Se si abilita l'uso di entrambe le notifiche per i dispositivi mobili tramite app per dispositivi mobili e codice di verifica dall'app, agli utenti che registrano l'app Microsoft Authenticator mediante una notifica sono in grado di utilizzare sia la notifica sia il codice per verificare la propria identità.

> [!NOTE]
> Se l'organizzazione dispone di personale che lavora o viaggia in Cina, la notifica tramite il metodo dell' **app mobile** nei **dispositivi Android** non funziona in quel paese. È necessario rendere disponibili metodi alternativi per tali utenti.

### <a name="verification-code-from-mobile-app"></a>Codice di verifica dall'app per dispositivi mobili

L'app Microsoft Authenticator (o altre app di terze parti) può essere usata come token software per generare un codice di verifica OATH. Dopo aver inserito il nome utente e la password, si immette il codice fornito dall'app nella schermata di accesso. Il codice di verifica offre una seconda forma di autenticazione.

> [!WARNING]
> Allo scopo di reimpostare la password in modalità self-service quando viene richiesto un solo metodo di ripristino, il codice di verifica rappresenta l'unica opzione disponibile per gli utenti **per garantire il livello più elevato di sicurezza**.
>

Gli utenti possono avere una combinazione di un massimo di cinque token hardware o applicazioni di autenticazione, ad esempio l'app Microsoft Authenticator configurata per l'uso in qualsiasi momento.

## <a name="oath-hardware-tokens-public-preview"></a>Token hardware OATH (anteprima pubblica)

OATH è uno standard aperto che specifica come vengono generati i codici per password monouso (OTP, One-Time Password). Azure AD supporterà l'uso di token OATH TOTP SHA-1 del tipo da 30 secondi o 60 secondi. I clienti possono procurarsi questi token dal fornitore preferito. Le chiavi segrete sono limitate a 128 caratteri, che potrebbero non essere compatibili con tutti i token. Le chiavi segrete devono essere codificate in Base32.

![Caricamento dei token del GIURAmento nel pannello token del GIURAmento server di autenticazione a più fattori](media/concept-authentication-methods/oath-tokens-azure-ad.png)

I token hardware OATH sono supportati come parte di un'anteprima pubblica. Per altre informazioni sulle funzioni in anteprima, vedere [Condizioni per l'utilizzo supplementari per le anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dopo aver acquisito i token, è necessario caricarli in un formato di file con valori delimitati da virgole (CSV) includendo nome dell'entità utente, numero di serie, chiave privata, intervallo di tempo, produttore e modello come illustrato nell'esempio seguente.

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567890abcdef1234567890abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Assicurarsi di includere la riga di intestazione nel file CSV come indicato in precedenza.

Dopo aver formattato correttamente il file CSV, un amministratore può quindi accedere al portale di Azure e passare ad **Azure Active Directory**, **Server MFA**, **Token OATH** e caricare il file CSV risultante.

A seconda delle dimensioni del file CSV, potrebbero essere richiesti alcuni minuti per l'elaborazione. Per ottenere lo stato corrente, fare clic sul pulsante **Aggiorna**. Se sono presenti errori nel file, si avrà la possibilità di scaricare un file CSV con gli eventuali errori per la risoluzione.

Dopo aver risolto eventuali errori, l'amministratore può quindi attivare ogni chiave facendo clic su **Attiva** per il token da attivare e immettendo la password monouso visualizzata nel token.

Gli utenti possono avere una combinazione di un massimo di cinque token hardware o applicazioni di autenticazione, ad esempio l'app Microsoft Authenticator configurata per l'uso in qualsiasi momento.

## <a name="phone-options"></a>Opzioni telefono

### <a name="mobile-phone"></a>Cellulare

Per gli utenti con telefoni cellulari esistono due opzioni.

Se gli utenti non desiderano che il numero di telefono cellulare sia visibile nella directory, ma vogliono comunque utilizzarlo per la reimpostazione della password, gli amministratori non devono inserire i relativi dati nella directory. Gli utenti devono popolare il proprio attributo **Telefono per autenticazione** tramite il [portale di registrazione per la reimpostazione della password](https://aka.ms/ssprsetup). Gli amministratori possono visualizzare queste informazioni nel profilo dell'utente, ma non vengono pubblicate altrove.

Per garantirne il corretto funzionamento, i numeri di telefono devono essere nel formato *+PrefissoInternazionale NumeroTelefonico*, ad esempio +1 4255551234.

> [!NOTE]
> È necessario uno spazio tra il prefisso internazionale e il numero di telefono.
>
> La reimpostazione della password non supporta le estensioni del telefono. Anche nel formato +1 4255551234X12345, le estensioni vengono rimosse prima della chiamata.

Microsoft non garantisce l'invio coerente di prompt Multi-Factor Authentication via SMS o vocali allo stesso numero. Nell'interesse degli utenti, Microsoft può aggiungere o rimuovere codici brevi in qualsiasi momento per eseguire modifiche di route per migliorare il recapito degli SMS. Microsoft non supporta i codici brevi per i paesi/aree geografiche oltre al Stati Uniti e al Canada.

#### <a name="text-message"></a>SMS

Viene inviato un SMS contenente un codice di verifica al numero di telefono cellulare. Immettere il codice di verifica fornito nell'interfaccia di accesso per continuare.

#### <a name="phone-call"></a>Telefonata

Viene inviata una chiamata vocale automatizzata al numero di telefono fornito. Per l'autenticazione, rispondere alla chiamata e premere # sul tastierino telefonico

> [!IMPORTANT]
> A partire da marzo 2019, le opzioni per le telefonate non saranno disponibili per gli utenti di multi-factor authentication e SSPR in un tenant gratuito/di valutazione Azure AD. Questa modifica non ha alcun effetto sui messaggi SMS. La telefonata continuerà a essere disponibile per gli utenti in tenant Azure AD a pagamento. Questa modifica ha effetto solo sui tenant di Azure AD gratuiti/di valutazione.

### <a name="office-phone"></a>Telefono ufficio

Viene inviata una chiamata vocale automatizzata al numero di telefono fornito. Per l'autenticazione, rispondere alla chiamata e premere # sul tastierino telefonico.

Per garantirne il corretto funzionamento, i numeri di telefono devono essere nel formato *+PrefissoInternazionale NumeroTelefonico*, ad esempio +1 4255551234.

L'attributo del telefono ufficio viene gestito dall'amministratore.

> [!IMPORTANT]
> A partire da marzo 2019, le opzioni per le telefonate non saranno disponibili per gli utenti di multi-factor authentication e SSPR in un tenant gratuito/di valutazione Azure AD. Questa modifica non ha alcun effetto sui messaggi SMS. La telefonata continuerà a essere disponibile per gli utenti in tenant Azure AD a pagamento. Questa modifica ha effetto solo sui tenant di Azure AD gratuiti/di valutazione.

> [!NOTE]
> È necessario uno spazio tra il prefisso internazionale e il numero di telefono.
>
> La reimpostazione della password non supporta le estensioni del telefono. Anche nel formato +1 4255551234X12345, le estensioni vengono rimosse prima della chiamata.

### <a name="troubleshooting-phone-options"></a>Risoluzione dei problemi relativi alle opzioni del telefono

Problemi comuni relativi ai metodi di autenticazione con un numero di telefono:

* ID chiamante bloccato in un singolo dispositivo
   * Risolvere i problemi del dispositivo
* Numero di telefono errato, codice paese errato, numero di telefono abitazione rispetto al numero di telefono dell'ufficio
   * Risolvere i problemi relativi a oggetti utente e metodi di autenticazione configurati Verificare che siano registrati i numeri di telefono corretti.
* PIN errato immesso
   * Confermare che l'utente ha usato il PIN corretto registrato nel server di autenticazione a più fattori di Azure.
* Chiamata da inoltri a Voicemail
   * Verificare che l'utente abbia attivato il telefono e che il servizio sia disponibile nell'area o usare un metodo alternativo.
* L'utente è bloccato
   * Chiedere all'amministratore di sbloccare l'utente nella portale di Azure.
* SMS non sottoscritto dal dispositivo
   * Chiedere all'utente di modificare i metodi o attivare SMS sul dispositivo.
* Provider di telecomunicazioni non funzionante (nessun input telefono rilevato, problemi relativi ai toni DTMF, ID chiamante bloccato su più dispositivi o SMS bloccati tra più dispositivi)
   * Microsoft usa più provider di telecomunicazioni per instradare telefonate e messaggi SMS per l'autenticazione. Se si verifica uno dei problemi indicati in precedenza, un utente tenta di usare il metodo almeno 5 volte in 5 minuti e le informazioni dell'utente sono disponibili quando Contatta il supporto tecnico Microsoft.

## <a name="app-passwords"></a>Password dell'app

Alcune app non basate su browser non supportano l'autenticazione a più fattori; se un utente è stato abilitato per l'autenticazione a più fattori e prova a usare le app non basate su browser, l'utente non sarà in grado di eseguire l'autenticazione. Una password dell'app consente agli utenti di continuare a eseguire l'autenticazione

Se si applica Multi-Factor Authentication tramite criteri di accesso condizionale e non tramite Multi-Factor Authentication per utente, è possibile creare password di app. Le applicazioni che usano criteri di accesso condizionale per controllare l'accesso non hanno bisogno di password di app.

Se l'organizzazione è federata e usa SSO con Azure AD e si prevede di usare Azure MFA, tenere presente i dettagli di seguito:

* La password dell'app viene verificata da Azure AD e di conseguenza ignora la federazione. La federazione viene usata solo quando si configura la password dell'app. Per gli utenti federati (SSO) le password vengono archiviate nell'ID dell'organizzazione. Se l'utente lascia l'azienda, tali informazioni devono essere trasmesse nell'ID organizzazione tramite DirSync. La disabilitazione o l'eliminazione dell'account può richiedere fino a 3 ore per la sincronizzazione, ritardando la disabilitazione o l'eliminazione delle password dell'app in Azure AD.
* Le impostazioni locali di Controllo dell'accesso Client non vengono rispettate dalla password dell'app.
* Per le password dell'app non è disponibile alcuna funzionalità di registrazione o controllo dell'autenticazione in locale.
* Alcune architetture avanzate possono richiedere una combinazione di nome utente e password dell'organizzazione e password dell'app quando si usa la verifica in due passaggi con i client, a seconda della posizione in cui viene eseguita l'autenticazione. Per i client che si autenticano con un'infrastruttura locale, verranno usati un nome utente e una password dell'organizzazione. Per i client che eseguono l'autenticazione con AD Azure, verrà usata la password dell'app.
* Per impostazione predefinita, gli utenti non possono creare password dell'app. Se si desidera consentire agli utenti di creare password dell'app, selezionare l'opzione **Consentire agli utenti di creare password dell'app per accedere alle applicazioni non basate su browser** nelle impostazione del servizio.

## <a name="next-steps"></a>Passaggi successivi

[Impossibile abilitare la reimpostazione della password in modalità self-service per l'organizzazione](quickstart-sspr.md)

[Abilitare Azure Multi-Factor Authentication per l'organizzazione](howto-mfa-getstarted.md)

[Abilitare la registrazione combinata nel tenant](howto-registration-mfa-sspr-combined.md)

[Documentazione sulla configurazione di un metodo di autenticazione per l'utente finale](https://aka.ms/securityinfoguide)
