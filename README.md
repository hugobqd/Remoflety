# Remoflety : *Re*sponsive, *mo*dular & *fle*xible *ty*pography
*(Nom provisoire)*

## [Démo interactive ici](http://www.hugobouquard.com/demo/)

Remoflety est un plugin Sass permettant de gérer l'agrandissement des textes responsive :
- Il permet de définir différents paliers de taille de texte.
- Les transitions de tailles de textes sont completements fluides.
- L'intégration est facilité grace a un mixin et des classes appropriées.
- Des ratios d'échelle modulaire permettent de définir les hierarchies typographique avec cohérence et rapidité.
- Le choix des unités est souple (rem, px…)

<a href="https://www.smashingmagazine.com/2016/05/fluid-typography/">
<img src="https://www.smashingmagazine.com/wp-content/uploads/2016/05/advanced-calc-800-opt.png">
<sup>smashingmagazine.com/2016/05/fluid-typography/</sup>
</a>

## 1. Incorporer

- Inclure le dossier *remoflety* et importer `remoflety/remoflety.scss`.
- Inclure les variables `remoflety/_variables.scss` dans son fichier de variables (facultatif).
- Modifier les variables.

## 2. Configurer


### 2.1 Les breakpoints

Puisque ce système répond à une problématique responsive, nous allons d'abord définir les breakpoints avec la clé `"bp"`. Il est possible d'ajouter autant de breakpoints que l'on souhaite :
```css
$remoflety: (
    "bp": (480px, 1800px),
    …
);
```

### 2.2 Le font-size de base

La clé `"html"` permet de définir les *font-size* attribués à la racine `<html>`. Chaque valeurs correspond a un breakpoint.
```css
$remoflety: (
    "bp":   (480px, 1800px),
    "html": (14px, 22px),
    …
);
```

### 2.3 Les "paliers"

Il y'a 4 clés spéciales (`"bp"`, `"html"`, `"ratio"` et `"prefix"`). Toutes les autres clés définies sont des paliers de texte libre. Chaque palier pourra être utilisé via le mixin `fs("toto")` ou via une classe créée automatiquement (`.fs-toto`).
```css
$remoflety: (
    "bp":   (480px, 1800px),
    "html": (14px, 22px),
    "toto": (30px, 60px), // Mon 1er palier
    …
);
```

### 2.3 Les valeurs

**Chaque palier peut avoir une seul valeur ou autant de valeurs qu'il y a de breakpoints.** (Les parenthèses sont indispensable quand il y'a plusieurs valeurs).
```css
$remoflety: (
    "bp":   (480px, 1800px),
    "html": (14px, 22px),
    "toto": 30px,         // Est toujours à 30px
    "tata": (20px, 40px), // Grandi progressivement de 20px à 40px.
    …
);
```
Les valeurs de `"bp"` et `"html"` doivent être défnis en pixels. En revanche les valeurs des paliers libre peuvent être définie en :
- `px`
- `rem` (s'il n'y a qu'une valeur, le rem est appliqué, s'il il y'a plusieurs valeur, les rem sont convertis en px en fonction des valeurs de `html` pour permettre la transition fluide).
- `em` et `%` (Ces valeurs non absolues sont appliquées sans transitions fluide).
- *ratio* (sans unités, voir plus-bas).

```css
$remoflety: (
    "bp":   (480px, 1800px),
    "html": (14px, 22px),
    "toto": 30px,         // Est toujours à 30px
    "tata": (20px, 40px), // Grandi progressivement de 20px à 40px.
    "titi": 2rem,         // Est toujours 2rem
    "tutu": (2rem, 3rem), // Grandi progressivement de 28px (14px*2rem) à 66px (22px*3rem).
    "tete": (100%, 120%), // 100% au début, 120% au dernier breakpoint, sans transitions fluides.
    …
);
```


### 2.3 Les ratios d'échèle modulaire

Les rapports de tailles dans la hierarchie doivent être différent sur petit ou grand écran. Écrire toutes les tailles pour chaques breakpoint peut être fastidieux et entrainer des incohérences.

```css
$remoflety: (
    "bp":   (320px, 768px, 1800px),
    "html": (16px, 16px, 22px),
    "h1":   (22px, 38px, 54px), // Really ?
    "h2":   (20px, 28px, 36px), // Really ?
    "h3":   (18px, 21px, 24px), // Really ?
    "h4":   1rem;
    …
);
```
Pour appliquer differents ratios de manière automatique il suffit de renseigner la clé `"ratio"`, puis d'attribuer un chiffre correspondant à l'exposant à chaque clé. Ce chiffre entier correspond à l'exposant du ratio. Exemple : 16px au ratio 1.5 exposant 3 = 14px * 1.5 * 1.5 * 1.5 = 54px;

```css
$remoflety: (
    "bp":    (320px, 768px, 1800px),
    "html":  (16px, 22px),
    "ratio": (1.125, 1.3, 1.5), // Les ratios à chaque breakpoint
    "h1":    3,    // De 22px à 54px Easy !
    "h2":    2,    // De 20px à 36px Easy !
    "h3":    1,    // De 18px à 24px Easy !
    "h4":    0,    // = "html" (de 16px à 22px)
    …
);
```

Puisque la perfection canonique ne correspond pas toujours au attentes dans notre bas monde, il est aussi possible d'écrire les valeurs de ratio avec des décimals.

```css
$remoflety: (
    …
    "ratio": (1.125, 1.3, 1.5),
    "h1":    3.5, // Un petit poil plus grand SVP
    "h2":    2,
    …
);
```

Et bien que l'intéret des ratios est d'écrire peu, il est néamoins possible de mettre plusieurs valeur pour corriger un breakpoint.

```css
$remoflety: (
    …
    "ratio": (1.125, 1.3, 1.5),
    "h1":    (3.5, 3, 3), // Un petit poil plus grand uniquement sur petit écran
    "h2":    2,
    …
);
```

Pour les niveaux inférieurs à 1rem (`0` de ratio), il est préférable de ne pas appliquer d'échelle modulaire, car ce serait le risque de voir le texte diminuer au fur et à mesure de l'agrandissement de l'écran, ce qui n'est sans doute pas souhaitable (tu me crois sur parole ou bien je dois t'expliquer ?). Il est préférable d'utiliser une simple valeur en `rem`.

```css
$remoflety: (
    …
    "h2":    2,
    "h3":    1,
    "h4":    0, // = 1rem
    "h5":    -1, // Peut rapetisser alors que l'écran s'élargit.
    "h6":    .75rem, // Reste raisonnablement petit
    …
);
```

### 2.3 Les classes

Des classes correspondantes aux paliers sont créées automatiquement pour facilter l'intégration. Pour modifier le prefix de ces classes, il faut utiliser la clé `prefix`. Sans réécriture la valeur par défaut est `fs-`.

```css
$remoflety: (
    …
    "prefix" = "fs-",
    "toto": (30px, 60px), // -> .fs-toto { … };
    …
);
```




## 3. Utiliser

Remoflety utilise une formule magique pour gérer les transitions de tailles entre chaques breakpoint :

```css
html {
    font-size: 14px;
}
@media screen and (min-width: 480px) {
    html {
        font-size: calc( 14px + (20 - 14) * (100vw - 480px) / 1320);
    }
}
@media screen and (min-width: 1800px) {
    html {
        font-size: 20px;
    }
}
```

Le font-size de la racine `<html>` est généré automatiquement. Pour les autres paliers libres il faut utiliser les **mixins** ou les **classes correspondantes** (générées automatiquement) :

```css
h1 {
    @include fs("h1");
}
h2 {
    @include fs("h2");
}
```

```html
<div class="fs-h1">
    Gros comme un h1
</div>
```

## 4. Méthodes et conseils

### 4.1 Nommage des paliers.

Libre à vous d'utiliser les noms de paliers de votre choix en fonction des projets. Un système ayant fait ses preuves[Référence nécessaire] est celui des ***dixaines***, où "10" correspond à `<h1>`, "20" à `<h2>`, etc. Un des avantage est de pouvoir mémoriser facilement l'ordre des paliers et leurs affectations respectives. De plus, les dixianes permettent de pouvoir à tout moment créer des paliers intermediaires sans avoir à repenser le système de nommage. Ensuite vous pouvez placer le font-size de base sur le palier que vous souhaitez :

```css
$remoflety: (
    …
    "05":    6,           // Plus grand que H1
    "10":    4,           // H1
    "15":    3.5,         // Entre h1 et h2
    "20":    3,           // H2
    "30":    2,           // H3
    "40":    1,           // H4
    "45":    1.2rem,      // Intermediaire en rem
    "50":    1rem,        // H5 et font-size de base
    "60":    .85rem,      // H6
    "70":    .75rem,      // Small
    "80":    (10px, 12px) // XXS
    …
);
```

```css

h1, .h1 { @include fs("10");}
h2, .h2 { @include fs("20");}
h3, .h3 { @include fs("30");}
h4, .h4 { @include fs("40");}
h5, .h5 { @include fs("50");}
h6, .h6, label { @include fs("60");}
small { @include fs("70");}
```

### 4.2 Intégration Bootstrap et autres framework.

Remoflety devrait pouvoir fonctionner en complément d'autres outils comme Boostrap :
```css
$remoflety: (
    …
    "bp":   (map-get($grid-breakpoints, "xs"), map-get($grid-breakpoints, "lg")),
    "ratio": (1.15, 1.33);
    "html": (15px, 18px),
    "d1": 9,
    "d2": 8,
    "d3": 7,
    "d4": 6,
    "h1": 5,
    "h2": 4,
    "h3": 3,
    "h4": 2,
    "h5": 1,
    "h6": 0,
    …
);
```

```css
.display1 { @include fs("d1"); }
.display2 { @include fs("d1"); }
.display3 { @include fs("d1"); }
.display4 { @include fs("d1"); }
h1, .h1   { @include fs("h1");}
h2, .h2   { @include fs("h2");}
h3, .h3   { @include fs("h3");}
h4, .h4   { @include fs("h4");}
h1, .h1   { @include fs("h1");}
h2, .h2   { @include fs("h2");}
h3, .h3   { @include fs("h3");}
h4, .h4   { @include fs("h4");}
h5, .h5   { @include fs("h5");}
h6, .h6   { @include fs("h6");}
```
