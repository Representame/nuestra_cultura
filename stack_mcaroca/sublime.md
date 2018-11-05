#Sublime + iTerm2

##How to install

###Sublime
Como [Sublime](https://www.sublimetext.com/3) existen otros editores de texto como [Atom](https://atom.io/), por mi parte siempre he preferido el primero por los recursos que utiliza, si bien la licencia es un poco elevada, no es tan molesto cancelar cada cierto tiempo el cuadro de compra.

Solo debes acceder [aquí](https://www.sublimetext.com/3) y descargar la última versión para **OSX**/Windows/Linux y luego instalar.

###iTerm2
Así como un buen editor de texto, es comodo tener un buen terminal, para esto, hace un tiempo atrás deje el tipico 'Terminal' de OSX e instale lo que presento a continuación, más comodo y personalizable.


```sh
$brew cask install iterm2
```
Si no tienes homebrew (aunque deberias): Puedes [descargar](http://www.iterm2.com/downloads.html) e instalar iTerm2.

iTerm2 me gusta mucho más por su fidelidad del color y puedes personalizarlo más que el Terminal por defecto de OSX, esto suele ser una maña del desarrollador, esto, siempre es una alternativa.


Aquí más configuraciones de color de iTerm2


[Solarized Dark theme](https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Solarized%20Dark%20-%20Patched.itermcolors)
[Solarized Light theme](https://raw.githubusercontent.com/altercation/solarized/master/iterm2-colors-solarized/Solarized%20Light.itermcolors)
Más templates [iterm2colorschemes](http://iterm2colorschemes.com/)

Si no sabes que hacer con estos links, solo abre el archivo y guardalo donde estimes conveniente.
Esta configuración de colores debes importarla en iTerm2, esto se realiza de la siguiente manera: 
iTerm → preferences → profiles → colors → load presets. 
Puedes crear un perfil diferente al por defecto para que no lo pierdas.

###Oh My Zsh
Más info [aquí](https://github.com/robbyrussell/oh-my-zsh)

Instalar con curl
```sh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
cuando la instalación termine, edita el archivo '~/.zshrc' y agrega la siguiente linea

```sh
ZSH_THEME="agnoster"
```

