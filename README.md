# TP : Configuration de deux modules XBee avec XCTU

## Objectif
Configurer deux modules XBee : 
- L’un en mode **Coordinateur**.
- L’autre en mode **Routeur**.  
Le but est de permettre la communication entre ces deux modules en mode point à point.

---

## Matériel requis
1. Deux modules XBee (par exemple, Série 2 ou Série 3).
2. Deux adaptateurs USB pour connecter les modules XBee à un PC.
3. Logiciel XCTU (disponible gratuitement sur le site de Digi).
4. Un ordinateur avec XCTU installé.

---

## Étapes de configuration

### 1. Installation du logiciel XCTU
- Téléchargez et installez XCTU depuis le site officiel de Digi.
- Connectez les modules XBee à votre PC à l’aide des adaptateurs USB.

### 2. Configuration du module 1 : Coordinateur
1. **Lancer XCTU** :
   - Ouvrez XCTU et ajoutez le module connecté en cliquant sur l’icône **Add devices**. Sélectionnez le port série correspondant et cliquez sur **Finish**.

2. **Mise à jour du firmware** :
   - Allez dans l’onglet **Configuration Modem**.
   - Cliquez sur **Update** pour mettre à jour le firmware.
   - Sélectionnez le firmware correspondant au **Coordinateur API** ou **Coordinateur AT**, selon vos besoins.

3. **Paramètres principaux** :
   - **CH (Channel)** : Définit le canal radio (valeurs possibles : 0x0B à 0x1A). Exemple : 0x0C. Assurez-vous que le Routeur utilise le même canal.
   - **PAN ID (Personal Area Network ID)** : Identifiant unique du réseau Zigbee (valeurs hexadécimales, par ex. 1234). Tous les modules d’un réseau doivent partager le même PAN ID.
   - **DH (Destination High)** : Partie haute de l’adresse 64 bits du destinataire. Par défaut, mettez `00000000`.
   - **DL (Destination Low)** : Partie basse de l’adresse 64 bits du destinataire. Pour envoyer au routeur, entrez l’adresse basse de son module (retrouvable dans **SH** et **SL**).
   - **MY (16-bit Source Address)** : Adresse 16 bits du module (ex. : 0x0000 pour le coordinateur).

4. **Enregistrement des paramètres** :
   - Cliquez sur **Write** pour enregistrer les paramètres dans le module.

---

### 3. Configuration du module 2 : Routeur
1. **Ajout du module** :
   - Déconnectez le Coordinateur et connectez le Routeur.
   - Ajoutez le module dans XCTU en répétant la procédure d’ajout.

2. **Mise à jour du firmware** :
   - Allez dans l’onglet **Configuration Modem**.
   - Sélectionnez le firmware **Routeur API** ou **Routeur AT**, selon vos besoins.

3. **Paramètres principaux** :
   - **CH (Channel)** : Doit être identique à celui du Coordinateur (exemple : 0x0C).
   - **PAN ID** : Identique à celui du Coordinateur (exemple : 1234).
   - **DH (Destination High)** : Partie haute de l’adresse 64 bits du destinataire. Par défaut, mettez `00000000`.
   - **DL (Destination Low)** : Adresse basse du Coordinateur (retrouvable dans **SH** et **SL** du Coordinateur).
   - **MY (16-bit Source Address)** : Adresse 16 bits du module (ex. : 0x0001 pour le routeur).

4. **Enregistrement des paramètres** :
   - Cliquez sur **Write** pour enregistrer les paramètres.

---

### 4. Vérification de la communication
1. Connectez les deux modules à deux ports USB différents sur votre PC.
2. Ouvrez deux terminaux dans XCTU :
   - Un pour le Coordinateur.
   - Un pour le Routeur.
3. Envoyez un message depuis le Coordinateur en mode Terminal.
4. Vérifiez que le message est reçu par le Routeur, et inversement.

---

## Explications des options principales

### 1. CH (Channel)
- Définit le canal radio utilisé par les modules XBee pour communiquer.  
- Les valeurs possibles vont de **0x0B** à **0x1A** (canaux 11 à 26 en Zigbee).  
- Tous les modules d’un même réseau doivent être sur le même canal.  
- Exemple : `0x0C`.

### 2. PAN ID (Personal Area Network ID)
- Identifiant unique pour un réseau Zigbee. Tous les modules du même réseau doivent avoir le même PAN ID.  
- Les valeurs possibles sont en hexadécimal, par exemple : `0x1234`.  
- Un PAN ID différent permet d’isoler des réseaux proches pour éviter les interférences.

### 3. DH (Destination High)
- Partie haute de l’adresse 64 bits du module destinataire.  
- Généralement, la valeur est `00000000` pour un réseau Zigbee classique.  

### 4. DL (Destination Low)
- Partie basse de l’adresse 64 bits du module destinataire.  
- Pour un point à point, configurez-le avec l’adresse basse (SL) du module destinataire.  
- Pour un broadcast (diffusion à tous les modules), configurez `0xFFFF`.

### 5. MY (16-bit Source Address)
- Adresse courte (16 bits) attribuée à chaque module dans le réseau.  
- Cette adresse doit être unique dans le réseau.  
- Exemple : `0x0000` pour le Coordinateur, `0x0001` pour le Routeur.

### 6. SH (Serial Number High) et SL (Serial Number Low)
- Adresse unique 64 bits du module, attribuée en usine.  
- SH représente les 32 bits supérieurs, et SL les 32 bits inférieurs.  
- Utilisé pour identifier de manière unique un module dans un réseau.

### 7. CE (Coordinator Enable)
- Permet de définir si le module est un Coordinateur.  
- Valeurs possibles :
  - `1` : Le module est configuré comme un Coordinateur.
  - `0` : Le module est un Routeur ou un End Device.  

### 8. NI (Node Identifier)
- Nom du module dans le réseau. Ce paramètre est optionnel mais utile pour identifier les modules facilement.  
- Exemple : `Coordinator_1` pour le Coordinateur, `Router_1` pour le Routeur.

### 9. BD (Baud Rate)
- Vitesse de communication série entre le module et l’ordinateur.  
- Valeurs possibles (en bps) : `9600`, `19200`, `38400`, etc.  
- Par défaut, la valeur est généralement `9600`.

### 10. AP (API Mode)
- Définit si le module fonctionne en mode **AT** ou **API**.
- Valeurs possibles :
  - `0` : Mode AT (commandes simples).
  - `1` ou `2` : Mode API (trames structurées pour des configurations avancées).  

### 11. D0 à D8 (Digital Input/Output)
- Configurations des broches d’E/S numériques.  
- Chaque broche peut être configurée comme une entrée ou une sortie :
  - `0` : Désactivée.
  - `1` : Entrée numérique.
  - `2` : Sortie numérique.

### 13. RR (Retries)
- Nombre de tentatives de retransmission si un message n’est pas accusé de réception.  
- Par défaut : `3`.  

### 14. NW (Network Watchdog Timeout)
- Définit une durée d’inactivité après laquelle un module se considère hors du réseau.  
- Permet de détecter les modules inactifs.  

### 15. JV (Join Verification)
- Vérifie si le module a rejoint un réseau valide.  
- Valeurs possibles :
  - `1` : Vérification activée.
  - `0` : Vérification désactivée.

### 16. PL (Power Level)
- Définit la puissance d’émission radio du module.  
- Valeurs possibles (selon le modèle) :
  - `0` : Faible puissance.
  - `4` : Puissance maximale.  

### 17. SM (Sleep Mode)
- Configure le mode d’économie d’énergie du module.  
- Valeurs possibles :
  - `0` : Mode veille désactivé (le module est toujours actif).
  - `1` : Veille activée avec temporisation.  

---

Avec ces explications détaillées, vous avez une compréhension complète des options disponibles dans XCTU pour configurer les modules XBee. Vous pouvez personnaliser les paramètres selon les besoins spécifiques de votre application.
