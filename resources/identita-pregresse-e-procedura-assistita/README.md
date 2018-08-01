# Identità pregresse e procedura assistita
Il modello di integrazione è basato sul SAML 2.0 in cui il possessore delle indentità (SP) genera una SAMLResponse verso il destinario delle indentità (IdP).
Il trust tra i due sistemi è basato sullo scambio offline di metadati come avviene per una configurazione di un SP o IdP in SPID.
Per il processo fare riferimento ai documenti:
 - [Identità pregresse](http://www.agid.gov.it/sites/default/files/circolari/spid-procedura-identita-pregresse_allegato_dt_n._27_-_7_feb_2018.pdf)
- [Procedura assistita](http://www.agid.gov.it/sites/default/files/circolari/spid-procedura-assistita_allegato_dt_n._27_-_7_feb_2018.pdf)

## Chiavi di firma e cifratura
Le chiavi di firma e cifratura devono rispettare i seguenti requisiti:
**alg: SHA256withRSA - Chiave RSA lughezza >= 2.048 bits**

## Metadata IdP
    <?xml version="1.0" encoding="UTF-8"?>
    <md:IdReuseDescriptor xmlns:md="http://spid.gov.it/prevProfiles/metadata" ID="...">
        <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
            <Reference URI="#...">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                    <Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
                <DigestValue>...</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>...</SignatureValue>
        <KeyInfo>
            <X509Data>
                <X509Certificate>...</X509Certificate>
            </X509Data>
        </KeyInfo>
    </Signature>
    <identityProviders>
        <identityProvider>
            <idpCode>...</idpCode>
            <idpName>IdP Test</idpName>
            <idpInfoPage>...</idpInfoPage>
            <idpEndpointResponse>...</idpEndpointResponse>
            <idpSigningCert>...</idpSigningCert>
            <idpEncryptionCert>...</idpEncryptionCert>
        </identityProvider>
    </identityProviders>
    </md:IdReuseDescriptor>
i metadati devono essere firmati da chi li emette in questo caso l'IDP le informazioni che riporta il metadata sono:

-   **idpCode**  codice univoco dell'idp all'interno del circuito (si consiglia di usare una convenzione basata su fqdn)
-   **idpName**  nome descrittivo dell'idp
-   **idpInfoPage**  url di una pagina informativa rispetto al processo di identita' pregresse
-   **idpEndpointResponse**  url dove l'SP deve eseguire il POST della SAMLResponse relativa all'identita' che deve cedere
-   **idpSigningCert**  riporta codificato in Base64 il certificato (X509) collegato alla chiave privata con cui l'IDP firma le risposte verso l'SP
-   **idpEncryptionCert**  riporta codificato in Base64 il certificato (X509) collegato alla chiave privata con cui l'IDP decifra le richieste dell'SP

il metadata e' firmato secondo lo standard  [xmldsig](https://www.w3.org/TR/xmldsig-core1/)  e deve essere trustato dall'SP.

Si riporta lo schema XSD con cui l'IdP puo' generare il metadata e scaricabile  [xsd metadata sp](https://github.com/italia/spid-resources/blob/master/identita-pregresse-e-procedura-assistita/xsd/spid-idreusemetadata-idp.xsd)

## Metadata SP
In maniera analoga l'SP deve esporre i propri metadati (si riporta un esempio di un gestore di identita' pregresse di test).

    <?xml version="1.0" encoding="UTF-8"?>
    <md:IdReuseDescriptor xmlns:md="http://spid.gov.it/prevProfiles/metadata" ID="...">
        <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
            <Reference URI="#...">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                    <Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
                <DigestValue>...</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>...</SignatureValue>
        <KeyInfo>
            <X509Data>
                <X509Certificate>...</X509Certificate>
            </X509Data>
        </KeyInfo>
    </Signature>
    <info>
        <idReuseInfoPage>...</idReuseInfoPage>
        <idReuseStartDate>...</idReuseStartDate>
        <idReuseEndDate>...</idReuseEndDate>
        <idReuseProfileType>...</idReuseProfileType>
    </info>
    <serviceProvider>
        <spCode>...</spCode>
        <spName>...</spName>
        <spInfoPage>...</spInfoPage>
        <spEndpointResult>...</spEndpointResult>
        <spSigningCert>...</spSigningCert>
        <spEncryptionCert>...</spEncryptionCert>
    </serviceProvider>
    </md:IdReuseDescriptor>

le informazioni che l'SP deve popolare sono

-   **info**
    -   **idReuseInfoPage**  pagina informativa dove descrive il processo di gestione delle identita'
    -   **idReuseStartDate**  data inizio del processo di migrazione delle identita'
    -   **idReuseEndDate**  data fine del processo di migrazione delle identita'
    -   **idReuseProfileType**  deve essere valorizzato con  **verified**  per le identita' pregresse verificate, oppure  **notverified**  per quelle che devono comunque essere riconosciute de viso
-   **serviceProvider**
    -   **spCode**  codice univoco dell'sp all'interno del circuito (si consiglia di usare una convenzione basata su fqdn).  **Lo stesso valore deve essere riportato nell'issuer della SAMLResponse e dell'Assertion**
    -   **spName**  nome descrittivo dell'sp. Viene mostrato a video all'utente che sta cedendo la propria identita' pregressa
    -   **spInfoPage**  url di una pagina dell'SP dove l'IDP invia l'esisto sincrono della presa in carico della registrazione o gli eventuali errori di validazione delle richieste. valido solo se  **idReuseProfileType=verified**
    -   **spEndpointResult**  url dove l'IDP invia l'esito asincrono della registrazione
    -   **spSigningCert**  riporta codificato in Base64 il certificato (X509) collegato alla chiave privata con cui l'SP firma le richieste verso l'SP
    -   **spEncryptionCert**  riporta codificato in Base64 il certificato (X509) collegato alla chiave privata con cui l'SP decifra le risposte dell'IDP

il metadata deve essere firmato secondo lo standard  [xmldsig](https://www.w3.org/TR/xmldsig-core1/)  e trustato dall'IDP
Si riporta lo schema XSD con cui l'SP puo' generare il metadata e scaricabile  [xsd metadata sp](https://github.com/italia/spid-resources/blob/master/identita-pregresse-e-procedura-assistita/xsd/spid-idreusemetadata-sp.xsd).

### Invio della SAMLResponse verso l'IDP
Il processo di migrazione dell'identita' inizia con l'invio dell'SP di una SAMLResponse che contiene gli attributi che qualificano l'utente (fare riferimento alla documentazione funzionale per i domini dei valori)

Una saml response e' strutturata nel seguente modo logico:

    ResponseSigned (con chiave privata dell'SP il cui certificato e' dichiarato in spSigningCert nei metadati dell'SP)
     |
     |--Response
    	|
    	|--EncryptedAssertion (con chiave pubblica contenuta nel certificato dichiarato in idpEncryptionCert nei metadati dell'IDP)
    		|
    		|--AssertionSigned (opzionale) (con chiave privata dell'SP il cui certificato e' dichiarato in spSigningCert nei metadati dell'SP)
    			|
    			|--Assertion

AssertionSigned e' opzionale quindi l'EncryptedAssertion puo' contenere direttamente una Assertion, se contiene AssertionSigned l'idp ne verifica comq la firma.

Si riporta come esempio una SAMLResponse di test (pretty printed):

    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <saml2p:Response xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol" ID="..." IssueInstant="..." Version="2.0">
        <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://test-sp.poste.it</saml2:Issuer>
        <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
            <Reference URI="#...">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                    <Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
                <DigestValue>...</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>...</SignatureValue>
        <KeyInfo>
            <X509Data>
                <X509Certificate>...</X509Certificate>
            </X509Data>
        </KeyInfo>
    </Signature>
    <saml2p:Status>
        <saml2p:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </saml2p:Status>
    <saml2:EncryptedAssertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">
        <xenc:EncryptedData xmlns:xenc="http://www.w3.org/2001/04/xmlenc#" Id="_c87081ed941d9ad4b66edaeffd3e2177" Type="http://www.w3.org/2001/04/xmlenc#Element">
        <xenc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#aes256-cbc"/>
        <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <ds:RetrievalMethod Type="http://www.w3.org/2001/04/xmlenc#EncryptedKey" URI="#..."/>
    </ds:KeyInfo>
    <xenc:CipherData>
        <xenc:CipherValue>...</xenc:CipherValue>
    </xenc:CipherData>
    </xenc:EncryptedData>
    <xenc:EncryptedKey xmlns:xenc="http://www.w3.org/2001/04/xmlenc#" Id="...">
    <xenc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#rsa-1_5"/>
    <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    <ds:KeyValue>
        <ds:RSAKeyValue>
            <ds:Modulus>...</ds:Modulus>
            <ds:Exponent>...</ds:Exponent>
        </ds:RSAKeyValue>
    </ds:KeyValue>
    </ds:KeyInfo>
    <xenc:CipherData>
        <xenc:CipherValue>...</xenc:CipherValue>
    </xenc:CipherData>
    <xenc:ReferenceList>
        <xenc:DataReference URI="#..."/>
    </xenc:ReferenceList>
    </xenc:EncryptedKey>
    </saml2:EncryptedAssertion>
    </saml2p:Response>

La response deve essere generata secondo gli standard definiti in  [saml 2.0 core](https://docs.oasis-open.org/security/saml/v2.0/).

Si riporta lo schema XSD con cui l'SP puo' generare il metadata e scaricabile  [saml-schema-protocol-2.0](https://github.com/italia/spid-resources/blob/master/identita-pregresse-e-procedura-assistita/xsd/saml-schema-protocol-2.0.xsd).

La saml response deve essere codifica in base64 e inviata in HTTP-POST all'endpoint indicato in idpEndpointResponse nei metadata dell'IDP.

Questo deve essere fatto attraverso una pagina autopostante eseguita dallo user-agent in sessione sull'SP es:

    <html>
       <head>
          <meta http-equiv='Content-Type' content='text/html; charset=utf-8'>
          <meta http-equiv='Cache-Control' content='no-cache, no-store' >
          <meta http-equiv='Pragma' content='no-cache' >
       </head>
       <body onLoad="javascript:document.xxxForm.submit()">
          <form action='...'  method='POST' name='xxxForm'>
             <input type='hidden' name='SAMLResponse' value='...' />
                <input type='hidden' name='RelayState' value='...' />
             <noscript>
                <h3 style='color: red;'>Javascript disabilitato</h3>
                <input type='submit' value='Invia Risposta di Autenticazione' />
             </noscript>
          </form>
       </body>
    </html>

Il field **SAMLResponse** deve contenere il b64 della SAMLResponse. L'SP puo' opzionalmente passare anche un valore opaco per l'idp nel campo **RelayState** che l'idp tornara' senza nessuna alterazione all'SP nelle risposte sincrone. Il RelayState e' utile per mantenere informazioni di stato oppure per tecniche anti CSRF.

### Risposte dell'IDP verso l'SP
L'IDP puo' inviare due tipi di risposte verso l'SP quelle sincronre veicolate dall'user-agent dell'utente in sessione e quelle asincrone alla fine del processo di validazione dell'identita'

Le risposte sincrone vengono inviate all'endpoint  **spInfoPage**  indicato nei metadata dell'IDP e quelle asincrone all'endpoint indicato in  **spEndpointResult**  sempre nei metadati dell'IDP

Tutte le risposte devono rispettare il seguente schema: ([spid-idreuseverified-result.xsd](https://github.com/italia/spid-resources/blob/master/identita-pregresse-e-procedura-assistita/xsd/spid-idreuseverified-result.xsd)).

    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:md="http://spid.gov.it/spIdentity/verified/result"  xmlns:ds="http://www.w3.org/2000/09/xmldsig#" targetNamespace="http://spid.gov.it/spIdentity/verified/result" version="1.0"> 
    <xs:import namespace="http://www.w3.org/2000/09/xmldsig#" schemaLocation="http://www.w3.org/TR/2002/REC-xmldsig-core-20020212/xmldsig-core-schema.xsd"/>
      <xs:element name="SpIdentity"  type="md:spVerifiedIndentity"/> 
      <xs:complexType name="spVerifiedIndentity">
        <xs:sequence>
         <xs:element ref="ds:Signature" minOccurs="0"/>
    	  <xs:element name="idpCode" type="xs:string"/>
          <xs:element name="responseID" type="xs:string"/>
          <xs:element name="result" type="xs:string"/>
          <xs:element name="fiscalCode" type="xs:string"/>
          <xs:element name="name" type="xs:string"/>
          <xs:element name="familyName" type="xs:string"/>
        </xs:sequence>
        <xs:attribute name="ID" type="xs:string" use="required"/>
      </xs:complexType>
    </xs:schema>

Il significato degli attributi:

-   **idpCode**  riporta il codice univoco dell'IDP che invia la risposta cioe' quello indicato nell'attributo idpCode nei metadata dell'idp
-   **responseID**  e' valorizzato con il valore dell'attributo ID della SAMLRespone inviata dall'SP
-   **result**  riporta un codice di esito dell'operazione (fare riferimento a  [tabella-messaggi-spid-v1.2.pdf](doc/integrazione-sp/tabella-messaggi-spid-v1.2.pdf))
-   **fiscalCode**,  **name**  e  **familyName**  riportano i dati anagrafici dell'utente registrato e attivato.  **Questi attributi sono valorizzati solo in caso di risposte con esito positivo che corrispondono ai codici "ErrorCode nr100"=registrazione presa in carico per la risposta sincrona e "ErrorCode nr25"=identita' attivata per la risposta asincrona**  e se  **idReuseProfileType=verified**  in tutti gli altri casi non saranno valorizzati.

Nel caso in cui l'SP dichiari  **idReuseProfileType=notverified**  l'xsd che descrive la risposta asincrona e' il seguente (in sostanza non riporta nessun attributo i cui valori possano ricondurre all'utente) e scaricabile da [spid-idreusenotverified-result.xsd](https://github.com/italia/spid-resources/blob/master/identita-pregresse-e-procedura-assistita/xsd/spid-idreusenotverified-result.xsd).

    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:md="http://spid.gov.it/spIdentity/notverified/result" xmlns:ds="http://www.w3.org/2000/09/xmldsig#" targetNamespace="http://spid.gov.it/spIdentity/notverified/result" version="1.0">
    <xs:import namespace="http://www.w3.org/2000/09/xmldsig#" schemaLocation="http://www.w3.org/TR/2002/REC-xmldsig-core-20020212/xmldsig-core-schema.xsd"/>
      <xs:element name="SpIdentity"  type="md:spVerifiedIndentity"/>
      <xs:complexType name="spVerifiedIndentity">
        <xs:sequence>
           <xs:element ref="ds:Signature" minOccurs="0"/>
    	  <xs:element name="idpCode" type="xs:string"/>
          <xs:element name="responseID" type="xs:string"/>
          <xs:element name="result" type="xs:string"/>
        </xs:sequence>
        <xs:attribute name="ID" type="xs:string" use="required"/>
      </xs:complexType>
    </xs:schema>

La casistica degli errori possibili si divide in due casistiche principali

-   l'idp non riesce ad identificare l'sp chiamante e quindi risponde con una pagina di cortesia di errore
-   l'idp riesce ad identificare l'sp chiamante (firma della SAMLResponse verificata e firmatario riconosciuto) e quindi risponde con uno SpIdentity descritto prima.

**nota:**  sull'ambiente di test di integrazione nella pagina di cortesia vengono riportati i veri errori per facilitare il debug, in quello di produzione verra' riportato solo un messaggio generico.

La struttura di una risposta è:

    EncryptedSpIdentity (con chiave pubblica contenuta nel certificato dichiarato in spEncryptionCert nei metadati dell'SP)
    	|
    	|--SpIdentitySigned (con chiave privata dell'IDP il cui certificato e' dichiarato in idpSigningCert nei metadati dell'IDP)
    			|
    			|--SpIdentity

Si riporta un esempio di risposta:

    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <md:SpIdentity xmlns:md="http://spid.gov.it/spIdentity/verified/result" ID="...">
        <idpCode>...</idpCode>
        <responseID>...</responseID>
        <result>ErrorCode nr25</result>
        <fiscalCode>...</fiscalCode>
        <name>...</name>
        <familyName>...</familyName>
    </md:SpIdentity>

e il suo cifrato e firmato:

    <?xml version="1.0" encoding="UTF-8"?>
    <md:SpIdentity xmlns:md="http://spid.gov.it/spIdentity/verified/result"  ID="...">
        <xenc:EncryptedData xmlns:xenc="http://www.w3.org/2001/04/xmlenc#" Type="http://www.w3.org/2001/04/xmlenc#Content">
        <xenc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#aes256-cbc"/>
        <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <xenc:EncryptedKey>
            <xenc:EncryptionMethod Algorithm="http://www.w3.org/2001/04/xmlenc#rsa-1_5"/>
            <xenc:CipherData>
                <xenc:CipherValue>...</xenc:CipherValue>
            </xenc:CipherData>
        </xenc:EncryptedKey>
    </ds:KeyInfo>
    <xenc:CipherData>
        <xenc:CipherValue>...</xenc:CipherValue>
    </xenc:CipherData>
    </xenc:EncryptedData>
    </md:SpIdentity>

Le risposte sincrone dell'IDP verso l'SP saranno sempre con delle pagine autopostanti interpretate dallo user-agent in sessione:

    <html>
       <head>
          <meta http-equiv='Content-Type' content='text/html; charset=utf-8'>
          <meta http-equiv='Cache-Control' content='no-cache, no-store' >
          <meta http-equiv='Pragma' content='no-cache' >
       </head>
       <body onLoad="javascript:document.xxxForm.submit()">
          <form action="..." method="POST" name="...">
    	<input name="Result" value="..." type="hidden">
    	<input name="RelayState" value="..." type="hidden">
             <noscript>
                <h3 style='color: red;'>Javascript disabilitato</h3>
                <input type='submit' value='Invia Risposta Registrazione presa in carico' />
             </noscript>
          </form>
       </body>
    </html>
    
Il relayState se passato dall'SP sara' presente solo nelle risposte sincrone.
