# Diagrama de Interação - MVP Simulado Adaptativo

```mermaid
flowchart LR
  subgraph FE["Frontend"]
    A[Actor] -->|Acessa| SITE[Site / App]
  end

  subgraph SIM["Domínio: Simulado"]
    SS[ServSimulado]
    MDBS[(MongoDB Simulado)]
  end

  subgraph MOD["Domínio: Modelo e LLM"]
    SM[ServModelo]
    PLAN[(MongoDB Plano do Aluno)]
    LLM[Serviço de Terceiro da LLM]
  end

  subgraph QST["Domínio: Questões"]
    SQ[ServQuestoes]
    MDBQ[(MongoDB Questões)]
  end

  SITE -->|1. Gerar simulado adaptativo| SS
  SITE -. 2. Buscar simulado recente (opcional) .-> SS
  SS -. 3. Se existir, retorna existente .-> SITE

  SS -->|4. Envia ID da conta e Perfil| SM
  SM -->|5. Salva/atualiza plano do aluno| PLAN

  SM -->|6. Envia prompt com contexto| LLM
  LLM -->|7. Retorna questões recomendadas| SM
  SM -->|8. Retorna questões ao ServSimulado| SS

  SS -->|9. Enviar questões para armazenamento| SQ
  SQ -->|10. Salvar questões| MDBQ
  SS -->|11. Salva simulado (metadados, vínculo)| MDBS

  SS -->|12. Retorna simulado + questões| SITE
```