# remote

### Étape 1 : Configuration du Port Forwarding dans VirtualBox

1. **Ouvrir VirtualBox** :
   - Lancez VirtualBox et sélectionnez la VM `01_remote`.

2. **Accéder aux Paramètres de la VM** :
   - Cliquez sur le bouton "Configuration" (ou "Settings") pour la VM sélectionnée.

3. **Configurer le Réseau** :
   - Dans la fenêtre des paramètres, allez dans l'onglet "Réseau" (ou "Network").
   - Assurez-vous que l'adaptateur réseau est activé (cochez "Activer l'adaptateur réseau") et qu'il est configuré pour utiliser le mode "NAT".

4. **Ajouter un Port Forwarding** :
   - Cliquez sur le bouton "Avancé" (ou "Advanced") puis sur "Port Forwarding".
   - Ajoutez une nouvelle règle avec les informations suivantes :
     - **Nom** : SSH
     - **Protocole** : TCP
     - **Port hôte** : Choisissez un port non utilisé sur votre machine hôte (par exemple, 2222).
     - **Adresse IP hôte** : Laissez vide (ou `127.0.0.1`).
     - **Port invité** : 22 (c'est le port par défaut pour SSH).
   - Cliquez sur "OK" pour enregistrer les paramètres.

### Étape 2 : Connexion à la VM via SSH

1. **Ouvrir un Terminal** :
   - Sur votre machine hôte, ouvrez un terminal.

2. **Exécuter la Commande SSH** :
   - Utilisez la commande suivante pour vous connecter à la VM :
     ```bash
     ssh -p 2222 root@localhost
     ```
   - Remplacez `2222` par le port que vous avez choisi dans les paramètres de port forwarding.

### Étape 3 : Changer le Port par Défaut de SSH

1. **Se Connecter à la VM** :
   - Une fois connecté, vous êtes sur la ligne de commande de la VM.

2. **Modifier le Fichier de Configuration SSH** :
   - Ouvrez le fichier de configuration SSH avec un éditeur de texte, par exemple `nano` :
     ```bash
     nano /etc/ssh/sshd_config
     ```
   
3. **Changer le Port** :
   - Recherchez la ligne qui dit `#Port 22` et décommentez-la (enlever le `#`) puis changez le port à un autre numéro, par exemple 2222 :
     ```bash
     Port 2222
     ```
   - Sauvegardez le fichier et quittez l'éditeur (`CTRL + O` puis `CTRL + X` dans nano).

4. **Redémarrer le Service SSH** :
   - Pour appliquer les changements, redémarrez le service SSH :
     ```bash
     systemctl restart ssh
     ```

### Étape 4 : Configurer le Pare-feu (UFW)

1. **Vérifier si UFW est installé** :
   - Vérifiez si UFW est installé :
     ```bash
     ufw status
     ```
   - Si ce n’est pas installé, vous pouvez l’installer avec :
     ```bash
     apt install ufw
     ```

2. **Autoriser le Nouveau Port** :
   - Ajoutez une règle pour autoriser le nouveau port que vous avez configuré pour SSH :
     ```bash
     ufw allow 2222/tcp
     ```

3. **Activer UFW (si ce n’est pas déjà fait)** :
   - Activez UFW si ce n’est pas déjà fait :
     ```bash
     ufw enable
     ```

4. **Vérifier les Règles UFW** :
   - Vérifiez que la règle a été ajoutée :
     ```bash
     ufw status
     ```

### Étape 5 : Tester la Connexion SSH

1. **Se Déconnecter de la VM** :
   - Déconnectez-vous de la session SSH actuelle (en tapant `exit`).

2. **Se Reconnecter avec le Nouveau Port** :
   - Utilisez la commande SSH pour vous connecter avec le nouveau port :
     ```bash
     ssh -p 2222 root@localhost
     ```

### Conclusion

Vous devriez maintenant être en mesure de vous connecter à votre VM via SSH en utilisant un port personnalisé et avec le pare-feu configuré pour permettre le trafic sur ce port. Cela renforcera la sécurité en réduisant les tentatives de connexion automatisées sur le port par défaut.