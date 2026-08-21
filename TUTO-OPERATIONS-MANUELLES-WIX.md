# Ce qui reste à faire à la main sur Wix, et comment

Site : Huglo Lepage, Wix Studio
Éditeur : https://editor.wix.com/studio/a4dd7130-225d-4deb-842d-8fa15d8d043f?metaSiteId=63aef5a3-97ac-4de8-8a17-a753139a626f
Site en ligne : https://www.huglo-lepage.com/

Repères vérifiés dont tu auras besoin :

| Élément | Identifiant |
|---|---|
| Page dynamique Expertises | `snatn` |
| Page dynamique Avocats | `ejtvw` |
| Collection des fiches | `EXPERTISES` |
| Champ créé pour le balisage | `jsonldGraph`, libellé « Balisage JSON-LD (head) » |

**Important sur la publication.** Tout ce que j'ai déjà fait (robots.txt, redirections,
JSON-LD d'identité, contenu CMS) est en ligne sans publication. En revanche, tout ce
qui suit et qui passe par l'éditeur visuel exige de publier le site à la fin. Les
réglages SEO du tableau de bord, eux, s'appliquent sans publication.

Une réserve honnête avant de commencer : je décris les écrans Wix Studio tels qu'ils
fonctionnent, mais Wix renomme régulièrement ses libellés. Si un intitulé diffère,
cherche l'équivalent, la logique reste la même. Les identifiants du tableau ci-dessus,
eux, sont vérifiés.

---

## Ordre conseillé

Fais d'abord le bloc 1, c'est le seul qui débloque une promesse faite à Roxane.
Les blocs 2 à 7 sont de l'éditeur visuel, à regrouper en une seule session pour ne
publier qu'une fois. Le bloc 8 est de la vérification.

---

## 1. Les quatre réglages de l'onglet SEO des pages dynamiques

C'est le plus important. Une seule visite sur deux écrans règle quatre points :
le balisage par fiche, les deux `og:type` et l'image sociale.

### 1.a Connecter le balisage JSON-LD aux fiches Expertise

**Pourquoi.** Le graphe par fiche (WebPage, BreadcrumbList, FAQPage pour l'Urbanisme)
est déjà écrit dans le CMS pour les douze fiches. Il ne sort pas encore dans le head
parce que rien ne le relie à la page.

**Où.** Tableau de bord du site, section Marketing et SEO, puis les réglages SEO des
pages. Choisis la page dynamique des Expertises. Tu peux aussi y arriver depuis
l'éditeur, en sélectionnant la page dynamique Expertises puis son panneau SEO.

**Étapes.**

1. Ouvre les réglages SEO de la page dynamique Expertises.
2. Descends jusqu'au bloc des données structurées, parfois nommé « Balisage de
   données structurées » ou « Structured data markup ».
3. Le champ attend du JSON. Utilise le bouton d'insertion de valeur dynamique, celui
   qui propose les champs de la collection, et choisis **Balisage JSON-LD (head)**.
4. Le champ doit contenir uniquement la variable, sans balise `<script>` autour.
   Wix ajoute lui-même la balise.
5. Enregistre.

**Vérification, à faire tout de suite.**

Ouvre https://www.huglo-lepage.com/expertises/droit-urbanisme-amenagement, affiche le
code source de la page, cherche `ld+json`. Tu dois voir apparaître un bloc contenant
`"@type":["WebPage","FAQPage"]` et les neuf questions.

Puis colle l'URL dans le test des résultats enrichis de Google et dans
validator.schema.org. Ce sont les deux captures que Roxane attend.

**Le piège à surveiller.** Wix peut échapper les guillemets du champ texte au moment
de l'injection. Si le code source montre `&quot;` au lieu de `"`, le balisage sera
invalide. Dans ce cas, dis-le moi : on bascule sur un embed head par fiche, ce qui
marche mais demande douze embeds au lieu d'un branchement.

**Ne teste qu'une fiche avant de considérer que c'est bon.** Si l'Urbanisme sort
proprement, les onze autres suivront, elles utilisent le même mécanisme.

### 1.b og:type sur les fiches Expertise

**Pourquoi.** Il vaut `website` sur des pages de contenu éditorial. Roxane attend
`article`.

**Où.** Même écran que le 1.a.

**Étapes.**

1. Dans les réglages SEO de la page dynamique Expertises, ouvre la partie des
   réglages avancés, ou la partie « Réseaux sociaux » selon le libellé.
2. Cherche le champ `og:type`. S'il n'est pas exposé, passe par les balises
   personnalisées et ajoute :
   ```html
   <meta property="og:type" content="article">
   ```
3. Enregistre.

**Vérification.** Code source de n'importe quelle fiche Expertise, chercher
`og:type`. Une seule occurrence doit apparaître, avec la valeur `article`.

**Attention.** S'il en apparaît deux, c'est que la balise personnalisée s'ajoute au
lieu de remplacer. Il faut alors désactiver la valeur d'origine dans le gabarit,
pas empiler.

### 1.c og:type sur les fiches avocats

Même opération, sur la page dynamique des Avocats (`ejtvw`), avec la valeur
`profile` au lieu de `article`.

**Vérification.** Code source de https://www.huglo-lepage.com/avocats/roxane-sageloli,
`og:type` doit valoir `profile`.

### 1.d Image sociale des fiches Expertise

**Pourquoi.** Les douze fiches ne produisent ni `og:image` ni `twitter:image`, parce
que le gabarit va chercher l'image dans le CMS et qu'aucune fiche n'en porte. La
carte de partage sort vide alors que le format déclaré est grand format.

**Où.** Même écran SEO de la page dynamique Expertises.

**Deux options, à trancher avec Roxane.**

Option simple : dans le champ d'image sociale du gabarit, retire la valeur dynamique
qui pointe vers le champ image de la collection et mets à la place l'image d'en-tête
du site, celle qui sert déjà de partage par défaut :
`https://static.wixstatic.com/media/1332e6_ecd26ebf541146ea8a3f421ce73eb17f~mv2.png`

Option propre : ajoute une image par matière dans le champ `image1` de chaque fiche
de la collection EXPERTISES. Le gabarit la reprendra automatiquement, sans rien
changer au réglage. Attention toutefois, ce champ peut être affiché sur la page,
vérifie le rendu sur une fiche avant de remplir les douze.

**Vérification.** Code source d'une fiche, `og:image` et `twitter:image` doivent
porter une URL.

---

## 2. Le second H1 du pied de page

**Pourquoi.** Le bloc newsletter du pied de page porte un titre en H1. Toutes les
pages du site ont donc deux H1.

**Où.** Éditeur, pied de page, bloc newsletter, titre « Inscrivez-vous à notre
newsletter ».

**Étapes.**

1. Ouvre l'éditeur et sélectionne le pied de page.
2. Clique sur le titre « Inscrivez-vous à notre newsletter ».
3. Dans le panneau de texte, change la balise de `H1` vers `H2`, ou vers un
   paragraphe si l'apparence doit rester identique.
4. Si l'apparence change, ne touche pas au thème : applique le style visuel du H1 à
   la nouvelle balise via les réglages de style du texte. La balise et l'apparence
   sont deux choses distinctes dans Wix Studio.

**Vérification.** Après publication, code source de n'importe quelle page, chercher
`<h1`. Il ne doit y en avoir qu'un seul, celui du contenu de la page.

---

## 3. La hiérarchie du pied de page et du menu

**Pourquoi.** « Huglo Lepage Avocats » est en H2 dans le pied de page, et les
rubriques de menu en H5. Ces balises se répètent sur toutes les pages et brouillent
la structure de chacune.

**Où.** Éditeur. Deux composants identifiés lors de l'audit :

| Composant | Élément |
|---|---|
| `comp-m290wkp9_r_comp-lw2gbing` | « Huglo Lepage Avocats » en H2, pied de page |
| `comp-lx1uf4nn` | menu burger, sept entrées en H5 |

**Étapes.**

1. Sélectionne « Huglo Lepage Avocats » dans le pied de page, passe la balise de
   `H2` à paragraphe.
2. Dans le menu burger, sélectionne chaque entrée et passe la balise de `H5` à
   paragraphe.
3. Une entrée fait exception, « PUBLICATIONS », qui est déjà en paragraphe avec la
   classe `font_5`. Aligne les sept autres sur elle, ce sera cohérent.
4. Reprends le style visuel si nécessaire, comme au point 2.

**Vérification.** Code source, chercher `<h2` et `<h5`. Aucun ne doit provenir du
pied de page ni du menu.

---

## 4. Les ancres « Add »

**Pourquoi.** C'est le libellé par défaut d'un composant de lien Wix laissé non
renseigné. Le libellé est masqué en CSS, mais le nom accessible du lien reste
« Add ». C'est ce que lisent les moteurs et les lecteurs d'écran.

**Le point clé :** il faut **renseigner** le libellé, pas le masquer davantage.

**Où.** Éditeur. Répartition relevée lors de l'audit :

| Emplacement | Nombre |
|---|---|
| Lien « Revenir aux expertises » (`comp-lyg0cjcr`) | 1 |
| Cartes avocats du carrousel | 4 |
| Menu burger (`comp-lx1uf4nn`) | 8 |
| Page /expertise, vignettes expertises | 12 |
| Page /expertise, vignettes avocats | 13 |

**Étapes.**

1. Sélectionne le composant de lien concerné.
2. Ouvre ses réglages et cherche le champ de texte du lien, souvent nommé
   « Libellé », « Texte du bouton » ou « Link label ».
3. Remplace « Add » par le texte réel.
   - Vers une fiche avocat : le nom de l'avocat, par exemple « Roxane Sageloli ».
   - Vers une expertise : l'intitulé de l'expertise, par exemple « Droit pénal de
     l'environnement ».
   - Pour le lien de retour : « Revenir aux expertises ».
4. Sur les vignettes générées à partir du CMS, ne saisis pas le texte à la main :
   connecte le libellé au champ de la collection. Pour les avocats c'est le champ
   `title` de EQUIPE, pour les expertises le champ `title` de EXPERTISES. Un seul
   réglage couvre alors toutes les vignettes du répéteur.
5. Si le libellé ne doit pas être visible à l'écran, garde-le masqué visuellement
   mais renseigné. Ne le vide pas.

**Vérification.** Après publication, code source de /expertise, chercher `>Add<`.
Zéro occurrence attendue.

---

## 5. Le bloc « Nos avocats » et le carrousel

**Pourquoi.** Le bloc textuel a été ajouté, mais l'ancien carrousel est resté avec
sa structure d'origine : un H5 « Équipe dédiée » placé après les noms en H4, ce qui
inverse la hiérarchie.

**Où.** Éditeur, page dynamique Expertises, section « Nos avocats ». Composant
identifié : `comp-lycuzo1k11` pour le H5 « Équipe dédiée ».

**Étapes.**

1. Supprime le titre « Équipe dédiée ». Il vient d'une version antérieure et fait
   doublon avec le H2 du bloc textuel.
2. Vérifie que le H2 « Nos avocats en droit de l'urbanisme et de l'aménagement »
   figure bien avant les noms.
3. Passe les noms d'avocats du carrousel de `H4` à `H3`, pour qu'ils soient
   subordonnés au H2 et non au même niveau que les sections de la page.
4. Déplace le carrousel sous le bloc textuel. C'est ce que Roxane a demandé au point
   7 de son mail : le bloc textuel porte le signal, le carrousel reste pour la vue
   d'ensemble.
5. L'ordre d'affichage du carrousel suit aujourd'hui le champ `ordre` de la
   collection EQUIPE, qui est un ordre cabinet et non un ordre par matière. Deux
   solutions : soit tu tries manuellement le carrousel sur la référence de la fiche,
   soit on ajoute un champ d'ordre par expertise. Dis-moi si tu veux que je crée ce
   champ, c'est faisable par API en quelques minutes.

**Vérification.** Sur la fiche Urbanisme, l'ordre attendu est Roxane Sageloli,
Benoît Denis, Valérie Saintaman, Théophile Bégel. C'est déjà l'ordre stocké dans le
CMS, je l'ai vérifié.

---

## 6. Les ancres stables du sommaire

**Pourquoi.** Onze des douze entrées du sommaire pointent vers des identifiants
générés automatiquement, du type `viewer-z89j3824`. Ces identifiants changent quand
on rééedite le bloc, et le sommaire casse en silence. Une seule entrée a une ancre
stable, `viewer-nosavocatsurbanisme`.

**Où.** Éditeur, page dynamique Expertises, sur chaque section de la page.

**Étapes.**

1. Sélectionne la section correspondant à un H2.
2. Ouvre ses réglages, onglet des paramètres avancés, et renseigne l'identifiant
   d'élément avec un nom lisible et stable. Utilise la même convention que l'ancre
   existante, en minuscules et sans accent. Par exemple `viewer-planificationplu`.
3. Répète pour les onze sections concernées.
4. Reprends ensuite chaque entrée du sommaire et fais pointer son lien vers la
   nouvelle ancre.

**Vérification.** Après publication, clique les douze entrées du sommaire sur la
page en ligne. Chacune doit faire défiler vers sa section, sans recharger la page.

**À contrôler après chaque intervention future sur cette page.** C'est le seul point
de la liste qui peut se recasser tout seul. Note-le dans la procédure de mise en
ligne des fiches.

---

## 7. Renommer la page /blank-1

**Pourquoi.** C'est la dernière page du site restée à l'adresse par défaut de
l'éditeur. Elle porte le titre « Plan du site ».

**Où.** Éditeur, gestionnaire de pages.

**Étapes.**

1. Ouvre le gestionnaire de pages et sélectionne la page « Plan du site ».
2. Ouvre ses réglages, onglet SEO, et modifie le slug de `blank-1` vers
   `plan-du-site`.
3. Wix propose en général de créer la redirection automatiquement. Accepte.
4. Si elle n'est pas proposée, dis-le moi, je crée la 301 par API en une minute.
5. Pense à corriger le lien « Site map » du pied de page, qui pointe aujourd'hui
   vers `sitemap.xml` au lieu de la page Plan du site.

**Vérification.** Je contrôlerai par API que la redirection
`/blank-1 -> /plan-du-site` existe.

---

## 8. L'affichage conditionnel des blocs vides

**Pourquoi.** Roxane demandait si le gabarit permet de masquer un bloc quand son
contenu est vide, ce qui réglerait la question sur tout le site sans lui demander
d'écrire un texte par matière. La réponse est oui.

**Où.** Éditeur, page dynamique Expertises, sur chaque bloc alimenté par le CMS.

**Étapes.**

1. Sélectionne le conteneur du bloc, pas le texte à l'intérieur.
2. Ouvre le panneau des données connectées.
3. Active la condition qui masque l'élément quand le champ source est vide. Selon la
   version, elle s'appelle « Masquer si vide », « Hide if empty » ou passe par les
   règles d'affichage conditionnel.
4. Refais-le pour chaque bloc optionnel du gabarit.

**Vérification.** Va sur https://www.huglo-lepage.com/expertises/rse-audits-reglementaires,
c'est la fiche où Roxane a repéré les blocs vides. Aucun cadre vide ne doit subsister.

---

## 9. Deux vérifications, pas des corrections

### 9.a Les fiches avocats en double

La collection EQUIPE porte deux champs d'URL, `link-equipe-1-title` en `/avocats/...`
et `link-equipe-2-title` en `/equipe-2/...` avec des accents encodés. Si les deux
pages dynamiques sont publiées, c'est du contenu dupliqué sur toutes les fiches
avocats.

**Comment vérifier.** Ouvre https://www.huglo-lepage.com/equipe-2/roxane-sageloli.
Si la page s'affiche, il faut soit dépublier la page dynamique `/equipe-2/`, soit la
rediriger en 301 vers `/avocats/`. Dis-moi ce que tu constates, je pose les
redirections.

### 9.b La page dynamique dp82b

Il existe un quatrième gabarit SEO, dont la meta description commence par
« Expertise en... ». Ça ressemble à une ancienne page Expertise encore servie.
Regarde aussi la collection `EXPERTISES_TEMP_RESTORE`, qui porte les mêmes champs que
EXPERTISES et qui pourrait l'alimenter.

**Comment vérifier.** Dans le gestionnaire de pages de l'éditeur, liste les pages
dynamiques et repère celle qui n'est ni Expertises, ni Avocats, ni Publications.

---

## 10. Publier, puis me redonner la main

Une fois les blocs 2 à 7 faits, publie le site.

Préviens-moi ensuite. Je repasse par API pour contrôler ce qui est contrôlable de
mon côté, à savoir la redirection de `/blank-1`, l'état des balises SEO des pages, et
l'absence de régression sur les quatre-vingts redirections. Pour le reste, H1, ancres
« Add », sommaire et hiérarchie, la vérification se fait dans le code source de la
page en ligne, avec les recherches indiquées à chaque bloc.

Ces captures constituent les preuves que Roxane attend point par point. Garde-les au
fil de l'eau, ça t'évitera de tout refaire à la fin.
