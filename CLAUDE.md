# Console tablette — instructions pour Claude Code

## Le projet
Console de gestion personnelle (TikTok @1fo.utile + boutique Etsy) affichée en plein écran
sur une **Samsung Galaxy Tab A 2016 (SM-T280)** fixée comme console centrale.

- Système : **LineageOS 14.1 non officiel (Android 7.1)** + Open GApps pico. La caméra d'origine ne marche pas (on utilise Open Camera).
- Affichage : **Fully Kiosk Browser** (version gratuite), défini comme écran d'accueil.
  - Start URL : `https://tim0yes.github.io/tablette/`
  - Auto Reload on Idle : 3600 s → les changements poussés apparaissent au prochain rechargement.
- Hébergement : **GitHub Pages**, branche `main`, racine `/`. Pousser sur `main` = mise en ligne (1–2 min).
- L'utilisateur n'est pas développeur : il parle en français, veut des étapes courtes et simples, et préfère que Claude fasse le maximum lui-même.

## Fichiers
- `index.html` : toute la console (HTML + CSS + JS dans un seul fichier, pas de build).
- `.nojekyll` : désactive Jekyll sur Pages.

## Contraintes techniques
- Écran 7" 1280×800, paysage par défaut (une mise en page portrait existe via media query).
- Matériel faible (SoC Spreadtrum, 1,5 Go RAM) : **pas de backdrop-filter, pas d'animations lourdes**, pas de framework.
- WebView Chrome d'Android 7 : JS moderne OK, mais rester sobre.
- **Lancer une app** : URL `intent:#Intent;action=android.intent.action.MAIN;category=android.intent.category.LAUNCHER;package=<pkg>;S.browser_fallback_url=<url>;end` (marche avec Fully gratuit ; l'API JS `fully.*` est payante, ne pas en dépendre).
- Données (listes, statut de publication, ville météo) en `localStorage` sur la tablette — clés : `list-idees`, `list-etsy`, `pub-<date>`, `city`. Ne pas renommer ces clés sans migration.
- Météo : Open-Meteo (sans clé API), ville par défaut Paris, modifiable en touchant la météo.

## Contenu actuel de la console
- Bandeau : heure, date, météo, « Console 1fo.utile ».
- Colonne TikTok : compte à rebours avant la publication quotidienne de **18 h**, bouton « vidéo publiée », série de la semaine, raccourcis Statistiques / Commentaires (TikTok Studio web).
- Grille d'apps (objet `CONFIG.apps` en haut du script) :
  - TikTok Studio `com.ss.android.tt.creator` (vérifié)
  - TikTok `com.zhiliaoapp.musically`
  - Etsy Vendeur `com.etsy.android.soe` (**non vérifié** — à corriger si la tuile ne s'ouvre pas)
  - Open Camera `net.sourceforge.opencamera`, Play Store, Chrome, Réglages, Fichiers, « Toutes les apps ».
- Colonne Etsy : liens Commandes / Messages / Stats / Annonces (etsy.com) + listes « Idées vidéos » et « À faire Etsy ».

## Workflow de modification
1. Modifier `index.html`.
2. Vérifier le rendu à 1280×800 (ex. Playwright) avant de pousser.
3. Commit + push sur `main`. Dire à l'utilisateur de tirer l'écran vers le bas pour recharger (ou attendre l'auto-reload).

## Historique
- Premier support envisagé : iPad 2 (A1395), abandonné (ne s'allume plus).
- La version locale `file:///sdcard/console.html` existe encore sur la tablette mais n'est plus utilisée.
