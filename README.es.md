# tmux Cheatsheet · Hoja de referencia de tmux

> 🌍 [中文](README.md) · [Português](README.pt.md)

Una **hoja de referencia de una sola página** con 30 usos reales de tmux: sesiones, ventanas, paneles, modo de copia y búferes, configuración en `~/.tmux.conf` y automatización. Cada entrada responde solo tres preguntas: **qué hace, un comando listo para copiar y pegar, y etiquetas de uso**. Sin teclas inventadas: cada comando CLI se ejecutó de verdad en tmux 3.4 con sesiones separadas y los resultados se comprobaron con `capture-pane` y los comandos `list-*`; cada tecla de prefijo se contrastó con la tabla de asignaciones por defecto que imprime `tmux list-keys`; y los ejemplos de configuración se cargaron con `tmux -f` y se releyeron con `show-options`. Y lo primero, porque es donde casi todo el mundo se atasca: **el prefijo por defecto de tmux es Ctrl-b**; en todo lo que sigue, `prefix` significa Ctrl-b — «`prefix` c» es pulsar Ctrl-b, soltar y luego pulsar c.

## Para quién es

Desarrolladores backend, ingenieros de operaciones (SRE/DevOps) y cualquiera que ejecute tareas largas en un servidor. Si cada vez que divides un panel dudas entre `%` y `"`, si entras en el modo de copia y no sabes salir, o si una caída de SSH se lleva tu trabajo por delante, esta hoja es para ti.

## Cómo usarla

1. Ve directo a **[CHEATSHEET.md](CHEATSHEET.md)**: el cuerpo de la referencia; los comandos en inglés los entiende cualquier desarrollador.
2. Necesitas tmux: en Linux, con el gestor de paquetes de tu distribución (`apt install tmux` / `dnf install tmux`); en macOS, `brew install tmux`. Para practicar no hace falta servidor: tu terminal local basta para todo.
3. Sigue el orden de la hoja —sesiones → ventanas → paneles → modo de copia → configuración → automatización— y prueba cada comando a mano. En una hora los dominas.

## Pruébalo en 30 segundos

```bash
tmux -V                  # imprime la versión (p. ej. tmux 3.4): está instalado
tmux new -s demo         # crea una sesión llamada demo y entra (barra de estado abajo)
# dentro, pulsa Ctrl-b y luego d (detach): vuelves a tu terminal y la sesión sigue viva
tmux ls                  # demo sigue ahí: no desapareció al separarte
tmux attach -t demo      # vuelve a entrar: la pantalla está tal como la dejaste
```

Con ese ida y vuelta de detach → attach ya verificaste con tus manos el argumento central de tmux: **cierres la terminal o se corte el SSH, la sesión y sus tareas siguen vivas**.

## Un ~/.tmux.conf listo para usar

```bash
set -g prefix C-a                              # el prefijo pasa a ser Ctrl-a
unbind C-b
bind C-a send-prefix
set -g mouse on                                # clic para elegir panel, arrastrar bordes, rueda para desplazarte
set -g history-limit 10000                     # 10000 líneas de historial (por defecto: 2000)
set -g base-index 1                            # las ventanas empiezan en 1
set-window-option -g pane-base-index 1         # los paneles también empiezan en 1
set -g renumber-windows on                     # al cerrar una ventana, se renumeran sin huecos
bind r source-file ~/.tmux.conf \; display "Reloaded!"   # prefix r recarga la configuración
bind | split-window -h -c "#{pane_current_path}"         # prefix | divide en vertical, misma carpeta
bind - split-window -v -c "#{pane_current_path}"         # prefix - divide en horizontal, misma carpeta
```

Guárdalo en `~/.tmux.conf` y reinicia tmux (o pulsa `prefix r` en una sesión ya abierta). Ojo: a partir de ahí el prefijo es Ctrl-a.

## Qué incluye

| Categoría | Contenido |
|---|---|
| 🖥️ Sesiones | crear con nombre / ls y attach para volver / detach / renombrar la sesión / kill-session y el árbol de sesiones (5) |
| 🧩 Ventanas | crear y renombrar / cambiar con n, p, dígitos y la lista / cerrar ventana / crear desde la terminal / swap y move (5) |
| 📐 Paneles | dividir con % y " / moverse con flechas, o y q / zoom con z / intercambiar y layouts / redimensionar / cerrar panel y break-pane (6) |
| 📋 Modo de copia y búferes | entrar y recorrer el historial / teclas de copia emacs y vi / pegar y la pila de búferes / capture-pane / save-buffer (5) |
| ⚙️ Configuración ~/.tmux.conf | prefijo C-a / ratón activado / historial y numeración / recarga con una tecla / divisiones que heredan la carpeta (5) |
| 🔗 Automatización y varios | sesiones en segundo plano y send-keys / pipe-pane para registros / list-keys / choose-tree y kill-server (4) |

La documentación oficial es solo https://github.com/tmux/tmux/wiki; cada comando está verificado contra tmux 3.4 ejecutándolo de verdad (CLI en sesiones reales, teclas contra `list-keys`, configuración cargada de verdad). Los nombres de sesión de los ejemplos son genéricos (work / demo): ningún servidor real.

## Contribuciones bienvenidas

¿Un comando incorrecto, una tecla que no coincide con tu versión o una traducción mejorable? Los *pull requests* son bienvenidos. Mantén el formato (descripción / bloque bash / etiquetas) y **solo comandos reales y verificados; ante la duda, mejor no incluirlo**.

## 📚 Más hojas de referencia

Otras hojas del mismo autor, también en 中文 / Español / Português:

- [FFmpeg 命令速查表](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 comandos de audio y vídeo
- [Git 实战速查表](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 comandos diarios y de rescate
- [Docker 命令速查表](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 comandos de contenedores
- [curl 命令速查表](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 comandos para depurar APIs
- [正则表达式速查表](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 patrones prácticos de expresiones regulares
- [jq 命令速查表](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 filtros para JSON
- [终端神器速查表](https://github.com/Ezra-Zhao/terminal-superpowers-cheatsheet) — 14 herramientas de eficiencia para la terminal
- [Adobe 免费替代速查表](https://github.com/Ezra-Zhao/adobe-free-alternatives-cheatsheet) — 16 alternativas gratuitas a Adobe
- [退订省钱速查表](https://github.com/Ezra-Zhao/desubscribe-cheatsheet) — 10 sustituciones de suscripciones, ≈ $1,260/año de ahorro
- [kubectl 命令速查表](https://github.com/Ezra-Zhao/kubectl-cheatsheet) — comandos habituales de Kubernetes
- [SQL 查询速查表](https://github.com/Ezra-Zhao/sql-cheatsheet) — consultas y funciones SQL habituales
- [SSH 命令速查表](https://github.com/Ezra-Zhao/ssh-cheatsheet) — 28 entradas de conexión, claves y túneles SSH

## Licencia

MIT © 2026 Guangyi Zhao. Ver [LICENSE](LICENSE).
