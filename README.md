# Celda Robótica de Inspección de Calidad

Proyecto del Parcial 3 de Programación Orientada a Objetos.

La guía completa del proyecto está en [PROYECTO.md](PROYECTO.md). Léanla antes de empezar.

## 1. Equipo

| Integrante | Matrícula | Nombre | Usuario de GitHub | Clase |
| --- | --- | --- | --- | --- |
| 1 |25173 |Cesar Leonardo Garrido |clgarridog | |
| 2 |25206 Alejandro Serrato de la Rosa Serraprogramador
| 3 |
| 4 | | | | |
| 5 | | | | |
| 6 (solo equipo de seis) | | | | |

## 2. Cómo compilar y ejecutar

Escriban el comando exacto que usa el equipo para compilar y el comando para ejecutar.

g++ -Wall -Wextra -std=c++17 -Iinclude src/*.cpp -o celda
.\celda.exe

## 3. Contrato de `Dispositivo` y `Calibrable`

**¿Qué errores dio el compilador al compilar el repositorio base y qué significaba cada uno?**

A- Error en dynamic_cast: la clase Dispositivo no era polimórfica porque no tenía métodos virtuales.
- Métodos no declarados: las interfaces estaban incompletas y no tenían todos los métodos necesarios para realizar la calibración y la inspección.
- Contratos incompletos: la celda no podía utilizar correctamente las capacidades de los dispositivos hasta completar sus clases base.
Estos problemas se solucionan declarando los métodos correspondientes y utilizando funciones virtuales para implementar el polimorfismo.    

**¿Qué métodos agregaron a `Dispositivo`? ¿Cuáles son virtuales puros, cuáles son `const` y por qué el destructor es virtual?**

virtual bool inspeccionar(const Pieza& pieza) const = 0; — evalúa si la pieza cumple la regla del dispositivo (const porque inspeccionar no modifica el estado del dispositivo ni de la pieza).

virtual std::string descripcion() const = 0; — devuelve el nombre/descripción del dispositivo para la salida del turno (const porque solo consulta información).
Constructor / ID: manejo del identificador del dispositivo (por ejemplo, id() const o constructor protegido/público según el diseño acordado).

Destructor virtual: virtual ~Dispositivo() = default; — es fundamental que sea virtual para que, si en algún momento se destruye un objeto derivado a través de un puntero Dispositivo*, se invoque primero el destructor de la clase derivada y luego el de Dispositivo, evitando comportamiento indefinido o fugas de recursos.

**¿Qué agregaron a `Calibrable` y por qué no tiene atributos?**

Se agregó el método virtual puro virtual bool calibrar() = 0; (y destructor virtual virtual ~Calibrable() = default;). Calibrable funciona como una interfaz que representa únicamente una capacidad (poder calibrarse), no un estado común compartido por todos los objetos calibrables. Cada clase concreta (Bascula, CalibradorLaser, o futuras clases calibrables) decide cómo almacena y reinicia su propio desfase (desfase_ = 0.0), manteniendo la interfaz limpia y desacoplada.

## 4. Cálculo a mano del reporte

Turno de 12 piezas, con la celda ya calibrada. Escriban `ACEPTA` o `RECHAZA` por dispositivo. La columna D-01 solo la llena el equipo de seis.

Pieza	B-01	L-01	V-01	T-01	D-01	Resultado
P-01	ACEPTA	ACEPTA	ACEPTA	ACEPTA	ACEPTA	ACEPTADA
P-02	ACEPTA	ACEPTA	ACEPTA	ACEPTA	ACEPTA	ACEPTADA
P-03	RECHAZA	ACEPTA	ACEPTA	ACEPTA	ACEPTA	RECHAZADA
P-04	ACEPTA	ACEPTA	ACEPTA	ACEPTA	ACEPTA	ACEPTADA
P-05	ACEPTA	RECHAZA	ACEPTA	ACEPTA	ACEPTA	RECHAZADA
P-06	ACEPTA	ACEPTA	RECHAZA	ACEPTA	ACEPTA	RECHAZADA
P-07	ACEPTA	ACEPTA	ACEPTA	RECHAZA	ACEPTA	RECHAZADA
P-08	RECHAZA	RECHAZA	RECHAZA	ACEPTA	ACEPTA	RECHAZADA
P-09	ACEPTA	ACEPTA	ACEPTA	ACEPTA	RECHAZA	ACEPTADA (equipo de 5) / RECHAZADA (equipo de 6)
P-10	ACEPTA	ACEPTA	ACEPTA	ACEPTA	RECHAZA	ACEPTADA (equipo de 5) / RECHAZADA (equipo de 6)
P-11	ACEPTA	ACEPTA	ACEPTA	RECHAZA	ACEPTA	RECHAZADA
P-12	ACEPTA	ACEPTA	ACEPTA	ACEPTA

**Reporte esperado (inspeccionadas, aceptadas, rechazadas y rechazos por dispositivo):**

[Inserta aquí tu respuesta]

**¿Coincide con lo que imprime el programa? Si no coincidió al primer intento, ¿qué encontraron y cómo lo corrigieron?**

=== Reporte de turno ===
Inspeccionadas: 12
Aceptadas: 6
Rechazadas: 6
Rechazos por dispositivo:
B-01: 2
L-01: 2
T-01: 2
V-01: 2

## 5. Diagrama de clases

Diagrama de clases de la solución completa, hecho en draw.io. Guarden la imagen en la raíz del repositorio como `diseno_solucion.png`.

![Diagrama de clases](diseno_solucion.png)

## 6. Documentación individual

| Integrante | Documentación de su clase | Rama del reto opcional |
| --- | --- | --- |
| | `docs/<matrícula>/<Clase>.md` | `<matrícula>` o "No" |
| | | |
| | | |
| | | |
| | | |
| | | |

Los resultados de los experimentos están en [docs/experimentos.md](docs/experimentos.md).

## 7. Prueba de extensibilidad
La arquitectura del proyecto busca permitir la incorporación de nuevos dispositivos sin modificar el funcionamiento interno de CeldaInspeccion.
Para agregar un dispositivo nuevo sería necesario:

Crear un nuevo archivo de encabezado .h.

Crear su implementación correspondiente .cpp.

Heredar de la clase abstracta Dispositivo.

Implementar los métodos virtuales inspeccionar() y descripcion() mediante override.

Crear el objeto en main.cpp.

Agregarlo a la celda mediante agregarDispositivo().
Practiquen agregar un dispositivo nuevo antes de entregar.

**¿Qué archivos tuvieron que crear o modificar? ¿Tuvieron que tocar `CeldaInspeccion`?**
De acuerdo con el diseño del proyecto, únicamente sería necesario crear los archivos correspondientes al nuevo dispositivo y modificar main.cpp para instanciarlo y agregarlo a la celda.
No sería necesario modificar CeldaInspeccion, ya que esta trabaja con punteros de tipo Dispositivo* y utiliza polimorfismo para ejecutar las inspecciones.
La prueba práctica de extensibilidad queda pendiente de realizar y documentar.
## 8. Reflexión del equipo

**¿Qué aprendieron en este proyecto?**

Durante el desarrollo del proyecto aprendimos a aplicar los principales conceptos de la programación orientada a objetos mediante la simulación de un sistema de inspección industrial.
Comprendimos la importancia de utilizar clases abstractas, herencia, polimorfismo y encapsulamiento para desarrollar programas organizados y escalables.
También aprendimos a utilizar la herencia múltiple mediante la interfaz Calibrable, así como a diferenciar entre agregación y composición dentro de una arquitectura de software.
Otro aprendizaje importante fue la implementación de contenedores de la biblioteca estándar de C++, como std::vector y std::map, para administrar dispositivos y registrar resultados.
Finalmente, reforzamos nuestras habilidades de trabajo colaborativo mediante Git y GitHub, la organización de archivos .h y .cpp, y la documentación técnica del proyecto.

**¿Qué cambiarían de su proceso de trabajo como equipo?**

Como equipo, mejoraríamos la distribución de actividades desde el inicio del proyecto, estableciendo responsabilidades claras para cada integrante.
También dedicaríamos más tiempo a integrar y probar las clases antes de llegar a las últimas etapas del desarrollo, con el objetivo de detectar errores de compilación y funcionamiento oportunamente.
Además, procuraríamos realizar commits más frecuentes, mantener una comunicación constante y revisar conjuntamente las implementaciones para asegurar que todas las clases cumplan con los contratos establecidos.
