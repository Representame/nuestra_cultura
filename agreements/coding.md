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

## Estructura de código para modelos
Nosotros somo creyentes de la filosofía *"Fat models, slim controllers"*. Por lo mismo, el desarrollo de nuevos modelos puede ser confuso.

Este es un ejemplo de un modelo:

```ruby
# Documentacion de la clase. Esta no debe superar mas de 140 caracteres.
class DogeExample < ApplicationRecord
  # Este es el orden en el que deben ir escritos los callbacks
  # [after_*, before_*, alter_*, attr_accessor, has_one, has_many, has_and_belongs...]
  before_create :set_default_name
  after_initialize :init
  belongs_to :coding_guidelines
  accepts_nested_attributes_for :examples, :author
  has_many   :examples
  has_many   :doges
  has_one    :author

  # Luego los validadores
  validates_numericality_of :doge_id, greater_than_or_equal_to: 0 less_than_or_equal_to: 999, allow_blank: true
  validates_presence_of :author

  # La máquina de estados
  state_machine :state, :initial => :new do

    # Acá primero se escriben los callback para las transiciones, en este orden
    # [before_transition, around_transition, after_transition]  
    after_transition barking: :any do |coding_example, transition|
      coding_example.doges.each do |doge|
        puts "Wao. Such Doge. Many bark. - (#{doge.name})"
        doge.wage_tail
      end
    end

    # Luego de los callback, vienen los eventos
    event :bark do
      transition [:running, :sleeping] => :bark
    end

    event :bother do
      transition any: :running
    end

    event :sleep do
      transition any: :sleeping
    end

    event :rest do
      transition [:new, :barking, :running] => :sleeping
    end
  
    # Luego los atributos privados a cada estado
    state :barking do
      def accept_food
        self.sleep
      end
    end
  end # End of state machine


  def set_default_name
    self.name = "Shibberino"
  end

  def can_bark?
    self.state?(:running) or self.state?(:sleeping)
  end

  private

  def init
    self.state ||= :new
  end

  def self.search(query)
    joins(:company).where("description ILIKE ?", "%#{query}%")
  end

end
```


## Herramientas que utilizamos
* [RuboCop](https://github.com/bbatsov/rubocop)

## The Ruby Style Guide

Nos guíamos de la guía de estilo de Ruby, que puedes ver [acá](https://github.com/alemohamad/ruby-style-guide)
