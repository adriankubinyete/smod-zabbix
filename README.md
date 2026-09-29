# zabbix

Sincroniza os alertas do Zabbix com o Discord. Feito para a **Phonevox**.

Módulo do bot de Discord construído sobre o framework **sora**. Foi criado pra ser usado em cima dele: a pasta do módulo vai em `src/usermodules/` e o loader a carrega sozinho, sem registro manual.

## O que faz

Recebe eventos de trigger por webhook e transforma cada um em uma thread num canal fórum (1 evento = 1 thread). Suporta ack, finalização e mudança de severidade por botões, reconciliação periódica com a API do Zabbix e arquivamento automático das threads resolvidas.

Comando: `zabbix` com o grupo `config` (`cargo-operador`, `canal-forum`, `ping-severidade`, `ver`) e o subcomando `reconciliar`.

## Instalação

Clone este repositório dentro de `src/usermodules/` do seu projeto sora e reinicie o bot (ou use `!bot reload`). O nome da pasta não importa: o módulo é identificado pelo `name` do cog (`zabbix`).

## Variáveis de ambiente

Lidas de `process.env` e centralizadas em `settings.ts` (`modConfig`).

| Variável | Descrição |
|---|---|
| `MOD_ZABBIX_WEBHOOK_SECRET` | Segredo compartilhado configurado no media type do Zabbix |
| `MOD_ZABBIX_API_URL` | URL do `api_jsonrpc.php` |
| `MOD_ZABBIX_WEB_URL` | URL do frontend (links de evento/host) |
| `MOD_ZABBIX_API_USER` | Usuário de serviço da API |
| `MOD_ZABBIX_API_PASSWORD` | Senha do usuário de serviço |
| `MOD_ZABBIX_INGEST_PORT` | Porta do servidor de webhook (padrão `3001`) |
| `MOD_ZABBIX_WEBHOOK_PATH` | Path do webhook (padrão `/webhook`) |
| `MOD_ZABBIX_ALLOWED_HOST` | Host permitido no webhook |
| `MOD_ZABBIX_FORUM_CHANNEL_ID` | Canal fórum de fallback (prefira `/zabbix config canal-forum`) |
| `MOD_ZABBIX_RECONCILE_INTERVAL_MS` | Intervalo da reconciliação (padrão 5 min) |
| `MOD_ZABBIX_RECONCILE_SINCE` | `start` (padrão), `ever`, `never` ou timestamp Unix |
| `MOD_ZABBIX_TZ_OFFSET_MINUTES` | Fuso do **servidor** Zabbix, em minutos (padrão `0`) |
| `MOD_ZABBIX_ARCHIVE_DELAY_MS` | Atraso pra arquivar thread resolvida (padrão 24 h) |

## Banco

As migrations rodam sozinhas no carregamento do módulo (Postgres do framework).

## Dependências

Sem as quatro primeiras variáveis o módulo carrega, mas a integração fica inativa.
