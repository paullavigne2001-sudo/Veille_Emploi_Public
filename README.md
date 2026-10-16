# Veille Emploi Public — PWA

PWA en français pour suivre les offres du flux RSS Choisir le service public, filtrer les offres, enregistrer des recherches, gérer des favoris et suivre des candidatures.

## Fonctionnalités présentes
- Interface mobile installable (PWA), manifeste, icône et cache de l'interface.
- Endpoint serveur `/api/offers` qui récupère le RSS et le convertit en JSON (nécessaire pour éviter la dépendance à un accès RSS direct depuis le navigateur).
- Filtres par mots-clés, domaine et départements (42, 43, 69, 38, 63, 71).
- Alertes enregistrées, favoris, statut de candidature et notes.
- Export/import JSON et stockage local sur l'appareil.
- Vérification au démarrage, à la demande et périodiquement tant que l'application est ouverte.

## Limites importantes
- L'accès direct à la source RSS renvoie du XML. L'outil de consultation web a signalé `text/xml` non pris en charge, ce qui ne permet pas de valider le contenu complet ici. L'API vérifie à l'exécution que le flux contient des éléments RSS et renvoie une erreur explicite sinon.
- La fréquence de 3 heures est appliquée lorsque l'application reste ouverte. Android ne garantit pas qu'une PWA fermée s'exécute à intervalle régulier.
- Les notifications de cette version sont locales et ne sont déclenchées que pendant que l'application fonctionne et que la permission est accordée. Pour des notifications fiables quand l'app est fermée, il faut un serveur planifié (cron), des abonnements Web Push et des clés VAPID. Aucun service payant n'est imposé par le code, mais l'hébergement gratuit doit permettre ces tâches.
- Le RSS général peut ne pas inclure la localisation sous une forme structurée. Le filtrage départemental s'appuie sur le texte disponible et exclut les offres sans localisation détectable, pour éviter de présenter de faux résultats locaux.

## Déploiement gratuit avec Vercel
1. Créer un dépôt GitHub et y envoyer le contenu de ce dossier.
2. Importer le dépôt dans Vercel (https://vercel.com/new).
3. Déployer comme projet Vercel sans commande de build; la route `/api/offers` est une fonction serverless Node.
4. Tester `https://VOTRE-DOMAINE/api/offers` : la réponse doit être JSON avec `count` supérieur à zéro et `items`.
5. Ouvrir l'URL en HTTPS dans Chrome sur Android, menu ⋮, puis « Installer l'application » ou « Ajouter à l'écran d'accueil ».

## Vérification RSS
Le lien source configuré est :
`https://place-ep-recrute.talent-soft.com/handlers/offerRss.ashx?LCID=1036`

Tester l'API avant de considérer la veille opérationnelle. Si le XML utilise une structure non prévue ou ne contient pas les champs attendus, adapter `api/offers.js` à l'exemple XML réel.
