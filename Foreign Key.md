Una Foreign Key (clave foránea) en **CodeIgniter** (o cualquier base de datos) es una columna en una tabla que establece una relación con otra tabla. Específicamente, una clave foránea apunta a la clave primaria de otra tabla para garantizar la integridad referencial.

¿Cómo usar Foreign Key en CodeIgniter?, CodeIgniter no maneja claves foráneas directamente como una funcionalidad específica, ya que las claves foráneas son configuradas en la base de datos. Sin embargo, puedes trabajar con ellas de las siguientes maneras:

---

1. Crear Claves Foráneas en la Base de Datos
   
Cuando defines tus tablas en la base de datos, puedes especificar una clave foránea. Por ejemplo, usando MySQL:
   
   ```php
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    order_date DATETIME NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
   ```
   
En este ejemplo:

- `user_id` en la tabla `orders` es una clave foránea que referencia la columna `id` en la tabla `users`.
- La opción `ON DELETE CASCADE` asegura que cuando un usuario es eliminado, sus órdenes asociadas también lo sean.
---

2. Trabajar con Clave Foráneas en CodeIgniter

- Insertar Datos en Tablas Relacionadas
  
  Cuando trabajas con claves foráneas, asegúrate de insertar primero los datos en la tabla principal (referenciada). Luego, puedes insertar en la tabla secundaria.
  
  ```php
  // Insertar en la tabla principal (users)
$data_user = [
    'name' => 'John Doe'
];
$this->db->insert('users', $data_user);

// Obtener el ID del usuario recién insertado
$user_id = $this->db->insert_id();

// Insertar en la tabla secundaria (orders)
$data_order = [
    'user_id' => $user_id,
    'order_date' => date('Y-m-d H:i:s')
];
$this->db->insert('orders', $data_order);
  ```

- Consultar Datos con Relaciones
  
  Para obtener datos relacionados, puedes usar joins. Por ejemplo:
  
  ```php
$this->db->select('users.name, orders.order_date');
$this->db->from('orders');
$this->db->join('users', 'users.id = orders.user_id');
$query = $this->db->get();

return $query->result();
  ```
  
  Esto devolverá una lista de órdenes con el nombre del usuario asociado.

- Eliminar Datos con Relaciones
  
  Si configuraste `ON DELETE CASCADE` al crear la clave foránea, simplemente elimina el registro en la tabla principal y la base de datos se encargará de eliminar automáticamente los registro relacionados.
  
  ```php
$this->db->where('id', $user_id);
$this->db->delete('users'); // También eliminará las órdenes relacionadas.
  ```

---

3. Validación de Claves Foráneas

Antes de insertar o actualizar datos, asegúrate de que las claves foráneas sean válidas. Por ejemplo, verifica que el `user_id` exista en la tabla `users` antes de insertar un pedido en la tabla `orders`.

```php
$user = $this->db->get_where('users', ['id' => $user_id])->row();

if ($user) {
    $this->db->insert('orders', $data_order);
} else {
    echo "El usuario no existe.";
}
```

---

4. Migraciones en CodeIgniter

Si usamos migraciones, puedes definir claves foráneas de esta forma:

```php
public function up()
{
    // Crear tabla users
    $this->dbforge->add_field([
        'id' => [
            'type' => 'INT',
            'constraint' => 11,
            'auto_increment' => true
        ],
        'name' => [
            'type' => 'VARCHAR',
            'constraint' => '255',
        ],
    ]);
    $this->dbforge->add_key('id', true);
    $this->dbforge->create_table('users');

    // Crear tabla orders
    $this->dbforge->add_field([
        'id' => [
            'type' => 'INT',
            'constraint' => 11,
            'auto_increment' => true
        ],
        'user_id' => [
            'type' => 'INT',
            'constraint' => 11,
        ],
        'order_date' => [
            'type' => 'DATETIME',
        ],
    ]);
    $this->dbforge->add_key('id', true);
    $this->dbforge->add_field('CONSTRAINT FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE');
    $this->dbforge->create_table('orders');
}
```

---

Algunos consejos que son recomendables seguir para aplicar Foreign Keys en CodeIgniter son los siguientes:

- Usa claves foráneas para mantener la integridad de los datos y evitar errores de inconsistencia.
- Configura correctamente las acciones (`ON DELETE`, `ON UPDATE`) según las necesidades de tu aplicación.
- Valida los datos antes de insertar o actualizar para evitar errores.