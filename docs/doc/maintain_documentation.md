# Maintenance de la doc

Ce document éxplique comment maintenir et mettre à jour la documentation.

La document utilise:

- MkDocs ([https://www.mkdocs.org/](https://www.mkdocs.org/)) pour générer la documentation.
- Mike ([https://github.com/jimporter/mike](https://github.com/jimporter/mike)) _un plugin de MkDocs_, pour le versionning

## Initialisation

1. Clonner le repository [A320_doc](https://github.com/rguilbeau/A320_doc.git)

2. Créer un environnement virtuel:
```bat
python -m venv venv
```

3. Activer cette evironnement
```bat
venv\Scripts\activate
```

4. Mettre à jour les dépendances python
```bat
pip install -r requirements.txt
```

## Preview

### Vscode

Dans vscode, le raccourci clavier `Ctrl + Shift + V` permet d'afficher un visualiseur markdown.

> **Note:** Le visualiseur de vscide ne supporte pas le rendu des graphiques Mermaid. Le plugin **Markdown Preview Enhanced** peut être utilisé pour améliorer le rendu markdown.

### Website

Pour vérifier les changement en local, utiliser la commande suivante:
```
mkdocs serve
```

Cette commande démarre un serveur local (par défaut [http://127.0.0.1:8000](http://127.0.0.1:8000)) pour vérifier le rendu réel de la documentation avant sa publication.


## Mise à jour de la doc

### Navigation

Les fichiers source de la doc sont en Markdown. Ces fichiers sont placés dans le répertoire `/doc`.

La gestion de la navigation se fait manuellement depuis le fichier de configuration `mkdocs.yml` dans la section `nav`.

### Assets

Tout les fichiers supplémentaire (images, fichiers...) doivent être placés dans le dossier `docs/_assets/`

**Utilisation:**

```markdown
![Example Image](_assets/images/example.png)
[Download File](_assets/files/manual.pdf)
```

> **Note:** le dossier `docs` ne doit pas être pécisé dans le markdown

### Diagrammes

La création des diagrammes peut se faire à l'aide de ces deux outils:
- Mermaid
- Drawio

#### Mermaid

Pour utiliser Mermaid:

- Créer le diagramme grâce à l'editeur en ligne: [https://mermaid.live/edit](https://mermaid.live/edit)
- Placer le code mermaid dans le markdown comme un code block avec le language mermaid:

        ```mermaid
        graph LR
            A[Square Rect] -- Link text --> B((Circle))
            A --> C(Round Rect)
            B --> D{Rhombus}
            C --> D
        ```

#### Drawio

Pour utiliser Drawio, télécharger l'application loudre: [Drawio Desktop app](https://github.com/jgraph/drawio-desktop/releases)

Créer un digramme avec l'application et le sauvegarder en `MyDiagramm.drawio` dans le répertoire `docs/_assets_/where/you/want`

Pour l'afficher dans une page:

```plaintext
![Page-Name](../_assets/where/you/want/MyDiagramm.drawio)
```

> **Note:** le dossier `docs` ne doit pas être pécisé dans le markdown

> **Note:** Le visualiseur de Vscode ne prend pas en charge cette affichage, il est donc impossible de prévisualiser depuis vscode




## Publication

Pour publier la documentation, utiliser cette commande:
```bat
mike deploy --push v1.0
```

Cette commande met à jour la documentation sur la version précisé dans la commande.


Pour choisir la version qui sera affiché par défaut utiliser cette commande:
```bat
mike set-default --push v1.0
```

> **Note:** Ces commandes mettent à jour la branche `gh-pages` du repository. Github est configuré pour afficher le site depuis cette branche.