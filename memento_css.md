# Résumé

Bienvenue sur le CSS Style guide de la société SLEEDE.
L'objectif de ce document est de présenter les outils et les bonnes pratiques de l'intégrateur web au sein de l'agence.
C'est avant tout une compilation d'outils et de pratiques que nous utilisons tous les jours. Vous pourrez facilement l'adapter à votre équipe!


## Sommaire

* [Bonnes pratiques: un code lisible et maintenable](#good_practice)
    * [Adopter une convention d'écriture](#convention)
    * [Un <head> à toute épreuve](#head)
    * [Stratégies de développement](#strategies)
    * [Les frameworks CSS](#frameworks)
* [Sass et Compass](#sass_compass)
    * [Sass](#sass)
    * [Compass](#compass)
    * [Optimisation du SCSS](#optimisation)
* [Les méthodes de travail](#methods)
    * [travailler en équipe](#team)
    * [Les erreurs de conception](#errors)
    * [Bien débuter un projet](#new_project)
    * [Résoudre un problème](#resolve)
* [Outils et veille technologiques](#tools)

<a name="good_pratice"/>
# Bonnes pratiques: un code lisible et maintenable

<a name="convention"/>
## Adopter une convention d'écriture

* Nom de classe en minuscule avec tiret: `name, name-header`.
* Choisir des noms de classe explicite.
* Indenter le code avec 2 espaces.
* Indenter et espacer correctement le code:

   ```css
   .element-simple { margin: 0; }
   
   .element-alpha,
   .element-simple { margin: 0; }
   
   #header .element {
   	top 0; right: 0; bottom: 0; left: 0;
   	margin: 1em;
   }
   ```  
* Indenter les relations entre les sélecteurs (parent > enfant).
* Classer les propriétés par type.
   * Boîte
   * Bordure
   * Fond
   * Texte
   * Autres
* Indenter les valeurs CSS3 complexes si besoin.
* Utiliser les [raccourcis CSS](http://www.dustindiaz.com/css-shorthand/): `border, margin, padding, font, list-style, background`.
* Commenter le code pour:
   * Détailler un usage particulier
   * Expliquer les bogues corrigés
   * Signaler une modification
* Utiliser majoritairement les class pour le CSS, et les ID pour le JS.
* Utiliser les micro données et les micro formats
  * [Schema.org](http://schema.org/)
  * [The Open Graph protocol](http://ogp.me/)


<a name="head"/>
## Un <head> à toute épreuve

* Ajouter un Doctype HTML5 `<!DOCTYPE html>`.
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
  * [Normalize CSS de Nicolas Gallagher](http://necolas.github.com/normalize.css/)
  * [Reset CSS d'Eric Meyer](http://meyerweb.com/eric/tools/css/reset/)
  * [Liste des resets CSS existant](http://www.cssreset.com/)
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

    <!-- Un seul fichier css minifiée et sans @import dedans !-->
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

* Réaliser des tests
  * Tester pendant le prototypage, et à la fin.
  * Réaliser un styles guide et le tester.
  * Utiliser les outils de diagnostics (Debugger, CSS Lint, Grep ...)


* Utiliser des frameworks si besoin
  * [HTML5 Boilerplate](http://html5boilerplate.com/)
  * [OOCSS](https://github.com/stubbornella/oocss/wiki)
  * [Zurb Foundation](http://foundation.zurb.com/)
  * [Bootstrap Twitter](http://twitter.github.com/bootstrap/) 

<a name="activerecord"/>
### OOCSS

* OOCSS permet de séparer la structure (dimensions, marges, position) de 
l'apparence (bordures, couleurs, images). 
L'apparence est décris par la classe et non par le tag.



<a name="flawed"/>
## Flawed Gems

This is a list of gems that are either problematic or superseded by
other gems. You should avoid using them in your projects.

* [rmagick](http://rmagick.rubyforge.org/) - this gem is notorious for its memory consumption. Use
[minimagick](https://github.com/probablycorey/mini_magick) instead.
* [autotest](http://www.zenspider.com/ZSS/Products/ZenTest/) - old solution for running tests automatically. Far
inferior to guard and [watchr](https://github.com/mynyml/watchr).
* [rcov](https://github.com/relevance/rcov) - code coverage tool, not
  compatible with Ruby 1.9. Use SimpleCov instead.
* [therubyracer](https://github.com/cowboyd/therubyracer) - the use of
  this gem in production is strongly discouraged as it uses a very large amount of
  memory.

This list is also a work in progress. Please, let me know if you know
other popular, but flawed gems.

<a name="processes"/>
## Managing processes

* If your projects depends on various external processes use
  [foreman](https://github.com/ddollar/foreman) to manage them.

<a name="testing"/>
# Testing Rails applications

The best approach to implementing new features is probably the BDD
approach. You start out by writing some high level feature tests
(generally written using Cucumber), then you use these tests to drive
out the implementation of the feature. First you write view specs for
the feature and use those specs to create the relevant
views. Afterwards you create the specs for the controller(s) that will
be feeding data to the views and use those specs to implement the
controller. Finally you implement the models specs and the models
themselves.


<a name="reading"/>
# Further Reading

There are a few excellent resources on Rails style, that you should
consider if you have time to spare:

* [The Rails 3 Way](http://tr3w.com/)
* [Ruby on Rails Guides](http://guides.rubyonrails.org/)
* [The RSpec Book](http://pragprog.com/book/achbd/the-rspec-book)

<a name="contributing"/>
# Contributing

Nothing written in this guide is set in stone. It's my desire to work
together with everyone interested in Rails coding style, so that we could
ultimately create a resource that will be beneficial to the entire Ruby
community.

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

<a name="spreadtheword"/>
# Spread the Word

A community-driven style guide is of little use to a community that
doesn't know about its existence. Tweet about the guide, share it with
your friends and colleagues. Every comment, suggestion or opinion we
get makes the guide just a little bit better. And we want to have the
best possible guide, don't we?

