# Arquitetura Cloud – Plataforma SaaS de IA para Monitoramento de Obras

## Visão Geral

Esta arquitetura foi projetada para suportar uma plataforma SaaS corporativa de monitoramento inteligente de obras baseada em Inteligência Artificial e Visão Computacional.

O objetivo é permitir o monitoramento contínuo de milhares de frentes de obra simultaneamente, identificando riscos operacionais, falhas de segurança e não conformidades em tempo real, com geração automática de alertas, rastreabilidade completa das ocorrências e retenção de evidências por longos períodos.

A solução foi desenhada seguindo princípios de:

* Cloud Native
* Microsserviços
* Event Driven Architecture
* MLOps
* Alta Disponibilidade
* Escalabilidade Horizontal
* Segurança por Design

---

# Arquitetura em Alto Nível

```text
Câmeras
    │
    ▼
Ingestão de Vídeo
    │
    ▼
Extração de Frames
    │
    ▼
Modelos de IA
    │
    ▼
Pub/Sub
    │
    ▼
Motor de Regras
    │
    ▼
Notificações
    │
    ▼
Portal SaaS

───────────────

Portal SaaS
    │
    ▼
API Gateway
    │
    ▼
Microserviços
    │
    ▼
Camada de Dados

Cloud SQL
BigQuery
Cloud Storage
Backup
```

---

# Objetivos da Solução

A plataforma foi concebida para atender aos seguintes objetivos:

* Monitorar obras remotamente
* Detectar automaticamente riscos de segurança
* Gerar alertas em tempo real
* Automatizar o tratamento de ocorrências
* Apoiar auditorias e fiscalização
* Reduzir acidentes
* Melhorar conformidade operacional
* Fornecer indicadores corporativos
* Suportar crescimento acima de 5.000 câmeras simultâneas

---

# Camada 1 – Fontes de Vídeo

## Objetivo

Capturar imagens e vídeos das obras.

## Componentes

* Câmeras IP
* Câmeras 4G/5G
* NVRs
* DVRs

## Responsabilidades

* Capturar imagens continuamente
* Transmitir streams para a nuvem
* Operar em ambientes distribuídos

## Benefícios

* Centralização do monitoramento
* Redução de visitas presenciais
* Cobertura geográfica ampla

---

# Camada 2 – Ingestão de Vídeo

## Objetivo

Receber e gerenciar os streams de vídeo enviados pelas obras.

## Tecnologias

* MediaMTX
* GStreamer
* WebRTC Gateway

## Responsabilidades

### Recebimento de Streams

Suporte a:

* RTSP
* ONVIF
* WebRTC

### Balanceamento

Distribuir conexões entre múltiplos nós.

### Recuperação

Reconectar streams automaticamente em caso de falha.

## Benefícios

* Alta disponibilidade
* Escalabilidade horizontal
* Tolerância a falhas

---

# Camada 3 – Extração de Frames

## Objetivo

Preparar o conteúdo para processamento pelos modelos de IA.

## Funcionamento

Vídeos não são processados diretamente.

Exemplo:

```text
Vídeo 30 FPS

↓
Amostragem

↓
1 ou 2 FPS
```

## Responsabilidades

* Extração de imagens
* Normalização
* Compressão
* Geração de metadados

## Benefícios

* Redução do custo computacional
* Menor uso de GPU
* Maior escalabilidade

---

# Camada 4 – Processamento de IA

## Objetivo

Identificar automaticamente situações de risco.

Esta é a camada central da plataforma.

---

## IA SST (Segurança do Trabalho)

### Detecta

* Capacete
* Óculos
* Luvas
* Uniforme
* Colete
* Botas

### Exemplo

```text
Pessoa detectada
+
Capacete ausente

↓

Alerta de SST
```

---

## IA Segurança Operacional

### Detecta

* Homem x Máquina
* Áreas restritas
* Permanência em área crítica
* Near Miss
* Isolamento inadequado

### Exemplo

```text
Pessoa próxima
à retroescavadeira

↓

Evento de risco
```

---

## IA Escavação e Valas

### Detecta

* Profundidade
* Escoramento
* Isolamento
* Estabilidade

### Tecnologias

* Visão Computacional
* LiDAR

### Exemplo

```text
Profundidade > 1,25m
+
Sem escoramento

↓

Alerta crítico
```

---

# Vertex AI e MLOps

## Objetivo

Gerenciar todo o ciclo de vida dos modelos.

## Responsabilidades

### Treinamento

Construção de novos modelos.

### Versionamento

Controle de versões.

### Monitoramento

Avaliação contínua da acurácia.

### Re-treinamento

Evolução contínua baseada em novos dados.

## Benefícios

* Governança dos modelos
* Melhoria contínua
* Rastreabilidade

---

# Camada 5 – Event Bus

## Objetivo

Desacoplar os sistemas.

## Tecnologia

Google Pub/Sub

## Funcionamento

Quando a IA detecta algo:

```json
{
  "evento": "SEM_CAPACETE",
  "obra": "OBRA-001",
  "criticidade": "ALTA"
}
```

O evento é publicado.

Todos os sistemas interessados podem consumi-lo.

---

## Benefícios

### Escalabilidade

Novos consumidores podem ser adicionados sem alterar a IA.

### Resiliência

Falhas não interrompem o fluxo.

### Flexibilidade

Novos módulos podem ser conectados facilmente.

---

# Camada 6 – Motor de Regras e Workflow

## Objetivo

Transformar eventos em ações.

---

## Exemplo

```text
Evento:
Sem capacete

↓

Classificação

↓

Criticidade Alta

↓

Abrir ocorrência

↓

Enviar alerta

↓

Controlar SLA
```

---

## Workflow

### 1ª Linha

Empreiteira

Responsável por:

* Tratar ocorrência
* Inserir evidências
* Corrigir problema

### 2ª Linha

Fiscalização

Responsável por:

* Validar tratativas
* Escalonar problemas

### 3ª Linha

SABESP

Responsável por:

* Auditoria
* Governança
* Indicadores

---

# Camada 7 – Notificações

## Objetivo

Informar rapidamente os responsáveis.

## Canais

* WhatsApp
* SMS
* E-mail
* Alertas sonoros

## Fluxo

```text
Evento

↓

Workflow

↓

Notificação

↓

Ação corretiva
```

## Benefícios

* Resposta rápida
* Redução de riscos
* Aumento da conformidade

---

# Camada 8 – Portal SaaS

## Objetivo

Disponibilizar a plataforma para os usuários.

## Tecnologias

* Next.js
* React

---

## Funcionalidades

### Dashboard Executivo

* KPIs
* Indicadores
* Mapa das obras

### Gestão de Ocorrências

* Acompanhamento
* Tratativas
* SLA

### Auditoria

* Evidências
* Histórico

### Administração

* Usuários
* Perfis
* Permissões

---

# API Gateway

## Objetivo

Centralizar toda comunicação da aplicação.

## Responsabilidades

### Autenticação

JWT e OAuth2.

### Autorização

RBAC por perfil.

### Rate Limiting

Controle de consumo.

### Auditoria

Registro de acessos.

### Versionamento

Evolução segura das APIs.

---

## Fluxo

```text
Usuário

↓

Frontend

↓

API Gateway

↓

Microserviços
```

---

# Camada de Microsserviços

Cada domínio possui seu próprio serviço.

---

## Auth Service

### Responsabilidades

* Login
* JWT
* OAuth2
* Sessões

---

## User Service

### Responsabilidades

* Usuários
* Perfis
* Permissões

---

## Occurrence Service

### Responsabilidades

* Ocorrências
* Workflow
* SLA

---

## Evidence Service

### Responsabilidades

* Evidências
* Imagens
* Vídeos

---

## Analytics Service

### Responsabilidades

* Dashboards
* Relatórios
* Indicadores

---

## Integration Service

### Responsabilidades

* ERP
* SAP
* SIG
* APIs externas

---

# Camada 9 – Dados

A camada de dados é acessada exclusivamente pelos microserviços.

O frontend nunca acessa bancos diretamente.

---

# Cloud SQL

## Objetivo

Banco transacional principal.

## Armazena

* Usuários
* Obras
* Contratos
* Ocorrências
* Workflow
* Auditoria

## Papel

Fonte oficial da verdade operacional.

---

# BigQuery

## Objetivo

Analytics e Business Intelligence.

## Utilização

* KPIs
* Relatórios
* Tendências
* Dashboards

## Exemplos

```sql
Quantidade de alertas
por obra
nos últimos 12 meses
```

---

# Cloud Storage

## Objetivo

Armazenamento de evidências.

## Tipos de Conteúdo

* Fotos
* Vídeos
* Evidências
* Arquivos exportados

---

## Camada Hot

### Período

0 a 90 dias

### Características

* Acesso frequente
* Baixa latência

---

## Camada Cold

### Período

90 dias até 5 anos

### Características

* Menor custo
* Acesso eventual

---

## Camada Archive

### Período

Longa retenção

### Características

* Menor custo possível
* Preservação histórica

---

# Lifecycle Management

## Objetivo

Mover arquivos automaticamente.

```text
Hot

↓

Cold

↓

Archive
```

Sem intervenção humana.

---

# Backup e Recuperação

## Objetivo

Garantir continuidade da operação.

## Recursos

* Backup automático
* Replicação entre regiões
* Recuperação de desastre

## Indicadores

### RPO

Máximo de perda aceitável.

### RTO

Tempo máximo de recuperação.

---

# Governança e Segurança

## Objetivo

Garantir proteção dos dados e conformidade.

---

## IAM

Controle de acesso.

---

## RBAC

Permissões por perfil.

---

## Criptografia

### Em trânsito

TLS 1.2+

### Em repouso

Google KMS

---

## Auditoria

Registro completo de:

* Logins
* Alterações
* Consultas
* Tratativas

---

## LGPD

Conformidade com legislação brasileira.

---

# Observabilidade

## Objetivo

Monitorar a plataforma.

## Ferramentas

* Cloud Monitoring
* Cloud Logging
* Cloud Trace

## Benefícios

* Identificação de falhas
* Monitoramento de performance
* Alertas proativos

---

# Alta Disponibilidade

## Estratégias

### Multi-Zona

Serviços distribuídos.

### Balanceamento

Distribuição automática de carga.

### Replicação

Proteção contra falhas regionais.

### Autoscaling

Escalabilidade automática.

---

# Escalabilidade

A arquitetura foi desenhada para crescer sem mudanças estruturais.

## Evolução

```text
Piloto
50 câmeras

↓

Fase 1
500 câmeras

↓

Fase 2
1.750 câmeras

↓

Produção
5.000+ câmeras
```

---

# Fluxo Completo de uma Ocorrência

```text
Câmera

↓

Ingestão

↓

Extração de Frames

↓

IA

↓

Evento

↓

Pub/Sub

↓

Motor de Regras

↓

Ocorrência

↓

Notificação

↓

Empreiteira

↓

Fiscalização

↓

SABESP

↓

Encerramento

↓

Armazenamento Histórico
```

---

# Conclusão

A arquitetura proposta separa claramente os domínios de captura, processamento, inteligência artificial, workflow, aplicação e dados.

Essa abordagem proporciona:

* Escalabilidade horizontal
* Alta disponibilidade
* Segurança corporativa
* Conformidade com LGPD
* Governança completa
* Evolução contínua dos modelos de IA
* Operação em larga escala
* Suporte a mais de 5.000 câmeras simultâneas

O resultado é uma plataforma robusta, preparada para monitoramento corporativo de obras com inteligência artificial em tempo real.
