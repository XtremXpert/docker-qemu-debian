## docker-qemu-debian

# English

Version française à la suite de la version anglaise / French version follow the english version.

### Building Docker images for ARM anywhere

The long explaination can be found on this [Resin.io blog post ](https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/) ( you are awerome guys) but the short answer is :

1. Build a static version of qemu for ARM
2. Build a script to start qemu in the Docker and clean it up at the end

The build.sh script is use to build resin-xbuild.go in the resulting bin/resin-xbuild (and should eventualy get build in the image itself).  The qemu-arm-static binary actualy come from the (Resin.io Github repository)[https://github.com/resin-io-projects/armv7hf-debian-qemu]  (should also eventualy be build from source in the image too, but the bigest problem is the cleanup after building it to make the image slimer).

All other files in the images are in fact symbolic link to either the resin-xbuild go script or the shell itself.

#### How to use it

If you know Dockerfile basic, all you have to do is to start from that image and just before your standard your code you need to run the cross-build-start script and just after your last line of code you need to run cross-build-end to clean it up.  So it look like this :

```Dockerfile
FROM xtremxpert/docker-qemu-debian

RUN [ "cross-build-start" ]

RUN apt-get update  
RUN apt-get install python  
RUN pip install virtualenv

   ...

RUN [ "cross-build-end" ]  
```

Then try an auto-build on Hub Docker and enjoy...

# Français

### Construire une image Docker pour ARM n'importe où

L'explication longue est disponible (en anglais) sur le [blog de Resin.io](https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/) (Vous êtes cool resin.io!) mais la réponse courte est  :

1. Construire une version statique de qemu pour ARM
2. Faire un script pour lancer qemu dans  Docker en clean it up at the end

Le script build.sh est utilisé pour compilé resin-xbuild.go pour en faire l'exécutable bin/resin-xbuild (devrait éventuellement être contruit dans l'image en non sur l'hôte).  l'exécutable qemu-arm-static binary provient actuellement du (dépôt Github de Resin.io)[https://github.com/resin-io-projects/armv7hf-debian-qemu]  (devrait également être compiler dans l'image, mais le gros problème vient du ménage à faire après la compilation pour permettre à cette image de demeurer aussi petite que possible).

Tous les autres fichiers dans cette images sont en fait des liens symboliques soit vers le fichier resin-xbuild ou l'interpréteur de commandes même.

#### Comment l'utiliser?

Si vous maîtrisez les bases des fichiers Dockerfile, tout ce que vous avez à faire, c'est d'utiliser cette image comme source et d'ajouter juste avant votre première commande l'exécution du script cross-build-start script et d'ajouter à le fin de votre fichier Dockerfile l'exécution du script cross-build-end pour tout bien nettoyer en quittant.  Ça ressemble donc à ça :

```Dockerfile
FROM xtremxpert/docker-qemu-debian

RUN [ "cross-build-start" ]

RUN apt-get update  
RUN apt-get install python  
RUN pip install virtualenv

   ...

RUN [ "cross-build-end" ]  
```

Lancez maintenant un auto-build sur Docker Hub et appréciez...
