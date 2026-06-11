# Arquitetura Híbrida – Plataforma SaaS de IA para Monitoramento de Obras

## Sumário

1. Introdução
2. Motivação da Arquitetura Híbrida
3. Visão Geral da Solução
4. Arquitetura de Alto Nível
5. Camada Edge (On-Premise)
6. Domínios de Inteligência Artificial
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

Esta arquitetura foi concebida para suportar uma plataforma de monitoramento inteligente de obras baseada em Inteligência Artificial, Visão Computacional e Edge Computing.

O objetivo da solução é identificar automaticamente riscos operacionais e não conformidades relacionadas ao uso de EPIs e presença de pessoas em áreas de risco, garantindo baixa latência e operação contínua mesmo sem internet.

---

# 2. Motivação da Arquitetura Híbrida

Em ambientes críticos, depender exclusivamente da nuvem pode representar riscos operacionais.

Problemas comuns:

- Internet instável
- Ambientes remotos
- Obras subterrâneas
- Latência elevada
- Interrupções temporárias de conectividade

Por esse motivo a arquitetura foi dividida entre Edge e Cloud.

---

# 3. Visão Geral da Solução

A solução possui duas camadas complementares:

## Edge

- Captura de vídeo
- Inferência local
- Alertas locais
- Banco local
- Operação offline

## Cloud

- Dashboards
- Analytics
- Auditoria
- Gestão de usuários
- Armazenamento permanente
- Treinamento dos modelos

---

# 4. Arquitetura de Alto Nível

EDGE

Câmeras
↓
Video Gateway
↓
IA Local
↓
Alertas
↓
Banco Local
↓
Sync Agent

⇅

CLOUD

Cloud Run
Cloud SQL
BigQuery
Cloud Storage
Vertex AI
Portal SaaS

---

# 5. Camada Edge (On-Premise)

## Edge Gateway

Hardware sugerido:

- NVIDIA Jetson Orin
- Intel NUC
- Dell Edge Gateway
- Servidor Industrial

Responsabilidades:

- Receber streams
- Executar IA
- Armazenar eventos
- Gerar alertas
- Sincronizar dados

## Video Gateway

Tecnologias:

- MediaMTX
- GStreamer

## Banco Local

Tecnologias:

- SQLite
- PostgreSQL

## Alert Service

Alertas locais:

- Sirene
- Giroflex
- Painel LED

## Sync Agent

Sincroniza dados quando houver conectividade.

---

# 6. Domínios de Inteligência Artificial

## MVP – IA de Conformidade de EPIs

Detecta:

- Capacete
- Colete refletivo
- Uniforme
- Botas de segurança

Benefícios:

- Fiscalização contínua
- Evidências automáticas
- Redução de acidentes

## MVP – IA de Pessoas em Áreas de Risco

Detecta:

- Entrada em área restrita
- Permanência em área de risco
- Aproximação indevida de equipamentos

Benefícios:

- Resposta rápida
- Monitoramento contínuo
- Redução de acidentes

## Evolução Futura

- Homem x Máquina
- Near Miss
- Escavações
- LiDAR
- Qualidade
- Produtividade

---

# 7. Plataforma Cloud (GCP)

## Cloud Load Balancer

Distribuição de tráfego.

## API Gateway

- Autenticação
- Autorização
- Auditoria
- Rate limiting

## Cloud Run

Hospeda:

- Auth Service
- User Service
- Occurrence Service
- Evidence Service
- Analytics Service
- Integration Service

## Cloud SQL

Banco operacional principal.

Armazena:

- Usuários
- Obras
- Contratos
- Ocorrências

## BigQuery

Analytics corporativo.

## Cloud Storage

Armazena:

- Fotos
- Vídeos
- Evidências

## Pub/Sub

Mensageria assíncrona.

## Vertex AI

Treinamento e gestão dos modelos.

## Model Registry

Distribuição de modelos para Edge.

---

# 8. Fluxo de Dados

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
Banco Local
↓
Sync Agent
↓
Cloud

---

# 9. Operação Offline

Mesmo sem internet:

- IA continua funcionando
- Alertas continuam funcionando
- Evidências continuam armazenadas
- Operação continua normalmente

---

# 10. Estratégia de Sincronização

Quando a internet retorna:

- Eventos são enviados
- Evidências são enviadas
- Logs são enviados
- Métricas são enviadas

---

# 11. Segurança

## Edge

- VPN
- Certificados
- Criptografia local

## Cloud

- IAM
- RBAC
- TLS
- KMS

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

---

# 13. Escalabilidade

Piloto: 50 câmeras

Fase 1: 500 câmeras

Fase 2: 1.750 câmeras

Produção: 5.000+ câmeras

---

# 14. Roadmap Evolutivo da IA

Fase 1:

- EPIs
- Áreas de risco

Fase 2:

- Homem x Máquina
- Near Miss

Fase 3:

- Escavações
- Escoramentos
- LiDAR

Fase 4:

- Qualidade

Fase 5:

- Produtividade

---

# 15. Fluxo Completo de uma Ocorrência

Câmera
↓
IA Local
↓
Evento
↓
Alerta Local
↓
Banco Local
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

---

# 16. Benefícios da Arquitetura

- Operação offline
- Baixa latência
- Alta disponibilidade
- Escalabilidade horizontal
- Governança centralizada
- Analytics corporativo

---

# 17. Conclusão

A arquitetura híbrida combina Edge Computing e Cloud Computing para entregar uma solução resiliente, escalável e adequada para ambientes críticos, garantindo que a operação continue funcionando mesmo sem conectividade com a internet.
