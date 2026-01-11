# Research Changelog

Voici ce qu'on a essayé et pourquoi on l'utilise ou non.

## Dinov3

On a finetuné le modèle avec la même procédure et le même dataset que dinov2 mais les résultats ne sont pas concluants.

- Loss: 9.0573
- F1 Micro: 0.6189
- F1 Macro: 0.5821
- Accuracy: 0.0065

[Github](https://github.com/SeatizenDOI/DinoVdeau/tree/feature/dinov3)


## Underwater color correction

Pour contrer l'effet bleu sur les orthophotos, on a voulu appliquer un script de correction de couleur mais les coraux apparaissait trop rouge.

Il faudrait mieux délimiter les zonees pour appliquer l'algo car si deux zones ont trop de différence dans la profondeur alors la zone la plus proche de la caméra va ressortir trop rouge.

[Code Originel](https://github.com/nikolajbech/underwater-image-color-correction)
[Code en Python](https://github.com/SeatizenDOI/the-point-is-the-mask/blob/master/src/utils/underwater_correction.py)


Try a depth model like

- [Midas](https://github.com/isl-org/MiDaS) => Trop vieux remplacé par DepthAnythingV2
- [ZoeDepth](https://github.com/isl-org/ZoeDepth) => Essayer de combiner pour avoir un reconstruction 3D entre plusieurs images
- [DepthAnythingV2](https://github.com/DepthAnything/Depth-Anything-V2)

- [DepthAnythingV3](https://github.com/ByteDance-Seed/Depth-Anything-3) => Moins bien que DepthAnythingV2


- [Video-Depth-Anything](https://github.com/DepthAnything/Video-Depth-Anything)

- [Sea-Thru-Impl](https://teragion.github.io/sea-thru)
=> Correction des couleurs, peut-être un potentiel mais trop long à exploiter (>10min pour une image)



## Calibmar

Outils pour obtenir une calibration de caméra sans la déformation sous marine. Le software marche super bien mais il semble qu'OpenDroneMap marche bien aussi.

Je n'ai pas plus creusé que ça.

[Discussion](https://github.com/MDSKiel/calibmar/issues/1)


## Sam 3


