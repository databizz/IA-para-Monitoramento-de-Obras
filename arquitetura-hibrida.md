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

O principal objetivo da solução é permitir a identificação automática de riscos, não conformidades e eventos operacionais críticos em tempo real, garantindo continuidade operacional mesmo em cenários com baixa conectividade ou ausência total de internet.

Diferentemente de arquiteturas puramente cloud, a solução proposta distribui responsabilidades entre a nuvem e equipamentos instalados localmente nas obras, reduzindo latência e aumentando a disponibilidade.

---

# 2. Motivação da Arquitetura Híbrida

Uma arquitetura exclusivamente baseada em nuvem apresenta limitações importantes em ambientes de infraestrutura crítica.

Fluxo tradicional:

Câmera → Internet → Cloud → IA → Alerta

Problemas:

* Dependência da internet
* Latência variável
* Risco de indisponibilidade
* Falhas de operadoras móveis
* Ambientes subterrâneos ou remotos

Para resolver esse problema foi adotado o conceito de Edge Computing.

Fluxo híbrido:

Câmera → IA Local → Alerta Local → Sincronização Cloud

Neste modelo a operação continua funcionando mesmo sem conexão com a internet.

---

# 3. Visão Geral da Solução

A solução é composta por duas camadas complementares.

## Edge Layer

Executa:

* Captura de vídeo
* Inferência de IA
* Alertas
* Banco local
* Armazenamento local
* Operação offline

## Cloud Layer

Executa:

* Gestão da plataforma
* Dashboards
* Analytics
* Auditoria
* Armazenamento permanente
* Treinamento dos modelos

---

# 4. Arquitetura de Alto Nível

```text
                 GOOGLE CLOUD

 Portal SaaS
 Dashboards
 Analytics
 Auditoria
 BigQuery
 Cloud SQL
 Cloud Storage
 Vertex AI

        ▲
        │
   Sincronização
        │
        ▼

==================================

               EDGE

       Edge Gateway
            │
            ├── Video Gateway
            ├── IA SST
            ├── IA Operacional
            ├── IA Engenharia
            ├── Banco Local
            ├── Alert Service
            └── Sync Agent

            ▲
            │
         Câmeras
```

---

# 5. Camada Edge (On-Premise)

A camada Edge é responsável pela operação crítica em tempo real.

## Edge Gateway

Equipamento responsável por executar todos os serviços locais.

Possíveis hardwares:

* NVIDIA Jetson Orin
* Intel NUC Industrial
* Dell Edge Gateway
* Servidor industrial

Responsabilidades:

* Receber streams de vídeo
* Executar modelos de IA
* Armazenar eventos
* Gerar alertas
* Sincronizar com a nuvem

## Video Gateway

Tecnologias:

* MediaMTX
* GStreamer

Responsável por:

* Receber RTSP
* Receber ONVIF
* Distribuir frames para os modelos

## Banco Local

Tecnologias:

* SQLite
* PostgreSQL

Armazena:

* Eventos
* Alertas
* Logs
* Evidências

## Alert Service

Responsável pelos alertas locais.

Exemplos:

* Sirene
* Giroflex
* Painel LED
* Totem sonoro

## Sync Agent

Responsável por sincronizar os dados locais com a nuvem quando houver conectividade.

---

# 6. Domínios de Inteligência Artificial

A solução é dividida em domínios especializados.

## IA de Conformidade SST

Responsável por verificar conformidade com normas de Segurança e Saúde no Trabalho.

Detecta:

* Capacete
* Óculos
* Luvas
* Botas
* Uniforme
* Colete refletivo
* Máscaras
* Trabalho em altura

Benefícios:

* Fiscalização contínua
* Evidências automáticas
* Redução de acidentes

## IA de Segurança Operacional

Responsável por identificar situações de risco operacional.

Detecta:

* Homem x Máquina
* Near Miss
* Áreas restritas
* Permanência indevida

Benefícios:

* Redução de acidentes graves
* Resposta rápida
* Monitoramento contínuo

## IA de Engenharia e Estruturas

Responsável pelo monitoramento técnico da obra.

Detecta:

* Escavações
* Profundidade
* Escoramentos
* Estabilidade
* Isolamentos

Integração opcional:

* LiDAR
* Sensores 3D

Benefícios:

* Redução de risco de soterramento
* Controle técnico automatizado

## IA de Qualidade e Execução (Roadmap)

Aplicações futuras:

* Pavimentação
* Concretagem
* Montagens
* Acabamentos

## IA de Produtividade e Operação (Roadmap)

Aplicações futuras:

* Produtividade de equipes
* Utilização de equipamentos
* Avanço físico da obra

---

# 7. Plataforma Cloud (GCP)

## Cloud Load Balancer

Distribui o tráfego dos usuários.

## API Gateway

Responsável por:

* Autenticação
* Autorização
* Auditoria
* Rate Limiting

## Cloud Run

Executa:

* Auth Service
* User Service
* Occurrence Service
* Evidence Service
* Analytics Service
* Integration Service

## Cloud SQL

Fonte oficial da operação.

Armazena:

* Obras
* Contratos
* Usuários
* Ocorrências

## BigQuery

Responsável por:

* KPIs
* Dashboards
* Relatórios
* Analytics

## Cloud Storage

Armazena:

* Imagens
* Vídeos
* Evidências

Camadas:

* Hot
* Cold
* Archive

## Pub/Sub

Comunicação assíncrona entre os serviços.

## Vertex AI

Responsável por:

* Treinamento
* Avaliação
* Versionamento
* Deploy

## Model Registry

Distribui modelos atualizados para os ambientes Edge.

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

* IA continua funcionando
* Alertas continuam funcionando
* Evidências continuam sendo armazenadas
* Operação continua normalmente

---

# 10. Estratégia de Sincronização

Quando a internet falha:

Eventos permanecem locais.

Quando a internet retorna:

Sync Agent envia:

* Eventos
* Evidências
* Logs
* Métricas

---

# 11. Segurança

## Edge

* VPN
* Certificados digitais
* Criptografia local

## Cloud

* IAM
* RBAC
* TLS
* Cloud KMS

## LGPD

* Controle de acesso
* Auditoria
* Retenção configurável

---

# 12. Observabilidade

Ferramentas:

* Cloud Monitoring
* Cloud Logging
* Cloud Trace

Monitoramento:

* Aplicação
* Infraestrutura
* IA
* Integrações

---

# 13. Escalabilidade

Piloto:

50 câmeras

Fase 1:

500 câmeras

Fase 2:

1.750 câmeras

Produção:

5.000+ câmeras

A arquitetura permanece a mesma.

---

# 14. Roadmap Evolutivo da IA

Fase 1

IA SST

Fase 2

IA Segurança Operacional

Fase 3

IA Engenharia e Estruturas

Fase 4

IA Qualidade

Fase 5

IA Produtividade

---

# 15. Fluxo Completo de uma Ocorrência

Câmera
↓
Video Gateway
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

* Operação offline
* Baixa latência
* Alta disponibilidade
* Escalabilidade horizontal
* Governança centralizada
* Analytics corporativo
* Atualização remota dos modelos
* Segurança corporativa

---

# 17. Conclusão

A arquitetura híbrida combina Edge Computing e Cloud Computing para fornecer uma solução resiliente, escalável e adequada para ambientes críticos.

A operação ocorre localmente na obra, enquanto a nuvem fornece governança, auditoria, analytics e evolução contínua dos modelos de Inteligência Artificial.

Essa abordagem é especialmente adequada para obras de infraestrutura crítica, onde a segurança operacional não pode depender exclusivamente da disponibilidade da internet.
