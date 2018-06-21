# Feature checklist per risorse SPID

[Developers Italia](https://developers.italia.it/) fornisce nel proprio account GitHub numerosi componenti mantenuti dalla community finalizzati ad integrare SPID.
Tali componenti si dividono in:

* gateway completi:
    * [spid-sp-playbook](https://github.com/italia/spid-sp-playbook)
    * [spid-sp-sapspid](https://github.com/italia/spid-sp-sapspid)
    * [spid-sp-simplesamlphp](https://github.com/italia/spid-sp-simplesamlphp)
* plugin per CMS:
    * [spid-concrete5](https://github.com/italia/spid-concrete5)
    * [spid-drupal-module](https://github.com/italia/spid-drupal-module)
    * [spid-joomla-plugin](https://github.com/italia/spid-joomla-plugin)
    * [spid-laravel](https://github.com/italia/spid-laravel)
    * [spid-liferay](https://github.com/italia/spid-liferay)
    * [spid-limesurvey-plugin](https://github.com/italia/spid-limesurvey-plugin)
    * [spid-magento-ext](https://github.com/italia/spid-magento-ext)
    * [spid-wordpress](https://github.com/italia/spid-wordpress)
* plugin per web framework
    * [spid-django](https://github.com/italia/spid-django)
    * [spid-passport](https://github.com/italia/spid-passport)
    * [spid-rails](https://github.com/italia/spid-rails)
    * [spid-perl-dancer2](https://github.com/italia/spid-perl-dancer2)
    * [spid-spring](https://github.com/italia/spid-spring)
    * [spid-symfony-bundle](https://github.com/italia/spid-symfony-bundle)
* librerie generiche
    * [spid-android-sdk](https://github.com/italia/spid-android-sdk)
    * [spid-dotnet-sdk](https://github.com/italia/spid-dotnet-sdk)
    * [spid-ios-sdk](https://github.com/italia/spid-ios-sdk)
    * [spid-perl](https://github.com/italia/spid-perl)

Questa tabella va inclusa nei README di tutti i componenti SPID al fine di censire la rispondenza alle Regole Tecniche (si veda [qui](https://github.com/italia/spid-perl) per un esempio di compilazione):

|<img src="https://github.com/italia/spid-graphics/blob/master/spid-logos/spid-logo-c-lb.png?raw=true" width="100" /><br />_Compliance with [SPID regulations](http://www.agid.gov.it/sites/default/files/circolari/spid-regole_tecniche_v1.pdf) (for Service Providers)_||
|:---|:---|
|**Metadata:**||
|parsing of IdP XML metadata (1.2.2.4)|?|
|parsing of AA XML metadata (2.2.4)|?|
|SP XML metadata generation (1.3.2)|?|
|**AuthnRequest generation (1.2.2.1):**||
|generation of AuthnRequest XML|?|
|HTTP-Redirect binding|?|
|HTTP-POST binding|?|
|`AssertionConsumerServiceURL` customization|?|
|`AssertionConsumerServiceIndex` customization|?|
|`AttributeConsumingServiceIndex` customization|?|
|`AuthnContextClassRef` (SPID level) customization|?|
|`RequestedAuthnContext/@Comparison` customization|?|
|`RelayState` customization (1.2.2)|?|
|**Response/Assertion parsing**||
|verification of `Response/Signature` value (if any)|?|
|verification of `Response/Signature` certificate (if any) against IdP/AA metadata|?|
|verification of `Assertion/Signature` value|?|
|verification of `Assertion/Signature` certificate against IdP/AA metadata|?|
|verification of `SubjectConfirmationData/@Recipient`|?|
|verification of `SubjectConfirmationData/@NotOnOrAfter`|?|
|verification of `SubjectConfirmationData/@InResponseTo`|?|
|verification of `Issuer`|?|
|verification of `Destination`|?|
|verification of `Conditions/@NotBefore`|?|
|verification of `Conditions/@NotOnOrAfter`|?|
|verification of `Audience`|?|
|parsing of Response with no `Assertion` (authentication/query failure)|?|
|parsing of failure `StatusCode` (Requester/Responder)|?|
|verification of `RelayState` (saml-bindings-2.0-os 3.5.3)|?|
|**Response/Assertion parsing for SSO (1.2.1, 1.2.2.2, 1.3.1):**||
|parsing of `NameID`|?|
|parsing of `AuthnContextClassRef` (SPID level)|?|
|parsing of attributes|?|
|**Response/Assertion parsing for attribute query (2.2.2.2, 2.3.1):**||
|parsing of attributes|?|
|**LogoutRequest generation (for SP-initiated logout):**||
|generation of LogoutRequest XML|?|
|HTTP-Redirect binding|?|
|HTTP-POST binding|?|
|**LogoutResponse parsing (for SP-initiated logout):**||
|parsing of LogoutResponse XML|?|
|verification of `Response/Signature` value (if any)|?|
|verification of `Response/Signature` certificate (if any) against IdP metadata|?|
|verification of `Issuer`|?|
|verification of `Destination`|?|
|PartialLogout detection|?|
|**LogoutRequest parsing (for third-party-initiated logout):**||
|parsing of LogoutRequest XML|?|
|verification of `Response/Signature` value (if any)|?|
|verification of `Response/Signature` certificate (if any) against IdP metadata|?|
|verification of `Issuer`|?|
|verification of `Destination`|?|
|parsing of `NameID`|?|
|**LogoutResponse generation (for third-party-initiated logout):**||
|generation of LogoutResponse XML|?|
|HTTP-Redirect binding|?|
|HTTP-POST binding|?|
|PartialLogout customization|?|
|**AttributeQuery generation (2.2.2.1):**||
|generation of AttributeQuery XML|?|
|SOAP binding (client)|?|

Questa tabella va inclusa solo nei progetti che si pongono l'obiettivo di supportare l'implementazione di Attribute Authority:

|<img src="https://github.com/italia/spid-graphics/blob/master/spid-logos/spid-logo-c-lb.png?raw=true" width="100" /><br />_Compliance with [SPID regulations](http://www.agid.gov.it/sites/default/files/circolari/spid-regole_tecniche_v1.pdf) (for Attribute Authorities)_| |
|:---|:---|
|**Metadata:**||
|parsing of SP XML metadata (1.3.2)|?|
|AA XML metadata generation (2.2.4)|?|
|**AttributeQuery parsing (2.2.2.1):**||
|parsing of AttributeQuery XML|?|
|verification of `Signature` value|?|
|verification of `Signature` certificate against SP metadata|?|
|verification of `Issuer`|?|
|verification of `Destination`|?|
|parsing of `Subject/NameID`|?|
|parsing of requested attributes|?|
|**Response/Assertion generation (2.2.2.2):**||
|generation of `Response/Assertion` XML|?|
|Signature|?|
