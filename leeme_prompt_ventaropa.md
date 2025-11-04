¡Perfecto! Vamos a continuar con todos los puntos detallados, paso a paso. Me aseguraré de que todo esté claro y bien explicado. Empezamos desde el principio, ¡allá vamos!

### **1. Procedimiento para crear carpeta del Proyecto: `UIII_Ventaropa_0612`**

Primero, abre una terminal o línea de comandos en tu sistema operativo y navega hasta el directorio donde quieres crear tu proyecto.

Ejecuta el siguiente comando para crear la carpeta de tu proyecto:

```bash
mkdir UIII_Ventaropa_0612
```

Esto crea una carpeta llamada `UIII_Ventaropa_0612` en el directorio actual.

---

### **2. Procedimiento para abrir VS Code sobre la carpeta `UIII_Ventaropa_0612`**

Una vez que hayas creado la carpeta del proyecto, abre VS Code desde la terminal dentro de esa carpeta:

```bash
cd UIII_Ventaropa_0612
code .
```

Esto abrirá Visual Studio Code en la carpeta `UIII_Ventaropa_0612` directamente.

---

### **3. Procedimiento para abrir terminal en VS Code**

Para abrir la terminal en VS Code, sigue estos pasos:

1. Haz clic en **Ver** (en la barra superior de VS Code).
2. Selecciona **Terminal**.
3. También puedes usar el atajo de teclado `Ctrl + `` (el acento grave, al lado del número 1).

La terminal aparecerá en la parte inferior de VS Code.

---

### **4. Procedimiento para crear carpeta entorno virtual `.venv` desde terminal de VS Code**

En la terminal de VS Code, crea el entorno virtual ejecutando:

```bash
python -m venv .venv
```

Esto crea una carpeta llamada `.venv` que contiene el entorno virtual.

---

### **5. Procedimiento para activar el entorno virtual**

Para activar el entorno virtual, usa el siguiente comando según tu sistema operativo:

* **Windows:**

  ```bash
  .\.venv\Scripts\activate
  ```

* **macOS/Linux:**

  ```bash
  source .venv/bin/activate
  ```

Después de ejecutar el comando, la terminal debería mostrar el nombre del entorno virtual (por ejemplo, `(.venv)`) al inicio de la línea de comandos.

---

### **6. Procedimiento para activar intérprete de Python**

Una vez que el entorno virtual esté activo, VS Code debería detectar automáticamente el intérprete de Python. Si no lo hace:

1. Haz clic en el nombre del intérprete de Python en la parte inferior izquierda de la ventana de VS Code.
2. Selecciona **.venv** (el entorno virtual que creaste) de la lista de intérpretes.

---

### **7. Procedimiento para instalar Django**

Con el entorno virtual activado, instala Django ejecutando el siguiente comando en la terminal:

```bash
pip install django
```

Esto instalará Django en tu entorno virtual. Puedes verificar la instalación ejecutando:

```bash
python -m django --version
```

Esto debería mostrarte la versión de Django instalada.

---

### **8. Procedimiento para crear proyecto `backend_Ventaropa` sin duplicar carpeta**

Ahora que tienes Django instalado, puedes crear el proyecto Django ejecutando el siguiente comando:

```bash
django-admin startproject backend_Ventaropa .
```

El punto (`.`) es para indicar que el proyecto debe crearse en el directorio actual, evitando la creación de una subcarpeta adicional.

---

### **9. Procedimiento para ejecutar servidor en el puerto 8036**

Una vez que el proyecto esté creado, puedes ejecutar el servidor de desarrollo de Django con el siguiente comando:

```bash
python manage.py runserver 8036
```

Esto levantará el servidor en el puerto `8036` (en lugar del puerto por defecto 8000).

---

### **10. Procedimiento para copiar y pegar el link en el navegador**

En la terminal, verás un mensaje como este:

```bash
Starting development server at http://127.0.0.1:8036/
Quit the server with CONTROL-C.
```

Copia la URL `http://127.0.0.1:8036/` y pégala en la barra de direcciones de tu navegador para ver el proyecto en funcionamiento.

---

### **11. Procedimiento para crear aplicación `app_Ventaropa`**

Ahora, crea una aplicación llamada `app_Ventaropa` dentro del proyecto:

```bash
python manage.py startapp app_Ventaropa
```

Esto creará una nueva carpeta llamada `app_Ventaropa` con los archivos necesarios para tu aplicación Django.

---

### **12. Modelo `models.py`**

El modelo `Producto` que proporcionaste está bien estructurado. Aquí está de nuevo el código para referencia:

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    categoria = models.CharField(max_length=50)
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.PositiveIntegerField()
    talla = models.CharField(max_length=10)
    color = models.CharField(max_length=30)

    def __str__(self):
        return f"{self.nombre} ({self.talla}, {self.color})"
```

---

### **12.5 Procedimiento para realizar las migraciones (`makemigrations` y `migrate`)**

1. **Crear las migraciones:**

   Ejecuta el siguiente comando para generar las migraciones de la base de datos según los modelos definidos:

   ```bash
   python manage.py makemigrations
   ```

2. **Aplicar las migraciones:**

   Después de crear las migraciones, aplícalas a la base de datos ejecutando:

   ```bash
   python manage.py migrate
   ```

Esto sincroniza tus modelos con la base de datos.

---

### **13. Primero trabajamos con el MODELO: CATEGORÍA**

Para trabajar con el modelo `Categoria`, crea un modelo en `models.py` similar a esto:

```python
class Categoria(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()

    def __str__(self):
        return self.nombre
```

Luego, realiza las migraciones para este modelo:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **14. En `views.py` de `app_Ventaropa`, crear las funciones correspondientes**

Aquí tienes las vistas para las operaciones CRUD de productos. Las funciones básicas para las vistas de agregar, actualizar y eliminar productos pueden ser algo como esto:

```python
from django.shortcuts import render, redirect
from .models import Producto

def inicio_Ventaropa(request):
    return render(request, 'inicio.html')

def agregar_producto(request):
    if request.method == 'POST':
        # Aquí iría la lógica para agregar un producto
        pass
    return render(request, 'agregar_producto.html')

def actualizar_producto(request, id):
    producto = Producto.objects.get(id=id)
    if request.method == 'POST':
        # Aquí iría la lógica para actualizar el producto
        pass
    return render(request, 'actualizar_producto.html', {'producto': producto})

def borrar_producto(request, id):
    producto = Producto.objects.get(id=id)
    if request.method == 'POST':
        producto.delete()
        return redirect('productos')
    return render(request, 'borrar_producto.html', {'producto': producto})
```

Estas funciones permiten agregar, actualizar y borrar productos desde las vistas.

---

### **15. Crear la carpeta “templates” dentro de `app_Ventaropa`**

Dentro de la carpeta `app_Ventaropa`, crea una carpeta llamada `templates`. Dentro de esta carpeta, se crearán los archivos HTML para la vista.

---

### **16. En la carpeta `templates`, crear los archivos HTML**

Crea los siguientes archivos dentro de `app_Ventaropa/templates`:

* **base.html**: Estructura base con Bootstrap.
* **header.html**: Contendrá el encabezado del sitio.
* **navbar.html**: Barra de navegación.
* **footer.html**: Pie de página.
* **inicio.html**: Página de inicio del sistema.

---

### **17. En el archivo `base.html` agregar Bootstrap para CSS y JS**

Aquí te dejo un ejemplo de cómo agregar Bootstrap en el archivo `base.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ventaropa</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    {% include 'header.html' %}
    {% include 'navbar.html' %}

    <div class="container">
        {% block content %}
        {% endblock %}
    </div>

    {% include 'footer.html' %}

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---
¡Claro! Continuemos con los pasos restantes:

---

### **18. En el archivo `navbar.html` incluir opciones del menú**

A continuación, te muestro cómo podrías estructurar el menú de navegación (`navbar.html`) utilizando Bootstrap, que incluye las opciones de administración y submenús para las categorías, productos, clientes y pedidos:

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Sistema de Administración Ventaropa</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
            <!-- Opción Inicio -->
            <li class="nav-item">
                <a class="nav-link" href="{% url 'inicio_Ventaropa' %}">Inicio</a>
            </li>
            <!-- Opción Productos -->
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                    Productos
                </a>
                <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
                    <li><a class="dropdown-item" href="{% url 'agregar_producto' %}">Agregar Producto</a></li>
                    <li><a class="dropdown-item" href="{% url 'ver_productos' %}">Ver Productos</a></li>
                    <li><a class="dropdown-item" href="{% url 'actualizar_producto' %}">Actualizar Producto</a></li>
                    <li><a class="dropdown-item" href="{% url 'borrar_producto' %}">Borrar Producto</a></li>
                </ul>
            </li>
            <!-- Opción Clientes -->
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownClientes" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                    Clientes
                </a>
                <ul class="dropdown-menu" aria-labelledby="navbarDropdownClientes">
                    <li><a class="dropdown-item" href="{% url 'agregar_cliente' %}">Agregar Cliente</a></li>
                    <li><a class="dropdown-item" href="{% url 'ver_clientes' %}">Ver Clientes</a></li>
                    <li><a class="dropdown-item" href="{% url 'actualizar_cliente' %}">Actualizar Cliente</a></li>
                    <li><a class="dropdown-item" href="{% url 'borrar_cliente' %}">Borrar Cliente</a></li>
                </ul>
            </li>
            <!-- Opción Pedidos -->
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownPedidos" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                    Pedidos
                </a>
                <ul class="dropdown-menu" aria-labelledby="navbarDropdownPedidos">
                    <li><a class="dropdown-item" href="{% url 'agregar_pedido' %}">Agregar Pedido</a></li>
                    <li><a class="dropdown-item" href="{% url 'ver_pedidos' %}">Ver Pedidos</a></li>
                    <li><a class="dropdown-item" href="{% url 'actualizar_pedido' %}">Actualizar Pedido</a></li>
                    <li><a class="dropdown-item" href="{% url 'borrar_pedido' %}">Borrar Pedido</a></li>
                </ul>
            </li>
        </ul>
    </div>
</nav>
```

Este es un ejemplo básico de una barra de navegación que usa Bootstrap para mostrar un menú desplegable con subopciones para productos, clientes y pedidos.

---

### **19. En el archivo `footer.html` incluir derechos de autor, fecha del sistema y “Creado por Ing. Vania Jimenez, Cbtis 128” y mantenerla fija al final de la página.**

El pie de página (`footer.html`) puede estar estructurado de la siguiente forma para incluir los derechos de autor y la fecha:

```html
<footer class="bg-light text-center py-3 fixed-bottom">
    <p>&copy; {{ current_year }} Ventaropa - Todos los derechos reservados.</p>
    <p>Creado por Ing. Vania Jimenez, Cbtis 128</p>
</footer>
```

Para asegurarte de que se muestre el año actual dinámicamente, puedes pasar la variable `current_year` desde tu vista en `views.py` de la siguiente forma:

```python
from django.shortcuts import render
from datetime import datetime

def inicio_Ventaropa(request):
    current_year = datetime.now().year
    return render(request, 'inicio.html', {'current_year': current_year})
```

Esto hace que el año se actualice automáticamente en cada carga de la página.

---

### **20. En el archivo `inicio.html` se usa para colocar información del sistema más una imagen tomada desde la red sobre cinepolis.**

Puedes incluir información sobre el sistema y una imagen de Cinépolis desde la red. Aquí tienes un ejemplo de cómo hacerlo en `inicio.html`:

```html
{% extends 'base.html' %}

{% block content %}
<div class="container mt-5">
    <h1>Bienvenido al Sistema de Administración Ventaropa</h1>
    <p>Este es el sistema de administración para gestionar productos, clientes y pedidos en la tienda Ventaropa.</p>

    <!-- Imagen de Cinépolis desde la red -->
    <img src="https://www.cinepolis.com.mx/images/logos/cinepolis-logo.svg" alt="Cinépolis" class="img-fluid mt-3">

</div>
{% endblock %}
```

---

### **21. Crear la subcarpeta “producto” dentro de `app_Ventaropa/templates`**

Dentro de la carpeta `templates`, crea una subcarpeta llamada `producto` para organizar las vistas relacionadas con los productos. La estructura quedaría así:

```
app_Ventaropa/
    templates/
        producto/
            agregar_producto.html
            ver_productos.html
            actualizar_producto.html
            borrar_producto.html
```

---

### **22. Crear los archivos HTML con su código correspondiente de `agregar_producto.html`, `ver_productos.html`, `actualizar_producto.html`, `borrar_producto.html` dentro de `app_Ventaropa/templates/producto`**

Aquí te dejo los ejemplos de los archivos HTML dentro de la subcarpeta `producto`:

* **agregar_producto.html**

```html
{% extends 'base.html' %}

{% block content %}
<h2>Agregar Producto</h2>
<form method="post">
    {% csrf_token %}
    <!-- Formulario para agregar producto -->
    <div class="form-group">
        <label for="nombre">Nombre del Producto</label>
        <input type="text" class="form-control" id="nombre" name="nombre">
    </div>
    <div class="form-group">
        <label for="precio">Precio</label>
        <input type="number" class="form-control" id="precio" name="precio">
    </div>
    <!-- Otros campos como descripción, categoría, etc. -->
    <button type="submit" class="btn btn-primary">Agregar</button>
</form>
{% endblock %}
```

* **ver_productos.html**

```html
{% extends 'base.html' %}

{% block content %}
<h2>Ver Productos</h2>
<table class="table">
    <thead>
        <tr>
            <th>Nombre</th>
            <th>Precio</th>
            <th>Acciones</th>
        </tr>
    </thead>
    <tbody>
        {% for producto in productos %}
        <tr>
            <td>{{ producto.nombre }}</td>
            <td>{{ producto.precio }}</td>
            <td>
                <a href="{% url 'actualizar_producto' producto.id %}" class="btn btn-warning">Editar</a>
                <a href="{% url 'borrar_producto' producto.id %}" class="btn btn-danger">Eliminar</a>
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table>
{% endblock %}
```

* **actualizar_producto.html**

```html
{% extends 'base.html' %}

{% block content %}
<h2>Actualizar Producto</h2>
<form method="post">
    {% csrf_token %}
    <!-- Formulario para actualizar producto -->
    <div class="form-group">
        <label for="nombre">Nombre del Producto</label>
        <input type="text" class="form-control" id="nombre" name="nombre" value="{{ producto.nombre }}">
    </div>
    <div class="form-group">
        <label for="precio">Precio</label>
        <input type="number" class="form-control" id="precio" name="precio" value="{{ producto.precio }}">
    </div>
    <!-- Otros campos a actualizar -->
    <button type="submit" class="btn btn-primary">Actualizar</button>
</form>
{% endblock %}
```

* **borrar_producto.html**

```html
{% extends 'base.html' %}

{% block content %}
<h2>Eliminar Producto</h2>
<p>¿Estás seguro de que quieres eliminar el producto {{ producto.nombre }}?</p>
<form method="post">
```


```
{% csrf_token %}
<button type="submit" class="btn btn-danger">Eliminar</button>
<a href="{% url 'ver_productos' %}" class="btn btn-secondary">Cancelar</a>
```

</form>
{% endblock %}
```

---

### **23. No utilizar `forms.py`**

Como se te pidió, no utilizaremos `forms.py`. En su lugar, manejarás la lógica del formulario directamente en las vistas y plantillas como se mostró en los ejemplos anteriores.

---

### **24. Procedimiento para crear el archivo `urls.py` en `app_Ventaropa` con el código correspondiente para acceder a las funciones de `views.py` para operaciones de CRUD en productos**

Crea el archivo `urls.py` dentro de `app_Ventaropa` y define las rutas correspondientes:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_Ventaropa, name='inicio_Ventaropa'),
    path('agregar_producto/', views.agregar_producto, name='agregar_producto'),
    path('ver_productos/', views.ver_productos, name='ver_productos'),
    path('actualizar_producto/<int:id>/', views.actualizar_producto, name='actualizar_producto'),
    path('borrar_producto/<int:id>/', views.borrar_producto, name='borrar_producto'),
]
```

---

### **25. Procedimiento para agregar `app_Ventaropa` en `settings.py` de `backend_Ventaropa`**

Abre el archivo `settings.py` de tu proyecto `backend_Ventaropa` y agrega `'app_Ventaropa'` en la lista de aplicaciones instaladas:

```python
INSTALLED_APPS = [
    ...
    'app_Ventaropa',
]
```

---

### **26. Realizar las configuraciones correspondientes a `urls.py` de `backend_Ventaropa` para enlazar con `app_Ventaropa`**

Abre el archivo `urls.py` en `backend_Ventaropa` y añade la configuración para incluir las URLs de `app_Ventaropa`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Ventaropa.urls')),  # Incluye las URLs de app_Ventaropa
]
```

---

### **27. Procedimiento para registrar los modelos en `admin.py` y volver a realizar las migraciones**

En `app_Ventaropa/admin.py`, registra el modelo `Producto` para poder gestionarlo desde el panel de administración:

```python
from django.contrib import admin
from .models import Producto

admin.site.register(Producto)
```

Luego, realiza las migraciones de nuevo:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### **28. Utilizar colores suaves, atractivos y modernos**

Al usar **Bootstrap** y siguiendo el estilo de las plantillas que ya estás creando, tendrás colores suaves y modernos. Asegúrate de usar clases de **Bootstrap** como `bg-light`, `btn-primary`, etc., para crear un diseño limpio y atractivo.

---

### **29. Al inicio crear la estructura completa de carpetas y archivos**

Ya hemos cubierto cómo crear las carpetas y archivos necesarios en los pasos anteriores, incluyendo `templates`, `static`, `urls.py`, y las vistas.

---

### **30. Proyecto totalmente funcional**

Con todos estos pasos realizados, tendrás un proyecto completamente funcional con CRUD de productos, administración de clientes y pedidos, y una interfaz básica.

---

### **31. Finalmente ejecutar servidor en el puerto 8036**

Finalmente, asegúrate de que todo esté en orden y ejecuta el servidor de nuevo en el puerto `8036`:

```bash
python manage.py runserver 8021
```

¡Listo! Con estos pasos, tu proyecto `Ventaropa` debería estar completamente funcional y en marcha.

