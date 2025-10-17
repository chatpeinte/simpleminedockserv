# Image de base avec Java 17
FROM openjdk:17-jdk-slim

# Répertoire de travail
WORKDIR /minecraft

# Variables d'environnement
ARG MINECRAFT_VERSION=1.21.1
ENV MEMORY=4G

# Installation des dépendances et téléchargement du jar officiel
RUN apt-get update && apt-get install -y wget jq && \
    SERVER_JAR_URL=$(wget -qO- https://launchermeta.mojang.com/mc/game/version_manifest.json | \
    jq -r --arg VER "$MINECRAFT_VERSION" '.versions[] | select(.id==$VER) | .url' | \
    xargs wget -qO- | jq -r '.downloads.server.url') && \
    wget -O server.jar "$SERVER_JAR_URL" && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Copie les fichiers de configuration
COPY eula.txt server.properties ./

# Expose le port Minecraft
EXPOSE 25565

# Lancement du serveur avec 4 Go de RAM
CMD ["bash", "-c", "java -Xmx${MEMORY} -Xms${MEMORY} -jar server.jar nogui"]
