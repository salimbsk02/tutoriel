# tutoriels 

### Merge Request en ligne de commande
- Commencer par créer une branche avec la commande `git checkout -b nouvelle_branche`
- Modifier un fichier
- Utiliser la commande `git add <fichier>` pour l'ajout suivie de la commande `git commit -m "Modification du fichier <fichier>"`
- Utiliser la commande `git push origin nouvelle_branche`
- Pour utiliser la fonctionnalité du merge request à partir du terminal, il faut tout d'abord intaller la commande `hub`
- Utiliser ensuite la commande `hub pull-request`

Hub est un outil qui enveloppe git afin de l'étendre avec des fonctionnalités supplémentaires qui le rendent meilleur lorsque vous travaillez avec GitHub.

### Pipeline passed
Le fichier .gitlab-ci.yml indique au runner de GitLab quoi faire. Par défaut, il exécute un pipeline en trois étapes: build, test, and deploy. Il arrive parfois que le test se fait avant que le build soit complété. Pour remédier à se problème, il suffit de preciser que le test doit être fait après le build. 

Dans le cas du TP1 le .gitlab-ci.yml était constituerde :

```before_script:
    - apt-get update -qq
    - git clone https://github.com/sstephenson/bats.git /tmp/bats
    - mkdir -p /tmp/local
    - bash /tmp/bats/install.sh /tmp/local
    - export PATH=$PATH:/tmp/local/bin

build:
    stage: build
    script:
        - make

test:
    stage: test
    script:
        - make test
````

Donc le build fait appel au make et le test fait appel au make test. Le problème pour les personnes qui ont eu un pipeline failed était que lorsqu'on fait appel au make test, ils ne précisaient pas le fichier qu'ils devaient tester. Il suffisait simplement d'ajouter tournament à test:.

```
tournament: tournament.o
	 gcc -std=c99 -o tournament tournament.o

tournament.o: tournament.c
	gcc -c tournament.c

clean:
	rm -rf *.o *.html *.swp

html:
	pandoc -f markdown -t html5 -o README.html README.md -c style.css

.PHONY: test

test: tournament
	bats check.bats
```