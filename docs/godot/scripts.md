---
title: Scripts
layout: default
parent: Godot
---

# Scripts

Différents scripts utiles pour la gestion d'un projet Godot

## Nouveau projet

```bash
git fetch upstream 4.7 # Mettre à jour ou installer une nouvelle version
git switch -c <project-name> upstream/4.7~1
git push -u origin <project-name>
```

## Mettre à jour

Généralement, on veut rester sur la même version tout du long, mais si c'est juste pour avoir le dernier patch, c'est raisonnable.

### Sans modifications

Le moteur n'a pas encore été modifié, donc on reprend juste les deux derniers commits.
```bash
git fetch upstream 4.7
git reset --hard upstream/4.7
```

### Avec modifications

Le moteur a été modifié, il faut donc reprendre tous les commits intermédiaires.
```bash
git fetch upstream 4.7
git rebase upstream/4.7~1
```