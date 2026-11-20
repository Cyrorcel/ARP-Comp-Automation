# ARP-Opto-comp (VST3 / AU, macOS)

Compresseur opto à tubes, interface dessinée entièrement en code (aucune image externe).

## Compiler (Mac, Apple Silicon ou Intel)

Xcode complet n'est PAS nécessaire. Il faut seulement les outils en ligne de commande
(`xcode-select --install`) et CMake (`brew install cmake`).

    cd ARPOptoComp
    cmake -B build -DCMAKE_BUILD_TYPE=Release
    cmake --build build -j 4

## Sans rien installer : compilation dans le cloud (GitHub Actions, gratuit)

1. Créez un dépôt GitHub (privé ou public) et envoyez-y le contenu de ce dossier
   (y compris `.github/workflows/build.yml`).
2. Onglet "Actions" > "Build VST3 (macOS)" > "Run workflow".
3. Une fois terminé, téléchargez l'artefact `ARP-Opto-comp-VST3-macOS`, décompressez-le,
   puis placez `ARP-Opto-comp.vst3` dans `~/Library/Audio/Plug-Ins/VST3/`.
4. Retirez la quarantaine : `xattr -cr "$HOME/Library/Audio/Plug-Ins/VST3/ARP-Opto-comp.vst3"`.

Le plugin est copié automatiquement dans :

- `~/Library/Audio/Plug-Ins/VST3/ARP-Opto-comp.vst3`
- `~/Library/Audio/Plug-Ins/Components/ARP-Opto-comp.component`

## Reaper

Options > Preferences > Plug-ins > VST > Re-scan (ou "Clear cache and re-scan"). Puis FX > "ARP-Opto-comp".

Si macOS bloque le plugin (non signé) : `xattr -cr "~/Library/Audio/Plug-Ins/VST3/ARP-Opto-comp.vst3"`.

## Paramètres

Input, Threshold, Ratio, Tube (saturation), Attack, Release (programme-dépendant : plus lent quand la réduction est profonde), Makeup, Mix (compression parallèle).

### Makeup automatique

Quand on baisse le Threshold, le bouton Makeup monte tout seul pour compenser la perte de niveau :
Threshold 0 dB -> Makeup 0 dB ; Threshold -40 dB -> Makeup +9 dB ; progression linéaire entre les deux
(0,225 dB de makeup par dB de seuil, ex. -12 dB -> +2,7 dB). Le Makeup reste réglable à la main ensuite ;
il est recalculé à chaque nouveau mouvement du Threshold. Voir `autoMakeupDb()` dans `CompCore.h`.
