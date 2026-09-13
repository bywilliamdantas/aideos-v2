# AIDE OS

App único (PWA) que reúne dois módulos de trabalho de campo:

- **Gerenciamento de Encerramento** — preenchimento de atendimentos e geração de relatórios de encerramento prontos para copiar no WhatsApp.
- **Solicitação Infraestrutura** — solicitação e acompanhamento de demandas de infraestrutura.

Os dois módulos compartilham o mesmo técnico salvo no aparelho e o mesmo histórico de atendimentos, acessível de qualquer lugar do app.

## Como funciona

Na primeira vez que o app é aberto, a tela inicial pede o **nome do técnico**. Esse nome fica salvo no aparelho e é reaproveitado automaticamente em todos os módulos — não é pedido novamente, a menos que o técnico toque em "Trocar" na tela inicial.

A partir da tela inicial, basta escolher um dos dois módulos e tocar em "Continuar".

Cada atendimento gerado (em qualquer um dos módulos) fica salvo no aparelho e pode ser consultado na tela de **Histórico**, acessível pelo link na tela inicial ou pelo atalho "Histórico" no topo de cada módulo. Lá é possível filtrar por módulo (Todos / Encerramento / Infraestrutura), ver o relatório completo, copiar o texto novamente, abrir o módulo de origem ou excluir um atendimento.

O app funciona **offline**: depois do primeiro carregamento, o service worker guarda em cache a tela inicial, os dois módulos, a tela de histórico e os ícones, então tudo continua acessível sem internet. Sempre que houver conexão, o app atualiza o cache em segundo plano.

## Estrutura do projeto

```
aide-os/
├── index.html                     # tela inicial (seleção de módulo + nome do técnico + link do histórico)
├── historico.html                 # histórico unificado dos dois módulos, com filtro
├── manifest.json                  # manifesto do PWA
├── sw.js                          # service worker (cache offline)
├── icons/                         # ícones do app (192, 512, maskable, apple-touch-icon)
└── modulos/
    ├── encerramento/index.html    # módulo Gerenciamento de Encerramento
    └── infraestrutura/index.html  # módulo Solicitação Infraestrutura
```

## Sistema visual

O app usa uma identidade visual escura pensada para uso em campo (baixa luminosidade, leitura rápida), com uma cor de destaque própria para cada módulo — assim o técnico reconhece em qual módulo está só pelo tom da tela:

- **Encerramento** → laranja âmbar (`#ff8a3d`), remetendo a fechamento/conclusão do atendimento.
- **Infraestrutura** → azul sinal (`#3aa3ff`), remetendo a rede/conectividade.
- Neutros compartilhados: fundo `#0a0b0d`, cartões em gradiente `#1b1e23 → #101216`, texto principal `#f3f4f6`.
- Estados: verde `#34d399` para "Encerramento/OK", âmbar `#f2b134` para "Em andamento/pendente" (independente do módulo), vermelho `#ff5c52` para campos obrigatórios não preenchidos.

Cada card de formulário é numerado automaticamente (1, 2, 3…) na ordem em que aparece, reforçando que o preenchimento é uma sequência, e a tela inicial e o histórico usam as mesmas cores de módulo para dar contexto instantâneo. Campos numéricos com unidade (potência, metragem de cabo, velocidade) mostram a unidade (dBm, m, Mbps) diretamente ao lado do rótulo.

## Publicar no GitHub Pages

1. Crie um repositório novo no GitHub (por exemplo `aide-os`) e suba todo o conteúdo desta pasta na branch `main`.
2. No repositório, vá em **Settings → Pages**. Em "Build and deployment → Source", selecione **GitHub Actions**.
3. O workflow em `.github/workflows/deploy.yml` já está pronto: a cada push na `main`, o app é publicado automaticamente — não é preciso configurar nada além do passo 2.
4. Após o primeiro push, acompanhe a aba **Actions** do repositório até o workflow terminar com sucesso (ícone verde).
5. A URL final fica no formato `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`. Abra essa URL no celular.
6. Para instalar como app: no Android (Chrome), toque no menu (⋮) e em "Adicionar à tela inicial" / "Instalar app". No iPhone (Safari), toque em Compartilhar → "Adicionar à Tela de Início".
7. Depois de instalado, o app abre em tela cheia (sem barra do navegador) e continua funcionando offline graças ao service worker.
8. Sempre que publicar uma atualização (novo push na `main`), pode ser necessário fechar e reabrir o app (ou aguardar alguns segundos com internet) para o service worker buscar a nova versão em segundo plano.

## Status do desenvolvimento

- [x] Etapa 1 — Base do app (tela inicial, roteamento, nome persistente, estrutura de arquivos)
- [x] Etapa 2 — Módulo de Encerramento + correção do texto de franquia do repetidor (2.0/3.0 x 4.0)
- [x] Etapa 3 — Correção/validação da Classificação (Andamento x Encerramento) — auditados os 13 tipos de atendimento
- [x] Etapa 4 — Módulo de Solicitação Infraestrutura (migrado do relatorio-infra, técnico e histórico unificados)
- [x] Etapa 5 — Fechamento do PWA (histórico unificado com filtro, manifest/service worker/ícones revisados, checklist visual entre os módulos, publicação documentada)
