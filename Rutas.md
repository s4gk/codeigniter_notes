Las rutas en CodeIgniter son una parte fundamental de su sistema de enrutamiento, que determina cómo las solicitudes HTTP son dirigidas a los controladores y métodos específicos de una aplicación. En CodeIgniter, puedes definir rutas personalizadas para crear URL amigables, manejar diferentes comportamientos y mejorar la experiencia del usuario.

Las rutas se configuran en el archivo `App/Config/Routes.php`. Este archivo contiene una instancia de la clase **RouteCollection**, que se utiliza para definir las rutas de tu aplicación.

Existen varios tipos de rutas tales como :

---

- Rutas predeterminadas: Estas rutas indican el controlador y el método que se ejecutará si no se especifica nada en la en la URL, por ejemplo:
  
```php
 $routes->setDefaultController('Home');
 $routes->setDefaultMethod('index');
```

   En este caso, si accedes a la raíz del sitio (por ejemplo, `http://example.com/`), se ejecutará el método `index` del controlador `Home`.

---

- Rutas Personalizadas: Puedes definir rutas personalizadas para controlar cómo se interpretan las URLs. Por ejemplo:
  
  ```php
  $routes->get('productos', 'Productos::index');
  $routes->get('productos/detalle/(:num)', 'Productos::detalle/$1');
  ```

  `productos` dirige la URL a `Productos::index` (método `index` del controlador `Productos`).
  
  `productos/detalle/(:num)` acepta un número como parámetro y lo pasa al método `detalle` del controlador `Productos`.
  
  Los placeholders comunes son:
  
  - `(:num)` -> Acepta solo números.
  - `(:alpha)` -> Acepta solo caracteres alfabéticos.
  - `(:any)` -> Acepta cualquier dato.

---

- Rutas RESTful: Si usas controladores RESTful, puedes configurar rutas para métodos HTTP como `GET`, `POST`, `PUT`, & `DELETE`. Ejemplo:
  
  ```php
  $routes->resource('api/productos');
  ```
  
  Esto genera automáticamente rutas para métodos RESTful.

---

- Rutas Closures: Puedes definir rutas con funciones anónimas en lugar de controladores. Ejemplo:
  
  ```php
  $routes->get('saludo', function () {
     return "¡Hola desde una ruta personalizada!";
  });  
  ```  
---

- Rutas de redirección: Útiles para redirigir una URL a otra:
  
  ```php
  $routes->addRedirect('antigua-ruta', 'nueva-ruta');
  ```

---

- Rutas con prefijos (Agrupadas) : Puedes agrupar relacionadas para mantener el código limpio:
  
  ```php
  $routes->group('admin', function ($routes) { 
	$routes->get('usuarios', 'Admin/Usuarios::index'); 
	$routes->get('configuracion', 'Admin/Configuracion::index'); 
  });
  ```
  
  Aquí, las rutas estarán bajo el prefijo `admin`, por ejemplo, `admin/usuarios` y `admin/configuración`. 
---

El orden en que se definen las rutas es importante por que CodeIgniter las evalúa de arriba hacia abajo. Si una ruta coincide con un patrón, las demás rutas posteriores no se evaluarán.

También podemos aplicar filtros a rutas especificas para manejar autenticación o middleware. Ejemplo:

```php
$routes->get('dashboard', 'Dashboard::index', ['filter' => 'auth']);
```

Puedes definir diferentes rutas dependiendo del entorno (producción, desarrollo, etc.) :

```php
if (ENVIRONMENT === 'development') {
    $routes->get('debug', 'Debug::index');
}
```
