# Compte rendu de conformité, site huglo-lepage.com

Destinataire : Maître Roxane Sageloli
Objet : suite donnée à votre audit point par point
Date d'exécution ferme : [À COMPLÉTER PAR JORDAN AVANT ENVOI]

---

## Avant-propos

Votre constat sur la méthode d'intégration était exact, et il portait sur un point
de fond. Nous l'avons repris à la racine plutôt que corrigé au cas par cas.

Nous reprenons ci-dessous chacun de vos points, avec pour chacun l'élément de
preuve correspondant. Les éléments de preuve sont des relectures du site par
l'interface de programmation de Wix, effectuées après correction. Les captures
d'outils externes que vous demandez (test des résultats enrichis de Google,
validateur schema.org, Search Console) vous sont adressées séparément, une fois
la connexion décrite au point 2 réalisée, afin qu'elles portent sur l'état final
et non sur un état intermédiaire.

---

## Point 1. Contenu rendu invisible par les composants HTML

**État : traité.**

Le site ne comporte plus aucun composant HTML embed dans le contenu des fiches
Expertise. Le contrôle a porté sur l'ensemble des douze fiches et sur l'ensemble
des champs de contenu de chacune, soit l'introduction, les deux blocs de corps de
page, la FAQ, le bloc de contact, le résumé, le texte, l'offre et le bloc plainte.

Élément de preuve, relevé après correction sur les douze fiches :

```
Nombre total de composants HTML restants dans la collection EXPERTISES : 0
```

Le détail par fiche :

| Fiche | Composants HTML |
|---|---|
| Droit de l'énergie et des énergies renouvelables | 0 |
| RSE/Audits réglementaires | 0 |
| Droit des collectivités territoriales | 0 |
| Droit de la Commande publique | 0 |
| Droit de l'expropriation et du domaine public | 0 |
| Economie circulaire | 0 |
| Droit pénal de l'environnement | 0 |
| Protection de l'environnement et de la santé | 0 |
| Environnement industriel et installations classées | 0 |
| RSE/Vigilance/Audits volontaires | 0 |
| Crédits carbone & Crédits biodiversité | 0 |
| Avocat droit de l'urbanisme et de l'aménagement | 0 |

Le dernier composant subsistant, celui qui portait le balisage FAQ de la fiche
Urbanisme, a été supprimé au cours de cette intervention. Son contenu n'a pas été
perdu : les neuf questions ont été reprises dans le balisage traité au point 2.

Sur le fond de votre remarque, elle est désormais une règle de production chez
nous. Aucun contenu destiné à être lu par les moteurs ne passe par un composant
HTML embed, quel qu'en soit le confort d'intégration.

---

## Point 2. Données structurées

**État : déployé pour l'identité du cabinet et des avocats, préparé et en attente
d'une connexion pour le balisage propre à chaque fiche.**

### 2.1 Ce qui est en ligne

Un balisage JSON-LD a été déployé dans le `<head>` du site, par le panneau Code
personnalisé de Wix, en position `HEAD`, catégorie `ESSENTIAL`. Cette catégorie
signifie que la balise est rendue immédiatement dans le document servi, sans
dépendre d'un consentement ni de l'exécution de JavaScript. Il ne s'agit ni d'un
composant HTML embed, ni d'une iframe.

Élément de preuve, relecture du panneau après déploiement :

```
Nom      : JSON-LD — Identite cabinet et avocats
Position : HEAD
Catégorie: ESSENTIAL
Activé   : oui
Taille   : 11 191 caractères
```

Ce balisage contient un `@graph` de onze nœuds :

- un nœud `LegalService` portant l'`@id` `https://www.huglo-lepage.com/#cabinet`,
  avec la dénomination, l'adresse postale du 42 rue de Lisbonne, les coordonnées
  géographiques, le courriel, le logo, l'image, la zone d'intervention et les
  deux fondateurs référencés par leur `@id` ;
- un nœud `WebSite` portant l'`@id` `https://www.huglo-lepage.com/#website`, dont
  le `publisher` pointe vers l'`@id` du `LegalService` ;
- neuf nœuds `Person`, un par avocat du cabinet, chacun portant son `@id` propre,
  son `url` vers sa fiche profil, son `jobTitle`, ses `sameAs`, ses `knowsAbout`
  et ses `hasCredential`.

Extrait du code effectivement servi :

```json
{"@type":"LegalService","@id":"https://www.huglo-lepage.com/#cabinet",
 "name":"Huglo Lepage Avocats","url":"https://www.huglo-lepage.com/",
 "address":{"@type":"PostalAddress","streetAddress":"42 rue de Lisbonne",
 "postalCode":"75008","addressLocality":"Paris","addressCountry":"FR"}}
```

Vos cinq corrections préalables sont traitées ainsi :

1. **`@type` cohérent avec le contenu.** Le cabinet est déclaré en `LegalService`.
   Chaque fiche Expertise est déclarée en `WebPage`, et la fiche Urbanisme, qui
   porte une FAQ, en `WebPage` et `FAQPage`.
2. **`@id` uniques et résolvables.** Chaque nœud porte un `@id` construit sur une
   URL réelle du site. L'adresse `https://www.huglo-lepage.com/droit-urbanisme-amenagement/`
   que vous aviez relevée, qui renvoyait une erreur 404, n'apparaît nulle part.
   L'URL retenue est `https://www.huglo-lepage.com/expertises/droit-urbanisme-amenagement`,
   sans barre finale.
3. **Dates au format ISO réel.** Aucune variable de gabarit ne subsiste. La fiche
   Urbanisme porte `datePublished` et `dateModified` au 4 août 2026, date que la
   page indique elle-même dans son bloc auteur. Si la date de première publication
   diffère de celle de dernière mise à jour, merci de nous l'indiquer, nous la
   corrigerons. Nous n'avons pas voulu la supposer. Les onze autres fiches ne
   portent aucune date, faute d'une date certaine ; nous vous proposons d'ajouter
   un champ de date de mise à jour au gabarit, alimenté par vos soins.
4. **`sameAs` et `hasCredential` vérifiés.** Ils sont générés à partir des fiches
   avocats du gestionnaire de contenu, sans ressaisie. Les `sameAs` reprennent le
   profil LinkedIn et, le cas échéant, le référencement Leaders League. Les
   `hasCredential` reprennent les diplômes et certificats renseignés sur la fiche.
   Une entrée a été écartée volontairement, la mention « M1 Droit public général,
   Université Paris I (2021 » de la fiche de Maître Faustine Sigronde, dont la
   parenthèse n'est pas fermée dans le gestionnaire de contenu. Merci de corriger
   la fiche, l'entrée sera alors reprise automatiquement.
5. **`worksFor` pointant vers l'`@id` du `LegalService`.** C'est le cas pour les
   neuf avocats. Le défaut que vous aviez relevé, un `LegalService` sans `@id` dans
   le `worksFor` conduisant à deux entités distinctes, ne peut plus se produire :
   le `worksFor` ne contient plus qu'une référence, `{"@id":"https://www.huglo-lepage.com/#cabinet"}`.

### 2.2 La `BreadcrumbList` et le balisage propre à chaque fiche

Vous demandiez une `BreadcrumbList`, absente. Elle est écrite, pour les douze
fiches, avec trois niveaux : Accueil, Expertises, puis la fiche.

Le balisage propre à chaque fiche, qui comprend le nœud `WebPage`, la
`BreadcrumbList`, le rattachement aux avocats de la matière et, pour l'Urbanisme,
les neuf questions de la `FAQPage`, est écrit dans un champ dédié du gestionnaire
de contenu, nommé « Balisage JSON-LD (head) », créé pour cet usage sur la
collection EXPERTISES.

Élément de preuve, longueur du balisage écrit par fiche :

| Fiche | Caractères |
|---|---|
| Avocat droit de l'urbanisme et de l'aménagement | 4 868 dont 9 questions |
| Droit de l'énergie et des énergies renouvelables | 1 601 |
| Crédits carbone & Crédits biodiversité | 1 587 |
| Protection de l'environnement et de la santé | 1 546 |
| Droit de l'expropriation et du domaine public | 1 528 |
| Droit des collectivités territoriales | 1 497 |
| RSE/Audits réglementaires | 1 477 |
| Environnement industriel et installations classées | 1 473 |
| Droit pénal de l'environnement | 1 438 |
| RSE/Vigilance/Audits volontaires | 1 363 |
| Economie circulaire | 1 326 |
| Droit de la Commande publique | 1 298 |

Il reste une opération pour que ce balisage sorte dans le `<head>` : connecter ce
champ au champ de balisage de données structurées de l'onglet SEO de la page
dynamique Expertises. C'est exactement la voie que vous indiquiez dans votre
courrier. Cette connexion se fait dans le tableau de bord et non par
l'interface de programmation, pour une raison technique que nous préférons vous
exposer plutôt que de la passer sous silence.

L'interface de programmation de Wix refuse d'écrire le gabarit SEO des pages
dynamiques dès lors que celui-ci utilise des variables de collection, alors même
que ce sont les variables que le tableau de bord de Wix a lui-même créées et qui
sont en production. Message d'erreur littéral obtenu :

```
INVALID_ARGUMENT: Pattern for pageType "WIX_DATA_PAGE_ITEM" references unknown
variable(s): wix-data-page-item.EXPERTISES.mtaDescription,
wix-data-page-item.EXPERTISES.title
```

Réécrire ce gabarit par programmation supposerait donc d'abandonner la variable
qui alimente les douze meta descriptions. Nous ne l'avons pas fait. La connexion
sera donc faite à la main, et c'est également à cette occasion que seront réglés
les deux points ci-dessous, qui dépendent du même écran.

---

## Point 3. Corrections annoncées et non faites

**`og:type`.** Non corrigé à ce jour, pour la raison technique exposée au point
2.2 : la valeur est portée par le gabarit des pages dynamiques, que l'interface de
programmation refuse de réécrire. La correction est faite dans le tableau de bord,
sur le même écran que la connexion du balisage, et consiste à porter `og:type` à
`article` sur le gabarit des fiches Expertise et à `profile` sur celui des fiches
avocats. Nous vous adressons la capture du code source une fois l'opération faite.

**Liens fractionnés, ancres « Add », bloc « Nos avocats », sommaire.** Ces quatre
points relèvent du contenu et de la mise en page. Les corrections de contenu
portées au gestionnaire de contenu sont en place, et nous les avons revérifiées :

- le sommaire de la fiche Urbanisme comporte bien douze entrées, la douzième
  étant « Nos avocats en droit de l'urbanisme et de l'aménagement », et l'entrée
  « Notre accompagnement » porte bien l'intitulé complet « Notre accompagnement en
  droit de l'urbanisme et de l'aménagement » ;
- les quatre encadrés sont désormais du texte de page, mis en forme par les
  styles du site ;
- les avocats rattachés à la fiche Urbanisme sont, dans cet ordre, Maître Roxane
  Sageloli, Maître Benoît Denis, Maître Valérie Saintaman et Maître Théophile
  Bégel.

Les ancres « Add » et la hiérarchie de titres du carrousel ne sont pas modifiables
autrement que dans l'éditeur visuel. Elles figurent au calendrier ci-dessous.

---

## Point 4. Traitement de l'ancien site

**État : audit complet réalisé, treize redirections défectueuses corrigées.**

Le site compte quatre-vingts redirections. Nous les avons toutes reprises, et non
seulement les deux que vous citiez.

Sur les deux adresses que vous citiez, la redirection existe et couvre la forme
avec barre finale, la documentation de Wix précisant qu'un chemin ne différant
d'un autre que par une barre finale est considéré comme le même chemin :

```
/category/nos-publications/commentaire-doctrine-2   ->  /publication
/equipe/benjamin-huglo/benjamin_huglo_lepage-2      ->  /avocats/benjamin-huglo
```

L'audit a en revanche révélé deux séries de défauts que votre relevé ne pouvait
pas voir, puisqu'ils portaient sur la cible et non sur la source.

**Cinq redirections pointaient vers une adresse morte**, du fait d'une apostrophe
écrite en entité HTML `&#39;` dans l'URL de destination. Ces cinq redirections
renvoyaient donc vers une page inexistante. Corrigées.

**Une sixième pointait vers une adresse à la fois encodée et porteuse de la même
entité**, `/expertises/droit-p%C3%A9nal-de-l&#39;environnement`. Corrigée vers
`/expertises/droit-penal-de-l-environnement`.

**Sept redirections formaient des chaînes** de deux à trois sauts successifs,
ce qui dilue le signal transmis et ralentit la redirection. Chaînes supprimées,
chaque adresse pointe désormais directement vers sa destination finale. Exemple :

```
Avant : /blank-2 -> /expertises-item -> /landing-expertises -> /expertise
Après  : /blank-2 -> /expertise
```

Élément de preuve, contrôle après correction sur les quatre-vingts redirections :

```
Chaînes de redirection restantes         : 0
Cibles comportant une entité HTML        : 0
```

Les points relatifs au sous-domaine www2, au sous-domaine dev et aux demandes de
suppression en Search Console ne relèvent pas du site Wix et sont traités
séparément. Nous vous adressons les captures de la Search Console avec le reste.

---

## Point 5. Défauts affectant l'ensemble du site

**URL des fiches Expertises.** Traité et vérifié. Les douze adresses sont
normalisées, sans accent, sans apostrophe et sans caractère encodé :

```
/expertises/protection-de-l-environnement
/expertises/environnement-industriel
/expertises/droit-de-l-energie
/expertises/droit-urbanisme-amenagement
/expertises/commande-publique
/expertises/droit-des-collectivites-territoriales
/expertises/economie-circulaire
/expertises/droit-penal-de-l-environnement
/expertises/domanialite-et-expropriation
/expertises/credits-marches-carbone
/expertises/rse-audits-reglementaires
/expertises/rse-vigilance
```

Les anciennes adresses accentuées sont redirigées en 301, et ces redirections ont
été revérifiées à l'occasion de l'audit du point 4.

**Meta descriptions.** Les douze meta descriptions des fiches Expertise ont été
contrôlées, caractère par caractère, pour le retour à la ligne, l'espace de début,
l'espace de fin et le double espace. Aucun défaut ne subsiste. Le contrôle a été
étendu aux seize pages statiques du site, et un défaut y a été trouvé et corrigé :
la meta description de la page Politique de confidentialité commençait par un
retour à la ligne.

```
Avant : "\nConsultez les mentions légales et la politique de confidentialité..."
Après  : "Consultez les mentions légales et la politique de confidentialité..."
```

**Titres.** Deux intitulés de fiche comportaient une espace finale, qui se
reportait dans la balise `title` de la page. Corrigés : « RSE/Audits
réglementaires » et « Economie circulaire ».

**Pages à URL par défaut.** Sur les seize pages statiques du site, une seule
conserve une adresse au format par défaut de l'éditeur : `/blank-1`, qui porte le
titre « Plan du site ». Les autres adresses que nous avions relevées
précédemment ne sont plus servies. Le renommage de cette page se fait dans
l'éditeur, avec redirection 301 de l'ancienne adresse, et figure au calendrier.

**Second H1, hiérarchie du pied de page.** Non corrigés à ce jour. Ces éléments
appartiennent à l'ossature du site et se modifient exclusivement dans l'éditeur
visuel. Ils figurent au calendrier.

---

## Point 7. Réponses à vos questions

**robots.txt.** Le fichier était resté le fichier par défaut de Wix, jamais
personnalisé. Aucun des sept agents que vous citez n'y était nommé, ils étaient
donc autorisés implicitement par la directive `Allow: /`. Votre analyse était
exacte.

Le fichier a été personnalisé pour que le traitement de ces agents soit désormais
écrit et non plus implicite. Leur autorisation d'accès est inchangée, seule sa
formulation l'est. Ce choix nous paraît cohérent avec l'objectif que vous
poursuivez, puisqu'un agent bloqué ne peut pas citer vos fiches. Si le cabinet
souhaite au contraire fermer l'accès à tout ou partie de ces agents, la
modification tient en une ligne par agent et nous la faisons à votre demande.

Contenu littéral du fichier servi à ce jour :

```
User-agent: *
Allow: /
Disallow: *?lightbox=

# Optimization for Google Ads Bot
User-agent: AdsBot-Google-Mobile
User-agent: AdsBot-Google
Disallow: /_partials*
Disallow: /pro-gallery-webapp/v1/galleries/*

# Block PetalBot
User-agent: PetalBot
Disallow: /

# Crawl delay for overly enthusiastic bots
User-agent: dotbot
Crawl-delay: 10
User-agent: AhrefsBot
Crawl-delay: 10

# Agents des moteurs generatifs et des modeles de langage
# Acces autorise a l'ensemble du site, decision du cabinet Huglo Lepage Avocats.
User-agent: GPTBot
Allow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Google-Extended
Allow: /

User-agent: Applebot-Extended
Allow: /

Sitemap: https://www.huglo-lepage.com/sitemap.xml
```

**Contenu servi sans exécution de JavaScript.** C'était le fond de votre question,
et la réponse est désormais celle du point 1 : plus aucun composant HTML embed
dans les fiches, et un balisage structuré servi en clair dans le `<head>`.

**Blocs vides de la page RSE/Audits réglementaires.** Le gabarit ne conditionne pas
aujourd'hui l'affichage d'un bloc à la présence d'un contenu. Cette condition est
réalisable et nous la mettons en place. Elle réglera la question sur l'ensemble
du site et vous évitera d'avoir à produire un texte court par matière.

---

## Deux points que vous n'aviez pas relevés

**Image sociale des fiches Expertise.** Les seize pages statiques du site
produisent bien un `og:image` et un `twitter:image`. Les douze fiches Expertise
n'en produisent aucun, parce que le gabarit va chercher l'image de la fiche dans
le gestionnaire de contenu et qu'aucune fiche n'en porte. La carte de partage sort
donc sans visuel alors que le `twitter:card` est en `summary_large_image`. Deux
solutions, au choix du cabinet : rattacher l'image d'en-tête du site par défaut,
ou renseigner une image par matière. Nous attendons votre préférence.

**Fiches avocats en double.** Chaque avocat dispose de deux adresses dans le
gestionnaire de contenu, l'une sous `/avocats/`, l'autre sous `/equipe-2/` avec
des accents encodés. Si les deux sont servies, il s'agit de contenu dupliqué sur
l'ensemble des fiches avocats. Nous le vérifions et vous le confirmons avec le
reste.

---

## Ce qui reste, et sous quelle forme

Les opérations suivantes se font dans l'éditeur visuel ou le tableau de bord de
Wix, et non par programmation. Nous les regroupons en une seule intervention pour
ne pas multiplier les publications.

1. Connexion du champ « Balisage JSON-LD (head) » à l'onglet SEO de la page
   dynamique Expertises.
2. `og:type` à `article` sur les fiches Expertise, à `profile` sur les fiches
   avocats.
3. Image sociale des fiches Expertise, selon votre choix.
4. Suppression du H1 du bloc newsletter du pied de page.
5. Hiérarchie du pied de page et du menu, « Huglo Lepage Avocats » et les
   rubriques de menu ramenés à un niveau qui n'altère pas la structure des pages.
6. Libellés des ancres « Add », par le renseignement du libellé et non par son
   masquage.
7. Bloc « Nos avocats », suppression du titre « Équipe dédiée » et remise en ordre
   de la hiérarchie de titres, carrousel placé sous le bloc textuel.
8. Ancres stables du sommaire, en remplacement des identifiants générés.
9. Renommage de la page `/blank-1` et redirection 301.
10. Affichage conditionnel des blocs vides.

Chacune de ces dix opérations vous sera confirmée individuellement, avec son
extrait de code source ou sa capture, et non par la mention « Intégré ».

Nous vous confirmons enfin qu'aucune nouvelle fiche ne sera mise en ligne avant
que l'ensemble de ces points soit effectif.

Bien à vous,
