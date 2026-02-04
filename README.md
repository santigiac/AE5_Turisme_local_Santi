# PROGRAMACIÓN ORIENTADA A OBJETOS - PHP
## Documento de Estudio Completo

---

## 📚 TEORÍA

### 1. Conceptos Fundamentales

#### **Clase**
Modelo o plantilla que define las propiedades (atributos) y métodos de un objeto como un tipo de dato.

```php
class Vehiculo {
    public $marca;
    public $modelo;
    
    public function arrancar() {
        return "El vehículo está arrancando";
    }
}
```

#### **Objeto**
Instancia de una clase. Es la materialización concreta de la plantilla definida por la clase.

```php
$miCoche = new Vehiculo();
$miCoche->marca = "Toyota";
$miCoche->modelo = "Corolla";
```

---

### 2. Tipos de Clases

#### **Clase Concreta**
Clase que se puede instanciar directamente.

```php
class Coche {
    public $color;
    
    public function acelerar() {
        return "Acelerando...";
    }
}

$miCoche = new Coche(); // ✅ Se puede instanciar
```

#### **Clase Abstracta**
- **Definición**: Clase que NO se puede instanciar directamente
- **Propósito**: Sirve como base para otras clases
- **Características**:
  - Permite definir métodos abstractos (sin implementación) que las subclases DEBEN implementar
  - Puede contener métodos concretos (con implementación) que se comparten con las subclases
  - Puede tener propiedades y constantes

```php
abstract class Animal {
    protected $nombre;
    
    // Método abstracto (sin implementación)
    abstract public function hacerSonido();
    
    // Método concreto (con implementación)
    public function dormir() {
        return "Zzz...";
    }
}

class Perro extends Animal {
    // OBLIGATORIO: implementar el método abstracto
    public function hacerSonido() {
        return "Guau!";
    }
}

// $animal = new Animal(); // ❌ ERROR: no se puede instanciar
$perro = new Perro(); // ✅ Correcto
```

---

### 3. Interfaces

#### **Definición**
Contrato que define qué métodos debe implementar una clase, pero no cómo implementarlos.

#### **Características de las Interfaces**
- Solo pueden tener métodos públicos (sin implementación)
- **NO** pueden tener atributos (solo constantes públicas)
- **NO** pueden tener métodos privados o protegidos
- **NO** pueden tener métodos estáticos
- Una clase puede implementar múltiples interfaces

```php
interface Volador {
    public function volar();
    public function aterrizar();
}

interface Nadador {
    public function nadar();
}

class Pato implements Volador, Nadador {
    public function volar() {
        return "El pato está volando";
    }
    
    public function aterrizar() {
        return "El pato aterriza";
    }
    
    public function nadar() {
        return "El pato está nadando";
    }
}
```

---

### 4. Diferencias Clave

#### **Clase vs Interfaz**

| Característica | Clase | Interfaz |
|----------------|-------|----------|
| **Atributos** | ✅ Puede tener | ❌ No puede tener |
| **Métodos abstractos** | ✅ Sí | ✅ Solo abstractos |
| **Métodos concretos** | ✅ Sí | ❌ No |
| **Constantes** | ✅ Sí | ✅ Solo públicas |
| **Métodos estáticos** | ✅ Sí | ❌ No |
| **Métodos privados** | ✅ Sí | ❌ No |
| **Métodos protegidos** | ✅ Sí | ❌ No |
| **Métodos públicos** | ✅ Sí | ✅ Solo públicos |
| **Herencia múltiple** | ❌ No (solo extends una) | ✅ Sí (implements múltiples) |

#### **Clase Normal vs Clase Abstracta**

| Característica | Clase Normal | Clase Abstracta |
|----------------|--------------|-----------------|
| **Se puede instanciar** | ✅ Sí | ❌ No |
| **Métodos abstractos** | ❌ No puede tener | ✅ Puede tener |
| **Métodos concretos** | ✅ Sí | ✅ Sí |
| **Atributos** | ✅ Sí | ✅ Sí |
| **Constantes** | ✅ Sí | ✅ Sí |
| **Métodos estáticos** | ✅ Sí | ✅ Sí |
| **Todos los modificadores** | ✅ Sí | ✅ Sí |

---

### 5. Herencia

Mecanismo que permite que una clase (hija) herede propiedades y métodos de otra clase (padre).

```php
class Vehiculo {
    protected $velocidad = 0;
    
    public function acelerar() {
        $this->velocidad += 10;
    }
}

class Moto extends Vehiculo {
    public function hacerCaballito() {
        return "¡Caballito!";
    }
}

$moto = new Moto();
$moto->acelerar(); // Heredado de Vehiculo
```

---

### 6. Encapsulamiento

Restricción del acceso directo a los componentes de un objeto.

#### **Modificadores de acceso:**

```php
class Persona {
    public $nombre;        // Accesible desde cualquier lugar
    protected $edad;       // Accesible solo en la clase y subclases
    private $contrasena;   // Accesible solo dentro de esta clase
    
    public function getEdad() {
        return $this->edad;
    }
    
    private function validarContrasena() {
        // Solo accesible dentro de Persona
    }
}
```

---

### 7. Métodos Estáticos

#### **Definición**
Método que pertenece a la clase, no a un objeto específico.

#### **Características**:
- Accesible sin necesidad de instanciar la clase
- Se puede llamar desde la clase o desde un objeto
- No puede acceder a propiedades de instancia (`$this`)

```php
class Calculadora {
    public static function sumar($a, $b) {
        return $a + $b;
    }
}

// Llamar sin instanciar
echo Calculadora::sumar(5, 3); // 8

// También se puede llamar desde un objeto (no recomendado)
$calc = new Calculadora();
echo $calc::sumar(2, 2); // 4
```

---

### 8. Constantes

Valores inmutables definidos en una clase.

```php
class Configuracion {
    const VERSION = "1.0.0";
    const MAX_USUARIOS = 100;
    
    public function getVersion() {
        return self::VERSION;
    }
}

echo Configuracion::VERSION; // "1.0.0"
```

---

### 9. Operador de Resolución de Ámbito (::)

Permite acceder a elementos estáticos, constantes y métodos sobrescritos.

```php
class Padre {
    public static $saludo = "Hola";
    const PI = 3.14;
}

class Hijo extends Padre {
    public static $saludo = "Buenos días";
    
    public function saludarComoPadre() {
        return parent::$saludo; // Accede a la propiedad del padre
    }
}

echo Hijo::$saludo;  // "Buenos días"
echo Hijo::PI;       // 3.14
```

---

### 10. Autoloading

Mecanismo para cargar automáticamente las clases cuando se necesitan.

```php
// Autoloader simple
spl_autoload_register(function($clase) {
    include "clases/{$clase}.php";
});

// Ahora puedes usar clases sin incluirlas manualmente
$usuario = new Usuario(); // PHP buscará y cargará Usuario.php automáticamente
```

---

## 💻 ACTIVIDADES RESUELTAS

### Actividad 1: Crear una Jerarquía de Clases con Herencia

**Enunciado**: Crea una clase abstracta `Figura` con un método abstracto `calcularArea()`. Luego crea las clases `Rectangulo` y `Circulo` que hereden de `Figura`.

**Solución**:

```php
<?php
// Clase abstracta base
abstract class Figura {
    protected $color;
    
    public function __construct($color) {
        $this->color = $color;
    }
    
    // Método abstracto que las subclases DEBEN implementar
    abstract public function calcularArea();
    
    // Método concreto compartido
    public function getColor() {
        return $this->color;
    }
}

// Clase concreta Rectangulo
class Rectangulo extends Figura {
    private $base;
    private $altura;
    
    public function __construct($color, $base, $altura) {
        parent::__construct($color);
        $this->base = $base;
        $this->altura = $altura;
    }
    
    public function calcularArea() {
        return $this->base * $this->altura;
    }
}

// Clase concreta Circulo
class Circulo extends Figura {
    private $radio;
    
    public function __construct($color, $radio) {
        parent::__construct($color);
        $this->radio = $radio;
    }
    
    public function calcularArea() {
        return pi() * pow($this->radio, 2);
    }
}

// Uso
$rectangulo = new Rectangulo("rojo", 5, 10);
echo "Área del rectángulo: " . $rectangulo->calcularArea() . "\n"; // 50
echo "Color: " . $rectangulo->getColor() . "\n"; // rojo

$circulo = new Circulo("azul", 3);
echo "Área del círculo: " . round($circulo->calcularArea(), 2) . "\n"; // 28.27
?>
```

---

### Actividad 2: Implementar Interfaces

**Enunciado**: Crea una interfaz `Reproducible` con los métodos `play()`, `pause()` y `stop()`. Implementa esta interfaz en las clases `Video` y `Audio`.

**Solución**:

```php
<?php
interface Reproducible {
    public function play();
    public function pause();
    public function stop();
}

class Video implements Reproducible {
    private $titulo;
    private $duracion;
    
    public function __construct($titulo, $duracion) {
        $this->titulo = $titulo;
        $this->duracion = $duracion;
    }
    
    public function play() {
        return "Reproduciendo video: {$this->titulo}";
    }
    
    public function pause() {
        return "Video pausado";
    }
    
    public function stop() {
        return "Video detenido";
    }
}

class Audio implements Reproducible {
    private $cancion;
    private $artista;
    
    public function __construct($cancion, $artista) {
        $this->cancion = $cancion;
        $this->artista = $artista;
    }
    
    public function play() {
        return "Reproduciendo: {$this->cancion} - {$this->artista}";
    }
    
    public function pause() {
        return "Música en pausa";
    }
    
    public function stop() {
        return "Música detenida";
    }
}

// Uso
$video = new Video("Tutorial PHP", "15:30");
echo $video->play() . "\n";
echo $video->pause() . "\n";

$audio = new Audio("Bohemian Rhapsody", "Queen");
echo $audio->play() . "\n";
?>
```

---

### Actividad 3: Métodos Estáticos y Constantes

**Enunciado**: Crea una clase `Matematicas` con métodos estáticos para operaciones básicas y constantes matemáticas.

**Solución**:

```php
<?php
class Matematicas {
    const PI = 3.14159;
    const E = 2.71828;
    
    public static function sumar($a, $b) {
        return $a + $b;
    }
    
    public static function restar($a, $b) {
        return $a - $b;
    }
    
    public static function multiplicar($a, $b) {
        return $a * $b;
    }
    
    public static function dividir($a, $b) {
        if ($b == 0) {
            return "Error: División por cero";
        }
        return $a / $b;
    }
    
    public static function areaCirculo($radio) {
        return self::PI * pow($radio, 2);
    }
}

// Uso sin instanciar la clase
echo Matematicas::sumar(10, 5) . "\n";        // 15
echo Matematicas::multiplicar(4, 7) . "\n";   // 28
echo Matematicas::PI . "\n";                  // 3.14159
echo Matematicas::areaCirculo(5) . "\n";      // 78.53975
?>
```

---

### Actividad 4: Encapsulamiento con Getters y Setters

**Enunciado**: Crea una clase `CuentaBancaria` con atributos privados y métodos públicos para acceder y modificarlos de forma controlada.

**Solución**:

```php
<?php
class CuentaBancaria {
    private $titular;
    private $saldo;
    private $numeroCuenta;
    
    public function __construct($titular, $numeroCuenta, $saldoInicial = 0) {
        $this->titular = $titular;
        $this->numeroCuenta = $numeroCuenta;
        $this->saldo = $saldoInicial;
    }
    
    // Getters
    public function getTitular() {
        return $this->titular;
    }
    
    public function getSaldo() {
        return $this->saldo;
    }
    
    public function getNumeroCuenta() {
        return $this->numeroCuenta;
    }
    
    // Métodos con lógica de negocio
    public function depositar($cantidad) {
        if ($cantidad > 0) {
            $this->saldo += $cantidad;
            return "Depósito exitoso. Nuevo saldo: {$this->saldo}";
        }
        return "Error: La cantidad debe ser positiva";
    }
    
    public function retirar($cantidad) {
        if ($cantidad > 0) {
            if ($this->saldo >= $cantidad) {
                $this->saldo -= $cantidad;
                return "Retiro exitoso. Nuevo saldo: {$this->saldo}";
            }
            return "Error: Saldo insuficiente";
        }
        return "Error: La cantidad debe ser positiva";
    }
}

// Uso
$cuenta = new CuentaBancaria("Juan Pérez", "ES1234567890", 1000);
echo $cuenta->depositar(500) . "\n";   // Depósito exitoso. Nuevo saldo: 1500
echo $cuenta->retirar(200) . "\n";     // Retiro exitoso. Nuevo saldo: 1300
echo $cuenta->getSaldo() . "\n";       // 1300
?>
```

---

### Actividad 5: Clase Abstracta con Herencia Múltiple de Interfaces

**Enunciado**: Crea una clase abstracta `Empleado` y dos interfaces `Programable` y `Reportable`. Implementa una clase `Desarrollador` que herede de `Empleado` e implemente ambas interfaces.

**Solución**:

```php
<?php
// Clase abstracta
abstract class Empleado {
    protected $nombre;
    protected $salario;
    
    public function __construct($nombre, $salario) {
        $this->nombre = $nombre;
        $this->salario = $salario;
    }
    
    abstract public function calcularBono();
    
    public function getNombre() {
        return $this->nombre;
    }
    
    public function getSalario() {
        return $this->salario;
    }
}

// Interfaces
interface Programable {
    public function programar($proyecto);
}

interface Reportable {
    public function generarReporte();
}

// Clase concreta que hereda e implementa múltiples interfaces
class Desarrollador extends Empleado implements Programable, Reportable {
    private $lenguajes = [];
    private $proyectos = [];
    
    public function agregarLenguaje($lenguaje) {
        $this->lenguajes[] = $lenguaje;
    }
    
    public function programar($proyecto) {
        $this->proyectos[] = $proyecto;
        return "{$this->nombre} está programando: {$proyecto}";
    }
    
    public function generarReporte() {
        $reporte = "=== REPORTE DE {$this->nombre} ===\n";
        $reporte .= "Salario: {$this->salario}€\n";
        $reporte .= "Bono: {$this->calcularBono()}€\n";
        $reporte .= "Lenguajes: " . implode(", ", $this->lenguajes) . "\n";
        $reporte .= "Proyectos: " . count($this->proyectos) . "\n";
        return $reporte;
    }
    
    public function calcularBono() {
        // Bono del 10% por cada proyecto
        return $this->salario * 0.10 * count($this->proyectos);
    }
}

// Uso
$dev = new Desarrollador("Ana García", 3000);
$dev->agregarLenguaje("PHP");
$dev->agregarLenguaje("JavaScript");
$dev->agregarLenguaje("Python");

echo $dev->programar("Sistema de Gestión") . "\n";
echo $dev->programar("API REST") . "\n";

echo "\n" . $dev->generarReporte();
?>
```

---

## ❓ PREGUNTAS DE EXAMEN

### Bloque 1: Conceptos Teóricos (Respuesta Corta)

1. **¿Qué es una clase abstracta y en qué se diferencia de una clase normal?**
   
   <details>
   <summary>Ver respuesta</summary>
   
   Una clase abstracta es una clase que no se puede instanciar directamente y sirve como base para otras clases. A diferencia de una clase normal, puede contener métodos abstractos (sin implementación) que las subclases están obligadas a implementar. Una clase normal sí se puede instanciar y no puede tener métodos abstractos.
   </details>

2. **¿Cuál es la principal diferencia entre una interfaz y una clase abstracta?**
   
   <details>
   <summary>Ver respuesta</summary>
   
   - **Interfaz**: Solo métodos públicos abstractos, no puede tener atributos ni implementación de métodos.
   - **Clase abstracta**: Puede tener métodos abstractos Y concretos, atributos, constantes, y cualquier modificador de acceso.
   - **Herencia múltiple**: Una clase puede implementar múltiples interfaces pero solo puede heredar de una clase abstracta.
   </details>

3. **¿Qué es el encapsulamiento y cuáles son los tres modificadores de acceso en PHP?**
   
   <details>
   <summary>Ver respuesta</summary>
   
   El encapsulamiento es el principio de ocultar los detalles internos de un objeto y exponer solo lo necesario. Los modificadores son:
   - **public**: Accesible desde cualquier lugar
   - **protected**: Accesible solo en la clase y sus subclases
   - **private**: Accesible solo dentro de la propia clase
   </details>

4. **¿Qué es un método estático y cómo se diferencia de un método de instancia?**
   
   <details>
   <summary>Ver respuesta</summary>
   
   Un método estático pertenece a la clase, no a objetos específicos. Se puede llamar sin instanciar la clase usando `::`. No puede acceder a propiedades de instancia (`$this`). Un método de instancia requiere crear un objeto y puede acceder a `$this`.
   </details>

5. **¿Qué hace el operador `::` (operador de resolución de ámbito)?**
   
   <details>
   <summary>Ver respuesta</summary>
   
   Permite acceder a elementos estáticos (propiedades y métodos), constantes de clase, y referencias a la clase padre (`parent::`), a la propia clase (`self::`) o a la clase que se llama en tiempo de ejecución (`static::`).
   </details>

---

### Bloque 2: Verdadero o Falso

1. **Una interfaz puede tener métodos privados.** ❌
   <details><summary>Explicación</summary>Las interfaces solo pueden tener métodos públicos.</details>

2. **Una clase abstracta puede tener métodos concretos (con implementación).** ✅
   <details><summary>Explicación</summary>Sí, puede tener tanto métodos abstractos como concretos.</details>

3. **Se puede instanciar directamente una clase abstracta.** ❌
   <details><summary>Explicación</summary>No, las clases abstractas no se pueden instanciar.</details>

4. **Una clase puede implementar múltiples interfaces.** ✅
   <details><summary>Explicación</summary>Sí, usando `implements Interface1, Interface2`.</details>

5. **Los métodos estáticos pueden acceder a `$this`.** ❌
   <details><summary>Explicación</summary>No, los métodos estáticos no tienen acceso a `$this`.</details>

6. **Una interfaz puede tener constantes.** ✅
   <details><summary>Explicación</summary>Sí, pero solo constantes públicas.</details>

7. **Una clase puede heredar de múltiples clases abstractas.** ❌
   <details><summary>Explicación</summary>PHP no permite herencia múltiple de clases.</details>

8. **Los atributos `protected` son accesibles desde clases hijas.** ✅
   <details><summary>Explicación</summary>Sí, protected permite acceso en subclases.</details>

---

### Bloque 3: Código (Identifica Errores)

**1. Encuentra el error:**

```php
abstract class Animal {
    abstract public function comer();
}

$animal = new Animal();
```

<details>
<summary>Ver respuesta</summary>

**Error**: No se puede instanciar una clase abstracta.

**Corrección**: Crear una clase concreta que herede de Animal:
```php
class Perro extends Animal {
    public function comer() {
        return "El perro come";
    }
}
$animal = new Perro();
```
</details>

---

**2. Encuentra el error:**

```php
interface Volador {
    private function volar();
}
```

<details>
<summary>Ver respuesta</summary>

**Error**: Las interfaces no pueden tener métodos privados, solo públicos.

**Corrección**:
```php
interface Volador {
    public function volar();
}
```
</details>

---

**3. Encuentra el error:**

```php
class Calculadora {
    public static function sumar($a, $b) {
        return $this->resultado = $a + $b;
    }
}
```

<details>
<summary>Ver respuesta</summary>

**Error**: Los métodos estáticos no pueden usar `$this`.

**Corrección**:
```php
class Calculadora {
    public static function sumar($a, $b) {
        return $a + $b;
    }
}
```
</details>

---

**4. ¿Este código es correcto?**

```php
abstract class Forma {
    public function dibujar() {
        return "Dibujando forma";
    }
}

class Cuadrado extends Forma {
}

$cuadrado = new Cuadrado();
echo $cuadrado->dibujar();
```

<details>
<summary>Ver respuesta</summary>

**✅ Correcto**. Una clase abstracta puede tener métodos concretos (no abstractos). La clase hija no está obligada a implementar métodos concretos, puede heredarlos directamente.
</details>

---

### Bloque 4: Preguntas de Desarrollo

**1. Explica con un ejemplo cuándo usar una clase abstracta y cuándo usar una interfaz.**

<details>
<summary>Ver respuesta</summary>

**Usar Clase Abstracta cuando:**
- Hay código común que quieres compartir entre clases relacionadas
- Las clases comparten atributos
- Ejemplo: Sistema de gestión de empleados

```php
abstract class Empleado {
    protected $nombre;
    protected $salario;
    
    public function __construct($nombre, $salario) {
        $this->nombre = $nombre;
        $this->salario = $salario;
    }
    
    // Método concreto compartido
    public function getNombre() {
        return $this->nombre;
    }
    
    // Método abstracto que cada tipo implementará diferente
    abstract public function calcularBono();
}
```

**Usar Interfaz cuando:**
- Solo quieres definir un contrato de comportamiento
- Las clases no están relacionadas jerárquicamente
- Necesitas herencia múltiple
- Ejemplo: Diferentes tipos de medios que se pueden reproducir

```php
interface Reproducible {
    public function play();
    public function pause();
    public function stop();
}

class Video implements Reproducible {
    public function play() { /* ... */ }
    public function pause() { /* ... */ }
    public function stop() { /* ... */ }
}

class Podcast implements Reproducible {
    public function play() { /* ... */ }
    public function pause() { /* ... */ }
    public function stop() { /* ... */ }
}
```
</details>

---

**2. Explica qué es el autoloading y proporciona un ejemplo.**

<details>
<summary>Ver respuesta</summary>

El autoloading es un mecanismo que permite cargar automáticamente las clases cuando se utilizan, sin necesidad de usar `require` o `include` manualmente.

**Ejemplo básico:**

```php
// autoload.php
spl_autoload_register(function($nombreClase) {
    $archivo = __DIR__ . "/clases/{$nombreClase}.php";
    if (file_exists($archivo)) {
        require_once $archivo;
    }
});
```

**Uso:**

```php
require_once 'autoload.php';

// PHP automáticamente cargará clases/Usuario.php
$usuario = new Usuario();

// PHP automáticamente cargará clases/Producto.php
$producto = new Producto();
```

**Ventajas:**
- Código más limpio (sin múltiples requires)
- Carga bajo demanda (solo se cargan las clases que se usan)
- Facilita la organización del código
</details>

---

**3. Crea una jerarquía de clases para un sistema de vehículos que incluya: clase abstracta, interfaz, herencia y encapsulamiento.**

<details>
<summary>Ver respuesta</summary>

```php
<?php
// Interfaz para vehículos eléctricos
interface Electrico {
    public function cargarBateria($porcentaje);
    public function obtenerAutonomia();
}

// Clase abstracta base
abstract class Vehiculo {
    protected $marca;
    protected $modelo;
    private $precio;
    
    public function __construct($marca, $modelo, $precio) {
        $this->marca = $marca;
        $this->modelo = $modelo;
        $this->precio = $precio;
    }
    
    // Método abstracto
    abstract public function tipoMotor();
    
    // Métodos concretos
    public function getPrecio() {
        return $this->precio;
    }
    
    public function getInfo() {
        return "{$this->marca} {$this->modelo}";
    }
}

// Clase concreta
class Coche extends Vehiculo {
    private $numeroPuertas;
    
    public function __construct($marca, $modelo, $precio, $puertas) {
        parent::__construct($marca, $modelo, $precio);
        $this->numeroPuertas = $puertas;
    }
    
    public function tipoMotor() {
        return "Motor de combustión";
    }
}

// Clase que hereda e implementa interfaz
class CocheElectrico extends Vehiculo implements Electrico {
    private $nivelBateria = 0;
    private $capacidadBateria;
    
    public function __construct($marca, $modelo, $precio, $capacidad) {
        parent::__construct($marca, $modelo, $precio);
        $this->capacidadBateria = $capacidad;
    }
    
    public function tipoMotor() {
        return "Motor eléctrico";
    }
    
    public function cargarBateria($porcentaje) {
        $this->nivelBateria = min(100, $this->nivelBateria + $porcentaje);
        return "Batería cargada al {$this->nivelBateria}%";
    }
    
    public function obtenerAutonomia() {
        return ($this->capacidadBateria * $this->nivelBateria / 100) . " km";
    }
}

// Uso
$tesla = new CocheElectrico("Tesla", "Model 3", 45000, 500);
echo $tesla->tipoMotor() . "\n";
echo $tesla->cargarBateria(80) . "\n";
echo "Autonomía: " . $tesla->obtenerAutonomia() . "\n";
?>
```
</details>

---

### Bloque 5: Casos Prácticos

**1. Sistema de Figuras Geométricas**

Implementa un sistema que:
- Tenga una clase abstracta `FiguraGeometrica`
- Una interfaz `Dibujable`
- Clases concretas: `Triangulo`, `Cuadrado`, `Circulo`
- Cada figura debe poder calcular su área y dibujarse

<details>
<summary>Ver solución completa</summary>

```php
<?php
interface Dibujable {
    public function dibujar();
}

abstract class FiguraGeometrica {
    protected $color;
    
    public function __construct($color) {
        $this->color = $color;
    }
    
    abstract public function calcularArea();
    abstract public function calcularPerimetro();
    
    public function getColor() {
        return $this->color;
    }
}

class Triangulo extends FiguraGeometrica implements Dibujable {
    private $base;
    private $altura;
    private $lado1, $lado2, $lado3;
    
    public function __construct($color, $base, $altura, $lado1, $lado2, $lado3) {
        parent::__construct($color);
        $this->base = $base;
        $this->altura = $altura;
        $this->lado1 = $lado1;
        $this->lado2 = $lado2;
        $this->lado3 = $lado3;
    }
    
    public function calcularArea() {
        return ($this->base * $this->altura) / 2;
    }
    
    public function calcularPerimetro() {
        return $this->lado1 + $this->lado2 + $this->lado3;
    }
    
    public function dibujar() {
        return "   /\\\n  /  \\\n /____\\";
    }
}

class Cuadrado extends FiguraGeometrica implements Dibujable {
    private $lado;
    
    public function __construct($color, $lado) {
        parent::__construct($color);
        $this->lado = $lado;
    }
    
    public function calcularArea() {
        return pow($this->lado, 2);
    }
    
    public function calcularPerimetro() {
        return $this->lado * 4;
    }
    
    public function dibujar() {
        return " ____\n|    |\n|    |\n ----";
    }
}

class Circulo extends FiguraGeometrica implements Dibujable {
    private $radio;
    const PI = 3.14159;
    
    public function __construct($color, $radio) {
        parent::__construct($color);
        $this->radio = $radio;
    }
    
    public function calcularArea() {
        return self::PI * pow($this->radio, 2);
    }
    
    public function calcularPerimetro() {
        return 2 * self::PI * $this->radio;
    }
    
    public function dibujar() {
        return "  ***\n *   *\n  ***";
    }
}

// Uso
$figuras = [
    new Triangulo("rojo", 10, 8, 10, 10, 10),
    new Cuadrado("azul", 5),
    new Circulo("verde", 7)
];

foreach ($figuras as $figura) {
    echo "Figura de color: " . $figura->getColor() . "\n";
    echo "Área: " . round($figura->calcularArea(), 2) . "\n";
    echo "Perímetro: " . round($figura->calcularPerimetro(), 2) . "\n";
    echo $figura->dibujar() . "\n\n";
}
?>
```
</details>

---

**2. Sistema de Pagos**

Crea un sistema que permita diferentes métodos de pago (tarjeta, PayPal, transferencia) usando interfaces y polimorfismo.

<details>
<summary>Ver solución completa</summary>

```php
<?php
interface MetodoPago {
    public function procesarPago($cantidad);
    public function validarPago();
}

interface Reembolsable {
    public function reembolsar($cantidad);
}

class PagoTarjeta implements MetodoPago, Reembolsable {
    private $numeroTarjeta;
    private $cvv;
    
    public function __construct($numero, $cvv) {
        $this->numeroTarjeta = $numero;
        $this->cvv = $cvv;
    }
    
    public function validarPago() {
        // Simulación de validación
        if (strlen($this->numeroTarjeta) == 16 && strlen($this->cvv) == 3) {
            return true;
        }
        return false;
    }
    
    public function procesarPago($cantidad) {
        if ($this->validarPago()) {
            return "Pago de {$cantidad}€ procesado con tarjeta ****" . 
                   substr($this->numeroTarjeta, -4);
        }
        return "Error en el pago con tarjeta";
    }
    
    public function reembolsar($cantidad) {
        return "Reembolso de {$cantidad}€ procesado a la tarjeta";
    }
}

class PagoPayPal implements MetodoPago, Reembolsable {
    private $email;
    
    public function __construct($email) {
        $this->email = $email;
    }
    
    public function validarPago() {
        return filter_var($this->email, FILTER_VALIDATE_EMAIL) !== false;
    }
    
    public function procesarPago($cantidad) {
        if ($this->validarPago()) {
            return "Pago de {$cantidad}€ procesado con PayPal ({$this->email})";
        }
        return "Error: Email de PayPal inválido";
    }
    
    public function reembolsar($cantidad) {
        return "Reembolso de {$cantidad}€ enviado a {$this->email}";
    }
}

class PagoTransferencia implements MetodoPago {
    private $iban;
    
    public function __construct($iban) {
        $this->iban = $iban;
    }
    
    public function validarPago() {
        return strlen($this->iban) == 24;
    }
    
    public function procesarPago($cantidad) {
        if ($this->validarPago()) {
            return "Transferencia de {$cantidad}€ iniciada a {$this->iban}";
        }
        return "IBAN inválido";
    }
}

// Procesador de pagos
class ProcesadorPagos {
    public static function procesar(MetodoPago $metodo, $cantidad) {
        return $metodo->procesarPago($cantidad);
    }
}

// Uso
$tarjeta = new PagoTarjeta("1234567890123456", "123");
$paypal = new PagoPayPal("usuario@example.com");
$transferencia = new PagoTransferencia("ES1234567890123456789012");

echo ProcesadorPagos::procesar($tarjeta, 150) . "\n";
echo ProcesadorPagos::procesar($paypal, 75.50) . "\n";
echo ProcesadorPagos::procesar($transferencia, 500) . "\n";

// Reembolso
echo $tarjeta->reembolsar(50) . "\n";
echo $paypal->reembolsar(25.50) . "\n";
?>
```
</details>

---

## 📝 CONSEJOS PARA EL EXAMEN

1. **Diferencia clave**: Recuerda que las interfaces NO pueden tener atributos ni métodos implementados
2. **Abstractas**: Pueden tener tanto métodos abstractos como concretos
3. **Estáticos**: No usan `$this`, se acceden con `::`
4. **Encapsulamiento**: `private` solo en la clase, `protected` en clase y subclases, `public` en cualquier lugar
5. **Herencia**: Una clase solo puede heredar de UNA clase, pero implementar MÚLTIPLES interfaces
6. **Autoloading**: Evita tener que usar `require`/`include` manualmente

---

## ✅ CHECKLIST DE REPASO

- [ ] Entiendo qué es una clase y un objeto
- [ ] Sé la diferencia entre clase abstracta y concreta
- [ ] Conozco las diferencias entre clase abstracta e interfaz
- [ ] Entiendo los modificadores de acceso (public, protected, private)
- [ ] Sé qué son y cómo usar métodos estáticos
- [ ] Entiendo el concepto de herencia
- [ ] Sé implementar interfaces
- [ ] Conozco el operador `::`
- [ ] Entiendo el autoloading
- [ ] Puedo crear una jerarquía de clases completa

---

**¡Buena suerte en tu examen! 🚀**
