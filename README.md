
# 🛡️ Système de gestion centralisée des utilisateurs avec FreeIPA et authentification réseau via FreeRADIUS

##  Objectif du projet

Ce projet a pour but de **sécuriser un réseau local** et de **centraliser la gestion des utilisateurs** à l’aide de **FreeIPA**, une solution combinant LDAP, Kerberos et d'autres services. L’**intégration de FreeRADIUS** permet d’assurer une authentification réseau sécurisée, notamment pour les connexions **Wi-Fi avec WPA2-Enterprise (PEAP, TTLS)**.

---

##  Technologies utilisées

* **FreeIPA** : Fournit un annuaire LDAP, Kerberos, et une interface de gestion des identités.
* **FreeRADIUS** : Serveur RADIUS utilisé pour authentifier les utilisateurs du réseau.
* **Kerberos** : Protocole d’authentification réseau sécurisé.
* **LDAP** : Protocole d’annuaire pour la gestion centralisée des utilisateurs.

---

## 🎯 Objectifs pédagogiques

* Comprendre le rôle d’un **annuaire LDAP** dans un réseau.
* Déployer **FreeIPA** pour centraliser la gestion des utilisateurs.
* Configurer **FreeRADIUS** pour l’authentification des utilisateurs via Wi-Fi.
* Appliquer des **politiques de mot de passe** et analyser les **logs d’authentification**.

---

##  Déploiement de FreeIPA

1. **Installation des outils de base** (`wget`, `vim`, `net-tools`, `firewalld`, etc.).
2. **Configuration du nom d’hôte** (`hostnamectl`) et du fichier `/etc/hosts`.
3. **Installation de FreeIPA Server** avec LDAP, Kerberos et éventuellement DNS.
4. **Configuration du pare-feu** pour autoriser les ports nécessaires.
5. **Exécution de `ipa-server-install`** pour lancer la configuration interactive.
6. **Vérification du bon fonctionnement** via `kinit admin` et `ipa user-find`.
7. **Accès à l’interface Web** : [https://192.168.X.X](https://192.168.X.X)

---

##  Intégration de FreeRADIUS avec FreeIPA

1. **Installation des paquets nécessaires** : `freeradius`, `freeradius-ldap`, `openldap-clients`.
2. **Création d’un compte de service LDAP** (`radiusd`) dans FreeIPA.
3. **Configuration du module LDAP** dans FreeRADIUS (`mods-available/ldap`).
4. **Activation du module LDAP** (`ln -s ldap mods-enabled/ldap`).
5. **Modification du fichier `sites-enabled/default`** pour inclure LDAP dans les sections `authorize` et `authenticate`.
6. **Déclaration des clients autorisés** (`clients.conf`).
7. **Ajout d’utilisateurs** dans FreeIPA via `ipa user-add`.
8. **Démarrage de FreeRADIUS en mode debug** : `radiusd -X`.
9. **Test d’authentification** avec `radtest`.

---

##  Simulation d’une connexion Wi-Fi sécurisée (WPA2-Enterprise)

###  Côté Serveur

* **Simulation de cartes Wi-Fi virtuelles** : `modprobe mac80211_hwsim radios=2`.
* **Configuration d’un point d’accès** avec `hostapd` :

  * Authentification WPA2-Enterprise via FreeRADIUS
  * Clé partagée : `testing123`

###  Côté Client

* **Utilisation de `wpa_supplicant`** pour se connecter au réseau Wi-Fi sécurisé.
* **Configuration PEAP/MSCHAPv2** dans `wpa_supplicant.conf` :

  ```ini
  network={
      ssid="Test-RADIUS"
      key_mgmt=WPA-EAP
      eap=PEAP
      identity="usertest"
      password="passer123"
      phase2="auth=MSCHAPV2"
      ca_cert="/etc/ssl/certs/ca-certificates.crt"
  }
  ```

---

##  Résultat attendu

* Authentification réussie des utilisateurs via FreeRADIUS connecté à FreeIPA.
* Connexion Wi-Fi sécurisée grâce à la validation centralisée des identifiants.
* Traçabilité via les logs RADIUS et FreeIPA.
* Application des politiques de sécurité : mots de passe, restrictions d’accès, etc.

---

## 📎 Ressources complémentaires

* [Documentation officielle FreeIPA](https://www.freeipa.org/page/Documentation)
* [Site officiel FreeRADIUS](https://freeradius.org/)
* [LDAP - RFC 4511](https://datatracker.ietf.org/doc/html/rfc4511)

---

