# Les bases de données de Mappendium

Les bases de données livrées avec le jeu Mappendium, publiées gratuitement sous la licence
**Open Data Commons Open Database License (ODbL) 1.0** — le texte est dans [`LICENCE-ODbL-1.0.txt`](LICENCE-ODbL-1.0.txt). Vous pouvez les
copier, les republier, les adapter et vous en servir, y compris commercialement, à condition de mentionner leur source et de
partager à votre tour sous la même licence. Données cartographiques © les contributeurs d'OpenStreetMap — openstreetmap.org/copyright.

L'adresse de cette publication : https://mappendium.com/donnees

Ces fichiers sont ceux des paquets du jeu, à l'octet : `canada` (le commit `fced6e91bf7e12e4816eec0fc737eca249278030`, configuration Shipping) ; préparés le 2026-11-15T11:33:39Z.

## Les fichiers

| Fichier | Octets | SHA-256 |
|---|---|---|
| `rdl.sqlite.xz` | 1 676 816 | `a8f5a899f9892f9af9fd2d085c3bcdb1fc2cdff7b913a2be54f8e0289d528ce2` |
| `rdl-vectors.sqlite.xz` | 1 173 764 | `fa189955cbda30f9ebc72974b641cb82f1d866235f06309696ebe961b914ea1a` |
| `canada.sqlite.xz` | 57 676 076 | `6c462560d3ef702259b5d98f5c3ceca6b4e7397489394d1cc76ca535fbbc172a` |
| `canada-vectors.sqlite.xz.001` | 2 040 109 465 | `9993fdc153345af6dd96f597bb89b3fca750f90315277c7061a7835d7f720566` |
| `canada-vectors.sqlite.xz.002` | 1 980 642 795 | `d81f44f676a4ee3be5305d2b18d02f8b697f9ff2a64cdfe2c87d073c6cfc1ac3` |

Les bases, une fois recollées et décompressées :

| Base | Dans le paquet | Octets | SHA-256 |
|---|---|---|---|
| `rdl.sqlite` | `Data/rdl.sqlite` (canada) | 7 331 840 | `ab3ec6cf02139af127f771142b027d1cc77f9c09e2e60f78e1ca6fdf2c0277c2` |
| `rdl-vectors.sqlite` | `Data/rdl-vectors.sqlite` (canada) | 1 310 720 | `875f9921dd307de8551c2277be8e731ea1147c8c388c7724eae55141ae97e947` |
| `canada.sqlite` | `Data/datasets/canada/canada.sqlite` (canada) | 275 460 096 | `4d1fa8234794e91ef3ed3d59b0edeb5042f9b1b334ce7d11189cacdc2afb26db` |
| `canada-vectors.sqlite` | `Data/datasets/canada/vectors.sqlite` (canada) | 4 518 535 168 | `d4786148b38c4d4c16c3dc520b129e71a2ee2e755ce643964b8cc260200477e0` |

## Recoller et vérifier

Une base trop grande pour un seul fichier est publiée en parties (`.001`, `.002`…) d'un même flux xz : on les concatène dans
l'ordre, puis on décompresse (xz, ou 7-Zip sous Windows).

```
sha256sum -c SHA256SUMS
xz -d -k rdl.sqlite.xz
xz -d -k rdl-vectors.sqlite.xz
xz -d -k canada.sqlite.xz
cat canada-vectors.sqlite.xz.0* > canada-vectors.sqlite.xz        # Windows : copy /b canada-vectors.sqlite.xz.001+canada-vectors.sqlite.xz.002 canada-vectors.sqlite.xz
xz -d canada-vectors.sqlite.xz
sha256sum -c SHA256SUMS-bases
```

## La méthode

Les bases `*.sqlite` de la hiérarchie administrative portent les limites et les lieux d'OpenStreetMap, rangés dans la hiérarchie
du jeu (GeoNames, Natural Earth, le géoschéma M49). Les magasins de vecteurs (`*-vectors.sqlite`) portent, par tuile de la grille
du jeu, les routes, l'eau, l'occupation du sol, les bâtiments, le stationnement, l'électricité et la signalisation
d'OpenStreetMap ; les empreintes de bâtiments y sont fusionnées avec celles de Microsoft et de Ressources naturelles Canada,
dédoublonnées, et les hauteurs LiDAR de Ressources naturelles Canada reportées sur des bâtiments d'OpenStreetMap ; au Québec, les
parcelles agricoles de la BDPPAD sont fusionnées à l'occupation du sol. Chaque base porte la licence dans sa table `meta`.

Le jeu en tire, sans les publier ici, des œuvres produites : les coquilles de bâtiments (cuites depuis les magasins de vecteurs),
l'image du socle (l'occupation du sol d'OpenStreetMap peinte sur ESA WorldCover) et, là où il l'est, le relief conditionné (les
tuiles Copernicus GLO-30 creusées le long des cours d'eau ou aplanies sous les routes d'OpenStreetMap) ; l'avis ci-dessous dit,
pour chaque jeu de tuiles de relief, s'il l'est ou s'il est Copernicus seul.

## Les sources, la date des extraits, les contenus ajoutés et leurs licences

L'avis livré avec le jeu (`Data/AVIS-DONNEES.txt`), tel quel :

```
AVIS SUR LES DONNÉES — Mappendium
==================================

Les bases de données livrées avec le jeu sont des bases dérivées d'OpenStreetMap. Elles sont mises à disposition
sous la licence Open Data Commons Open Database License (ODbL) 1.0, et seulement sous elle : vous pouvez les copier,
les republier, les adapter et vous en servir, y compris commercialement, à condition de mentionner leur source et
de partager à votre tour sous la même licence. Le texte de la licence est dans le fichier LICENCE-ODbL-1.0.txt, à côté
de cet avis, et à https://opendatacommons.org/licenses/odbl/1-0/. Chaque base le porte aussi dans sa table meta (les clés license,
license_url, attribution et data_url).

Données cartographiques © les contributeurs d'OpenStreetMap — openstreetmap.org/copyright.

Les bases sont publiées gratuitement, sous la même licence, à l'adresse : https://mappendium.com/donnees

1. Les fichiers sous ODbL (les bases dérivées)

  - Data/rdl.sqlite
  - Data/rdl-vectors.sqlite
  - Data/datasets/canada/canada.sqlite
  - Data/datasets/canada/vectors.sqlite

   Les bases *.sqlite de la hiérarchie administrative portent les limites et les lieux d'OpenStreetMap ; les magasins
   de vecteurs (vectors.sqlite, rdl-vectors.sqlite) portent les routes, l'eau, l'occupation du sol, les bâtiments,
   le stationnement, l'électricité et la signalisation d'OpenStreetMap, mêlés aux contenus ajoutés de la section 3.

2. Les fichiers produits depuis ces bases (des œuvres produites, ODbL §4.3 ; les bases ci-dessus en sont la source)

  - Data/rdl-building-shell.bin
  - Data/rdl-heightmap-2017x2017.r16
  - Data/datasets/canada/building-shell.bin
  - Data/datasets/canada/coarse-landcover.png
  - Data/TerrainTiles/ : le relief Copernicus creusé le long des cours d'eau et aplani sous les routes d'OpenStreetMap

   Les coquilles de bâtiments sont cuites depuis les magasins de vecteurs ; l'image du socle peint l'occupation du sol
   d'OpenStreetMap sur ESA WorldCover.

   Hors de l'ODbL — le relief Copernicus GLO-30 seul, rééchantillonné, sans rien d'OpenStreetMap :

  - Data/datasets/canada/TerrainTiles/

   Hors de l'ODbL — les codes ESA WorldCover seuls, sous CC BY 4.0 :

  - Data/rdl-landcover.bin
  - Data/datasets/canada/landcover.bin

3. Les sources et leurs licences

  - OpenStreetMap — ODbL 1.0 — © les contributeurs d'OpenStreetMap. Extraits Geofabrik : le Québec du 2026-06-23, les autres
    provinces et territoires du 2026-09-06.
  - Microsoft Canadian Building Footprints — ODbL 1.0 — des empreintes de bâtiments, fusionnées à celles
    d'OpenStreetMap.
  - Ressources naturelles Canada, Bâtiments extraits automatiquement — Licence du gouvernement ouvert – Canada 2.0 —
    des empreintes et leurs hauteurs LiDAR, reportées sur des bâtiments d'OpenStreetMap. Contient de l'information visée par la Licence du gouvernement ouvert – Canada.
  - BDPPAD, Base de données des parcelles et productions agricoles déclarées — CC BY 4.0 — La Financière agricole
    du Québec ; les parcelles agricoles du Québec et leurs cultures, fusionnées à l'occupation du sol.
  - GeoNames (geonames.org) — CC BY 4.0 — la hiérarchie des pays et les identifiants des municipalités.
  - Natural Earth (naturalearthdata.com) — domaine public — les contours des pays.
  - Géoschéma M49 de la Division de statistique des Nations Unies (unstats.un.org) — les grandes régions du monde.
  - ESA WorldCover 2021 — CC BY 4.0 — le socle de couverture du sol. © ESA WorldCover project 2021 / Contains modified Copernicus Sentinel data (2021) processed by ESA WorldCover consortium.
  - Copernicus GLO-30 — le relief, rééchantillonné : produced using Copernicus WorldDEM-30 © DLR e.V. 2010-2014 and © Airbus Defence and Space GmbH 2014-2018 provided under COPERNICUS by the European Union and ESA; all rights reserved.


DATA NOTICE — Mappendium
========================

The databases shipped with the game are derivative databases of OpenStreetMap. They are made available under the
Open Data Commons Open Database License (ODbL) 1.0, and only under it: you may copy, republish, adapt and use them,
commercially included, as long as you credit their source and share alike. The licence text is in LICENCE-ODbL-1.0.txt,
next to this notice, and at https://opendatacommons.org/licenses/odbl/1-0/. Each database also carries it in its meta table.

Map data © OpenStreetMap contributors — openstreetmap.org/copyright.

The databases are published free of charge, under the same licence, at: https://mappendium.com/donnees

Under the ODbL (section 1 above, the same paths): the *.sqlite files. Produced from them (section 2): the
building shells, the land-cover floor image, Data/TerrainTiles/ (the Copernicus terrain carved along the waterways and flattened under the roads of OpenStreetMap). Copernicus GLO-30 only (the terrain, resampled, nothing of OpenStreetMap): Data/datasets/canada/TerrainTiles/. Outside the ODbL: landcover.bin, ESA
WorldCover codes only (CC BY 4.0). Sources (section 3): OpenStreetMap (ODbL 1.0; Geofabrik extracts of 2026-06-23 for Québec,
2026-09-06 elsewhere); Microsoft Canadian Building Footprints (ODbL 1.0); Natural Resources Canada, Automatically
Extracted Buildings (Open Government Licence – Canada 2.0 — Contains information licensed under the Open Government Licence – Canada.); BDPPAD, La Financière agricole du Québec
(CC BY 4.0); GeoNames (CC BY 4.0); Natural Earth (public domain); the UN M49 geoscheme; ESA WorldCover 2021
(CC BY 4.0 — © ESA WorldCover project 2021 / Contains modified Copernicus Sentinel data (2021) processed by ESA WorldCover consortium); Copernicus GLO-30 (produced using Copernicus WorldDEM-30 © DLR e.V. 2010-2014 and © Airbus Defence and Space GmbH 2014-2018 provided under COPERNICUS by the European Union and ESA; all rights reserved).
```
