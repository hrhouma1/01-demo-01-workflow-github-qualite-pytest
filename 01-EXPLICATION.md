# Démo 01 — Expliquée simplement

> Objectif : comprendre, sans rien connaître au départ, **pourquoi on teste du code**
> et **comment GitHub vérifie ce code tout seul** à chaque modification.

---

## 1. L'idée en une phrase

On écrit un petit programme qui **calcule la moyenne de notes**, et on demande à
l'ordinateur de **vérifier automatiquement** qu'il donne toujours les bonnes réponses.

---

## 2. Une image pour comprendre

Imaginez une **calculatrice** que vous venez de fabriquer.

Avant de la vendre, vous voulez être sûr qu'elle ne se trompe pas. Alors vous
préparez une **liste de contrôles** :

- « 2 + 2, est-ce que ça donne bien 4 ? » ✅
- « 10 ÷ 0, est-ce que ça affiche bien une erreur ? » ✅

Ces contrôles, en informatique, s'appellent des **tests**.
Chaque fois que vous modifiez la calculatrice, vous **rejouez toute la liste**
pour vérifier que rien n'est cassé. C'est exactement ce que fait ce projet.

---

## 3. Ce que fait le programme (le dossier `app/`)

Dans le fichier `app/notes.py`, il y a 4 petites « machines » (on dit des *fonctions*) :

| La fonction | Ce qu'elle fait | Exemple |
|---|---|---|
| `valider_note` | Vérifie qu'une note est correcte (entre 0 et 100) | `120` → refusé |
| `calculer_moyenne` | Fait la moyenne d'une liste de notes | `[80, 90, 100]` → `90` |
| `determiner_mention` | Donne une appréciation | `90` → « Excellent » |
| `est_reussi` | Dit si c'est réussi (60 ou plus) | `59` → non |

Une *fonction*, c'est simplement une **recette** : on lui donne quelque chose en
entrée (des notes), elle nous rend un résultat (la moyenne).

---

## 4. Les tests (le dossier `tests/`)

Dans `tests/test_notes.py`, on a écrit la fameuse **liste de contrôles**. Exemple :

```python
def test_calculer_moyenne_valide():
    resultat = calculer_moyenne([80, 90, 100])
    assert resultat == 90.0
```

À lire comme une phrase :

> « Je calcule la moyenne de 80, 90 et 100. **J'affirme** (`assert`) que le
> résultat **doit** être 90. »

Si le résultat est bien 90 → le test **passe** (vert ✅).
Si le résultat est différent → le test **échoue** (rouge ❌) et on est prévenu.

Pour lancer tous les contrôles d'un coup, on tape une seule commande :

```bash
pytest
```

Résultat attendu : `13 passed` (13 contrôles réussis).

---

## 5. La partie « magique » : GitHub vérifie à votre place

Jusqu'ici, c'est **vous** qui lancez `pytest` sur votre ordinateur.
Mais on peut demander à **GitHub** de le faire automatiquement.

C'est le rôle du fichier `.github/workflows/ci.yml`.
Pensez-y comme à une **consigne laissée à un assistant** :

> « À chaque fois que quelqu'un envoie du nouveau code, installe Python,
> installe les outils, et relance TOUS les tests. Préviens-moi si quelque chose casse. »

Cet assistant automatique a un nom : **GitHub Actions**.
Le fait de tester le code automatiquement à chaque envoi s'appelle l'**intégration continue** (CI).

---

## 6. Comment ça se passe concrètement

1. Vous modifiez le code sur votre ordinateur.
2. Vous l'envoyez sur GitHub (commande `git push`).
3. GitHub voit le fichier de consignes et lance les tests **tout seul**.
4. Dans l'onglet **Actions** de GitHub, une pastille apparaît :
   - 🟢 **verte** = tout fonctionne ;
   - 🔴 **rouge** = quelque chose est cassé, à corriger avant de continuer.

---

## 7. La petite expérience à faire en classe

1. Lancez `pytest` → tout est vert.
2. Dans `app/notes.py`, remplacez volontairement `>=` par `>` à la dernière ligne.
3. Relancez `pytest` → un test devient **rouge** 🔴.

Vous venez de voir, en direct, qu'un test **attrape une erreur** que l'oeil
humain aurait pu laisser passer. Remettez `>=` et tout redevient vert. ✅

---

## 8. Les 3 mots à retenir

- **Test** : un contrôle automatique qui vérifie qu'un bout de code donne la bonne réponse.
- **pytest** : l'outil qui lance tous les tests d'un coup.
- **CI (intégration continue)** : GitHub relance ces tests automatiquement à chaque envoi de code.

> Ce projet est le **niveau 1**. Les démos 02 et 03 ajouteront une vraie
> application connectée à une base de données et lancée dans un conteneur.
