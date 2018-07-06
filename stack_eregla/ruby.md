# RBENV + bundler
Rbenv es una herramienta para el manejo de ambientes para Ruby. A diferencia de rvm, rbenv otorga mayor control entregando entornos aislados con sus propias gemas globales y variables.

# Instalación
El método recomendado de instalación es por medio de [homebrew](./eregla_homebrew.md).
1. Instala rbenv con el siguiente comando `brew install rbenv`
2. Ejecuta `rbenv init` para obtener las instrucciones de integración hacia tu intérprete de comandos. Este puede variar dependiendo si usas bash o zsh.

# Instalación de Ruby por medio de rbenv
```bash
rbenv install -l        # lista las versiones disponibles
rbenv install 2.4.1p111 # instala una versión en específico
gem install bundler     # instala bundler
rbenv global 2.4.1p111  # setea la versión de ruby instalada como global
```