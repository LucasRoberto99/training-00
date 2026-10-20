# Git workflow

Ce projet suit le modèle **GitFlow** et les **Conventional Commits**.

## Branches

| Branche      | Rôle                                              | Durée de vie |
|--------------|---------------------------------------------------|--------------|
| `main`       | Code en production, uniquement des versions taggées | Permanente   |
| `develop`    | Intégration des fonctionnalités terminées          | Permanente   |
| `feature/*`  | Nouvelle fonctionnalité                            | Temporaire   |
| `release/*`  | Préparation d'une version (`release/1.2.0`)        | Temporaire   |
| `hotfix/*`   | Correctif urgent sur `main` (`hotfix/1.2.1`)       | Temporaire   |

Règles :
- Ne jamais committer directement sur `main` ou `develop`.
- Une `feature/*` part de `develop` et revient dans `develop`.
- Une `release/*` part de `develop`, se merge dans `main` (avec tag) **et** dans `develop`.
- Un `hotfix/*` part de `main`, se merge dans `main` (avec tag) **et** dans `develop`.

## Démarrer une fonctionnalité

```bash
git checkout develop
git pull origin develop
git checkout -b feature/nom-court
```

Terminer :

```bash
git push -u origin feature/nom-court
# Ouvrir une Pull Request vers develop sur GitHub, puis merger (squash ou merge commit)
```

## Release

```bash
git checkout develop
git checkout -b release/1.2.0
# ajustements de version / changelog uniquement
git checkout main && git merge --no-ff release/1.2.0
git tag -a v1.2.0 -m "Version 1.2.0"
git checkout develop && git merge --no-ff release/1.2.0
git branch -d release/1.2.0
git push origin main develop --tags
```

## Hotfix

```bash
git checkout main
git checkout -b hotfix/1.2.1
# correctif
git checkout main && git merge --no-ff hotfix/1.2.1
git tag -a v1.2.1 -m "Version 1.2.1"
git checkout develop && git merge --no-ff hotfix/1.2.1
git branch -d hotfix/1.2.1
git push origin main develop --tags
```

## Conventional Commits

Format : `<type>(<scope optionnel>): <description>`

| Type       | Usage                                      |
|------------|--------------------------------------------|
| `feat`     | Nouvelle fonctionnalité                    |
| `fix`      | Correction de bug                          |
| `docs`     | Documentation uniquement                   |
| `refactor` | Refactorisation sans changement de comportement |
| `test`     | Ajout ou modification de tests             |
| `chore`    | Maintenance, configuration, dépendances    |

Exemples :

```
feat(auth): ajouter la connexion par token
fix: corriger le calcul de la moyenne vide
chore: mettre à jour les dépendances
```

Un changement cassant s'indique avec `!` ou un pied de message `BREAKING CHANGE:`.

## Pull Requests

- Une PR = un sujet, la plus petite possible.
- Titre au format Conventional Commits.
- Au moins une revue approuvée avant merge.
- CI verte avant merge.
