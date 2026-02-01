---
Title: Factorio Docker
Date: 2022-11-04
Tags:
- terminal
- docker
- factorio
slug: factorion-on-docker
Summary: Notes on how I hosted Factorio in a Docker container
---

[Docker Hub Guide](https://hub.docker.com/r/factoriotools/factorio/)

Command to create: (Stored in `start_factorio.bash` on Gus):
```bash
sudo docker run -it \
    -p 34197:34197/udp \
    -p 27105:27105/tcp \
    -v /home/ubuntu/factorio:/factorio \
    --name factorio \
    --restart=always \
    factoriotools/factorio
```

Connect to console via:
```bash
docker attach factorio
```

Disconnect (detach) from container: <kbd>Ctrl</kbd>+<kbd>P</kbd>,<kbd>Ctrl</kbd>+<kbd>Q</kbd>
