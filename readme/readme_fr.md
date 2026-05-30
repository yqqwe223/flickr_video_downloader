# Flickr Video Downloader - Documentation du projet

> Outil léger pour extraire et sauvegarder des vidéos publiques depuis Flickr. Développé pour un usage personnel et l'étude technique. Merci de respecter les conditions d'utilisation de la plateforme et la législation en vigueur dans votre pays.

🔗 Démo en ligne : [https://twittervideodownloaderx.com/flickr_downloader_fr](https://twittervideodownloaderx.com/flickr_downloader_fr)

---

## 📌 Pourquoi j'ai codé ça ?

Soyons honnêtes deux minutes : quand on traîne sur Flickr pour chercher de l'inspiration visuelle, on tombe parfois sur des pépites en vidéo. Des timelapses de paysages à couper le souffle, des behind-the-scenes de shootings photo, des mini-vlogs de voyage qui racontent plus qu'une image... Le problème ? Aucun bouton téléchargement à l'horizon. On bookmark en se disant "je regarderai plus tard" et on oublie jusqu'au prochain siècle. Enregistrer l'écran ? La qualité prend un coup, ça prend un temps fou, et le fichier final pèse trois tonnes.

Du coup, je me suis dit : "Et si je me faisais un truc simple, juste pour moi, qui fait le boulot sans chichis ?". C'est comme ça que ce projet a vu le jour. Pas d'usine à gaz, pas d'interface surchargée, juste l'essentiel : coller un lien → récupérer la vidéo. Le code est propre, les dépendances sont au strict minimum, et l'installation ne devrait pas vous arracher les cheveux. Si ça peut vous dépanner aussi, tant mieux. Si vous voulez fouiller dans le code ou proposer des améliorations, c'est encore mieux.

---

## ✨ Ce que ça fait, concrètement

- ✅ Parse les liens de vidéos publiques Flickr (albums embed, page de profil, liens partagés, etc.)
- ✅ Détecte auto les différentes qualités disponibles et privilégie la qualité originale quand c'est possible
- ✅ Tout le traitement lourd côté backend ; le frontend n'est qu'un formulaire minimal → chargement rapide, zéro script de tracking
- ✅ CORS déjà configuré pour une intégration fluide avec d'autres projets frontend
- ✅ Logs basiques des requêtes + statut du parse pour debugger plus efficacement
- ✅ Mécanisme de limitation de fréquence intégré pour réduire les risques de blocage par Flickr

---

## 🛠️ Stack technique

- Langage : Python 3.9+
- Framework : Django 4.x (léger, modulable, parfait pour ce genre de projet)
- Client HTTP : requests en principal, httpx en option pour le mode asynchrone
- Parsing : regex + BeautifulSoup (uniquement quand c'est vraiment nécessaire)
- Déploiement : Gunicorn + Nginx recommandés ; support Docker pour ceux qui veulent aller vite
- Gestion de config : variables d'environnement + settings.py découpé par environnement (dev/prod)

J'ai volontairement limité les dépendances externes pour éviter les conflits de versions et rendre l'installation la plus fluide possible.

---

## 🚀 Démarrage rapide

### Option 1 : Depuis le code source

```bash
# 1. Cloner le repo
git clone https://github.com/yourname/flickr-downloader.git
cd flickr-downloader

# 2. Créer un venv et installer les dépendances
python -m venv venv
source venv/bin/activate  # Windows : venv\Scripts\activate
pip install -r requirements.txt

# 3. Copier et personnaliser la config
cp .env.example .env
# Éditer .env : renseigner SECRET_KEY, ALLOWED_HOSTS, etc.

# 4. Lancer les migrations (si vous souhaitez logger en base)
python manage.py migrate

# 5. Démarrer le serveur de dev
python manage.py runserver 0.0.0.0:8000

# Pour la prod, passez par Gunicorn :
gunicorn core.wsgi:application --bind 0.0.0.0:8000 --workers 3
```

### Option 2 : Docker (pour les pressés)

```bash
# Build de l'image
docker build -t flickr-dl:latest .

# Lancer le container
docker run -d -p 8000:8000 --env-file .env flickr-dl:latest
```

> 💡 Petit conseil d'ami : en production, configurez toujours Nginx en reverse proxy et forcez le HTTPS. La sécurité, ce n'est pas un luxe.

---

## 📋 Exemple d'utilisation de l'API

```bash
# Test rapide avec curl
curl -X POST https://votre-domaine.com/api/parse \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.flickr.com/photos/xxx/video/12345678"}'

# Réponse JSON typique
{
  "code": 200,
  "data": {
    "title": "Timelapse coucher de soleil en Provence",
    "author": "Marie Photographe",
    "video_url": "https://cdn.flickr.com/video/xxx.mp4",
    "thumbnail": "https://cdn.flickr.com/thumb/xxx.jpg",
    "duration": "04:12"
  }
}
```

Côté interface web, c'est volontairement spartiate : coller le lien → cliquer sur "Parser" → obtenir le bouton de téléchargement. Trois clics, c'est tout. Pas de fioritures, pas de distractions.

---

## ⚠️ À lire absolument avant d'utiliser

1. Cet outil ne fonctionne qu'avec des vidéos **publiques** sur Flickr. Les contenus nécessitant une connexion ou marqués privés ne seront pas traités ;
2. Interdiction formelle d'utiliser ce script pour du scraping massif, de la redistribution commerciale, ou toute action contraire aux Conditions d'Utilisation de Flickr ;
3. Les droits sur les vidéos appartiennent à leurs créateurs ou à la plateforme. Utilisez le contenu téléchargé uniquement pour un usage personnel, de la recherche, ou des citations raisonnables ;
4. Envoyer trop de requêtes en peu de temps peut déclencher des mécanismes de protection. Le code inclut un système de délai basique – activez-le, ça vous évitera des ennuis ;
5. Ce projet NE stocke aucun fichier vidéo. Les liens retournés pointent directement vers le CDN officiel de Flickr et peuvent expirer à tout moment selon les politiques de la plateforme ;
6. Le développeur décline toute responsabilité légale ou technique concernant les problèmes liés à l'utilisation de cet outil. Vous l'utilisez en toute connaissance de cause.

---

## 🤝 Envie de contribuer ?

Signalements de bugs, suggestions d'améliorations, pull requests : tout est le bienvenu. Juste quelques petites règles avant d'envoyer :

- Décrivez clairement comment reproduire le souci, avec le lien concerné et le message d'erreur exact ;
- Les nouvelles fonctionnalités doivent avoir une utilité générale – évitez les ajustements trop spécifiques à votre cas personnel ;
- Respectez le style de code existant (PEP8 + .editorconfig du projet) pour garder une base cohérente ;
- Si vous touchez à la logique de parsing, pensez à ajouter des tests pour éviter de casser ce qui fonctionnait avant.

Pour les petites corrections, une PR directe suffit. Pour les changements plus importants, ouvrez d'abord une Issue pour qu'on puisse discuter de l'approche et gagner du temps ensemble.

---

## 📄 Licence

MIT License ©   
Vous pouvez utiliser, modifier et redistribuer ce code librement, à condition de conserver l'attribution à l'auteur original. Pour un usage commercial, c'est à vous de vérifier la conformité avec les lois et réglementations applicables.

---

> 🌱 Pour être totalement transparent : j'ai développé cet outil avant tout pour mes propres besoins, donc il n'est pas parfait et ne prétend pas l'être. Si vous tombez sur des erreurs "parse failed" ou des liens qui ne fonctionnent plus, c'est très probablement que Flickr a modifié sa structure HTML ou renforcé ses protections anti-bot. Ouvrez une Issue et je jetterai un œil dès que possible – ou si vous êtes à l'aise avec le code, n'hésitez pas à proposer une correction vous-même. Parfois, le meilleur moyen de vraiment comprendre comment un système fonctionne, c'est de mettre les mains dans le cambouis.  
>   
> Un dernier mot, et je suis sincère : respectez le travail des créateurs de contenu et utilisez ce genre d'outils avec discernement. C'est la seule façon de préserver ce type de projets utiles et de les maintenir accessibles pour tous. Merci d'avoir pris le temps de lire ce README – j'espère que ce petit outil vous fera gagner du temps ou vous aidera dans vos recherches ! 🙏✨

---

## 🔧 Dépannage rapide

- **Le parse plante soudainement** : Flickr a probablement mis à jour son HTML. Consultez les Issues récentes ou tirez les dernières modifications du code.
- **Erreurs 403/429** : Vous avez dépassé les limites de requêtes. Activez le délai entre les appels dans la config ou réduisez le nombre de requêtes simultanées.
- **Le lien vidéo expire trop vite** : C'est normal – les URLs du CDN Flickr ont une durée de vie courte. Téléchargez rapidement après le parse.
- **Docker refuse de démarrer** : Vérifiez la syntaxe de votre fichier .env et assurez-vous que le port 8000 est libre.

---

## 📦 Structure du projet (simplifiée)

```
flickr-downloader/
├── core/               # Config Django principale
├── parser/             # Logique d'extraction vidéo
├── static/             # Assets frontend minimalistes
├── templates/          # Templates HTML
├── .env.example        # Modèle de configuration
├── requirements.txt    # Dépendances Python
├── Dockerfile          # Instructions de build container
└── README.md           # Vous êtes ici
```

Garder les choses simples, c'est garder les choses maintenables. C'est toute la philosophie du projet.

---

> Dernier petit mot : si cet outil vous a rendu service, cool. Si vous l'avez amélioré, encore mieux. Partagez ce que vous apprenez, restez curieux, et bon code à tous. 🚀