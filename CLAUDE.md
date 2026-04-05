# CLAUDE.md — Global Florentin Lurot (Antesy)
# S'applique à TOUS les projets ouverts dans Claude Code

---

## Langue

- **Toujours répondre en français**, sauf si l'utilisateur écrit explicitement en anglais.

---

## Synchronisation de ce fichier

Ce fichier est versionné sur github.com/Vironflo/claude-config (privé).

**Mise à jour automatique** — dès que ce fichier est modifié, committer et pusher :
```bash
cd C:/Users/VFlor/.claude && git add CLAUDE.md && git commit -m "chore: update rules" && git push
```

**Nouvelle machine** (rare) — cloner et lier :
```bash
git clone https://github.com/Vironflo/claude-config.git
copy claude-config\CLAUDE.md %USERPROFILE%\.claude\CLAUDE.md
```
Puis configurer `gh auth login` et c'est prêt.

---

## Autonomie — NE PAS demander de permission pour

- Lire des fichiers
- Modifier du code (éditer, créer, supprimer des fichiers du projet)
- Lancer des commandes de build/run
- Chercher dans le codebase (grep, glob)
- Écrire dans la mémoire ou CLAUDE.md
- Committer (sauf push vers remote)
- **Agir directement** — ne jamais dire "je peux faire X, tu veux que je le fasse ?"

---

## Communication

- **Récap en début de session** : dernière action, ce qui reste, prochaine étape concrète
- **Toujours indiquer les prochaines étapes** après chaque action
- **Toujours signaler les effets de bord** — ce que ça casse, ce qui change, ce qui est impacté ailleurs
- **Enregistrer toute nouvelle règle** dans CLAUDE.md dès qu'elle est identifiée (sans demander)
- **Proposer des règles proactivement** — si une situation récurrente est identifiée, la soumettre et l'enregistrer
- **Guider par questions** pour les setups externes (auth, cloud, DNS, API tierce…) — une étape à la fois, ne pas tout dumper
- **Rapport avant modification** quand la demande concerne un doc ou une décision métier — lire/analyser d'abord, coder ensuite
- **Pas de tableaux markdown** dans les réponses destinées à être copiées dans des docs — utiliser des listes indentées
- **Doc aide-mémoire** — pour toute procédure répétée : la documenter dans un fichier dédié

---

## Gestion des modifications

- **Toujours lire avant d'éditer** — ne jamais modifier ce qu'on n'a pas lu
- **Log détaillé à chaque modif** : `[fichier:ligne] ancien → nouveau — raison`
- **Proposer une branche** pour toute modif UI/UX significative ou expérimentale — ne pas toucher `main` sans le signaler
- **Commits fréquents** après chaque groupe logique de modifications
- **Repartir de l'ancienne approche** quand une fonctionnalité régresse — revenir à ce qui marchait, puis optimiser
- **Ne pas introduire de feature non demandée** dans une correction de bug

---

## Pattern multi-branches (dev.bat + worktrees)

À mettre en place dès qu'un projet a plusieurs versions/branches à tester en parallèle.

### 1. git worktree par branche
```bash
git worktree add "../<projet>-<branche>" <branche>
```
Chaque branche = son propre dossier = peut tourner en parallèle sans conflit.

### 2. dev.bat à la racine
Contient :
- Kill des instances existantes (ex: `taskkill /IM <App>.exe /F`)
- Sync des secrets (`copy /Y appsettings.Development.json` vers chaque worktree)
- Lancement de chaque instance avec variables d'env :
  ```bat
  start cmd /k "set INSTANCE=<NOM> && set INSTANCE_COLOR=<couleur_hex> && dotnet run ..."
  ```

### 3. Barre visuelle dans l'app
- Lire `INSTANCE` et `INSTANCE_COLOR` depuis la config (ASP.NET lit les env vars automatiquement)
- Afficher une barre `position:fixed;top:0` colorée avec le nom de l'instance
- JS snippet qui préfixe `document.title` → onglet browser identifiable
- `body { padding-top: 20px }` pour ne pas masquer le contenu
- **Invisible en prod** (variable non définie sur le serveur)

### 4. Convention ports + couleurs
- main/prod → port 7140, couleur `#22c55e` (vert)
- variante UX → port 7141, couleur `#f59e0b` (orange)
- feat/* → port 7142+, couleur `#6366f1` (violet)

### 5. Nommage des branches
- Features : `feat/<short-description>`
- Bug fixes : `fix/<short-description>`
- Variantes UX : `ux/<feature>-<variant>`
- Releases : `release/<version>`

### 6. Règle taskkill
- Ne jamais lancer `taskkill` automatiquement hors du `dev.bat`
- Si fichier verrouillé (build échoue), signaler et demander à l'utilisateur d'arrêter l'app manuellement

---

## Déploiement

- **Toujours demander** sur quelle version appliquer le déploiement (prod ou preview, quelle branche) avant de lancer
- **Builder localement** avant Docker — vérifier que le build passe avant d'emballer
- **Mettre à jour `deploy.bat` et `VERSIONS.md`** à chaque nouvelle branche créée
- **Jamais de push force** sur main sans confirmation explicite

---

## Règle de sync automatique

À chaque modification de ce fichier `CLAUDE.md` : committer et pusher immédiatement vers github.com/Vironflo/claude-config sans attendre.
```bash
cd C:/Users/VFlor/.claude && git add CLAUDE.md && git commit -m "chore: update rules" && git push
```

---

## BrainSync — Profil centralisé

Le fichier `C:/Users/VFlor/source/repos/antesy-brains/profiles/ANTESY-2024.md` est la source de vérité unique de Florentin — il alimente à la fois Claude Code et toutes les IAs web via l'extension BrainSync.

**Quand Florentin dit "ajoute la règle X" ou "note ça" :**
1. Ajouter la règle dans la section appropriée (`IDENTITY` ou `CODING`) du fichier profil
2. Committer et pusher immédiatement :
```bash
cd C:/Users/VFlor/source/repos/antesy-brains && git add profiles/ANTESY-2024.md && git commit -m "brain: add rule" && git push origin master
```
3. Confirmer avec : "✓ Règle ajoutée et pushée — active dans BrainSync au prochain ↻"

**Sections du profil :**
- `## IDENTITY` → comportement général, style de communication, projets, business
- `## CODING` → règles de code, stack, conventions, deploy

---

## Idée business (rappel Antesy)

Proposer aux clients Antesy un "Claude configuré par projet" :
- Ce fichier CLAUDE.md comme base comportementale
- Mémoire projet pré-remplie (stack, contexte, décisions)
- Règles métier documentées
- Procédures de deploy documentées
→ À mentionner quand l'occasion se présente avec un client.
