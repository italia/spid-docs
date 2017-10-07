Architetture di sistemi implementati con SPID Saml
==================================================

Premessa di tutto è che SPID è un sistema di autenticazione, pertanto, nell'architettura, se necessario, dovrà essere previsto e implementato un sistema di autorizzazione (acl, abac, rbac...).

Possiamo individuare due modalità di implementazione di SPID in sistemi applicativi e sono:

1. architettura "monolitica"
2. architettura a layer
3. architettura mista

# 1. Architettura monolitica

**Pro:** sistema pronto per SPID senza ulteriori componenti.
**Contro:** maggiore sviluppo e manutenzione applicativa e di difficile scalabilità.

L'architettura monolitica si rende standalone da sistemi di accesso (iam) e contiene all'interno tutte le funzionalità necessarie ad autenticare, ed eventualmente autorizzare, l'utente. Questa modalità è più indicata per sistemi verticali, sistemi portal, cms, app mobile.

L'infrastruttura deve prevedere di:

* verificare se l'utente è già autenticato o no (come un qualsiasi plugin di autenticazione)
* esporre il [bottone di login](https://github.com/italia/spid-sp-access-button) che consente all'utente di scegliere l'Identity Provider a cui inviare la richiesta; per indicazioni vedere [questo commento](https://github.com/italia/spid-sp-playbook/issues/3#issuecomment-331719991)
* preparare la request e inviarla all'Identity Provider
* ricevere la response e leggerne gli attributi inviati dall'Identity Provider


# 2. Architettura a layer

**Pro:** sviluppo più rapido gestione di un numero di applicazioni anche non omogenee (scalabilità).
**Contro:** necessità di installare un componente iam (sp) e di manutenerlo e differenti modalità di rilascio degli attributi da parte dei middleware.

Questa architettura prevede l'utilizzo di un middleware che, chiamato dall'applicazione che necessita di autenticazione, prepara e invia la request e riceve e gestisce la response contenente gli attributi dell'utente autenticato. Questa modalità è più indicata per federare un buon numero di sistemi anche non omogenei, app mobile.

Questa architettura necessita di:

* dover configurare un middleware (su github.com/italia sono stati rilasciati shibboleth - installazione e playbook - e simplesamlphp - installazione)
* importare gli attributi sul sistema (differenti tra sistema e sistema di middleware)


# 3. Architettura mista

**Pro:** sistema completo.
**Contro:** maggiore effort nello sviluppo e manutenzione applicativa.

Prevede le due modalità attivabili su necessità di chi la utilizza. Questa modalità è più indicata per sdk.
