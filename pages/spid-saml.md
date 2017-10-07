SPID Saml
=========

![SPID](https://raw.githubusercontent.com/umbros/spid-docs/master/images/spid-saml.png)

1. L'utente richiede l'accesso ad un servizio che necessita autenticazione
2. L'SP invia una request <AuthnRequest> all'Identity Provider
3. L'IDP richiede le credenziali, secondo il livello SPID stabilito, all'utente
4. L'IDP invia una response <Response> all'SP
5. L'SP eventualmente chiederà ulteriori attributi ad un' Attribute Authority
6. L'SP con i dati provenienti dall'IDP ed eventuali ulteriori di un' Attribute Authority provvede ad autorizzare l'utente nel servizio richiesto

Si rimanda alla documentazione SPID per maggiori informazioni:
- [Documentazione SPID](http://www.agid.gov.it/agenda-digitale/infrastrutture-architetture/spid)
- [Regole tecniche beta](https://spid-regole-tecniche.readthedocs.io/en/latest/)
