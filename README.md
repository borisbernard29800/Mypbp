# MyBikePlanner

Application Android **autonome** pour suivre une sortie longue distance (Paris-Brest-Paris, BRM, sortie libre) par rapport au planning prévu. Aucune donnée n'est envoyée à un serveur : tout est stocké sur le téléphone (IndexedDB). Seule la météo est téléchargée (Open-Meteo, sans clé).

## Installer
APK le plus récent : https://github.com/borisbernard29800/Mypbp/releases/download/latest/mybikeplanner.apk
Copier sur le téléphone, ouvrir, autoriser les « sources inconnues ». Une mise à jour s'installe **par-dessus** l'ancienne version et conserve les données (même signature).

## Fonctions
- **Circuits multiples** : import de GPX, type choisi par l'utilisateur (Sortie libre, BRM, PBP) ; le délai max dépend du type (BRM 200 = 13 h 30, 300 = 20 h, 400 = 27 h, 600 = 40 h, 1000 = 75 h ; PBP = 90 h ; la limite horaire est facultative : interrupteur dans Plan, activé par défaut pour BRM et PBP, désactivé pour une sortie libre).
- **Agenda** : calendrier des sorties, planification par date avec nouveau GPX ou copie d'un circuit existant.
- **Plan** : date/heure de départ, étapes (noms modifiables, effaçables d'un geste) (km, vitesse moyenne, arrêt/sommeil), limite, tolérance GPS, projection « plan décalé » ou « rythme réel ».
- **Suivi** : position GPS à l'ouverture (pas d'arrière-plan, batterie préservée) ou saisie manuelle des passages ; avance/retard, marge, vitesse mini, prochains passages.
- **Distance jusqu'à la prochaine étape** : affichée dans Plan (+ X km depuis la précédente), Accueil (« dans X km ») et mode course (étape suivante et liste).
- **Route** : feuille de route prévu / réel / écart ; toucher une étape pour saisir, corriger (autant de fois que voulu) ou supprimer son heure de passage.
- **Noter un passage depuis la carte** : toucher le tracé puis « Noter un passage ici » (km prérempli).
- **Carte météo** (style Epic Ride Weather) : flèches de vent le long du tracé (face/dos/côté), trajet coloré par température, halo de pluie, courbes (température et ressentie, précipitations et nuages, vent et rafales, altitude), météo à l'heure de passage ou à heure fixe, prévisions sur 7 jours.
- **Profil d'altitude** sous la carte : pente en couleur, position, étapes ; curseur et lecture ▶ du parcours (la carte suit), avec heure de passage, pente et D+ restant.
- **Mode course** : affichage épuré (Course / Étapes / Météo / Pointage), gros chiffres, un seul bouton.
- **Réglages (⚙)** : thème Automatique (soleil) / Téléphone / Jour / Nuit ; **Sauvegarde** : export (partage Android) et import (fusion) de tous les circuits.
- **Couleur du trajet** : Pente (par défaut : bleu descente, vert < 3 %, jaune 3–6 %, orange 6–9 %, rouge > 9 %) ou Température.
- **Multilingue** : français, English, Español, Deutsch ; choix au premier lancement (app vierge) puis dans ⚙ Réglages → Langue. Traduction à l'exécution : dictionnaire `TR` + règles `RX` + aide `HELP` dans `index.html` (textes sources en français).
- **Aide (?)** : manuel intégré à l'app (4 langues).
- Accueil enrichi : écran d’ouverture avec photo (splash.jpg), menu latéral (☰), météo de la position actuelle en tête d’accueil (Open-Meteo, relevée à l’ouverture, dernière valeur conservée hors connexion) ; menu ☰ accessible partout (accueil et circuit) ; boutons ⚙ et ? retirés des en-têtes (dans le menu). Pas de connexion directe à Strava/Komoot/Garmin (API fermées ou nécessitant un serveur) : import par export GPX.

## Structure
- `www/index.html` : toute l'application (HTML/JS/CSS). Le bloc `// ==CORE==` … `// ==/CORE==` contient la logique pure (testable sous Node).
- `www/lib/` : Leaflet (embarqué, fonctionne hors ligne). `www/sw.js`, `manifest.webmanifest` : usage navigateur.
- `android/` : projet Capacitor (appId `fr.mypbp.app`). Plugins : Filesystem et Share (export).
- `.github/workflows/apk.yml` : compilation de l'APK sur GitHub Actions et publication dans la release `latest`.

## Compiler
- **GitHub** : chaque `git push` sur `main` lance « Build APK » (~5 min) ; l'APK est publié dans la release `latest` (`mybikeplanner.apk`).
- **Local** : `npm ci && npx cap sync android`, puis ouvrir `android/` dans Android Studio (Build > Build APK).
- Clé de signature fixe commitée (`android/app/mybikeplanner.keystore`, usage personnel) pour garder la même signature d'une version à l'autre.

## Données et limites
- Les prévisions Open-Meteo ne couvrent que 7 jours.
- La projection est indicative, ce n'est pas un chronométrage officiel.
- Désinstaller l'app efface les données : exporter avant.

## Règle de maintenance
Toute évolution de fonction doit être reportée dans : l'aide intégrée (`HELP` / `renderHelp` dans `index.html`, dans les 4 langues), les **traductions** (tout nouveau texte français doit avoir son entrée dans `TR`/`RX` pour EN/ES/DE), ce README.
