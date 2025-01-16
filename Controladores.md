
En CodeIgniter, los **controladores** (**Controllers**) son una parte esencial del patrón MVC (Model-View-Controller). Los Controladores actúan como intermediarios entre el modelo, la vista, y cualquier lógica que el usuario interactúe en una aplicación.

Un controlador es una clase PHP que contiene métodos que responden a solicitudes específicas realizadas a tu aplicación Web. Cada controlador se encarga de:

- Recibir las solicitudes del navegador.
- Procesar esas solicitudes (Generalmente llamando modelos para acceder a datos).
- Pasar datos a las vistas.
- Retornar las vistas al navegador.

Todos los controladores deben estar en el directorio `App/Controllers/` y extender la clase base `CodeIgniter\Controller`.

```php
<?php

namespace App\Controllers;

use CodeIgniter\Controller;

class MiControlador extends Controller
{
    public function index()
    {
        // Este método responde a la URL /miControlador
        return view('welcome_message');
    }

    public function saludo()
    {
        // Este método responde a la URL /miControlador/saludo
        return "¡Hola, CodeIgniter!";
    }
}
```

En CodeIgniter los controladores son llamados mediante el parámetro **namespace**, hacemos referencia a los directorios donde se encuentre nuestro Controller.

```php
namespaces App\Controllers;
```
