# PRACTICA-EDES-5.2

- a) Una breve lista de los conceptos que te has encontrado en los diagramas [UML](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=639928 "UML") que se asemejan a los conceptos de programación orientada a objetos. Por [ejemplo](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=655395 "Ejemplo"): Clases: GestorPedidos

Las clases UML son básicamente iguales a las clases de POO en kotlin, los atributos de las clases UML asemejan a los constructores en POO ya sean primarios o secundarios, las operaciones son básicamente métodos, como calcularTotal().Una clase vacía en UML con atributos se le podría considerar una DataClass si nos vamos a POO.

- b) Explicación de la herramienta que has utilizado para generar el diagrama [UML](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=639928 "UML"), y si la has contrastado con otra y conclusiones de porque has elegido esa.
La herramienta usada en este caso fue LucidChart, aunque tambien probé a usar Paint pero la primera es ligeramente más cómoda porque está hecha para crear esta clase de esquemas.


- c) Una explicación sobre la conversión del diagrama [UML](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=639928 "UML") al código.
Como dije antes, la conversión se basa básicamente en traducir cada cosa al código, atributos-constructores, operaciones-metodos y las propias clases que se representan igual en ambos ámbitos.



# Codigo Kotlin
``

````
data class Cliente(
    val nombre: String,
    val direccion: String,
    val contacto: String,
    val pedidos: MutableList<Pedido> = mutableListOf()
)

data class Producto(
    val nombre: String,
    val descripcion: String,
    val precio: Double,
    val impuestos: Double,
    var stock: Int
)

data class Pedido(
    val cliente: Cliente,
    val fecha: String,
    var estado: Estado = Estado.PENDIENTE,
    val productos: MutableList<Producto> = mutableListOf(),
    val pagos: MutableList<Pago> = mutableListOf()
) {
    fun calcularTotal(): Double {
        return productos.sumOf { it.precio + it.impuestos }
    }
}

enum class Estado {
    PENDIENTE, PAGADO, PROCESADO, ENVIADO, ENTREGADO
}

open class Pago(val monto: Double)

class PagoTarjeta(monto: Double, val numero: String, val fechaCaducidad: String, val tipo: String) : Pago(monto)

class PagoEfectivo(monto: Double, val moneda: String) : Pago(monto)

class PagoCheque(monto: Double, val nombre: String, val banco: String) : Pago(monto)
```
