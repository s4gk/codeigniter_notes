Las **vistas** son una parte fundamental del patrón **MVC** (Model-View-Controller). Sirven para manejar la presentación de los datos, es decir, la capa de interfaz de usuario. Las vistas suelen contener el código HTML mezclado con algo de PHP para mostrar los datos dinámicos que provienen del controlador.

Por lo general las vistas se almacenan en el directorio `App/Views/` y puedes organizarlas en subdirectorios según tus necesidades.

Una vista es simplemente un archivo PHP que contiene HTML (y, opcionalmente, código PHP para la lógica de presentación). Ejemplo vamos a crear una vista llamada `welcome_message.php`:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome to CodeIgniter</title>
</head>
<body>
    <h1>Bienvenido a CodeIgniter</h1>
    <p>Este es un ejemplo de vista.</p>
</body>
</html>
```

Ahora procedemos a cargar una vista desde un controlador, Utilizando el método `view()` del servicio `\CodeIgniter\View\View` para cargar una vista desde un controlador.

```php
namespace App\Controllers;

class Welcome extends BaseController
{
    public function index()
    {
        return view('welcome_message');
    }
}
```

Aquí, CodeIgniter busca el archivo `welcome_message.php` en el directorio `App/Views`.

También podemos pasar datos a una vista. Puedes pasar datos desde el controlador a la vista utilizando un array asociativo.

```php
namespace App\Controllers;

class Welcome extends BaseController
{
    public function index()
    {
        $data = [
            'title' => 'Página de Inicio',
            'message' => 'Hola, este es un mensaje dinámico.'
        ];

        return view('welcome_message', $data);
    }
}
```

Y en la vista `welcome_message.php`, puedes acceder a esos datos:

```php
<h1><?= $title ?></h1>
<p><?= $message ?></p>
```

Puedes dividir tu interfaz en varias vistas y cargarlas como partes individuales (por ejemplo, un encabezado, un pie de página y una barra lateral).

```php
// header.php
<header>
    <h1>Este es el encabezado</h1>
</header>

// footer.php
<footer>
    <p>Este es el pie de página</p>
</footer>

// main.php
<?php include 'header.php'; ?>
<main>
    <h2>Contenido principal</h2>
</main>
<?php include 'footer.php'; ?>
```

O puedes usar el método view() para incluirlos:

```php
<?= view('header') ?>
<main>
    <h2>Contenido principal</h2>
</main>
<?= view('footer') ?>
```

Cargar vistas con plantillas es otra opción de generar vistas, las plantillas son útiles para mantener una estructura común en varias páginas.

```php
// layout.php (plantilla principal)
<!DOCTYPE html>
<html lang="en">
<head>
    <title><?= $title ?></title>
</head>
<body>
    <header>
        <h1><?= $header ?></h1>
    </header>
    <main>
        <?= $content ?>
    </main>
    <footer>
        <p><?= $footer ?></p>
    </footer>
</body>
</html>
```

En el controlador este seria el resultado:

```php
namespace App\Controllers;

class Welcome extends BaseController
{
    public function index()
    {
        $data = [
            'title' => 'Página de Inicio',
            'header' => 'Bienvenido',
            'content' => view('welcome_message'),
            'footer' => 'Pie de página'
        ];

        return view('layout', $data);
    }
}
```

Ademas contamos con algunas funciones útiles para la creación de vistas como : 

- `esc()` : Escapa cadenas de texto para proteger contra ataques XSS.
  
  ```php
  <p><?= esc($userInput) ?></p>
  ```
  
- **Ayudas de URL**: Para generar enlaces dinámicos.
  
  ```php
  <a href="<?= base_url('ruta') ?>">Enlace</a>
  ```

Algunas recomendaciones para buenas practicas son las siguientes:

- Mantén la lógica de negocio en el controlador, no en las vistas.
- Usa vistas parciales y plantillas para mantener tu código modular y limpio
- Escapa siempre las variables de usuario para evitar vulnerabilidades XSS.