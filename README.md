# Licitavia Web

Landing page oficial da Licitavia, organizada como projeto React + TypeScript.

## Objetivo atual

Apresentar a Licitavia como a plataforma B2G completa para empresas que querem competir e executar contratos públicos com mais controle:

`oportunidades → análise → habilitação → proposta → disputa → contrato → inteligência`

## Rodando localmente

Requisitos: Node.js 22+ e pnpm.

```bash
pnpm install
pnpm dev
```

Build de validação:

```bash
pnpm build
```

Gerar os HTMLs independentes:

```bash
pnpm build:html
```

Esse comando compila React, ReactDOM, os componentes de interface, os ícones, o CSS atual da aplicação e o JavaScript para dentro de três arquivos:

- `index.html`: landing page.
- `portal.html`: acesso, planos, onboarding com CNPJ/CNAEs, radar e operação.
- `admin.html`: console administrativo de empresas, usuários, integrações e auditoria.

Os HTMLs são autônomos e permitem conferir toda a interface sem instalar dependências. Chamadas de API, autenticação e persistência exigem o servidor do projeto.

Validação completa antes de uma entrega:

```bash
pnpm check:delivery
```

Além dessa verificação automatizada, o checklist de `AGENTS.md` exige inspeção visual do artefato entregue em desktop e celular.

## Estrutura

- `app/`: página, estilos e metadados.
- `app/api/`: consulta CNPJ, cadastro multiempresa, PNCP, radar e operações administrativas protegidas.
- `db/migrations/`: esquema SQL versionado para o banco D1.
- `lib/cnpj/`, `lib/pncp/` e `lib/matching/`: conectores, normalização e aderência explicável.
- `components/ui/`: componentes de interface acessíveis.
- `public/`: favicon, imagem social, robots e sitemap.
- `index.html`: versão autônoma com toda a aplicação incorporada.
- `portal.html`: versão autônoma e interativa do esboço do portal SaaS.
- `admin.html`: versão autônoma do console administrativo.
- `standalone/`: entrada e artefatos usados para gerar o HTML único.
- `docs/ESCOPO-MESTRE.txt`: escopo integral fornecido para o projeto.
- `docs/AUDITORIA-LANDING.md`: diagnóstico e prioridades.
- `docs/ESTRATEGIA-MARKETING.md`: pesquisa, posicionamento, SEO e aquisição.
- `docs/CHANGELOG-LANDING.md`: registro detalhado de alterações.
- `docs/PORTAL-ESCOPO.md`: decisões do esboço e próximos passos para transformar a interface em produto operacional.
- `docs/PORTAL-ARQUITETURA.md`: fluxo de acesso, regras dos planos, onboarding e fundação necessária para produção.
- `docs/PNCP-INTEGRACAO.md`: contrato técnico inicial, fontes oficiais e checklist de produção da integração PNCP.
- `docs/BANCO-E-INTEGRACOES.md`: banco, migração, CNPJ/CNAEs, sincronização e segurança administrativa.
- `AGENTS.md`: checklist obrigatório de revisão, testes e validação visual antes de entregas.
- `docs/archive/landing-original.html`: cópia imutável da landing recebida.

## Estado do formulário

O formulário de demonstração abre o cliente de e-mail do visitante com os dados preenchidos. Antes de tráfego pago ou lançamento público, deve ser conectado a CRM/backend com consentimento, registro de origem e política de privacidade.

## Banco e variáveis

O projeto usa o binding D1 `DB`, declarado em `.openai/hosting.json`. Aplique, em ordem, todas as migrações listadas em `db/schema.ts`. A versão 2 vincula a identidade autenticada ao usuário e registra a última empresa selecionada, eliminando empresas fixas na sessão. Copie `.env.example`, configure os provedores e nunca envie segredos ao repositório.

A consulta pública de CNPJ usa `CNPJ_PROVIDER=brasilapi` apenas como conveniência de desenvolvimento. Para produção, configure o serviço oficial contratado do Serpro ou mantenha uma base própria alimentada pelos dados abertos da Receita Federal.

## Proteção antibot

O portal está preparado para Cloudflare Turnstile no login, criação/recuperação de conta e cadastro empresarial. Para ativar, crie um widget para cada hostname autorizado, configure `TURNSTILE_SITE_KEY` e `TURNSTILE_SECRET_KEY` no ambiente do site e habilite **Turnstile** em Supabase Auth → Bot and Abuse Protection, usando a chave secreta do mesmo widget. Só então defina `TURNSTILE_REQUIRED=1` e teste login, falha de senha, cadastro de usuário e cadastro de empresa no domínio real. Não coloque a chave secreta em variáveis públicas ou no repositório. Na entrada, o desafio fica visível após a primeira senha incorreta; a validação de login é feita pelo Supabase Auth e a do cadastro empresarial pelo servidor da Licitavia. Sem essas configurações externas, a proteção não está ativa.

## Homologação e situação de entrega

O laboratório de `scripts/serve-homologation.mjs` usa apenas `127.0.0.1`, banco temporário e dados sintéticos **identificados como teste**; ele não acessa PNCP, Supabase nem produção. Os dados sintéticos jamais devem ser apresentados como licitações reais. O `portal.html` autônomo, por sua vez, inicia vazio e não tem persistência. Consulte [o relatório de evolução](docs/relatorio-evolucao-operacional-2026-09-28.md) antes de migrar dados ou publicar: as integrações oficiais, a operação ao vivo e os testes com usuários autorizados ainda têm pendências.
