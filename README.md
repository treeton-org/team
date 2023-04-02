# Team server infrastructure

![cover](docs/cover.png)

## Server [ansible](https://www.ansible.com) initialization

```shell
ansible-playbook -i playbook/inventory/all.yaml playbook/init.yaml
```

## Up local

```shell
docker network create traefik
docker compose --env-file .env.local up
```

## Docker network scheme

```mermaid
flowchart TD
    %% Traefik
    traefik(traefik)-->|traefik|tempo(tempo)
    traefik(traefik)-->|traefik|focalboard(focalboard)
    traefik(traefik)-->|traefik|focalboard-postgres-exporter(focalboard-postgres-exporter)
    
    %% Focalboard
    focalboard(focalboard)-->|focalboard|focalboard-postgres(focalboard-postgres)
    focalboard-postgres(focalboard-postgres)-->|focalboard-postgres|focalboard-postgres-exporter(focalboard-postgres-exporter)

subgraph Focalboard
    focalboard(focalboard)
    focalboard-postgres(focalboard-postgres)
    focalboard-postgres-exporter(focalboard-postgres-exporter)
end

subgraph Grafana
    tempo(tempo)
end
```