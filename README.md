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

## Deploy on server

### Set secrets

* `SSH_DEPLOY_PRIVATE_KEY` - e.g. `AAAwEAA ...`
* `BASIC_AUTH_USER` - e.g. `admin`
* `BASIC_AUTH_PASSWORD` - e.g. `admin`

### Run GitHub action manually

[Deploy action](https://github.com/treeton-org/team/actions/workflows/deploy.yml)

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