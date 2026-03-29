# TP : Découverte de Strapi, un CMS Headless

> Projet réalisé dans le cadre du TP - Spécialité Web JS / BUT3
> 
> IUT Charlemagne - Université de Lorraine

- Réalisé par : **[Vivien Herrmann](https://github.com/Vivienhrm)**

![Nuxt](https://img.shields.io/badge/Nuxt_3-00C58E?style=flat-square&logo=nuxtdotjs&logoColor=white)
![Vue](https://img.shields.io/badge/Vue.js-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![Strapi](https://img.shields.io/badge/Strapi_v5-2E7EEA?style=flat-square&logo=strapi&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)

Bienvenue sur ce projet ! L'objectif ici était d'explorer l'architecture Headless CMS en séparant le back-office du front-end dans le cadre d'un blog Tech. 

Il s'agissait donc de créer :
- D'une API de gestion de contenu (Back-End) propulsée par **Strapi** et une base de données locale **SQLite**.
- D'une interface de consultation (Front-End) développée avec **Nuxt 3**.



## Pré-requis

- Node.js
- Npm (ou yarn / pnpm)

## Installation et démarrage

Le dépôt contient deux sous-dossiers distincts : `mon-blog-backend` (le serveur Strapi) et `mon-blog-frontend` (le client Nuxt). **Il est nécessaire de lancer les deux en même temps dans deux terminaux séparés.**

1. **Cloner le projet**
```bash
git clone https://github.com/Vivienhrm/tp-strapi-nuxt.git
cd tp-strapi-nuxt
```

---

### Lancer le Back-End (Strapi)

Le dossier Strapi contient déjà la base de données SQLite (`data.db`) avec les schémas des collections (Author, Category, Article) et les données de test associées.

Dans un **premier terminal** :
```bash
cd mon-blog-backend
npm install
npm run develop
```
*Le **fichier .env** doit être créé à partir du **.env.example** et doit contenir les clés d'API et les clés de chiffrement. Le port de Strapi a été configuré sur le port `1338` dans le `.env`.*

- Panel d'administration Strapi : `http://localhost:1338/admin`
- Endpoint API des articles : `http://localhost:1338/api/articles?populate=*`

*Vous pourrez ensuite **ajouter, modifier ou supprimer** des articles, des catégories et des auteurs depuis l'interface d'administration. Vous trouverez les images utilisés pour mes blogs dans le dossier `mon-blog-frontend/public/images`.*


---

### Lancer le Front-End (Nuxt 3)

Le projet Nuxt interroge l'API Strapi locale lors du rendu côté serveur pour afficher les articles avec toutes leurs métadonnées (auteurs, catégories, images).

Dans un **deuxième terminal** (en laissant tourner Strapi) :
```bash
cd mon-blog-frontend
npm install
npm run dev
```

- Blog en ligne (l'interface publique) : `http://localhost:3000` *(ou `3001` si le 3000 est occupé).*

---

## Rendu final
> **Aperçu de l'interface d'administration Strapi :**
> ![alt text](renduAdmin.png)

> **Aperçu du Blog :**
> ![alt text](renduBlog.png)
> ![alt text](renduBlog2.png)
