# Arquitetura Híbrida – Plataforma SaaS de IA para Monitoramento de Obras

## Sumário

1. Introdução
2. Motivação da Arquitetura Híbrida
3. Visão Geral da Solução
4. Arquitetura de Alto Nível
5. Camada Edge (On-Premise)
6. Domínios de Inteligência Artificial (MVP)
7. Plataforma Cloud (GCP)
8. Fluxo de Dados
9. Operação Offline
10. Estratégia de Sincronização
11. Segurança
12. Observabilidade
13. Escalabilidade
14. Roadmap Evolutivo da IA
15. Fluxo Completo de uma Ocorrência
16. Benefícios da Arquitetura
17. Conclusão

---

# 1. Introdução

Esta arquitetura foi concebida para suportar uma plataforma de monitoramento inteligente de obras baseada em Inteligência Artificial, Visão Computacional, Edge Computing e Cloud Computing.

O objetivo da solução é identificar automaticamente riscos operacionais relacionados ao uso de Equipamentos de Proteção Individual (EPIs) e à presença de pessoas em áreas de risco, gerando alertas em tempo real e mantendo a operação ativa mesmo em cenários sem conectividade com a internet.

A arquitetura foi desenhada para ambientes críticos onde a indisponibilidade da nuvem ou da internet não pode interromper o monitoramento.

---

# 2. Motivação da Arquitetura Híbrida

Uma arquitetura puramente cloud apresenta limitações em obras críticas.

Fluxo tradicional:

Câmera → Internet → Cloud → IA → Alerta

Problemas:

- Dependência total da internet
- Latência variável
- Risco de indisponibilidade
- Ambientes remotos
- Obras subterrâneas
- Falhas temporárias de operadoras

Para eliminar esses riscos, a inferência da IA passa a ocorrer localmente na obra.

Fluxo proposto:

Câmera → IA Local → Alerta Local → Sincronização Cloud

A nuvem passa a exercer papel de governança, analytics e gestão centralizada.

---

# 3. Visão Geral da Solução

A solução é composta por duas camadas.

## Edge Layer

Executa localmente:

- Recepção dos vídeos
- Processamento de IA
- Banco operacional local
- Alertas
- Armazenamento temporário
- Operação offline

## Cloud Layer

Executa:

- Portal SaaS
- Gestão de usuários
- Dashboards
- Analytics
- Auditoria
- Treinamento dos modelos
- Armazenamento corporativo

---

# 4. Arquitetura de Alto Nível

```text
                 GOOGLE CLOUD

     Portal SaaS
     Cloud Run
     Cloud SQL
     BigQuery
     Cloud Storage
     Vertex AI

            ▲
            │
       Sincronização
            │
            ▼

=====================================

                 EDGE

           Edge Gateway
                 │
                 ├── Video Gateway
                 ├── IA EPIs
                 ├── IA Áreas de Risco
                 ├── PostgreSQL Local
                 ├── Alert Service
                 └── Sync Agent

                 ▲
                 │
              Câmeras
```
---

# 5. Camada Edge (On-Premise)

A camada Edge é responsável pela operação crítica da obra.

## Edge Gateway

Equipamento responsável pela execução dos serviços locais.

### Hardware sugerido

#### Pequenas obras

- Intel NUC
- 16 GB RAM
- SSD 512 GB

#### Obras críticas

- NVIDIA Jetson Orin
- 32 GB RAM
- SSD 1 TB

#### Grandes operações

- Servidor industrial
- GPU dedicada
- RAID SSD

### Responsabilidades

- Receber streams de vídeo
- Executar modelos de IA
- Armazenar eventos
- Armazenar evidências
- Gerar alertas
- Sincronizar dados

## Video Gateway

Tecnologias sugeridas:

- MediaMTX
- GStreamer

Funções:

- Receber RTSP
- Receber ONVIF
- Gerenciar streams
- Distribuir frames

## PostgreSQL Local

O PostgreSQL local é um componente permanente da arquitetura.

Não é apenas cache.

### Armazena

#### Operação

- Eventos
- Alertas
- Ocorrências
- Status

#### Configuração

- Áreas de risco
- Regras
- Câmeras
- Usuários locais

#### Auditoria

- Logs
- Histórico

#### Sincronização

- Fila de envio
- Controle de retries
- Controle de conflitos

## Alert Service

Responsável pelos alertas locais.

Exemplos:

- Sirene
- Giroflex
- Painel LED
- Totem sonoro

## Sync Agent

Responsável pela sincronização entre Edge e Cloud.

---

# 6. Domínios de Inteligência Artificial (MVP)

O MVP será focado em dois casos de uso.

## IA de Conformidade de EPIs

### Objetivo

Verificar automaticamente o uso correto dos equipamentos obrigatórios.

### Detecções

- Capacete
- Colete refletivo
- Uniforme
- Botas de segurança

### Exemplo

Pessoa detectada + sem capacete = não conformidade.

### Benefícios

- Fiscalização contínua
- Evidências automáticas
- Redução de acidentes

## IA de Pessoas em Áreas de Risco

### Objetivo

Identificar trabalhadores em locais perigosos.

### Detecções

- Entrada em área restrita
- Permanência em área crítica
- Aproximação indevida de equipamentos

### Benefícios

- Resposta rápida
- Redução de acidentes
- Monitoramento contínuo

## Evoluções Futuras

- Homem x Máquina
- Near Miss
- Escavações
- LiDAR
- Qualidade
- Produtividade

---

# 7. Plataforma Cloud (GCP)

## Cloud Load Balancer

Distribuição global de tráfego.

## API Gateway

Responsável por:

- Autenticação
- Autorização
- Auditoria
- Rate Limiting

## Cloud Run

Hospeda os microsserviços.

### Serviços

- Auth Service
- User Service
- Occurrence Service
- Evidence Service
- Analytics Service
- Integration Service

## Cloud SQL

Banco corporativo principal.

Armazena:

- Usuários
- Obras
- Contratos
- Ocorrências
- Workflow

## BigQuery

Responsável por:

- Analytics
- Dashboards
- KPIs
- Relatórios

## Cloud Storage

Armazena:

- Fotos
- Vídeos
- Evidências

Camadas:

- Hot
- Cold
- Archive

## Pub/Sub

Mensageria assíncrona.

## Vertex AI

Responsável por:

- Treinamento
- Avaliação
- Versionamento
- Deploy

## Model Registry

Distribui modelos para os ambientes Edge.

## Secret Manager

Armazena:

- Senhas
- Tokens
- Chaves

## Cloud Monitoring

Monitoramento da plataforma.

## Cloud Logging

Centralização dos logs.

---

# 8. Fluxo de Dados

```text
Câmera
 ↓
Video Gateway
 ↓
Extração de Frames
 ↓
IA Local
 ↓
Evento
 ↓
PostgreSQL Local
 ↓
Outbox
 ↓
Sync Agent
 ↓
Cloud
```
---

# 9. Operação Offline

A plataforma foi projetada para operar sem internet.

Mesmo offline:

- IA continua funcionando
- Alertas continuam funcionando
- Eventos continuam sendo armazenados
- Evidências continuam sendo armazenadas

A obra permanece protegida.

---

# 10. Estratégia de Sincronização

A sincronização utiliza Outbox Pattern.

```text
Evento
 ↓
PostgreSQL Local
 ↓
Outbox
 ↓
Sync Agent
 ↓
Cloud
```

Benefícios:

- Não perde eventos
- Retry automático
- Idempotência
- Resiliência

---

# 11. Segurança

## Edge

- VPN
- Certificados digitais
- Criptografia local
- Hardening do equipamento

## Cloud

- IAM
- RBAC
- TLS
- Cloud KMS

## LGPD

- Controle de acesso
- Auditoria
- Retenção configurável

---

# 12. Observabilidade

Ferramentas:

- Cloud Monitoring
- Cloud Logging
- Cloud Trace

Monitoramento:

- Infraestrutura
- Aplicação
- IA
- Integrações

---

# 13. Escalabilidade

## Piloto

50 câmeras

## Fase 1

500 câmeras

## Fase 2

1.750 câmeras

## Produção

5.000+ câmeras

A arquitetura permanece a mesma.

---

# 14. Roadmap Evolutivo da IA

## Fase 1

- EPIs
- Áreas de risco

## Fase 2

- Homem x Máquina
- Near Miss

## Fase 3

- Escavações
- Escoramentos
- LiDAR

## Fase 4

- Qualidade

## Fase 5

- Produtividade

---

# 15. Fluxo Completo de uma Ocorrência

```text
Câmera
 ↓
IA Local
 ↓
Evento
 ↓
Alerta Local
 ↓
PostgreSQL Local
 ↓
Sync Agent
 ↓
Cloud SQL
 ↓
Dashboard
 ↓
Fiscalização
 ↓
Auditoria
```
---

# 16. Benefícios da Arquitetura

- Operação offline
- Baixa latência
- Alta disponibilidade
- Escalabilidade horizontal
- Governança centralizada
- Analytics corporativo
- Atualização remota dos modelos
- Segurança corporativa

---

# 17. Conclusão

A arquitetura híbrida combina Edge Computing e Cloud Computing para fornecer uma solução resiliente, escalável e adequada para ambientes críticos.

A operação ocorre localmente na obra através do Edge Gateway e do PostgreSQL local, enquanto a nuvem fornece governança, auditoria, analytics, armazenamento corporativo e evolução contínua dos modelos de Inteligência Artificial.

Essa abordagem garante que a segurança operacional não dependa da disponibilidade da internet.
