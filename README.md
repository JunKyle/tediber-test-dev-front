# tediber-test-dev-front

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Objectif

Le but de ce test est de reproduire la maquette jointe afin de tester vos connaissances techniques mais surtout la façon dont vous appréhendez un projet.
Vous devez utiliser le mock API fournie pour peupler dynamiquement la page et créer un sélecteur fonctionnel.
Le sélecteur doit faire au maximum 100vh de hauteur, la partie droite doit donc être scrollable. Une fois le scroll fini, le reste de la page peut scroller. La page ne doit pas être scrollable tant que l'utilisateur n'a pas scrollé la partie droite du sélecteur.
(Comme sur la page https://www.tediber.com/pack-lit-2-places par exemple)

Vous devez faire l'intégration de la maquette desktop uniquement (1280px et plus), le header et footer peuvent être des images, pas besoin d'en faire l'intégration.

### Pré-requis

- Utilisez VueJS (ou un framework utilisant VueJS)
- Utilisez les données APIs fournies, certaines informations peuvent légèrement différer de la maquettes (notamment le prix)
- Le sélecteur a pour vocation à être envoyé au backend lors du clic sur le bouton "Ajouter au panier", il faudra donc prévoir le payload simple indiquant le code du produit correspondant à la sélection faite sur le sélecteur (libre à vous sur le format).
- Le rendu doit inclure le code source et un moyen de voir le résultat (hébergement, commande de build, etc.)

### API

- l'API renvoie le produit canapé, dont le code est "sofa-simple"
- Les différentes combinaisons possibles sont des variants
- Les différentes options sont dans options

### En bonus

- La page ne doit pas être responsive, mais vous pouvez réflechir à la version mobile, nous échangerons à ce sujet lors de l'entretien.
- Vous êtes libre de modifier la mise en page si vous estimez que cela est pertinent.

### Notes

- Cette maquette existe déjà sur le site et votre code ne sera pas utilisé
- Vous pouvez utiliser les images et textes fournis
- Prenez le temps qu'il vous faudra
- N'hésitez pas à nous contacter si vous avez une question :)

