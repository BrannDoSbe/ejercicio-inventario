lass Producto:
    def __init__(self, nombre, categoria, precio, cantidad):
        self.__nombre = nombre
        self.__categoria = categoria
        self.__precio = precio
        self.__cantidad = cantidad

    # Métodos getter y setter para acceder a los atributos privados
    @property
    def nombre(self):
        return self.__nombre

    @property
    def categoria(self):
        return self.__categoria
    
    @property
    def precio(self):
        return self.__precio
    
    @precio.setter
    def precio(self, precio):
        self.__precio = precio

    @property
    def cantidad(self):
        return self.__cantidad
    
    @cantidad.setter
    def cantidad(self, cantidad):
        self.__cantidad = cantidad

    def actualizar(self, nuevo_precio=None, nueva_cantidad=None):
        if nuevo_precio is not None and nuevo_precio > 0:
            self.precio = nuevo_precio
        if nueva_cantidad is not None and nueva_cantidad >= 0:
            self.cantidad = nueva_cantidad

    def __str__(self):
        return f"Producto: {self.nombre}, Categoría: {self.categoria}, Precio: {self.precio}, Cantidad: {self.cantidad}"


class Inventario:
    def __init__(self):
        self.productos = []

    def agregar_producto(self, producto):
        if not any(p.nombre == producto.nombre for p in self.productos):
            self.productos.append(producto)
            print(f"Producto '{producto.nombre}' agregado al inventario.")
        else:
            print(f"El producto '{producto.nombre}' ya existe en el inventario.")

    def actualizar_producto(self, nombre, precio=None, cantidad=None):
        # Verifica si el producto existe antes de proceder con la actualización
        producto = next((p for p in self.productos if p.nombre == nombre), None)
        if not producto:
            print(f"El producto '{nombre}' no existe en el inventario. No se han realizado cambios.")
            return  # Termina el método si el producto no existe

        # Si el producto existe, pregunta por el nuevo precio y cantidad
        nuevo_precio = ingresar_precio()
        nueva_cantidad = ingresar_cantidad()

        # Actualiza el producto con los nuevos valores
        producto.actualizar(nuevo_precio, nueva_cantidad)
        print(f"Producto '{nombre}' actualizado.")

    def eliminar_producto(self, nombre):
        for producto in self.productos:
            if producto.nombre == nombre:
                self.productos.remove(producto)
                print(f"Producto '{nombre}' eliminado del inventario.")
                return
        print(f"Producto '{nombre}' no encontrado en el inventario.")

    def mostrar_inventario(self):
        if not self.productos:
            print("El inventario está vacío.")
        else:
            print("Inventario:")
            for producto in self.productos:
                print(producto)

    def buscar_producto(self, nombre):
        for producto in self.productos:
            if producto.nombre == nombre:
                print(producto)
                return
        print(f"Producto '{nombre}' no encontrado en el inventario.")


# Validación de datos de entrada
def validar_precio(mensaje):
    while True:
        try:
            valor = float(input(mensaje))
            if valor > 0:
                return valor
            else:
                print("El valor debe ser mayor que 0.")
        except ValueError:
            print("Entrada inválida. Por favor, ingresa un número.")

def validar_cantidad(mensaje):
    while True:
        try:
            valor = int(input(mensaje))
            if valor >= 0:
                return valor
            else:
                print("La cantidad debe ser mayor o igual a 0.")
        except ValueError:
            print("Entrada inválida. Por favor, ingresa un número entero.")

# Solicitud de ingreso de Precio
def ingresar_precio():
    while True:
        input_precio = input("¿Quieres cambiar el precio? (s/n): ").lower()
        if input_precio == 's':
            return validar_precio("Nuevo precio: ")
        elif input_precio == 'n':
            break
        elif input_precio.strip() == '':
            return None
        
# Solicitud de ingreso de Cantidad        
def ingresar_cantidad():
    while True:
        input_cantidad = input("¿Quieres cambiar la cantidad? (s/n): ").lower()
        if input_cantidad == 's':
            return validar_cantidad("Nueva cantidad: ")
        elif input_cantidad == 'n':
            break
        elif input_cantidad.strip() == '':
            return None

# Función para mostrar el menú
def mostrar_menu():
    print("\n--- Menú de Inventario ---")
    print("1. Agregar producto")
    print("2. Actualizar producto")
    print("3. Eliminar producto")
    print("4. Buscar producto")
    print("5. Mostrar inventario")
    print("6. Salir")
    return input("Selecciona una opción: ")


# Ejemplo de uso con un menú
inventario = Inventario()

while True:
    opcion = mostrar_menu()

    if opcion == '1':
        # Agregar producto
        nombre = input("Nombre del producto: ")
        categoria = input("Categoría del producto: ")
        precio = validar_precio("Precio del producto: ")
        cantidad = validar_cantidad("Cantidad en stock: ")
        producto = Producto(nombre, categoria, precio, cantidad)
        inventario.agregar_producto(producto)

    elif opcion == '2':
        # Actualizar producto
        nombre_actualizar = input("Nombre del producto a actualizar: ")
        inventario.actualizar_producto(nombre_actualizar)

    elif opcion == '3':
        # Eliminar producto
        nombre_eliminar = input("Nombre del producto a eliminar: ")
        inventario.eliminar_producto(nombre_eliminar)

    elif opcion == '4':
        # Buscar producto
        nombre_buscar = input("Nombre del producto a buscar: ")
        inventario.buscar_producto(nombre_buscar)

    elif opcion == '5':
        # Mostrar inventario
        inventario.mostrar_inventario()

    elif opcion == '6':
        # Salir del programa
        print("Saliendo del sistema de inventario...")
        break

    else:
        print("Opción no válida, por favor selecciona una opción del 1 al 6.")
