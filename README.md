# MyGoldEX — Android 0.2.0

Panneau flottant noir et or pour consulter une analyse CSV au-dessus des applications.
Le bouton **Afficher sur MetaTrader 5** détecte MT5 officiel, guide l'autorisation
Android, affiche le panneau compact puis ouvre MT5. Position et transparence sont
mémorisées ; le panneau est déplaçable, réductible et fermable.

## Télécharger et essayer

- [APK MyGoldEX 0.2.0](https://github.com/Code9797/MyGoldEX-/actions/runs/34964172739/artifacts/10394259360)
- [Sources Android et Gradle Wrapper](https://github.com/Code9797/MyGoldEX-/actions/runs/34964172739/artifacts/10394581256)
- [Résultats de compilation et tests](https://github.com/Code9797/MyGoldEX-/actions/runs/34964172739)
- [Captures et journaux du test Android/MT5](https://github.com/Code9797/MyGoldEX-/actions/runs/34964172739/artifacts/10394965517)

Les artefacts GitHub sont conservés 14 jours et peuvent demander une connexion GitHub.
Extraire `MyGoldEX.apk` de l'archive et l'installer sur Android 8 ou supérieur.
Installer MT5 officiel dans le même profil Android.
Dans MyGoldEX, appuyer sur **Afficher sur MetaTrader 5**, autoriser l'affichage
par-dessus les autres applications, puis revenir dans MyGoldEX.
Le bouton **Ouvrir** déplie l'analyse ; tirer le titre déplace le panneau.

APK debug pour essais : la signature peut changer entre deux compilations GitHub.
Une installation précédente peut donc empêcher la mise à jour sur place.
Une clé de signature stable est nécessaire pour une distribution durable.

## Vérification du 15 septembre 2026

Environnement : émulateur Android 14, Pixel 2, x86_64, image Google APIs.
MT5 officiel : paquet `net.metaquotes.metatrader5`, version `500.6160` (build 6160),
téléchargé depuis le lien du [site MetaQuotes](https://www.metatrader5.com/en/mobile-trading/android).
URL et SHA-256 du téléchargement conservés dans les preuves.

| Contrôle | Résultat |
| --- | --- |
| Compilation MyGoldEX et vérification de signature APK | Réussi |
| Tests unitaires du moteur | Réussis |
| Ouverture, réduction, déplacement, rotation et fermeture du panneau sur l'accueil Android | Réussi |
| Refus d'autorisation puis retour des réglages avec autorisation | Étapes franchies dans le test MT5 |
| Lancement de MT5 après affichage du panneau | Étape franchie |
| Affichage utilisable sur un graphique MT5 | Non validé : le processus MT5 redémarre dans l'émulateur |
| Vérification sur téléphone physique | Non effectuée |

Le workflow est marqué en échec parce que le test de stabilité MT5 échoue.
L'APK est bien généré et téléchargeable avant cette étape. La capture MT5 montre
le panneau sur un écran de démarrage blanc, pas sur un graphique fonctionnel.
Ce résultat ne prouve pas une incompatibilité sur téléphone ; un essai réel reste
nécessaire. Aucun compte MT5 créé et aucun ordre envoyé pendant les tests.

## Analyse et limites

Import de 60 à 5 000 bougies CSV : `time,open,high,low,close`, dates Unix UTC,
décimales avec un point, bougies clôturées et chronologiques.
Calculs locaux : EMA 20/50, ATR de Wilder 14, plage 20 bougies, FVG et balayage potentiel.
Ces calculs ne sont pas des signaux validés.

Cette version n'a pas d'IA connectée, de capture d'écran, de lecture des cours MT5
ou de passage d'ordres. Le panneau est une superposition Android indépendante.
Certaines applications ou certains écrans protégés peuvent masquer les superpositions.

## Construire

Les sources sont embarquées dans `generate_project.py` :

```sh
python3 generate_project.py
cd generated/MyGoldEX
gradle wrapper --gradle-version 8.11.1 --distribution-type bin
bash ./gradlew :app:testDebugUnitTest :app:assembleDebug
```

Prérequis : JDK 17, Android SDK 35, Build Tools 35.0.0 et Gradle 8.11.1.
Le générateur refuse d'écraser un dossier existant.
Le workflow `.github/workflows/android.yml` automatise la compilation et les tests.
Il peut être relancé depuis **Actions → Generer APK MyGoldEX → Run workflow**.
