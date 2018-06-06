# API Guidelines

Manual de construcción de una API de y para Representame.cl.

## Tips de Diseño de la APIs 
### Use Versioning

Siempre que se parta la construcción de una API tanto interna como externa una de las cosas que puede 
generar un alto costo es la incertidumbre de cuánto la utilizaremos realmente, es por esto que necesitamos 
agregar valor a lo construido, configurando desde un principio versionamiento, si el producto prospera 
y se vuelve exitoso tendremos que hacer cambios en la API y tendremos que liberar una nueva versión con 
las modificaciones respectivas.

El contexto es simple, se modifica una interfaz API, se genera una nueva versión, lo antiguo se irá 
deprecando cada 2 versiones, por ejemplo si tenemos la versión 1, versión 2 y versión 3, al construir 
la versión 4, la 1 deja de existir en nuestro proyecto y será eventualmente deprecada, por lo que es 
necesario revisar que algo no se pierda en su totalidad.

Afortunadamente en Rails, el versionamiento es simple:

```ruby
namespace :api do
  namespace :v1 do
    resources :projects do
      resources :books
    end
  end
end
```

Donde el resultado será: `/api/v1/projects` (en el path)

Además, debes mover los controladores dentro de app/controllers/api/v1 sin perder la convención de Rails:

```

app/controllers
├── api
│     └── v1
│          └── authors_controller.rb
│          └── books_controller.rb
├── application_controller.rb
├── concerns
...
```
Además, debes agregar estos namespaces en los controladores:

```ruby
module Api
  module V1
    class AuthorsController < ApplicationController
      # ...
    end
  end
end
```

Para crear una nueva versión pero que no sea compatible con lo que has realizado previamente solo 
debes agregar un módulo nuevo de la siguiente forma:

```ruby
module Api
  module V2
    class AuthorsController < ApplicationController
      # cambios no compatibles con el pasado
    end
  end
end
```

En el caso de que la nueva versión necesite incluir funciones nuevas y/o necesite un fix por lo 
que quede casi idéntica, se debe seguir la siguiente estructura, intentando mantener el versionamiento 
de la API de la forma más limpia posible:

```ruby
module Api
  module V2
    class BooksController < Api::V1::BooksController
    end
  end
end
```

### HTTP Status Codes 

HTTP ofrece un mecanismo excelente para quien consume o consumirá nuestra API o cualquier API para 
indicar si la solicitud fue exitosa o no, los famosos HTTP status code o códigos de estado.

Estos códigos de estado hay que usarlos y además, usarlos de forma correcta, para que los 
consumidores tengan información útil, al final siempre lo agradecerán.

La siguiente tabla muestra el uso de los códigos (en inglés) de estados HTTP en una API-REST tradicional:


| Código  | Nombre                | Significado                                                                                           |
|---------|-----------------------|-------------------------------------------------------------------------------------------------------|
| 200     | OK                    | Todo está perfect! Retornaremos el recurso solicitado.                                                |
| 201     | Created               | Voilá! Creamos el recurso de forma exitosa.                                                           |
| 204     | No Content            | No hay nada que te podamos mostrar, útil si se acaba de eliminar un objeto por ejemplo.               |
| 401     | Unauthorized          | No nos has proporcionado credenciales validas.                                                        |
| 404     | Not found             | Si el objeto solicitado no fue encontrado o si la solicitud no existe, se debe retornar este codigo.  |
| 422     | Unprocessable Entity  | El recurso no pudo ser guardado, quizás hay un error de validación.                                   | 

Lo controladores en Rails deben usar de forma correcta estos códigos, por ejemplo de la siguiente forma:

```ruby
render json: @object, status: :created
```

### Estamos hablando en HTTP, siempre.
Además de los códigos de estado HTTP, que ya son increíblemente útiles, también se deben usar de 
forma correcta los verbos HTTP. Los más comunes (en ingles) API-REST son:

| Verbo   | Uso                                                                           |
|---------|-------------------------------------------------------------------------------|
| GET     | Retrieve and only retrieve data. Never change any data within a GET request.  |
| POST    | Create a new resource.                                                        | 
| PUT     | Update an existing resource (partially).                                      | 
| PATCH   | Update an existing resource (partially).                                      |
| DELETE  | Remove a resource.                                                            | 

El uso de estos verbos guía al consumidor y al desarrollador hacia el uso correcto de la API. 
Los métodos de enrutamiento de Rails facilitan la implementación del metodo HTTP correcto para 
su acción. Si hay dudas, esta es nuestra [guia oficial](http://guides.rubyonrails.org/routing.html).

### Combinación de los verbos primarios/CRUD y un ejemplo de collections

| **HTTP Method** | **CRUD**            | **Entire Collection (e.g. /customers)**                                                                 |   **Specific Item (e.g. /customers/{id})**                                  |
|-----------------|---------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| POST            | Create              | 201 (Created), 'Location' header with link to /customers/{id} containing new ID.	                      | 404 (Not Found), 409 (Conflict) if resource already exists.                 |
| GET             | Read                | 200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists.               | 200 (OK), single customer. 404 (Not Found), if ID not found or invalid.     |
| PUT             | Update/Replace      | 405 (Method Not Allowed), unless you want to update/replace every resource in the entire collection.	  | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.  |
| PATCH           | Update/Modify       | 405 (Method Not Allowed), unless you want to modify the collection itself.                              | 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.  |
| DELETE          | Delete              | 405 (Method Not Allowed), unless you want to delete the whole collection—not often desirable.           | 200 (OK). 404 (Not Found), if ID not found or invalid.                      |

### Rate Limiting

Cuando nuestros usuarios comiencen a utilizar el API inicialmente probablemente no tengamos
que preocuparnos mucho por el rendimiento o el límite de recursos.

Sin embargo, si nuestra aplicación es un éxito y la cantidad de usuarios comienza a aumentar
explosivamente, el flujo de trabajo y el consumo de la API será mayor, por lo que las cosas
pueden salir mal: quizás algo del MVP quedo mal programado o diseñado y llamaremos a algún endpoint
en bucles infinitos, con una concurrencia increíblemente alta y CRON mal configurados, que puede
provocar solicitar una URL miles de veces en una hora por ejemplo.

Es por esto que consideraremos implementar un límite de velocidad desde el principio o desde
cualquier API que sea construida de aquí en adelante.

Esto no solo puede evitar que el rendimiento nos afecte, si no que también les da a nuestros
usuarios una indicación sobre cómo usar la plataforma (API externa por ej.) de manera adecuada
al recomendar un número máximo de solicitudes para un intervalo de tiempo dado.

Se implementará un límite de frecuencia muy simple para algunos de los endpoints más intensos.
Para infraestructuras aún más grandes, un enfoque más sofisticado (por ejemplo, a través de
Nginx `limit_req`).

```nginx
X-Rate-Limit-Limit: 1000
X-Rate-Limit-Remaining: 1920
X-Rate-Limit-Reset: 2015-03-10T13:00:00Z
```

[Fuente](https://developer.github.com/v3/#rate-limiting)
tdd 