# Résumé

Bienvenue sur le CSS Style guide de la société SLEEDE.
L'objectif de ce document est de présenter les outils et les bonnes pratiques de l'intégrateur web au sein de l'agence.
C'est avant tout une compilation d'outils et de pratiques que nous utilisons tous les jours. Vous pourrez facilement l'adapter à votre équipe!

## Sommaire

* [Bonnes pratiques: un code lisible et maintenable](#good_practice)
    * [Adopter une convention d'écriture](#convention)
    * [Un head à toute épreuve](#head)
    * [Stratégies de développement](#strategies)
    * [Les frameworks CSS](#frameworks)
      - [En particulier: OOCSS](#oocss)
    * [Allégement de bonnes pratiques](#limits)
* [Sass et Compass](#sass_compass)
    * [Sass](#sass)
    * [Compass](#compass)
    * [Optimisation du SCSS](#optimisation)
* [Les méthodes de travail](#methods)
    * [travailler en équipe](#team)
    * [Les erreurs de conception](#errors)
    * [Bien débuter un projet](#new_project)
    * [Résoudre un problème](#resolve)
* [Outils et veille technologiques](#tools_learn)
    * [Veille technologique](#learn)
    * [Références & docs](#docs)
    * [Outils](#tools)

<a name="good_pratice"/>
# Bonnes pratiques: un code lisible et maintenable

<a name="convention"/>
## Adopter une convention d'écriture

* Nom de classe en minuscule avec tiret: `name, name-header`.
* Choisir des noms de classe explicite.
* Indenter le code avec 2 espaces.
* Indenter les relations entre les sélecteurs (parent > enfant).
* Classer les propriétés par type:
  + Boîte
  + Bordure
  + Fond
  + Texte
  + Autres
* Indenter les valeurs CSS3 complexes si besoin
* Utiliser les [raccourcis CSS](http://www.dustindiaz.com/css-shorthand/): `border, margin, padding, font, list-style, background`.
* Commenter le code pour:
   * Détailler un usage particulier
   * Expliquer les bogues corrigés
   * Signaler une modification
* Utiliser majoritairement les class pour le CSS, et les ID pour le JS.
* Garder une spécificité basse dans votre code: éviter par exemple `#id .class #id2`
* Utiliser les micro données et les micro formats
  * [Schema.org](http://schema.org/)
  * [The Open Graph protocol](http://ogp.me/)
* Exemple: indenter et espacer correctement le code:
   ```css
   .element-simple { margin: 0; }
   .element-alpha,
   .element-simple { margin: 0; }

   #header .element {
     top: 0; right: 0; bottom: 0; left: 0;
     margin: 1em;
   }
   ```

<a name="head"/>
## Un head à toute épreuve
* Ajouter un Doctype HTML5 `<!DOCTYPE html>`
* Utiliser l'élément HTML pour cibler IE:

  ```html
  <!--[if lt IE 7]>      <html class="ie6"> <![endif]-->
  <!--[if IE 7]>         <html class="ie7"> <![endif]-->
  <!--[if IE 8]>         <html class="ie8"> <![endif]-->
  <!--[if IE 9]>         <html class="ie9"> <![endif]-->
  <!--[if gt IE 9]><!--> <html>         <!--<![endif]-->
  ```
* Utiliser le script Modernizr.js pour détecter les capacités du navigateur.
  ```html
  <html lang="fr" class="js no-touch postmessage history multiplebgs no-boxshadow
  opacity cssanimations csscolumns cssgradients csstransforms csstransitions
  fontface localstorage sessionsstorage svg inlinesvg blobbuilder bloburls download
  formdata">
  ```
  ```css
  .box { box-shadow: #999 0 0 5px; }
  .no-boxshadow .box { border: 1px solid #ddd; }
  ```
* Déclencher le mode compatibilité le plus élevé d'Internet Explorer
  ```html
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  ```
* Utiliser l'encodage Unicode
  ```html
  <meta charset="utf-8" />
  ```
* Utiliser un reset CSS sélectif
  > [Normalize CSS de Nicolas Gallagher](http://necolas.github.com/normalize.css/)
  >
  > [Reset CSS d'Eric Meyer](http://meyerweb.com/eric/tools/css/reset/)
  >
  > [Liste des resets CSS existant](http://www.cssreset.com/)
* Bien ranger ses fichiers et fragmenter le code > 500 lignes
(reset, typo, form, grid, layout, global, application, pages, print, ie...)
* Ajouter une css pour le média Print.
* Récapitulatif de l'entête d'une page html
  ```html
  <!DOCTYPE html>
  <!--[if lt IE 7]>      <html class="ie6"> <![endif]-->
  <!--[if IE 7]>         <html class="ie7"> <![endif]-->
  <!--[if IE 8]>         <html class="ie8"> <![endif]-->
  <!--[if IE 9]>         <html class="ie9"> <![endif]-->
  <!--[if gt IE 9]><!--> <html>         <!--<![endif]-->
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <!-- Meta Charset (à placer avant title pour des raisons de sécurité) -->
    <meta charset="utf-8" />
    <title>Titre de la page</title>
    <!-- Un seul fichier css minifiée et sans @import dedans -->
    <link href="styles.css" media="screen,projection" rel="stylesheet" />
    <link href="print.css" media="print" rel="stylesheet" />
    <!-- A charger dans le head, après les CSS, pour éviter un "Flash of unstyled content" -->
    <script src="modernizr.js"></script>
  </head>
  ```

<a name="strategies"/>
## Stratégies de développement

* L'amélioration progressive
  ```css
  /* Par défaut, on affiche le contenu de l'accordéon */
  .accordeon-contenu { display: block; }

  /* Si JavaScript est activé, on masque le contenu de l'accordéon */
  .js .accordeon-contenu { display: none; }
  ```
* La dégradation gracieuse (ou tolérance à l'erreur)
  ```css
  /* Par défaut, on masque le contenu de l'accordéon */
  .accordeon-contenu { display: none; }

  /* Si JavaScript n'est pas activé, on affiche le contenu de l'accordéon */
  .no-js .accordeon-contenu { display: block; }
  ```
* Tester encore et encore
  * Tester pendant le prototypage, et à la fin.
  * Réaliser un styles guide (gros projet) et le tester.
    * [Les patterns portfolio](http://24ways.org/2011/front-end-style-guides)
    * [Générer un style guide avec KSS](https://github.com/kneath/kss)
  * Utiliser les outils de diagnostics (Debugger, CSS Lint, Grep ...)
  * Utiliser des machines virtuelles ou des outils de tests (type Litmus).

<a name="frameworks"/>
## Les frameworks CSS

* Utiliser des frameworks si besoin
  * [HTML5 Boilerplate](http://html5boilerplate.com/)
  * [OOCSS](https://github.com/stubbornella/oocss/wiki)
  * [Zurb Foundation](http://foundation.zurb.com/)
  * [Bootstrap Twitter](http://twitter.github.com/bootstrap/)

<a name="oocss"/>
### En particulier: OOCSS

OOCSS permet de séparer la structure (dimensions, marges, position) de l'apparence (bordures, couleurs, images).
L'apparence est décrit par la classe et non par le tag.

* Voici un exemple de boutons sans OOCSS
  ```html
  <a class="primary-action">Envoyer le message</a>
  <a class="secondary-action">Annuler</a>
  ```
  ```css
  .primary-action {
    padding: .5em 1em;
    display: inline-block;
    line-height: 1.5em;
    height: 1.5em;
    border-radius: 10px;
    margin: 0 5px 0 0;
    background: black;
    color: #fff;
    font-weight: bold;
    box-shadow: #000 0 0 5px;
  }

  .secondary-action {
    padding: .5em 1em;
    display: inline-block;
    line-height: 1.5em;
    height: 1.5em;
    border-radius: 10px;
    margin: 0 5px 0 0;
    background: #eee;
    border: 1px solid #ddd
    color: #555;
    padding: 0.25em 1em;
    text-shadow: #fff 0 1px 0;
  }
  ```
* Avec OOCSS
  ```html
  <a class="button primary-action">Envoyer le message</a>
  <a class="button secondary-action">Annuler</a>
  ```
  ```css
  /* Structure */
  .button {
    padding: .5em 1em;
    display: inline-block;
    line-height: 1.5em;
    height: 1.5em;
    border-radius: 10px;
    margin: 0 5px 0 0;
  }

  /* habillage */
  .primary-action {
    background: black;
    color: #fff;
    font-weight: bold;
    box-shadow: #000 0 0 5px;
  }

  .secondary-action {
    background: #eee;
    border: 1px solid #ddd
    color: #555;
    padding: 0.25em 1em;
    text-shadow: #fff 0 1px 0;
  }
  ```
* Exemple de code à éviter:
  ```css
  #sidebar h2 {
    ...
  }
  ```
  Les styles relatifs à #sidebar h2 ne seront pas réutilisable et h2 ne pourra être déplacé ou devenir un h1.
  Préférez:
  ```css
  .en-tete {
    ...
  }
  ```
* OOCSS prend toute sa mesure sur des projets plus important avec de nombreux gabarits.
  Plus d'informations: [Documentation OOCSS](https://github.com/stubbornella/oocss/wiki)

<a name="limits"/>
## Allégement de bonnes pratiques

* Utiliser de manière simplifié une checklist (type OpQuast)
  * [Voir SLEEDE - Checklists](https://docs.google.com/spreadsheet/pub?key=0AtLuTKz2ikTydFktNkhHdDNSdHZRdU85Rm9qWUdrRUE&output=html)
* Ne pas rechercher la perfection sémantique (dans le doute, on utilise `div`).
* Accepter (et faire accepter) un rendu différent sur certains navigateurs tout en
  assurant un contenu toujours accessible.
* Utiliser l'unité `em dans des contextes relatifs mais pas à outrance.


<a name="sass_compass"/>
# Sass et Compass

<a name="sass"/>
## Sass

* Règles imbriqués: attention à ne pas en abuser, spécialisation importante des sélecteurs
  * Exemple de code mauvais généré: `body#homepage #navigation #home-link { ... }`
  * Règle de l'inception: 4 niveaux max d'imbrication.
* Référencer le sélecteur courant ou parent avec &
  ```scss
  /* Référencer le sélecteur courant */
  .bouton {
    /* Propriétés du bouton */
    &.rechercher {
      /* Propriétés du bouton de recherche */
      &:hover {
        /* Propriétés du bouton de recherche survolé */
      }
    }
  }

  /* Référencer le sélecteur parent
     Code en sortie: .ie7 .element { ... } */
  .element {
    .ie7 & {
      margin-left: 5px;
    }
  }
  ```
* Les commentaires
  ```scss
  /* Commentaire apparent dans la source après compilation */
  // Commentaire Sass, ne sera pas compilé
  ```
* Les variables
  ```scss
  $links: #08F;
  a { color: $links }
  ```
* Les mixins
   ```scss
   @mixin mon-mixin {
    color: red;
   }
   .ma-regle {
    @include mon-mixin;
   }
   ```
* Les Mixin avec arguments
   ```scss
   @mixin box-shadow($arguments: none) {
   -moz-box-shadow: $arguments; // Firefox
   -webkit-box-shadow: $arguments; // WebKit
   box-shadow: $arguments; // Standard
   }
   .button {
   @include box-shadow(1px 1px 3px red);
   }
   ```
  * Attention aux volumes de code css générés!
* Etendre une class: `@extend`
  ```scss
  .rounded {
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    -border-radius: 5px;
  }

  .button { @extend .rounded; }
  input[type="text"] { @extend .rounded; }

  /* CSS compilée: les propriétés n'apparaissent plus qu'une seule fois dans le code. */
  .rounded, .button, input[type="text"] {
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    -border-radius: 5px;
  }
  ```
* Import de partial (_partial.scss) avec la directive `@import 'partial'`
* Utilisation du module de couleurs: `rgba, transparentize, opacify, saturate, lighten, darken`


+ [Documentation SASS](http://www.kaelig.fr/bettersassdocs/)

<a name="compass"/>
## Compass

Les avantages à utiliser compass:
* Reset embarqué
* Helper images, urls ...
* CSS3 sans préfixes
* Data URI
* Génération de sprites
* Effets CSS3 plus facile

<a name="optimisation"/>
## Optimisation du CSS avec SASS

* Utiliser des variables
* Définir des jeux de polices (stack-fonts)
* Créer des mixins
* Etendre des classes (@extend)
* Mieux, une combinaison de mixin avec @extend
* Gestion des navigateurs particuliers avec les sélecteurs parents (ie)
* Compresser le css (avec Least par exemple)


<a name="methods"/>
# Les méthodes de travail

<a name="team"/>
## Travailler en équipe

* Communiquer avec:
  * Le graphiste (typo, css3, design mobile, hover, js, bande passante)
    * Ajuster la maquette graphique en amont afin de détecter:
      - Les images trop lourdes
      - Un comportement non natif (ex formulaire)
      - Une Structure incompatible avec le SEO
      - Trop de teintes ou de typographies différentes.
  * Le dev (qu'est ce que je peux modifier sans risques ?)
* Documenter son code(commentaire, style guide ...)
* Faire des commits intelligibles (Git)
* S'inspirer des bonnes pratiques (pattern)
* Créer des objets simple, facile à combiner / comprendre.
* Favoriser le code simple et compréhensible

<a name="errors"/>
## Les erreurs de conception

* Ne pas documenter
* Réinventer la roue
* Factoriser trop tôt le code
* Ne pas tester
* Utiliser des techniques derniers cris
* Varier les conventions
* Ne pas communiquer au sein de l'équipe
* Ne pas versionner son code
* Ne pas faire de veille.
* Rechercher la perfection
* Repousser le nettoyage de son code
* Utiliser des styles inline et des !important
* Sur spécifier les propriétés
* Multiplier les propriétés (couleurs, tailles, margin 0, padding 0)

<a name="new_project"/>
## Bien débuter un projet

* Etudier le design (cahier des charges, maquettes ...)
* Déduire des motifs récurrents (boîtes, menus, éléments dynamiques, boutons ...)
* Définir une base typographique
* Coder la structure générale (Header, menu, colonnage, footer)
* Coder les modules
* Coder les exceptions (par pages): ex 1 page = 1 class (utilisation du body)

<a name="resolve"/>
## Résoudre un problème

* Valider le code
* Rechercher une solution dans les moteurs
* Isoler le problème dans un environnement contrôlé (reduced test)
* Demander de l'aide (JS Fiddle, forum Alsacréation, Stack overflow, doctype, dabblet ...)
* Communiquer la résolution du problème (blog, twitter)


<a name="tools_learn"/>
# Outils et veille technologique

<a name="learn"/>
## Veille technologique

Il ne s'agit pas d'une activité pour se tenir au courant, mais d'une activité intrinsèque à notre métier, inscrite dans nos missions.
Cela comporte de nombreux avantanges:

* Anticiper les avancées technologiques
* Améliorer la qualité et la productivité
* Créer d'avantages
* Ne pas réinventer la roue
* Augmenter ses compétences
* Diffuser l'information à l'équipe

Mise en place:
  * [Organiser sa veille](http://fr.slideshare.net/mcfaure/veille-technologique-mcf)

<a name="docs"/>
## Références & docs

### Docs
* [Google best practices & tools](https://developers.google.com/speed/docs/best-practices/rendering?hl=fr-FR)
* [Documentation SASS](http://www.kaelig.fr/bettersassdocs/)
* [Raccourcis CSS](http://www.dustindiaz.com/css-shorthand/)

### Références
* Simple as Milk
* Clearleft.com
* Smashing magazine
* A list apart
* Alsacréation
* Pompage.net
* Openweb
* wdfriday.com
* 24ways
* Css4design
* Blogs & twitters

<a name="tools"/>
## Outils

* [CSS Lint: diagnostic en ligne](http://csslint.net/)
* [Litmus: tester vos pages et vos e-mails](http://litmus.com/)
* [HTML5 Boilerplate: boîte à outils de l'intégrateur](http://html5boilerplate.com/)
* [styleneat: ordonner les propriétés CSS](http://styleneat.com)
* [Least: imbriquer vos sélecteurs automatiquement](http://toki-woki.net/p/least/)
* [W3C Checklink: validateur de liens](http://validator.w3.org/checklink/)
* [Colour Contrast Check: analyseur de contrastes](http://snook.ca/technical/colour_contrast/colour.html)
* [GTmetric: analyse des performances](http://gtmetrix.com/)
