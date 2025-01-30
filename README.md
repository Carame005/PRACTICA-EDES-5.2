# PRACTICA-EDES-5.2

- a) Una breve lista de los conceptos que te has encontrado en los diagramas [UML](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=639928 "UML") que se asemejan a los conceptos de programación orientada a objetos. Por [ejemplo](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=655395 "Ejemplo"): Clases: GestorPedidos

Las clases UML son básicamente iguales a las clases de POO en kotlin, los atributos de las clases UML asemejan a los constructores en POO ya sean primarios o secundarios, las operaciones son básicamente métodos, como calcularTotal().Una clase vacía en UML con atributos se le podría considerar una DataClass si nos vamos a POO.

- b) Explicación de la herramienta que has utilizado para generar el diagrama [UML](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=639928 "UML"), y si la has contrastado con otra y conclusiones de porque has elegido esa.
La herramienta usada en este caso fue LucidChart, aunque tambien probé a usar Paint pero la primera es ligeramente más cómoda porque está hecha para crear esta clase de esquemas.


- c) Una explicación sobre la conversión del diagrama [UML](https://educacionadistancia.juntadeandalucia.es/centros/cadiz/mod/resource/view.php?id=639928 "UML") al código.
Como dije antes, la conversión se basa básicamente en traducir cada cosa al código, atributos-constructores, operaciones-metodos y las propias clases que se representan igual en ambos ámbitos.
``

```
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


# Programa completo
```
// Definición de clases

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
    
    fun actualizarEstado(nuevoEstado: Estado) {
        estado = nuevoEstado
    }
}

enum class Estado {
    PENDIENTE, PAGADO, PROCESADO, ENVIADO, ENTREGADO
}

open class Pago(val monto: Double)

class PagoTarjeta(monto: Double, val numero: String, val fechaCaducidad: String, val tipo: String) : Pago(monto)

class PagoEfectivo(monto: Double, val moneda: String) : Pago(monto)

class PagoCheque(monto: Double, val nombre: String, val banco: String) : Pago(monto)

// Funcionalidad del sistema

fun realizarPedido(cliente: Cliente, productos: List<Producto>): Pedido {
    val pedido = Pedido(cliente, "2025-01-30")
    for (producto in productos) {
        if (producto.stock > 0) {
            pedido.productos.add(producto)
            producto.stock--
        } else {
            println("Producto ${producto.nombre} sin stock disponible.")
        }
    }
    cliente.pedidos.add(pedido)
    return pedido
}

fun registrarPago(pedido: Pedido, pago: Pago) {
    pedido.pagos.add(pago)
    if (pedido.pagos.sumOf { it.monto } >= pedido.calcularTotal()) {
        pedido.actualizarEstado(Estado.PAGADO)
    }
}

fun main() {
    // Creación de clientes y productos
    val cliente = Cliente("Juan Pérez", "Calle 123", "123456789")
    val producto1 = Producto("Laptop", "Portátil de gama alta", 1200.0, 200.0, 10)
    val producto2 = Producto("Mouse", "Ratón inalámbrico", 50.0, 10.0, 5)
    
    // Realización de pedido
    val pedido = realizarPedido(cliente, listOf(producto1, producto2))
    println("Total del pedido: ${pedido.calcularTotal()}")
    
    // Registro de pago
    val pago = PagoTarjeta(1410.0, "1234-5678-9012-3456", "12/27", "Visa")
    registrarPago(pedido, pago)
    println("Estado del pedido: ${pedido.estado}")
}
```
