# Diagrama de Interação - MVP Simulado Adaptativo

```mermaid
flowchart LR
  %% Frontend
  subgraph FE["Frontend"]
    A[Actor] -->|Acessa| SITE[Site / App];
  end

  %% Domínio: Simulado
  subgraph SIM["Dominio: Simulado"]
    SS[ServSimulado];
    MDBS[(MongoDB Simulado)];
  end

  %% Domínio: Modelo e LLM
  subgraph MOD["Dominio: Modelo e LLM"]
    SM[ServModelo];
    PLAN[(MongoDB Plano do Aluno)];
    LLM[Servico de Terceiro da LLM];
  end

  %% Domínio: Questões
  subgraph QST["Dominio: Questoes"]
    SQ[ServQuestoes];
    MDBQ[(MongoDB Questoes)];
  end

  %% Fluxo principal
  SITE -->|1. Gerar simulado adaptativo| SS;
  SITE -. "2. Buscar simulado recente (opcional)" .-> SS;
  SS -. "3. Se existir, retorna existente" .-> SITE;

  SS -->|4. Envia ID da conta e perfil| SM;
  SM -->|5. Salva/atualiza plano do aluno| PLAN;

  SM -->|6. Envia prompt com contexto| LLM;
  LLM -->|7. Retorna questoes recomendadas| SM;
  SM -->|8. Retorna questoes ao ServSimulado| SS;

  SS -->|9. Enviar questoes para armazenamento| SQ;
  SQ -->|10. Salvar questoes| MDBQ;
  SS -->|11. Salva simulado (metadados, vinculo)| MDBS;

  SS -->|12. Retorna simulado + questoes| SITE;
```