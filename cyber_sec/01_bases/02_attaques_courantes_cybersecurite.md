# Attaques courantes en cybersécurité

## Définition générale

Une attaque informatique désigne toute action menée dans le but d’obtenir un accès non autorisé, perturber un service, voler des données ou affaiblir la sécurité d’un système. Les attaques peuvent cibler un utilisateur, un serveur, une application web, un réseau, un service cloud ou même la chaîne d’approvisionnement d’une entreprise.

---

# Phishing

Le phishing est une méthode d’attaque basée sur la tromperie. L’attaquant envoie un message (souvent un courriel ou un SMS) en se faisant passer pour une organisation légitime. L’objectif est d’amener la victime à révéler des informations sensibles, telles qu’un mot de passe ou des informations bancaires.

Un exemple classique consiste à imiter un service de livraison et demander à l’utilisateur de “régler des frais” via un lien malveillant. Dans un contexte professionnel, l’attaquant peut cibler des employés et leur faire croire qu’un dirigeant leur demande une action urgente.

Source utile : ANSSI — Hameçonnage.

---

# Spear-phishing

Contrairement au phishing généraliste, le spear-phishing est ciblé. L’attaquant collecte des informations sur une personne spécifique pour rendre son message crédible. Il peut s’appuyer sur des données trouvées sur des réseaux professionnels comme LinkedIn, ou sur des communications internes fuitées.

Une attaque fréquente vise le service comptabilité : un employé reçoit un courriel imitant parfaitement le supérieur hiérarchique lui demandant de procéder à un “virement urgent”. Ce type d’attaque représente une part significative des fraudes financières modernes (source : FBI IC3 Report).

---

# Ransomware

Le ransomware est un logiciel malveillant conçu pour chiffrer les fichiers d’un système. Lorsque l’attaque réussit, la victime perd l’accès à ses données et un message l’invite à payer une rançon pour obtenir une clé de déchiffrement.

Les attaques les plus dangereuses se propagent dans tout un réseau informatique, comme ce fut le cas avec WannaCry ou LockBit. Les conséquences sont souvent graves : interruption d’activité, perte de données, coûts financiers élevés et atteinte à la réputation.

Les mesures de défense reposent sur les sauvegardes régulières, la segmentation réseau, la mise à jour des systèmes et la limitation des privilèges.

---

# Exploitation de vulnérabilités

Une vulnérabilité est une faille dans un logiciel ou un système. Lorsqu’un attaquant exploite cette faille, il peut exécuter du code, contourner des protections ou accéder à des données confidentielles.

Les attaques peuvent viser un simple navigateur, un serveur web, une bibliothèque logicielle ou un système d’exploitation entier. Certaines vulnérabilités, dites *zero-day*, n’ont pas encore été découvertes par l’éditeur du logiciel et ne disposent donc d’aucun correctif.

Exemples notables :

- Log4Shell (2021), permettant l’exécution de code à distance via un simple paramètre.

- Heartbleed (2014), qui exposait la mémoire interne de serveurs utilisant OpenSSL.

Sources : MITRE CVE Database, NIST NVD.

---

# Attaques web (OWASP)

Dans les applications web, plusieurs techniques d’attaque reviennent constamment. La plus connue est l’injection SQL, où l’attaquant modifie une requête afin d’accéder à des données auxquelles il ne devrait pas avoir accès. Une autre attaque fréquente est le Cross-Site Scripting (XSS), qui permet d’injecter du JavaScript dans des pages web afin de voler des cookies ou rediriger un utilisateur.

Les erreurs d’authentification, les failles dans la gestion des sessions et le manque de validation des données entrantes sont responsables d’un grand nombre d’incidents.

Référence complète : OWASP Top 10.

---

# Attaque de type « Man-in-the-Middle »

Une attaque MITM consiste à intercepter les communications entre deux parties. L’attaquant se place entre la victime et un service (site web ou réseau) et observe les échanges ou les modifie.

Un exemple courant survient dans les réseaux WiFi publics non chiffrés, où un attaquant peut capturer les informations transmises, y compris des identifiants si le site n’utilise pas HTTPS. L’utilisation d’un VPN, de certificats TLS valides et la vigilance sur les réseaux utilisés permettent de réduire ce risque.

---

# Attaques par mots de passe

Plusieurs techniques existent pour compromettre des comptes utilisateur.

Le *brute force* consiste à tester toutes les combinaisons possibles. Le *dictionnaire* s’appuie sur des mots de passe fréquents. Le *credential stuffing* réutilise des identifiants volés dans des fuites de données, car beaucoup d’utilisateurs utilisent le même mot de passe partout. Enfin, le *password spraying* consiste à essayer un petit nombre de mots de passe courants sur un grand nombre de comptes, afin d’éviter les verrouillages.

Source recommandée : HaveIBeenPwned pour vérifier l’exposition de mots de passe.

---

# Ingénierie sociale

L’ingénierie sociale repose sur la manipulation psychologique. L’attaquant exploite la confiance, la précipitation ou l’émotion de la victime. Cela peut prendre la forme d’un appel téléphonique prétendant provenir du support technique, ou d’un individu qui se glisse dans un bâtiment en suivant une personne ayant un badge d’accès.

Cette technique contourne totalement les protections techniques et cible l’humain, souvent considéré comme le maillon le plus vulnérable.

Source : ANSSI — Ingénierie sociale.

---

# Attaques DDoS

Une attaque par déni de service distribué vise à rendre un service indisponible en submergeant son serveur de requêtes. Pour cela, les attaquants utilisent généralement une botnet, c’est-à-dire un ensemble d’appareils compromis (ordinateurs, routeurs, caméras IP…).

Lorsqu’un système est visé par une attaque DDoS de grande ampleur, il devient incapable de répondre aux utilisateurs légitimes et sature.

Source utile : Cloudflare — rapports DDoS trimestriels.

---

# Attaques de la chaîne d’approvisionnement

Dans une attaque de supply chain, le cybercriminel s’introduit chez un fournisseur logiciel ou matériel pour atteindre indirectement la véritable cible. Ce type d’attaque est difficile à détecter, car les mises à jour logicielles sont habituellement considérées comme fiables.

L’attaque SolarWinds (2020) en est un exemple majeur : des milliers d’entreprises ont installé une mise à jour compromise.

Source : CISA — SolarWinds Analysis.

---

# Attaques visant l’IoT

Les objets connectés sont souvent mal protégés : mots de passe par défaut, mises à jour absentes, ports ouverts par défaut. Des botnets comme Mirai ont exploité ces faiblesses pour infecter des dizaines de milliers d’appareils et mener des attaques massives.

La sécurisation de l’IoT repose principalement sur la mise en place de mots de passe robustes, la désactivation des services inutiles et la mise à jour régulière du firmware.
