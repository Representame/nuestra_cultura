# Homebrew
Homebrew es un gestor de paquetes para OSX, el cual cumple la misma función que desempeña `apt-get`, `yum` y herramientas similares en distribuciones GNU Linux. Este descarga los binarios en una carpeta llamada `cellar` la cual es alojada en un directorio que está bajo el control del usuario y luego conecta los binarios respectivos al PATH sin mayor intervención. Adicionalmente, ofrece una funcionalidad llamada `cask`, la cual permite instalar aplicaciones para OSX que no necesariamente requieren de una compilación propiamente tal, o bien que son privativas, como es el caso de iTerm2.

## Instalación
El programa de instalación indica paso a paso lo que el usuario debe hacer. Incluso asiste en la instalación de las herramientas de consola para OSX (las cuales son un requisito). Para iniciar la instalación, ejecute el siguiente comando en una terminal.

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## ¿Cómo utilizarlo?
```bash
brew install el_paquete_que_quiero instalar
```

### Ejemplo
```bash
brew install graphviz #instala el paquete grapviz
```