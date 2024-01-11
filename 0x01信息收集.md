# 0x01 Collecte d'informations

## Aperçu de la collecte d'informations

1) Comme le dit l'adage, l'essence de la pénétration est la collecte d'informations, et l'étendue de la collecte d'informations détermine jusqu'où le test de pénétration peut aller.
2) Plus la collecte d'informations est détaillée, mieux c'est.

## Types d'informations collectées

1. informations sur le nom de domaine
2. informations sur le serveur
3. informations sur le logiciel intermédiaire
4. informations sur le port
5. informations sur le répertoire
6. informations sensibles
7. les informations relatives au site
8. les informations relatives aux travailleurs sociaux, telles que les boîtes aux lettres, les numéros de téléphone et d'autres informations sur les comptes.

## Collecte d'informations sur les noms de domaine

1) Le nom de domaine comprend le nom de domaine racine et le nom de sous-domaine. Par exemple, baidu.com, qui est le nom de domaine racine/principal, pan.baidu.com, qui est un sous-domaine.
2. avant le début du test de pénétration général, on peut savoir que les informations ne concernent principalement que le nom de domaine principal. Il faut partir du nom de domaine principal, collecter les informations d'enregistrement du site cible, les informations d'archivage, les informations DNS, les noms de sous-domaines, etc.

3. méthodes de collecte d'informations sur les noms de domaine
   - Requête Whois. Requête en ligne et le système est fourni avec un outil de requête.
   - Demande d'enregistrement. L'enregistrement concerne généralement le serveur situé en Chine continentale. S'il existe un enregistrement, le bas du site comporte généralement un numéro d'enregistrement, que l'on peut utiliser pour interroger l'enregistrement pertinent du corps principal de l'information.
4. les méthodes de collecte d'informations sur les sous-domaines :
   - Layer Sub-Domain Miner
   - oneforall
   - Plateformes de cartographie du cyberespace telles que shodan, fofa, 360quake, zoomeye, hunter, censys, etc.
   - Syntaxe Google, site : xxx.com
   - Outils en ligne tels que DNSdumpster, etc.
   - Transparence des certificats, énumération des journaux publics
   - Outils d'énumération fournis avec kali
   - k8

## Collecte d'informations sur le serveur

1. collecte principalement des informations sur le système et l'IP du serveur.
2. méthode de collecte :
   - whatweb propre à kali
   - nmap
   - Plugin Google Chrome Wappalyzer, WhatRuns
   - Selon les caractéristiques sensibles de linux et de windows sur la casse du répertoire, linux est sensible à la casse, windows insensible à la casse.
   - Plateformes de cartographie du cyberespace, telles que shodan, fofa, 360quake, zoomeye, hunter, censys, etc.

3. la collecte de données IP réelles :
   - Plateformes de cartographie du cyberespace telles que shodan, fofa, 360quake, zoomeye, hunter, censys, etc.
   - Super ping
   - Identification par courrier électronique
   - Utilisation de sous-domaines
   - Certificats de site
   - Fichiers de sites web sensibles
   - Sites proxy étrangers en ligne tels que https://asm.ca.com/en/ping.php
   - Consulter l'historique des résolutions
   - nslookup


## Collecte d'informations sur l'intergiciel

1. collecter principalement le type et la version de l'intergiciel, si la station et la bibliothèque sont séparées, etc.
2. méthode de collecte :
   - nmap
   - whatweb
   - Wappalyzer, WhatRuns
   - Serveur du site web, X-Powerd-By, Cookie et autres informations.
   - Plateformes de cartographie du cyberespace telles que shodan, fofa, 360quake, zoomeye, hunter, censys, etc.

## Informations sur le port

1) En fonction du port, on peut juger du type de système de serveur, de l'utilisation ciblée de différentes vulnérabilités de l'hôte, telles que l'accès non autorisé et l'éclatement du mot de passe sur le port 21 et le port 22, ms17010 sur le port 445 du système Windows, cve-2019-0708 sur le port 3389 du système Windows, etc.
2. méthodes de collecte
   - nmap
   - masscan
   - Plateformes de cartographie du cyberespace, telles que shodan, fofa, 360quake, zoomeye, hunter, censys, etc.

## Collecte d'informations sur les répertoires

1) À partir des informations de répertoire, vous pouvez rechercher des accès non autorisés, des traversées de répertoire, des fuites de code source, des fuites de fichiers sensibles, etc.
2. méthodes de collecte
   - Analyse de répertoire Mikado, analyse d'arrière-plan Mikado
   - dirsearch
   - dirbuster
   - dirb
   - subDomainsbrute

## Informations sensibles

1. y compris les fichiers de sauvegarde, le code source, robots.txt, les pages de gestion de l'arrière-plan, les points de téléchargement de fichiers, les interfaces API, les informations de configuration du serveur, les informations de configuration du site web, les fichiers de base de données, etc.
2. méthode de collecte :
   - L'outil de balayage Imperial Sword
   - dirbuster
   - dirsearch
   - dirb
   - subDomainsbrute
   - JSFinder
   - syntaxe de Google hack

## Informations sur le site

1. y compris l'empreinte du CMS du site, le WAF, le langage de programmation, etc.
2. méthodes de collecte :
   - Plateformes d'identification en ligne, telles que Tidal Fingerprint, CloudSyders Fingerprint, BugScanner
   - whatweb
   - Wappalyzer, WhatRuns
   - Caractéristiques html
   - Identification du WAF : wafw00f

## Collecte d'informations sur les travailleurs sociaux

1. y compris les informations relatives aux mots de passe de divers comptes, les numéros de téléphone, les adresses électroniques, etc.
2. Méthodes de collecte :
   - Phishing de travailleurs sociaux, honeypot yyds
   - Informations sur le mot de passe du compte, numéro de téléphone, adresse électronique, etc. existant sur le site
   - Association de contre-vérification du numéro d'enregistrement
   - Base de données sur les travailleurs sociaux
