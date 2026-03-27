# Arquitetura

## Visão geral

O `mrsanpool-platform` é a camada de plataforma e controle do ecossistema Mr.Sanpool.

Seu papel é centralizar capacidades de governança, administração e operação da plataforma, incluindo autenticação, tenancy, billing, observabilidade e operações do plano de controle.

## Componentes principais

### API
Responsável por expor os endpoints da plataforma para onboarding, autenticação, administração e operações enterprise.

### Auth e acesso
Camada responsável por autenticação, autorização e validação de acesso por contexto de tenant e perfil administrativo.

### Tenancy e billing
Responsável por tenants, usuários, planos, subscriptions e registro de uso para billing.

### Control plane
Coordena operações de deploy, reconciliação, recuperação e observação de runtime.

### Estado e auditoria
Mantém persistência de estado operacional e trilha de eventos administrativos e técnicos.

## Organização lógica

- `api/`: endpoints e contratos
- `auth/`: autenticação e dependências de acesso
- `db/`: models e persistência
- `services/`: regras de negócio e serviços de plataforma
- `control_plane/`: operações do plano de controle
- `state/`: persistência de estado
- `audit/`: trilha de auditoria

## Princípios

- isolamento por tenant
- separação entre camada SaaS e operações enterprise
- persistência explícita de estado relevante
- base para evolução de governança, quotas e entitlements

## Estado atual

A fundação atual já suporta:

- tenancy
- auth JWT
- planos
- onboarding/admin
- billing por uso
- enforcement inicial em rotas enterprise
