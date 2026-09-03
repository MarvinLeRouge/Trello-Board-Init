🇫🇷 Version française | [🇬🇧 English version](SECURITY.md)

---

# Politique de sécurité

## Signaler une vulnérabilité

Ceci est un petit projet personnel sans équipe de sécurité dédiée. Si vous découvrez un problème de sécurité, merci d'ouvrir une [issue GitHub](https://github.com/MarvinLeRouge/Trello-Board-Init/issues) décrivant le problème. Évitez d'inclure de véritables clés API, tokens ou autres secrets Trello dans le rapport.

Il n'y a pas de garantie de délai de réponse fixe, mais les rapports seront examinés et traités dès que possible.

## Gestion des credentials

Cet outil lit les credentials de l'API Trello (`TRELLO_API_KEY`, `TRELLO_TOKEN`) depuis un fichier `.env` local, chargé via `python-dotenv`. Pour les garder en sécurité :

- Ne jamais committer `.env` — il est exclu par `.gitignore` ; seul `.env.example` (un modèle sans vraies valeurs) est versionné.
- Traitez votre token Trello comme un mot de passe : il donne un accès en lecture/écriture à vos boards. Régénérez-le depuis vos [paramètres de compte Trello](https://trello.com/app-key) s'il a été exposé.
- Évitez de copier de vrais credentials dans des notes brouillon, des logs de conversation, ou tout fichier non couvert par `.gitignore`.

## Périmètre

Ce projet ne communique qu'avec l'API REST officielle de Trello via HTTPS, via la librairie `requests`. Il ne fait tourner aucun serveur et n'expose aucun point d'entrée réseau qui lui soit propre.
