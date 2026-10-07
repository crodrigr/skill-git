# Ejemplo 01 — Instalación y configuración de Git

## Estado inicial

Una computadora con Linux (los mismos comandos finales aplican, cambiando solo el paso
de instalación, en Windows y macOS — ver la Explicación conceptual) sin Git instalado.

## Comandos

```bash
# 1. Instalar Git (Debian/Ubuntu)
sudo apt update
sudo apt install git

# 2. Verificar que la instalación funcionó
git --version

# 3. Configurar la identidad (una sola vez por computadora)
git config --global user.name "Ada Lovelace"
git config --global user.email "ada@ejemplo.com"

# 4. Verificar la configuración
git config --list
```

## Explicación paso a paso

1. `sudo apt install git` descarga e instala el programa `git` desde los repositorios
   del sistema. En Windows se usa el instalador de "Git for Windows"; en macOS,
   `brew install git` o las Herramientas de Línea de Comandos de Xcode.
2. `git --version` confirma que el comando `git` quedó disponible en la terminal y
   muestra la versión instalada.
3. `git config --global user.name` y `git config --global user.email` guardan el
   nombre y el correo que van a aparecer en cada commit que se haga desde esta
   computadora. El flag `--global` significa que aplica a todos los repositorios, no
   solo a uno.
4. `git config --list` imprime toda la configuración activa, incluidas las dos líneas
   que acabamos de definir, para confirmar que quedaron guardadas correctamente.

## Resultado esperado

```console
$ git --version
git version 2.43.0

$ git config --list
user.name=Ada Lovelace
user.email=ada@ejemplo.com
```

(La versión exacta de Git puede variar; cualquier versión 2.x reciente sirve para toda
la clase.)
