# Hacking éthique

## Démarrer en sécurité informatique

### Qu'est-ce qu'un hacker ?

To hack : découper quelque chose à l'aide d'un outil (viande ou végétation). En informatique, découper des informations en blog logique et les réassembler ; modifier le fonctionnement de quelque chose pour l'améliorer ; détourner quelque chose de son but premier. En sécurité informatique : détourner la protection d'un système. Deux choix : malveillant ou bienveillant. Un bricoleur, un bidouilleur. Quelqu'un qui est passionné par le fonctionnement d'un système informatique.


### Les types de hacker

- Le hacker bienveillant ou éthique, agit dans la légalité et apprend la sécurité pour se protéger ou protéger d'autres personnes.
- Le hacker malveillant agit de façon illégale, cherche à pirater les autres pour de l'argent ou pour acquérir des données personnels.
- Le justicier va s'attaquer à des sites parce qu'il estime que le site fait du tort à quelqu'un, il essaie de rétablir la justice. Du point de vue de la loi, ses actions sont également illégales. Il va également rechercher des vulnérabilité pour avertir, mais sans autorisation, ce qui est illégal.

### Les bases de la sécurité informatique

Quatre grand piliers :
- Confidentialité : un message est confidentiel si seuls l'expéditeur et le destinataire peuvent le lire
- Authenticité : s'assurer qu'on s'adresse bien à la bonne personne
- Intégrité : s'assurer que le fichier reçu est le même que le fichier envoyé
- Disponibilité : un système non disponible ne peut plus répondre à des requêtes

Exemple : HTTPS = HTTP + SSL/TLS. Le certificat prouve l'authenticité du site. Les échanges de données sont chiffrées (confidentialité + intégrité).

Une vulnérabilité informatique :
- a des origines diverses (bug, erreur, ou volontaire)
- crée une faiblesse dans un système qu'une menace peut exploiter
- classée en catégorie : matériel, logiciel, réseau, physique, humain, etc.
- a un identifiant standardisé : CVE (Common Vulnerabilities and Exposures)
- peut être divulgée de différentes manières : full disclosure (partagée publiquement, illégal), responible disclosure (avertir le responsable, légal), dark web (vendue, illégal), bug bounty
- peut être corrigée avec des tests, des patchs, une sensibilisation

Exemple : faille XSS dans Wordpress (CVE-2016-5834)

Une menace informatique :
- un danger possible qui peut exploiter une vulnérabilité
- peut être intentionelle ou accidentelle
- classée en catégorie : usurpation d'identité, altération des données, répudiation des données, fuite de données, déni de service

Un *exploit* informatique :
- programme ou technique qui exploite une vulnérabilité
- peut être une preuve de concept ou une utilisation illégale

Exemple : PHPMailer (CVE-2016-10033)


### Rappel sur les lois

- Article 323-1 du code pénal : 30000 euros d'amende pour un accès, 45000 euros pour une modification
- Article 323-2 : 75000 euros pour entraver ou fausser un système
- Article 323-3 : 75000 euros pour supprimer ou modifier des données
- Article 323-3-1 : détenir ou diffuser un programme permettant de pirater est puni des mêmes peines, sauf motif légitime (recherche ou sécurité informatique)
- Article 323-7 : la simple tentative, même sans réussite, est punie des mêmes peines
