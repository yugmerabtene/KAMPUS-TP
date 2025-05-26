## TP — LANCER UNE INSTANCE EC2 ET DÉPLOYER UN SERVEUR WEB APACHE

### 🎯 Objectifs pédagogiques :

* Créer une **instance EC2 gratuite** (type `t2.micro`) sous **Ubuntu Server 24.04 LTS**.
* Générer et utiliser une **clé de chiffrement (key pair)** pour accéder à l’instance.
* Installer automatiquement **Apache2** via **User Data**.
* Créer une page d’accueil HTML personnalisée dans `/var/www/html/index.html`.

---

### 📋 Étapes à suivre :

#### 🟦 1. Connexion à AWS Console

Connectez-vous à la console AWS et accédez au service **EC2**.

---

#### 🟩 2. Lancement d'une instance EC2

Cliquez sur **"Launch Instance"** et configurez les sections suivantes :

##### a. **Nom de l’instance**

* `MyApacheServer`

##### b. **AMI (Amazon Machine Image)**

* Choisissez : `Ubuntu Server 24.04 LTS (64-bit x86)`

##### c. **Type d’instance**

* `t2.micro` (gratuite si vous êtes dans le Free Tier)

##### d. **Clé de chiffrement**

* Créez une nouvelle paire de clés RSA : `apache-keypair`
* Téléchargez bien le fichier `.pem` pour vous y connecter plus tard

---

#### 🟨 3. Paramètres réseau

* **VPC** : laissez par défaut
* **Auto-assign public IP** : activé
* **Security Group** :

  * Ouvrir les ports suivants :

    * `22 (SSH)` : depuis votre IP
    * `80 (HTTP)` : depuis `0.0.0.0/0`

---

#### 🟫 4. Stockage

* Volume : `8 GiB`
* Type : `gp3`
* Laissez le disque root par défaut, non chiffré

---

#### 🟥 5. Section “Advanced Details” ➜ **User Data**

Collez ce **script shell** dans le champ `User Data` :

```bash
#!/bin/bash
apt update -y
apt install apache2 -y
echo "<h1>Bienvenue sur mon premier serveur Apache déployé via EC2 ! 🚀</h1>" > /var/www/html/index.html
systemctl start apache2
systemctl enable apache2
```

---

#### 🟦 6. Lancer l’instance

Cliquez sur **"Launch instance"**.

---

### 🖥️ 7. Tester depuis un navigateur

Une fois l’instance **en état “running”**, cliquez sur l’**adresse IPv4 publique**, puis visitez-la dans votre navigateur :

```
http://<votre-ip-public-ec2>
```

🔍 Vous devriez voir le message HTML s'afficher automatiquement.

---

### ✅ Bonus

* Connectez-vous via SSH avec votre clé :

```bash
ssh -i apache-keypair.pem ubuntu@<IP-PUBLIC>
```

* Vérifiez que Apache fonctionne :

```bash
sudo systemctl status apache2
```

---

### 📌 Ce que vous aurez appris :

* Manipulation de base d’EC2 et sécurité SSH
* Utilisation de `User Data` pour automatiser les déploiements
* Installation de logiciels et configuration web en mode cloud

---
