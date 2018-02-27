#Ruby
## Guía de estilo de Desarrollo

* Usamos UTF-8 como encoding por defecto
* Usamos indentación con 2 espacios.
```ruby
  # malo - 4 espacios
  def some_method
      do_something
  end

  # bien! :)
  def some_method
    do_something
  end
```
* Usamos Unix-style line endings
** Como utilizamos Git, te pedimos que agregues la siguiente configuración para proteger nuestros proyectos de las terminaciones de linea de Windows:
```
$ git config --global core.autocrlf true
```
* No uses `;` para separar declaraciones o expresiones. Usa una expresión por linea.
```ruby
  # mal
  puts 'foobar'; # innecesario

  puts 'foo'; puts 'bar' # dos expresiones en una misma linea

  # bien! :)
  puts 'foobar'

  puts 'foo'
  puts 'bar'

  puts 'foo', 'bar'
```
* Prefiere usar el formato de una linea si una clase no tiene contenido.
```ruby
  # mal
  class FooError < StandardError
  end

  # ;)
  class FooError < StandardError; end

  # bien! :)
  FooError = Class.new(StandardError)
```

##Herramientas que utilizamos
* [RuboCop](https://github.com/bbatsov/rubocop)

##The Ruby Style Guide

Nos guíamos de la guía de estilo de Ruby, que puedes ver [acá](https://github.com/alemohamad/ruby-style-guide)
