Las migraciones en **CodeIgniter 4** son una forma estructurada de gestionar los cambios en la base de datos de tu aplicación. Te permiten realizar cambios como la creación, modificación o eliminación de tablas y campos de forma controlada, asegurándote de que el esquema de la base de datos esté sincronizado entre los entornos de desarrollo, prueba y producción.

- **Archivo de migración**: Es un archivo PHP que contiene las instrucciones para aplicar (método `up()`) y revertir (método `down()`) los cambios en la base de datos.
- **Versiones**: Cada migración tiene un número de versión basado en una marca de tiempo que ayuda a identificar el orden de ejecución.
- **Comandos CLI**: CodeIgniter 4 proporciona comando útiles para manejar migraciones desde la línea de comandos.

Antes de usar migraciones, asegúrate de configurar el archivo `Config/Migrations.php`, allí puedes especificar:

- `enabled`: Activa o desactiva las migraciones (`true` o `false`).
- `type`: Define si las migraciones se cargan desde archivos o una base de datos. Por defecto, se usa `timestamp` para versiones basadas en marca de tiempo.
- `table`: Especifica el nombre de la tabla que almacena el estado de las migraciones (por defecto: `migrations`).

---

A continuación vamos a proceder a crear una migración

1. Usa el comando CLI:
   
   ```php
   php spark make:migration CreateTableUsers
   ```

2. Esto creará un archivo en la carpeta `app/Database/Migrations` con un nombre como:
   
   ```php
   20250118123456_CreateTableUsers.php
   ```

3. Abre el archivo generado y define las instrucciones en el método `up()` y `down()`
   
   ```php
   <?php

namespace App\Database\Migrations;

use CodeIgniter\Database\Migration;

class CreateTableUsers extends Migration
{
    public function up()
    {
        $this->forge->addField([
            'id' => [
                'type'           => 'INT',
                'unsigned'       => true,
                'auto_increment' => true,
            ],
            'username' => [
                'type'       => 'VARCHAR',
                'constraint' => '100',
            ],
            'password' => [
                'type'       => 'VARCHAR',
                'constraint' => '255',
            ],
            'created_at' => [
                'type' => 'DATETIME',
            ],
        ]);
        $this->forge->addKey('id', true);
        $this->forge->createTable('users');
    }

    public function down()
    {
        $this->forge->dropTable('users');
    }
}
   ```

---

Ahora procedemos a ejecutar las migraciones:

1. Aplicar todas las migraciones pendientes
   
   ```php
   php spark migrate
   ```

2. Revertir la última migración ejecutada
   
   ```php
   php spark migrate:rollback
   ```

3. Revertir todas las migraciones
   
   ```php
   php spark migrate:reset
   ```

4. Forzar una migración a un estado específico (por ejemplo, una versión)
   
   ```php
   php spark migrate --to 20250118123456
   ```

---

Las migraciones son como una forma programada y estructurada de escribir y ejecutar las consultas que modifican el esquema de tu base de datos. En lugar de escribir consultas **SQL** directamente en tu terminal o cliente de base de datos, las escribes en código dentro de las migraciones, lo que tiene varias ventajas.

Migración en CodeIgniter 4:

```php
$this->forge->addField([
    'id' => [
        'type'           => 'INT',
        'unsigned'       => true,
        'auto_increment' => true,
    ],
    'name' => [
        'type'       => 'VARCHAR',
        'constraint' => '100',
    ],
    'created_at' => [
        'type' => 'DATETIME',
    ],
]);
$this->forge->addKey('id', true);
$this->forge->createTable('users');
```

Consulta SQL Directa:

```sql
CREATE TABLE users (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    created_at DATETIME
);
```

Algunas de las razones por las que es buena opción usar Migraciones en CodeIgniter son las siguientes:

- Control de versiones: Cada migración tiene un identificador único (**timestamp**), lo que asegura que las modificaciones se apliquen en orden.
- Reversibilidad: Con el método `down()`, puedes deshacer los cambios automáticamente.
- Colaboración: Si trabajas con un equipo, todos pueden ejecutar las mismas migraciones para tener el mismo esquema de base de datos.
- Automatización: Las migraciones pueden ejecutarse como parte del proceso de despliegue o pruebas automáticas.
