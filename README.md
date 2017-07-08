# Remoflety

## [Demo here](http://www.hugobouquard.com/demo/)

## Responsive, modular & flexible typography

- Base REM Responsive calculée selon le viewport.
- Ratios des niveaux de font-size augmentés aux breakpoints
- Classes et mixins de font-size : `@include fs(10)` = `.fs-10` = font-size du `<h1>`

## Incorporer

- Inclure le dossier *remoflety* et importer `remoflety/remoflety.scss`.
- Inclure les variables `remoflety/_variables.scss` dans son fichier de variables (facultatif).
- Modifier les variables (supprimer `'!default` si les variables sont copiées).

## Configurer

![Alt text](https://www.smashingmagazine.com/wp-content/uploads/2016/05/advanced-calc-800-opt.png "Optional title")



### ~~Variables du font-size global~~
~~La typographie est réglé en *rem*, le font-size attribué à `<html>` sert donc de base. Il évolue selon des breakpoint et un calcul incluant la largeur de l'écran (*vw*)~~
- ~~Font-size fixe de base initial (mobile-first) : `$fs-xs`~~
- ~~Font-size responsive minimum : `$fs-min`~~
- ~~Font-size responsive maximum : `$fs-max`~~
- ~~Breakpoint correspondant au font-size responsive minimum : `$fs-min-bp`~~
- ~~Breakpoint correspondant au font-size responsive maximum : `$fs-max-bp`~~

### ~~Variable des ratios d'échelle modulaires~~
~~Un ratio permet de définir une harmonie de taille entre les différentes tailles typographiques (comme les titres).
Au départ les niveaux ont peu de différences de taille les uns par rapport aux autres grâce à un petit ratio. Des ratios plus grand correspondant à des breakpoints sont donc définis:~~


### Tableau de Font-size global responsive :
Nouveau tableau unique pour configurer les variables global responsives.
```css
$fs-bigmap: (
    (
        break: 320px,
        fsize: 15px,
        ratio: 1.067
    ),
    (
        break: 1800px,
        fsize: 20px,
        ratio: 1.5
    )
);
```

### Variable des niveaux de font-size

Définir un jeu de niveaux de font-size et leurs niveaux correspondants sur l'échelle modulaire. Pour une simplicité d'utilisation les chiffres doivent correspondre au niveau de titrage, exemple 10 pour les h1, 20 pour les h2, etc. Chaque niveau sera applicable grâce à un mixin Scss `@include fs(10)`, ou une classe HTML `.fs-10`.
Ces ratios peuvent être définis en nombre entier (`2`) ou décimal (`2.7`).
Il est aussi possible de définir des valeurs en `rem` plutôt qu'en ratio (voir explication sur les niveaux inférieurs), en `em` pour des éléments héritant, ou en `px` pour supprimer tout effet responsive.

```css
$fs-map: (
  10: 4,
  20: 3,
  30: 2,
  40: 1,
  50: 0,
  60: .75rem
);
```

- ici c'est le niveau "50" qui a été définie à un font-size de 1rem (car aucun ratio ne lui est appliqué), il est donc égual au font-size de base de la page.
- Le niveau "40" (qui correspondra aux `<h4>`) est multiplé 1x par le ratio.
- Le niveau "30" (qui correspondra aux `<h3>`) est multiplé 2x par le ratio, soit : `1rem * ratio * ratio`. Etc.
- Pour les niveaux inférieurs à 1rem (= 0x le ratio), il est préférable de ne pas appliquer d'échelle modulaire, car ce serait le risque de voir le texte diminuer au fur et à mesure de l'agrandissement de l'écran, ce qui n'est sans doute pas souhaitable (tu me crois sur parole ou bien je dois t'expliquer ?). Il faut donc utiliser directement des valeurs en `rem`.
- Le niveau "60" (qui correspondra aux `<h6>`) est simplement de .75rem.
- Il est aussi possible de définir des niveaux intermédiaires, ainsi que des niveaux plus haut ou plus bas que 10 et 60 :

```css
$fs-map: (
  05: 6,
  10: 4,
  15: 3.5,
  20: 3,
  …
  70: .5rem;
);
```

### Autres variables

- `$debug`: si `true` affiche le Breakpoint actuel et le ratio correspondant.
- `$fs-class-prefix`: Prefix des noms des classes de font-size '.fs-XX'. Default: `'fs-'`;


## Intégration

Lorsque les variables sont configurées, les niveaux de font-size sont disponibles est exploitables avec le mixin `fs(XX)` ou les classes `.fs-XX`.

```scss
.toto {
  @include fs(10);
}
```

```html
<div class="fs-10">…</div>
```

Il est aussi préférable d'initialiser sa feuille de style :

```css
/* remoflety/_assignment.scss */

h1, .h1 { @include fs(10);}
h2, .h2 { @include fs(20);}
h3, .h3 { @include fs(30);}
h4, .h4 { @include fs(40);}
h5, .h5 { @include fs(50);}
h6, .h6, label { @include fs(60);}
small { @include fs(70);}
```

## TODO

- Préfixer les noms de variables et mixin pour éviter les conflits ?
- ~~Améliorer l'application des ratios avec calc(…) pour que ce soit progressif plutôt que par paliers.~~ *FAIT*
- ~~Si cela fonctionne, les variables du REM et des ratios pourront être uniformisées dans un tableau unique~~ *FAIT*
- Tester si on définie les clés en texte (`.fs-label`).
- Tester si on définie les BP en em ou autres.
- Ajouts de paliers intermédiaires de font-size et de ratio ? : `bp: (320px, 768px, 1200px), fs: (14px, 15px, 22px),`.
- Possibilié de mettre plusieurs ratio (ou unités) pour être plus souple :
```css
$fs-map: (
  10: (4, 3),
  20: 3,
  30: (22px, 36px),
  30: (2rem, 2.5rem),
);
```
- Fusionner les deux tableaux pour plus de simplicité et pouvoir faire plusieurs groupes ? :

```css
$fs-variables: (
    'fs-': (
        bp: (320px, 1200px),
        fs: (14px, 16px),
        ratio: (1.1, 1.5),
        niveaux: (
            10: 4,
            20: 3,
            30: (2.1, 2),
            …
        )
    ),
    'fs-sidebar-': (
        bp: (768px, 1200px),
        fs: (12px, 13px),
        ratio: (1.1, 1.5),
        niveaux: (
            10: 4,
            20: 3,
            30: (2.1, 2),
            …
        )
    )
);
```

## Bugs
- Sur la demo de la v0.1: .fs-45 vari trop autour de 824px de VW. Ressemble à une modification du letter-spacing. je pense que c'est dû à la typo san-franscico (mac-os) qui a des réglages différents celon le font-size.
