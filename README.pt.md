# tmux Cheatsheet · Cola de referência de tmux

> 🌍 [中文](README.md) · [Español](README.es.md)

Uma **cola de uma página só** com 30 usos reais de tmux: sessões, janelas, painéis, modo de cópia e buffers, configuração em `~/.tmux.conf` e automação. Cada entrada responde a apenas três perguntas: **o que ela faz, um comando pronto para copiar e colar, e etiquetas de uso**. Sem teclas inventadas: cada comando CLI foi executado de verdade no tmux 3.4 em sessões separadas, com resultados conferidos via `capture-pane` e os comandos `list-*`; cada tecla de prefixo foi contrastada com a tabela de atalhos padrão impressa por `tmux list-keys`; e os exemplos de configuração foram carregados com `tmux -f` e relidos com `show-options`. E o ponto onde quase todo mundo trava, logo de cara: **o prefixo padrão do tmux é Ctrl-b**; em tudo o que segue, `prefix` significa Ctrl-b — «`prefix` c» é apertar Ctrl-b, soltar e depois apertar c.

## Para quem é

Desenvolvedores backend, engenheiros de operações (SRE/DevOps) e qualquer pessoa que rode tarefas longas num servidor. Se a cada divisão de painel você hesita entre `%` e `"`, se entra no modo de cópia e não sabe sair, ou se uma queda de SSH leva seu trabalho junto, esta cola é para você.

## Como usar

1. Vá direto ao **[CHEATSHEET.md](CHEATSHEET.md)**: o corpo da referência; os comandos em inglês qualquer desenvolvedor entende.
2. Você precisa do tmux: no Linux, pelo gerenciador de pacotes da sua distribuição (`apt install tmux` / `dnf install tmux`); no macOS, `brew install tmux`. Para praticar não precisa de servidor: o terminal local basta para tudo.
3. Siga a ordem da cola —sessões → janelas → painéis → modo de cópia → configuração → automação— e teste cada comando na mão. Em uma hora você domina tudo.

## Teste em 30 segundos

```bash
tmux -V                  # imprime a versão (ex.: tmux 3.4): está instalado
tmux new -s demo         # cria uma sessão chamada demo e entra (barra de status embaixo)
# dentro, aperte Ctrl-b e depois d (detach): você volta ao terminal e a sessão segue viva
tmux ls                  # a demo continua lá: não sumiu quando você se separou
tmux attach -t demo      # volte a entrar: a tela está exatamente como você deixou
```

Com esse vai-e-volta de detach → attach você já verificou com as próprias mãos o argumento central do tmux: **feche o terminal ou caia o SSH, a sessão e as tarefas dentro dela continuam vivas**.

## Um ~/.tmux.conf pronto para usar

```bash
set -g prefix C-a                              # o prefixo passa a ser Ctrl-a
unbind C-b
bind C-a send-prefix
set -g mouse on                                # clique para escolher o painel, arraste bordas, roda do mouse para rolar
set -g history-limit 10000                     # 10000 linhas de histórico (padrão: 2000)
set -g base-index 1                            # as janelas começam em 1
set-window-option -g pane-base-index 1         # os painéis também começam em 1
set -g renumber-windows on                     # ao fechar uma janela, renumera sem buracos
bind r source-file ~/.tmux.conf \; display "Reloaded!"   # prefix r recarrega a configuração
bind | split-window -h -c "#{pane_current_path}"         # prefix | divide lado a lado, mesma pasta
bind - split-window -v -c "#{pane_current_path}"         # prefix - divide empilhado, mesma pasta
```

Salve em `~/.tmux.conf` e reinicie o tmux (ou aperte `prefix r` numa sessão já aberta). Atenção: a partir daí o prefixo é Ctrl-a.

## O que está incluído

| Categoria | Conteúdo |
|---|---|
| 🖥️ Sessões | criar com nome / ls e attach para voltar / detach / renomear a sessão / kill-session e a árvore de sessões (5) |
| 🧩 Janelas | criar e renomear / trocar com n, p, dígitos e a lista / fechar janela / criar pelo terminal / swap e move (5) |
| 📐 Painéis | dividir com % e " / mover-se com setas, o e q / zoom com z / trocar e layouts / redimensionar / fechar painel e break-pane (6) |
| 📋 Modo de cópia e buffers | entrar e percorrer o histórico / teclas de cópia emacs e vi / colar e a pilha de buffers / capture-pane / save-buffer (5) |
| ⚙️ Configuração ~/.tmux.conf | prefixo C-a / mouse ligado / histórico e numeração / recarregar com uma tecla / divisões que herdam a pasta (5) |
| 🔗 Automação e extras | sessões em segundo plano e send-keys / pipe-pane para logs / list-keys / choose-tree e kill-server (4) |

A documentação oficial é só https://github.com/tmux/tmux/wiki; cada comando foi verificado contra o tmux 3.4 executando de verdade (CLI em sessões reais, teclas contra `list-keys`, configuração carregada de verdade). Os nomes de sessão dos exemplos são genéricos (work / demo): nenhum servidor real.

## Contribuições são bem-vindas

Achou um comando errado, uma tecla que não bate com a sua versão ou uma tradução a melhorar? *Pull requests* são bem-vindos. Mantenha o formato (descrição / bloco bash / etiquetas) e **só comandos reais e verificados; na dúvida, melhor não incluir**.

## 📚 Mais folhas de referência

Outras colas do mesmo autor, também em 中文 / Español / Português:

- [FFmpeg 命令速查表](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 comandos de áudio e vídeo
- [Git 实战速查表](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 comandos do dia a dia e de resgate
- [Docker 命令速查表](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 comandos de contêineres
- [curl 命令速查表](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 comandos para depurar APIs
- [正则表达式速查表](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 padrões práticos de expressões regulares
- [jq 命令速查表](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 filtros para JSON
- [终端神器速查表](https://github.com/Ezra-Zhao/terminal-superpowers-cheatsheet) — 14 ferramentas de eficiência para o terminal
- [Adobe 免费替代速查表](https://github.com/Ezra-Zhao/adobe-free-alternatives-cheatsheet) — 16 alternativas gratuitas ao Adobe
- [退订省钱速查表](https://github.com/Ezra-Zhao/desubscribe-cheatsheet) — 10 trocas de assinaturas, ≈ $1,260/ano de economia
- [kubectl 命令速查表](https://github.com/Ezra-Zhao/kubectl-cheatsheet) — comandos habituais do Kubernetes
- [SQL 查询速查表](https://github.com/Ezra-Zhao/sql-cheatsheet) — consultas e funções SQL habituais
- [SSH 命令速查表](https://github.com/Ezra-Zhao/ssh-cheatsheet) — 28 entradas de conexão, chaves e túneis SSH

## Licença

MIT © 2026 Guangyi Zhao. Veja [LICENSE](LICENSE).
