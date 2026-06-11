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

# Infraestrutura Google Cloud Platform (GCP)

## Visão Geral

A plataforma será executada integralmente na Google Cloud Platform (GCP), utilizando serviços gerenciados para reduzir a complexidade operacional, aumentar a escalabilidade e garantir alta disponibilidade.

A arquitetura foi desenhada para suportar desde a fase piloto com dezenas de câmeras até ambientes corporativos com mais de 5.000 câmeras simultâneas, sem necessidade de mudanças estruturais.

### Princípios da Arquitetura

* Cloud Native
* Event Driven Architecture
* Microsserviços
* MLOps
* Segurança por Design
* Escalabilidade Horizontal
* Alta Disponibilidade
* Observabilidade Completa

---

## Arquitetura GCP

```text
Internet
    │
    ▼
Cloud Load Balancer
    │
    ▼
API Gateway
    │
    ▼
Cloud Run
    │
    ├── Auth Service
    ├── User Service
    ├── Occurrence Service
    ├── Evidence Service
    ├── Analytics Service
    └── Integration Service

    │
    ├── Cloud SQL
    ├── BigQuery
    ├── Cloud Storage
    └── Pub/Sub

IA
│
└── Vertex AI
```

---

# Cloud Load Balancer

## Objetivo

Distribuir o tráfego entre os serviços da plataforma.

## Responsabilidades

* Balanceamento global
* Encerramento TLS
* Alta disponibilidade
* Distribuição automática de carga
* Failover transparente

## Benefícios

* Redução de indisponibilidade
* Melhor experiência para usuários
* Escalabilidade horizontal automática

---

# API Gateway

## Objetivo

Centralizar todo o acesso às APIs da plataforma.

## Responsabilidades

### Segurança

* Autenticação
* Autorização
* Validação de tokens

### Governança

* Versionamento de APIs
* Controle de consumo
* Rate Limiting

### Observabilidade

* Logs de acesso
* Métricas de utilização
* Auditoria

## Benefícios

Evita exposição direta dos microserviços e simplifica a governança da plataforma.

---

# Cloud Run

## Objetivo

Executar todos os microserviços da aplicação.

## Serviços Hospedados

### Auth Service

Responsável por:

* Login
* JWT
* OAuth2
* Sessões

### User Service

Responsável por:

* Usuários
* Perfis
* Permissões

### Occurrence Service

Responsável por:

* Ocorrências
* Workflow
* SLA

### Evidence Service

Responsável por:

* Evidências
* Imagens
* Vídeos

### Analytics Service

Responsável por:

* KPIs
* Dashboards
* Relatórios

### Integration Service

Responsável por:

* SAP
* ERP
* APIs externas

## Por que Cloud Run?

### Escalabilidade Automática

Escala automaticamente conforme a demanda.

### Operação Simplificada

Não exige administração de clusters Kubernetes.

### Pagamento por Uso

Custos proporcionais à utilização.

### Deploy Simplificado

Integração nativa com CI/CD.

---

# Cloud SQL (PostgreSQL)

## Objetivo

Banco de dados transacional principal da plataforma.

## Dados Armazenados

### Cadastros

* Usuários
* Perfis
* Obras
* Contratos
* Empreiteiras

### Operação

* Ocorrências
* Workflow
* Tratativas
* SLA

### Auditoria

* Histórico de alterações
* Logs operacionais

## Benefícios

### Consistência

Garantias ACID para dados críticos.

### Relacionamentos

Ideal para entidades fortemente relacionadas.

### Alta Disponibilidade

Replicação automática e backups gerenciados.

---

# BigQuery

## Objetivo

Analytics corporativo e Business Intelligence.

## Casos de Uso

### Indicadores Operacionais

* Eventos por obra
* Eventos por empreiteira
* Tempo médio de resolução

### Indicadores Corporativos

* Ranking de conformidade
* Evolução histórica
* Tendências de risco

### Auditoria

* Consultas históricas
* Relatórios gerenciais

## Benefícios

Permite analisar milhões de registros sem impactar o ambiente operacional.

---

# Cloud Storage

## Objetivo

Armazenar evidências da plataforma.

## Conteúdo Armazenado

### Imagens

Frames capturados pelos modelos de IA.

### Vídeos

Trechos associados às ocorrências.

### Documentos

Relatórios
Exportações
Arquivos gerados pela plataforma

---

## Estratégia de Armazenamento

### Hot Storage

Período:

```text
0 a 90 dias
```

Utilizado para dados acessados frequentemente.

---

### Cold Storage

Período:

```text
90 dias até 5 anos
```

Utilizado para retenção de longo prazo.

---

### Archive Storage

Período:

```text
Acima de 5 anos
```

Menor custo possível para retenção histórica.

---

## Lifecycle Management

Movimenta automaticamente os arquivos entre as camadas de armazenamento.

```text
Hot
 ↓
Cold
 ↓
Archive
```

---

# Pub/Sub

## Objetivo

Implementar comunicação assíncrona entre os componentes.

## Fluxo

```text
IA
 ↓
Pub/Sub
 ↓
Workflow
 ↓
Notificações
 ↓
Analytics
```

## Benefícios

### Desacoplamento

Serviços não dependem diretamente uns dos outros.

### Escalabilidade

Novos consumidores podem ser adicionados sem alterar os produtores.

### Resiliência

Mensagens permanecem disponíveis mesmo em falhas temporárias.

---

# Vertex AI

## Objetivo

Gerenciar todo ciclo de vida dos modelos de Inteligência Artificial.

## Responsabilidades

### Treinamento

Criação e evolução dos modelos.

### Deploy

Publicação dos modelos em produção.

### Versionamento

Controle de versões e rollback.

### Monitoramento

Acompanhamento de acurácia.

### Re-treinamento

Evolução contínua baseada em novos dados.

---

## Modelos Utilizados

### SST

* Capacete
* Luvas
* Óculos
* Botas
* Uniforme

### Segurança Operacional

* Homem x Máquina
* Área Restrita
* Near Miss

### Escavação

* Profundidade
* Escoramento
* Isolamento

---

# Cloud Monitoring

## Objetivo

Monitorar toda a infraestrutura.

## Métricas

### Aplicação

* Latência
* Throughput
* Taxa de erro

### Infraestrutura

* CPU
* Memória
* Rede

### IA

* Tempo de inferência
* Acurácia
* Taxa de detecção

---

# Cloud Logging

## Objetivo

Centralizar logs da plataforma.

## Registros

### Aplicação

Logs dos microserviços.

### Segurança

Autenticações e acessos.

### Auditoria

Ações realizadas pelos usuários.

### IA

Resultados das inferências.

---

# Secret Manager

## Objetivo

Gerenciar credenciais da plataforma.

## Exemplos

* Credenciais SAP
* Chaves JWT
* Senhas de banco
* Tokens de APIs externas

## Benefícios

Nenhuma credencial é armazenada no código-fonte.

---

# Cloud KMS

## Objetivo

Gerenciar criptografia corporativa.

## Utilização

### Dados

* Cloud SQL
* Cloud Storage
* Backups

### Compliance

* LGPD
* Auditorias
* Políticas corporativas

---

# Estratégia de Escalabilidade

## Fase Piloto

```text
50 câmeras
```

Infraestrutura mínima utilizando Cloud Run, Cloud SQL e Vertex AI.

---

## Expansão

```text
500 câmeras
```

Escalabilidade automática dos serviços.

---

## Produção

```text
1.750 câmeras
```

Múltiplas instâncias de inferência executando paralelamente.

---

## Escala Máxima

```text
5.000+ câmeras
```

Sem mudança arquitetural.

Apenas ampliação horizontal dos recursos computacionais.

---

# Estratégia de Segurança

## Controle de Acesso

* IAM
* RBAC

## Comunicação

* HTTPS
* TLS 1.2+

## Dados

* Criptografia em repouso
* Criptografia em trânsito

## Auditoria

* Logs imutáveis
* Rastreabilidade completa

## LGPD

* Controle de acesso
* Retenção configurável
* Histórico auditável

---

# Benefícios da Arquitetura GCP

* Infraestrutura totalmente gerenciada
* Menor custo operacional
* Escalabilidade automática
* Alta disponibilidade
* Integração nativa com IA
* Segurança corporativa
* Facilidade de auditoria
* Preparada para milhares de câmeras simultâneas
* Evolução contínua sem mudança arquitetural

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
