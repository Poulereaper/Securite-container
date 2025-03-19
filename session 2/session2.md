# Session 2

## Activité Pratiques - Gestion des Réseaux et des Secrets

**1. Éviter l’Exposition Involontaire de Ports**

- Lancer un container avec restriction de ports :
```bash
docker run -d -p 8080:80 nginx
```

- Vérifier si le port est exposé avec netstat ou ss.

**2. Restreindre les permissions d’accès aux fichiers sensibles**

- Monter un volume avec des permissions spécifiques :
```bash
docker run -it --rm -v /etc/passwd:/mnt/passwd:ro alpine sh
```
- Pouvez vous lire le fichier /mnt/passwd ?
- Pouvez-vous écrire le fichier /mnt/passwd ?

**3. Auditer la configuration d’un container avec Docker Bench**

- Installer et exécuter Docker Bench for Security :
```bash
git clone https://github.com/docker/docker-bench-security.git
cd docker-bench-security/
```

- Auditer votre host, quelle est votre score ?
- Auditer le container vulnerables/web-dvwa, que remarquez-vous ?

**4. Stocker et Utiliser des Secrets**

- Lancer un container Vault :

```bash
docker run --cap-add=IPC_LOCK -e 'VAULT_LOCAL_CONFIG={"storage": {"file": {"path": "/vault/file"}}, "listener": [{"tcp": { "address": "0.0.0.0:8200", "tls_disable": true}}], "default_lease_ttl": "168h", "max_lease_ttl": "720h", "ui": true}' -p 8200:8200 vault:1.13.3 server
```

- Se rendre sur l'UI localhost:8200, se connecter avec le root-token puis créer un user/mot de passe (on aurait pu utiliser d'autres méthode d'authentification type approle, tls..)
- Créer une ACL qui permet de lire un secret au chemin containers/mon-secret
- Créer un user/mot de passe et associer l'ACL créer auparavant
- Créer un secret dans le path containers/mon-secret
- Lancer un container alpine et avec l'outil curl faire une authentification au Vault, puis récuperer le secret

**5. Trouver la clé Contexte** : Un développeur pas très habile (moi) à builder une image docker. Celle-ci comporte une clé API utilisé dans une requête assez suspecte... En tant qu'attaquant, trouver la clé API dans l'image docker

```bash
docker pull ety92/demo:v1
```
- Comment le développeur aurait dû faire pour éviter ceci ?

**6. Pour les plus rapide ; Rootless mode**

- Installer docker sur votre machine Ubuntu/Debian en mode rootless
- Lancer un container docker nginx et accéder au container dans votre navigateur sur le port 80. Est-ce que ça fonctionne?

```bash
docker run -d --name nginx -p 80:80 nginx
```
- Si cela ne fonctionne pas comment remédier au problème ?
- Lancer un security-bench docker comme le .3. Remarquez-vous des différences par rapport au premier security-bench ?