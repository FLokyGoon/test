# test

FROM ubuntu:latest

# Mise à jour et installation de xdotool et de X11 utilities
RUN apt-get update && apt-get install -y \
    xdotool \
    x11-apps \
    && rm -rf /var/lib/apt/lists/*

# Répertoire de travail
WORKDIR /app

# Copie du script dans le conteneur
COPY ./mouse_move.sh /app/mouse_move.sh

# Rendre le script exécutable
RUN chmod +x /app/mouse_move.sh

# Commande par défaut pour exécuter le script
CMD ["/app/mouse_move.sh"]

#!/bin/bash

# Boucle infinie
#cmd to do : docker run -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix mouse-mover /bin/bash


while true; do
    # Starting position
    x=100
    y=100

    # Size of the square
    size=200

    # Move to starting position
    xdotool mousemove $x $y
	sleep 6
    # Move in a square pattern
    xdotool mousemove --sync $(($x + $size)) $y
   	sleep 6
    xdotool mousemove --sync $(($x + $size)) $(($y + $size))
    	sleep 6
    xdotool mousemove --sync $x $(($y + $size))
    	sleep 6
    xdotool mousemove --sync $x $y

    # Pause entre les mouvements pour le rendre visible et moins consommateur de ressources
    sleep 6
done

