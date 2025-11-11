# Diagrama de Interação - MVP Simulado Adaptativo

```mermaid
flowchart LR
    %% Direção esquerda → direita
    Actor([Actor])
    Site((Site))

    %% === Domínio: Usuário & Perfil ===
    subgraph DomUsuarioPerfil["Domínio: Usuário & Perfil"]
      direction LR
      ServUsuario[ServUsuário]
      MDB_Usuario[(MongoDB Usuário)]
      ServPerfil[ServPerfil]
      MDB_Perfil[(MongoDB Perfil)]

      %% Regras do domínio
      ServUsuario -->|Buscar conta| MDB_Usuario
      ServUsuario -->|Salvar/validar conta| MDB_Usuario
      ServPerfil -->|Salvar Perfil| MDB_Perfil
    end

    %% === Domínio: Simulado ===
    subgraph DomSimulado["Domínio: Simulado"]
      direction LR
      ServSimulado[ServSimulado]
      MDB_Simulado[(MongoDB Simulado)]
      ServSimulado -->|Salva simulado| MDB_Simulado
      ServSimulado -->|Busca Simulado Recente| MDB_Simulado
      ServSimulado -->|Apaga Simulado| MDB_Simulado
    end

    %% === Domínio: Modelo & LLM ===
    subgraph DomModelo["Domínio: Modelo & LLM"]
      direction LR
      ServModelo[ServModelo]
      MDB_Plano[(MongoDB Plano do Aluno)]
      LLM[Serviço de Terceiro da LLM]

      ServModelo -->|Envia Prompt| LLM
      LLM -->|Retorna Questões| ServModelo
      ServModelo -->|Salva plano do aluno| MDB_Plano
    end

    %% === Domínio: Questões ===
    subgraph DomQuestoes["Domínio: Questões"]
      direction LR
      ServQuestoes[ServQuestoes]
      MDB_Questoes[(MongoDB Questões)]
      ServQuestoes -->|Salvar Questões| MDB_Questoes
    end

    %% === Fluxos entre domínios ===
    Actor -->|Acessa| Site

    %% Autenticação: Site <-> ServUsuário
    Site -->|Cadastrar/logar conta| ServUsuario
    Site -->|Buscar informações da conta| ServUsuario
    ServUsuario -->|Informações da conta| Site

    %% Perfil: apenas via ServUsuário / ServSimulado
    ServUsuario -->|Enviar Perfil| ServPerfil

    %% Buscar informações do usuário
    ServSimulado -->|Envia email| ServUsuario
    ServUsuario -->|Retorna informações| ServSimulado

    %% Geração do simulado
    Site -->|Gerar simulado adaptativo| ServSimulado
    ServSimulado -->|Envia ID conta| ServModelo
    ServModelo -->|Retorna Questões| ServSimulado

    %% Encaminhar questões para serviço de questões
    ServSimulado -->|Enviar Questões| ServQuestoes

    %% Recuperação / Finalização (retornam apenas o simulado)
    Site -->|Buscar Simulado Recente| ServSimulado
    Site -->|Finalizar Simulado| ServSimulado
    Site -->|Apagar simulado| ServSimulado
    ServSimulado -->|Retorna Simulado| Site
    ServSimulado -->|Retorna Simulado e questões do simulado| Site

    %% (Opcional) Atualizações de perfil após finalização
    ServSimulado -->|Enviar Perfil| ServPerfil

    %% Estilos
    classDef store fill:#fff9,stroke:#9370db;
    class MDB_Usuario,MDB_Perfil,MDB_Simulado,MDB_Plano,MDB_Questoes store;
```