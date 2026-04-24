# The Ridery — Simulateur Devis (Wing + Kite)

Double simulateur interne pour envoyer rapidement des devis clients.

**URL publique (après push + GitHub Pages) :**
`https://<user>.github.io/theridery-marketing/`

## Fonctionnement

1. Le client choisit **Wingfoil** ou **Kitesurf**
2. Renseigne : sexe, poids, taille, niveau, budget libre
3. Reçoit un **setup recommandé** (tailles d'aile, planche, foil, barre selon les tables Ridery)
4. Voit un **catalogue de matos en stock**, automatiquement filtré via `theridery.com/products.json` (ruptures exclues). Items marqués **RECO** = correspondent à la taille idéale.
5. Coche les articles qui l'intéressent
6. Renseigne email → reçoit un lien direct `/cart/<variantId>:1,...` qui ouvre le panier déjà rempli sur theridery.com

## Déploiement GitHub Pages

```bash
# depuis ce dossier
gh repo create theridery-marketing --public --source=. --push
gh api -X POST repos/:owner/theridery-marketing/pages -f source[branch]=main -f source[path]=/
```

## Configuration (éditable en haut du `<script>`)

```js
var CFG={
  STORE:'https://theridery.com',
  PRODUCTS_URL:'https://theridery.com/products.json?limit=250',
  STAFF_EMAIL:'marketing@theridery.com',
  FORMSPREE_ID:null   // mettre 'xxxxxx' si tu crees un Formspree public
};
```

### Envoi auto sans ouvrir le mail client

Par défaut : `mailto:` ouvre le client mail du staff pré-rempli (il clique Envoyer).

Pour un envoi **100% automatique** :
1. Crée un form gratuit sur https://formspree.io avec `marketing@theridery.com`
2. Copie l'ID du form (ex. `xvojdabc`)
3. Colle-le dans `CFG.FORMSPREE_ID`

## Notes

- **Filtre stock** : la page fetche `/products.json` au chargement et ne montre **que** les produits avec au moins 1 variant `available:true`. Shopify expose cette route en CORS ouvert.
- **Catégorisation auto** : par mots-clés dans `product_type` et `tags`. Si un produit est mal rangé, ajuste les `kw` dans `CATEGORY_RULES`.
- **Test local** : ouvrir directement `index.html` peut bloquer le fetch par CORS navigateur. Serve avec `python3 -m http.server 8080` puis http://localhost:8080.
- **Tables de tailles** : reprises verbatim des simus en prod (wingfoil + kitesurf). À éditer dans `WING_SIZING` / `KITE_SIZING`.
