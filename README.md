---
titulo: Plataforma, Microserviços e APIs
edição: 2025.2
---

## Plataforma, Microserviços e APIs - 2025.2

``` mermaid
flowchart LR
    subgraph api [Subnet API]
        direction TB
        gateway --> account
        gateway --> others
        gateway --> auth:::red
        auth --> account
        account --> db@{ shape: cyl, label: "Database" }
        others --> db
    end
    internet ==> |request| gateway:::orange
    classDef orange fill:#FCBE3E
```

## Repositórios

Principal: 
[https://github.com/repo-classes/pma.25.2](https://github.com/repo-classes/pma.25.2)

| Microservice | Interface | Implementation |
|-|-|-|
| Account | [account](https://github.com/repo-classes/pma252.account) | [account-service](https://github.com/repo-classes/pma252.account-service) |
| Auth | [auth](https://github.com/repo-classes/pma252.auth) | [auth-service](https://github.com/repo-classes/pma252.auth-service) |
| Gateway |  | [gateway-service](https://github.com/repo-classes/pma252.gateway-service) |