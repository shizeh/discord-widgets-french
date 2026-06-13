# Guide Discord Widget pour Débutants

Ce guide explique comment j'ai créé un widget Discord personnalisé à partir de zéro.

> [!NOTE]
> Ce guide nécessite **Visual Studio Code**.
>
> Node.js n'est **pas nécessaire** pour cette méthode, car nous utilisons uniquement le terminal intégré inclus dans Visual Studio Code.

## 1. Créer une application Discord

1. Ouvrez le [Discord Developer Portal](https://discord.com/developers/home).
2. Allez dans **Applications**.
3. Cliquez sur **New Application**.
4. Donnez à votre application un nom correspondant à votre widget.

Exemples :

* `Stats`
* `Steam`
* `Letterboxd`

---

## Activer le Social SDK

Avant de continuer, vous devez activer le Social SDK pour votre application.

1. Retournez sur le Discord Developer Portal.
2. Sélectionnez votre application.
3. Naviguez vers :

```text
Games
→ Social SDK
```

4. Remplissez le formulaire requis.
5. Acceptez les conditions si elles vous sont proposées.
6. Soumettez le formulaire.

Cette étape est nécessaire pour accéder aux fonctionnalités du Social SDK utilisées par les Widgets Discord.

Si vous ignorez cette étape, vous risquez de rencontrer des problèmes plus tard lors de la configuration d'OAuth2, comme des scopes manquants ou des erreurs d'autorisation.

Une fois le formulaire soumis et accepté, vous pouvez passer à l'étape suivante du tutoriel.

## 2. Activer l'éditeur de widgets

L'éditeur de widgets est caché par défaut.

1. Téléchargez le fichier `widget-access.txt` depuis le dépôt.
2. Ouvrez la page listant toutes vos applications Discord.
3. Appuyez sur `F12`.
4. Ouvrez l'onglet **Console**.
5. Collez le contenu de `widget-access.txt`.
6. Appuyez sur `Entrée`.

Si l'opération réussit, Discord débloquera l'éditeur de widgets.

---

## 3. Ouvrir l'éditeur de widgets

1. Retournez sur votre application nouvellement créée.
2. Cliquez sur le menu **trois points**.
3. Ouvrez **Games**.
4. Cliquez sur **Widget**.

Vous pouvez maintenant commencer à créer votre widget.

---

## 4. Concevoir votre widget

Personnalisez le widget comme vous le souhaitez.

Exemples :

* Statistiques Steam
* Statistiques Last.fm
* Profil Letterboxd
* Succès de jeux vidéo
* Statistiques personnelles

Vous pouvez personnaliser :

* Les images
* La mise en page
* Les libellés
* Les champs de données utilisateur

Une fois satisfait du résultat, enregistrez vos modifications.

---

## 5. Récupérer les informations nécessaires

Avant de continuer, récupérez les informations suivantes.

### Bot Token

Naviguez vers :

```text
Bot
```

Copiez votre Bot Token.

### Application ID

Naviguez vers :

```text
General Information
```

Copiez votre Application ID.

### Discord User ID

Activez le mode développeur :

```text
User Settings → Advanced → Developer Mode
```

Puis faites un clic droit sur votre profil et sélectionnez :

```text
Copy User ID
```

---

## 6. Sauvegarder toutes les informations

Créez un fichier texte et stockez :

```text
Application ID:
XXXXXXXXXXXXXXX

Bot Token:
XXXXXXXXXXXXXXX

User ID:
XXXXXXXXXXXXXXX
```

Assurez-vous que tout est clairement identifié.

Vous aurez besoin de ces valeurs plus tard dans le processus de configuration.

## 7. Configurer OAuth2

Retournez sur le Discord Developer Portal et ouvrez la section **OAuth2** de votre application.

### Créer une URL de redirection

Sous **Redirects**, ajoutez l'URL suivante :

```text
https://discord.com
```

Cliquez sur **Save Changes**.

---

### Générer l'URL OAuth2

Faites défiler jusqu'à la section **OAuth2 URL Generator**.

Sélectionnez les scopes suivants :

```text
openid
sdk.social_layer
```

Après avoir sélectionné ces scopes, une nouvelle option appelée **Select Redirect URL** devrait apparaître.

Choisissez :

```text
https://discord.com
```

dans le menu déroulant.

---

### Modifier l'URL générée

Discord générera une URL d'autorisation similaire à :

```text
https://discord.com/oauth2/authorize?client_id=YOUR_CLIENT_ID&response_type=code&redirect_uri=...
```

Copiez cette URL.

Avant de l'ouvrir, remplacez :

```text
response_type=code
```

par :

```text
response_type=token
```

L'URL doit désormais contenir :

```text
response_type=token
```

---

### Autoriser l'application

Collez l'URL modifiée dans votre navigateur et ouvrez-la.

Discord vous demandera d'autoriser l'application.

Acceptez toutes les autorisations demandées.

Cette étape permet à l'application de se connecter à votre compte Discord et d'associer par la suite votre widget personnalisé à votre profil.

---

### Vérifier l'autorisation

Ouvrez Discord et naviguez vers :

```text
User Settings
→ Authorized Apps
```

Recherchez votre application.

L'application devrait apparaître avec **7 autorisations activées**.

Si l'application est présente et que toutes les autorisations sont accordées, vous êtes prêt à poursuivre le tutoriel.

## 8. Publier votre widget

Une fois votre widget terminé, retournez sur le Developer Portal et publiez votre projet.

Après la publication :

1. Ouvrez **Visual Studio Code**.
2. Cliquez sur **Terminal** dans le menu supérieur.
3. Sélectionnez **New Terminal**.

Vous devez maintenant exécuter une commande pour créer l'identité de votre application.

### Important

Avant d'exécuter la commande ci-dessous, remplacez :

* `ApplicationID` par votre Application ID
* `UserID` par votre Discord User ID
* `BOT_TOKEN` par votre Bot Token
* `Shizeh` par votre nom d'utilisateur Discord

Exemple :

```text
Shizeh → Votre nom d'utilisateur Discord
ApplicationID → Votre Application ID
UserID → Votre Discord User ID
BOT_TOKEN → Votre Bot Token
```

### Commande

```powershell
Invoke-RestMethod -Uri https://discord.com/api/v9/applications/ApplicationID/users/UserID/identities/0/profile -Method PATCH -Headers @{"Content-Type"="application/json"; "Authorization"="Bot BOT_TOKEN";"User-Agent"="DiscordBot (https://github.com/discord/discord-api-docs, 1.0.0)"} -Body '{"username":"Shizeh","data":{"dynamic":[{"type":1,"name":"full_name","value":"Shizeh"}]}}'
```

Assurez-vous de remplacer `ApplicationID`, `UserID`, `BOT_TOKEN` et `Shizeh` par vos propres valeurs avant d'exécuter la commande.

Collez la commande dans le terminal puis appuyez sur Entrée.

Si aucune erreur n'apparaît, l'identité a été créée avec succès.

### Dernière étape

Ouvrez Discord puis allez dans :

```text
User Settings
→ Profiles
→ Add Widget
```

Votre widget devrait maintenant apparaître dans la liste des widgets.

Si ce n'est pas le cas, essayez de redémarrer Discord puis d'ajouter le widget à nouveau.

Si vous avez correctement suivi toutes les étapes, le widget devrait apparaître.

Ajoutez-le à votre profil puis enregistrez vos modifications.

Le tutoriel est maintenant terminé.

> [!TIP]
> Si vous avez des questions ou rencontrez des problèmes en suivant ce guide, n'hésitez pas à me contacter.
>
> J'essaierai d'aider autant que possible et de mettre à jour le dépôt en fonction des retours et des problèmes fréquemment rencontrés.

---

## Crédits

Ce guide est basé sur des informations recueillies à partir de différents guides communautaires, tutoriels et expérimentations personnelles.

L'objectif de ce dépôt n'est pas de revendiquer la paternité des découvertes originales, mais de fournir une explication plus simple et plus accessible aux débutants.

Un grand merci à toutes les personnes ayant contribué par leurs informations, tutoriels et recherches concernant les Widgets Discord.

---

## Ressources

Liens utiles :

* Guide original : [Chloe Cinders](https://chloecinders.com/blog/discord-widgets)
* Discord Developer Portal : [Discord Developer Portal](https://discord.com/developers/home)
* Documentation supplémentaire : `LINK_HERE`

---

## Auteur

Créé par **Baptiste (Shizeh)**

GitHub : [Shizeh](https://github.com/shizeh)
