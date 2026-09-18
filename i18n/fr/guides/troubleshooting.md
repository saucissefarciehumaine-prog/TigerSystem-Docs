---
sourceHash: 16f23e15f93fb06c9be12e36f8d3f4ec5850e90210ee7826fe685bea9646c050
sourcePath: docs/guides/troubleshooting.md
---

# Dépannage : les problèmes de puce, symptôme par symptôme

Les problèmes de lecture, d'écriture et de vérification de la puce elle-même,
du plus fréquent au plus rare. Partez de votre symptôme et suivez les
vérifications dans l'ordre.

Cette page s'arrête à la puce. Une imprimante qui n'apparaît pas dans Tiger
Studio, ou un emplacement qui ne se met pas à jour, relève du lien
imprimante : la [FAQ](../faq/README.md) donne les deux premières
vérifications, et les [pages par marque](../compatibility/README.md) le reste.

## La puce n'est pas détectée

| Vérification | Détail |
|---|---|
| NFC activé, antenne trouvée ? | Vérifiez que le NFC est activé, puis trouvez le point sensible de l'antenne de votre téléphone — une puce se lit le mieux posée à plat contre lui ([FAQ](../faq/README.md)). Balayez lentement plutôt que de rester sur un point. |
| Quelque chose entre les deux ? | Une coque épaisse, ou la plaque métallique d'un support magnétique, se trouve exactement là où le champ doit passer. Retirez-la pour la lecture. |
| Essayez l'autre puce | Une bobine porte deux puces, et chacune sert de secours à l'autre ([pourquoi deux puces](../concepts/tigertag-chip.md)). Si la seconde se lit, la première est abîmée — un autocollant plié ou déchiré a l'antenne coupée. La puce survivante identifie toujours la bobine ; pour retrouver une **paire**, on réécrit les deux puces ensemble en une seule session, jamais une puce de remplacement seule ([quand deux puces se lisent comme deux bobines](./twin-tag-pair.md)). |
| Un autre lecteur ? | N'importe quel téléphone NFC, un ACR122U sur un ordinateur, une TigerScale ou un [TigerSpool](../products/tigerspool.md) : si la puce se lit sur l'un d'eux, la puce va bien et le problème est du côté du lecteur. |

## La puce se lit comme vide

Ce n'est pas une panne : les puces vendues seules sont livrées **vierges**,
avec ou sans logo ([la puce TigerTag](../concepts/tigertag-chip.md)). Tiger
NFC Connect la lit, voit qu'elle est vide et propose de créer un filament pour
elle — c'est l'étape 2 de
[votre première bobine intelligente](../tutorials/first-smart-spool.md).

## La puce se lit, mais les données sont fausses

La puce a été écrite avec les mauvaises valeurs, ou elle vient d'une autre
bobine. Les puces ne sont **jamais verrouillées en écriture** : réencodez-la
([la puce TigerTag](../concepts/tigertag-chip.md)) — d'un geste dans Tiger
NFC Connect, ou par l'écriture guidée et contrôlée par UID de Tiger Studio sur
un lecteur de bureau.

Une puce d'usine réécrite par accident est le seul cas où il existe mieux : si
vous l'aviez sauvegardée dans Tiger Studio, restaurez-la. La sauvegarde est
liée à l'UID de cette puce et la remet exactement dans son état d'origine,
signature comprise ([sauvegarder une puce](../products/tigertag-plus.md)).

## Une bobine, deux entrées

Chaque puce se lit, et chaque puce ouvre sa propre bobine : les deux puces
n'ont presque certainement jamais formé une paire. Le seul test qui tranche,
et la réparation, sont dans
[quand les deux puces d'une bobine se lisent comme deux bobines](./twin-tag-pair.md).

## La marque ou la matière apparaît comme inconnue, ou sous un mauvais nom

La puce porte des **identifiants**, pas des noms ; l'application les résout
avec sa copie de la base de référence partagée
([identité universelle du filament](../concepts/universal-filament-identity.md)).
Une marque ou une matière ajoutée récemment peut se trouver sur une puce avant
que votre copie des tables ne la connaisse — la puce a raison, la table est en
retard. Tiger Studio embarque les tables et les rafraîchit depuis le CDN ; une
TigerScale rafraîchit sa propre copie au plus une fois par jour
([inventaire et synchronisation cloud](../concepts/inventory-and-cloud-sync.md)).
Laissez l'application se rafraîchir, ou mettez-la à jour, puis rescannez.

> **Note :** une table périmée change la façon dont une puce est *affichée*,
> jamais le fait qu'une signature TigerTag+ Certified se *vérifie* — voir
> ci-dessous.

## Le téléphone la lit, mais l'imprimante ne fait rien

C'est normal. Aucune imprimante ne lit une puce TigerTag par elle-même
aujourd'hui — la seule exception est la Snapmaker U1 sous le
[firmware étendu](../compatibility/snapmaker.md) de la communauté. Partout
ailleurs, les données de la puce atteignent l'imprimante **via Tiger Studio**,
pour les six marques intégrées : c'est le
[pont smartphone](../philosophy/smartphone-bridge.md). Vérifiez où en est
votre machine dans la [matrice de compatibilité](../compatibility/README.md).

## TigerTag+ Certified : la vérification échoue

La vérification contrôle la signature présente sur la puce avec des **clés
publiques publiées** — gratuite, hors ligne, sans compte, et sans aucune base
de référence ([TigerTag+](../products/tigertag-plus.md)). Aucun problème
réseau ne peut la faire échouer. Un échec a deux causes, et dans les deux cas
c'est le modèle de confiance qui fait son travail :

| Cause | Ce que cela veut dire |
|---|---|
| La puce a été réécrite après sa signature | La signature ne correspond plus aux données. La puce reste entièrement lisible — elle n'est simplement plus certifiée. Si vous l'aviez sauvegardée dans Tiger Studio, restaurez-la, signature comprise. |
| Les données ont été copiées sur une autre puce | La signature est liée à l'UID de la puce d'origine. Un clone échoue, sur le téléphone même du client — c'est voulu. |

## Une écriture échoue, ou s'arrête en cours

- **Deux contacts, et on maintient.** L'écriture, c'est le *second* contact :
  appuyez sur **Make**, puis maintenez le téléphone contre la puce jusqu'à ce
  que l'application confirme
  ([votre première bobine intelligente](../tutorials/first-smart-spool.md)).
  Retirez-le trop tôt et l'écriture ne va pas au bout.
- **Une puce à moitié écrite n'est pas perdue.** Les puces ne sont jamais
  verrouillées en écriture : rescannez-la et réencodez-la depuis le début.
- **La puce contient déjà autre chose** — un ancien TigerTag, un autre
  protocole, un simple tag NDEF ? Elle s'écrit par-dessus de la même façon.
  Pour repartir de zéro, effacez-la d'abord depuis Tiger NFC Connect ou Tiger
  Studio ([la puce TigerTag](../concepts/tigertag-chip.md)).
- **Vous écrivez une paire ?** En *Dual NFC*, la puce qui a ouvert la création
  est celle que l'application attend en 1/2 — commencez par la puce que vous
  avez scannée ([tutoriel](../tutorials/first-smart-spool.md)).
- **Sur un lecteur de bureau**, l'écriture de Tiger Studio est contrôlée par
  UID ([TigerPOD](../products/tigerpod.md)) : laissez la puce qu'il a scannée
  sur le lecteur jusqu'à la fin.

## Toujours bloqué ?

Demandez sur le [Discord](https://discord.gg/3Qv5TSqnJH), ou ouvrez un ticket
sur le dépôt de l'application concernée
([carte des dépôts](../developers/repositories.md)). Un rapport précis est une
contribution en soi ([soutenir le projet](../support.md)) — il lui faut cinq
choses :

1. la puce — NTAG213 / 215 / 216, officielle ou générique, ou une bobine
   taguée en usine et sa marque ;
2. le lecteur — modèle de téléphone, ACR122U / TigerPOD, TigerScale ;
3. l'application et sa version ;
4. ce que vous avez fait, dans l'ordre ;
5. ce que l'application a affiché, mot pour mot, ou une capture d'écran.

---

**▲ [Index de la documentation](../../README.md)** · **Voir aussi :** [La puce TigerTag](../concepts/tigertag-chip.md), [TigerTag+](../products/tigertag-plus.md), [Compatibilité](../compatibility/README.md), [FAQ](../faq/README.md)
