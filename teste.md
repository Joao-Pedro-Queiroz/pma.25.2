# Diagrama de Interação - MVP Simulado Adaptativo

```mermaid
flowchart LR
  %% Diagrama de Interação – MVP: Criar Simulado Adaptativo
  %% flowchart LR

  subgraph FE["Frontend"]
    A[Actor] -->|Acessa| SITE[Site / App]
  end

  %% ---- Domínio: Simulado ----
  subgraph SIM["Domínio: Simulado"]
    SS[ServSimulado]
    MDBS[(MongoDB Simulado)]
  end

  %% ---- Domínio: Modelo & LLM ----
  subgraph MOD["Domínio: Modelo & LLM"]
    SM[ServModelo]
    PLAN[(MongoDB Plano do Aluno)]
    LLM[Serviço de Terceiro da LLM]
  end

  %% ---- Domínio: Questões ----
  subgraph QST["Domínio: Questões"]
    SQ[ServQuestoes]
    MDBQ[(MongoDB Questões)]
  end

  %% ------ Fluxo principal de criação do Simulado Adaptativo ------
  SITE -->|1. Gerar simulado adaptativo (req)| SS

  %% (opcional) checar simulado recente para evitar duplicidade
  SITE -. "Buscar simulado recente (opcional)" .-> SS
  SS -. "Se existir, retorna existente" .-> SITE

  %% enviar dados do aluno ao motor adaptativo
  SS -->|2. Envia ID da conta & Perfil| SM
  SM -->|3. Salva/atualiza plano do aluno| PLAN

  %% geração adaptativa via LLM
  SM -->|4. Envia Prompt com contexto do aluno| LLM
  LLM -->|5. Retorna questões recomendadas| SM
  SM -->|6. Retorna questões ao ServSimulado| SS

  %% persistência e publicação do simulado
  SS -->|7. Enviar Questões para armazenamento| SQ
  SQ -->|8. Salvar Questões| MDBQ
  SS -->|9. Salva simulado (metadados, vínculo)| MDBS

  %% resposta ao frontend
  SS -->|10. Retorna Simulado + Questões| SITE
```