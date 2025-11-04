# Proyecto Ventaropa - Guía de Implementación

Te proporciono una guía completa paso a paso para crear el proyecto Ventaropa:

## 1. Crear carpeta del proyecto
```bash
mkdir UIII_Ventaropa_0612
```

## 2. Abrir VS Code en la carpeta
```bash
cd UIII_Ventaropa_0612
code .
```

## 3. Abrir terminal en VS Code
- `Ctrl + Ñ` (Windows/Linux) o `Cmd + Ñ` (Mac)
- O menú: View → Terminal

## 4. Crear entorno virtual
```bash
python -m venv .venv
```

## 5. Activar entorno virtual
**Windows:**
```bash
.venv\Scripts\activate
```
**Mac/Linux:**
```bash
source .venv/bin/activate
```

## 6. Activar intérprete de Python
- `Ctrl + Shift + P` → "Python: Select Interpreter"
- Elegir el del entorno virtual: `./.venv/Scripts/python.exe`

## 7. Instalar Django
```bash
pip install django
```

## 8. Crear proyecto Django
```bash
django-admin startproject backend_Ventaropa .
```
(el punto evita duplicar carpetas)

## 9. Estructura de archivos a crear
```
UIII_Ventaropa_0612/
├── .venv/
├── backend_Ventaropa/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_Ventaropa/
│   ├── migrations/
│   ├── templates/
│   │   ├── producto/
│   │   │   ├── agregar_producto.html
│   │   │   ├── ver_productos.html
│   │   │   ├── actualizar_producto.html
│   │   │   └── borrar_producto.html
│   │   ├── base.html
│   │   ├── header.html
│   │   ├── navbar.html
│   │   ├── footer.html
│   │   └── inicio.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   └── urls.py
└── manage.py
```

## 10. Crear aplicación
```bash
python manage.py startapp app_Ventaropa
```

## 11. Configurar models.py
**app_Ventaropa/models.py:**
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

## 12. Realizar migraciones
```bash
python manage.py makemigrations
python manage.py migrate
```

## 13. Configurar views.py
**app_Ventaropa/views.py:**
```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Producto

def inicio_Ventaropa(request):
    return render(request, 'inicio.html')

def agregar_producto(request):
    if request.method == 'POST':
        Producto.objects.create(
            nombre=request.POST['nombre'],
            descripcion=request.POST['descripcion'],
            categoria=request.POST['categoria'],
            precio=request.POST['precio'],
            stock=request.POST['stock'],
            talla=request.POST['talla'],
            color=request.POST['color']
        )
        return redirect('ver_productos')
    return render(request, 'producto/agregar_producto.html')

def ver_productos(request):
    productos = Producto.objects.all()
    return render(request, 'producto/ver_productos.html', {'productos': productos})

def actualizar_producto(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    return render(request, 'producto/actualizar_producto.html', {'producto': producto})

def realizar_actualizacion_producto(request, producto_id):
    if request.method == 'POST':
        producto = get_object_or_404(Producto, id=producto_id)
        producto.nombre = request.POST['nombre']
        producto.descripcion = request.POST['descripcion']
        producto.categoria = request.POST['categoria']
        producto.precio = request.POST['precio']
        producto.stock = request.POST['stock']
        producto.talla = request.POST['talla']
        producto.color = request.POST['color']
        producto.save()
        return redirect('ver_productos')
    return redirect('ver_productos')

def borrar_producto(request, producto_id):
    producto = get_object_or_404(Producto, id=producto_id)
    if request.method == 'POST':
        producto.delete()
        return redirect('ver_productos')
    return render(request, 'producto/borrar_producto.html', {'producto': producto})
```

## 14. Crear templates

**app_Ventaropa/templates/base.html:**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ventaropa - Sistema de Administración</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f8f9fa;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .navbar-custom {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .footer-custom {
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
            color: white;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
        .main-content {
            margin-bottom: 100px;
        }
        .card {
            border: none;
            border-radius: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            transition: transform 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
        }
        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
        }
    </style>
</head>
<body>
    {% include 'header.html' %}
    {% include 'navbar.html' %}
    
    <div class="container-fluid main-content">
        {% block content %}
        {% endblock %}
    </div>
    
    {% include 'footer.html' %}

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

**app_Ventaropa/templates/navbar.html:**
```html
<nav class="navbar navbar-expand-lg navbar-dark navbar-custom">
    <div class="container">
        <a class="navbar-brand" href="{% url 'inicio' %}">
            🛍️ Sistema de Administración Ventaropa
        </a>
        
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
            <span class="navbar-toggler-icon"></span>
        </button>
        
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav me-auto">
                <li class="nav-item">
                    <a class="nav-link" href="{% url 'inicio' %}">
                        🏠 Inicio
                    </a>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="productosDropdown" role="button" data-bs-toggle="dropdown">
                        👕 Productos
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="{% url 'agregar_producto' %}">Agregar Producto</a></li>
                        <li><a class="dropdown-item" href="{% url 'ver_productos' %}">Ver Productos</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="clientesDropdown" role="button" data-bs-toggle="dropdown">
                        👥 Clientes
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Ver Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Clientes</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Clientes</a></li>
                    </ul>
                </li>
                
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="pedidosDropdown" role="button" data-bs-toggle="dropdown">
                        📦 Pedidos
                    </a>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Agregar Pedidos</a></li>
                        <li><a class="dropdown-item" href="#">Ver Pedidos</a></li>
                        <li><a class="dropdown-item" href="#">Actualizar Pedidos</a></li>
                        <li><a class="dropdown-item" href="#">Borrar Pedidos</a></li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</nav>
```

**app_Ventaropa/templates/footer.html:**
```html
<footer class="footer-custom text-center py-3">
    <div class="container">
        <p class="mb-0">
            &copy; {% now "Y" %} - Todos los derechos reservados | 
            <script>document.write(new Date().toLocaleDateString());</script> | 
            Creado por Ing. Vania Jimenez, Cbtis 128
        </p>
    </div>
</footer>
```

**app_Ventaropa/templates/inicio.html:**
```html
{% extends 'base.html' %}

{% block content %}
<div class="container mt-5">
    <div class="row">
        <div class="col-md-6">
            <div class="card p-4">
                <h2 class="text-primary mb-4">Bienvenido a Ventaropa</h2>
                <p class="lead">
                    Sistema de administración para tienda de ropa. Gestiona productos, 
                    clientes y pedidos de manera eficiente.
                </p>
                <div class="mt-4">
                    <h5>Funcionalidades:</h5>
                    <ul>
                        <li>Gestión de inventario de productos</li>
                        <li>Control de clientes</li>
                        <li>Administración de pedidos</li>
                        <li>Reportes y estadísticas</li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="col-md-6">
            <div class="card">
                <img src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1000&q=80" 
                     class="card-img-top" alt="Tienda de Ropa" style="height: 400px; object-fit: cover;">
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

## 15. Templates de Productos

**app_Ventaropa/templates/producto/agregar_producto.html:**
```html
{% extends 'base.html' %}

{% block content %}
<div class="container mt-4">
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header bg-primary text-white">
                    <h4 class="mb-0">➕ Agregar Nuevo Producto</h4>
                </div>
                <div class="card-body">
                    <form method="POST">
                        {% csrf_token %}
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label class="form-label">Nombre</label>
                                <input type="text" class="form-control" name="nombre" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label class="form-label">Categoría</label>
                                <input type="text" class="form-control" name="categoria" required>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label class="form-label">Descripción</label>
                            <textarea class="form-control" name="descripcion" rows="3" required></textarea>
                        </div>
                        
                        <div class="row">
                            <div class="col-md-4 mb-3">
                                <label class="form-label">Precio</label>
                                <input type="number" step="0.01" class="form-control" name="precio" required>
                            </div>
                            <div class="col-md-4 mb-3">
                                <label class="form-label">Stock</label>
                                <input type="number" class="form-control" name="stock" required>
                            </div>
                            <div class="col-md-4 mb-3">
                                <label class="form-label">Talla</label>
                                <input type="text" class="form-control" name="talla" required>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label class="form-label">Color</label>
                            <input type="text" class="form-control" name="color" required>
                        </div>
                        
                        <div class="d-grid gap-2">
                            <button type="submit" class="btn btn-primary">Guardar Producto</button>
                            <a href="{% url 'ver_productos' %}" class="btn btn-secondary">Cancelar</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

**app_Ventaropa/templates/producto/ver_productos.html:**
```html
{% extends 'base.html' %}

{% block content %}
<div class="container mt-4">
    <div class="d-flex justify-content-between align-items-center mb-4">
        <h2>📦 Lista de Productos</h2>
        <a href="{% url 'agregar_producto' %}" class="btn btn-primary">➕ Agregar Producto</a>
    </div>
    
    <div class="card">
        <div class="card-body">
            <div class="table-responsive">
                <table class="table table-striped table-hover">
                    <thead class="table-dark">
                        <tr>
                            <th>ID</th>
                            <th>Nombre</th>
                            <th>Categoría</th>
                            <th>Precio</th>
                            <th>Stock</th>
                            <th>Talla</th>
                            <th>Color</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        {% for producto in productos %}
                        <tr>
                            <td>{{ producto.id }}</td>
                            <td>{{ producto.nombre }}</td>
                            <td>{{ producto.categoria }}</td>
                            <td>${{ producto.precio }}</td>
                            <td>{{ producto.stock }}</td>
                            <td>{{ producto.talla }}</td>
                            <td>{{ producto.color }}</td>
                            <td>
                                <a href="{% url 'actualizar_producto' producto.id %}" class="btn btn-warning btn-sm">✏️ Editar</a>
                                <a href="{% url 'borrar_producto' producto.id %}" class="btn btn-danger btn-sm">🗑️ Borrar</a>
                            </td>
                        </tr>
                        {% empty %}
                        <tr>
                            <td colspan="8" class="text-center">No hay productos registrados</td>
                        </tr>
                        {% endfor %}
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

**app_Ventaropa/templates/producto/actualizar_producto.html:**
```html
{% extends 'base.html' %}

{% block content %}
<div class="container mt-4">
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header bg-warning text-dark">
                    <h4 class="mb-0">✏️ Actualizar Producto</h4>
                </div>
                <div class="card-body">
                    <form method="POST" action="{% url 'realizar_actualizacion_producto' producto.id %}">
                        {% csrf_token %}
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label class="form-label">Nombre</label>
                                <input type="text" class="form-control" name="nombre" value="{{ producto.nombre }}" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label class="form-label">Categoría</label>
                                <input type="text" class="form-control" name="categoria" value="{{ producto.categoria }}" required>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label class="form-label">Descripción</label>
                            <textarea class="form-control" name="descripcion" rows="3" required>{{ producto.descripcion }}</textarea>
                        </div>
                        
                        <div class="row">
                            <div class="col-md-4 mb-3">
                                <label class="form-label">Precio</label>
                                <input type="number" step="0.01" class="form-control" name="precio" value="{{ producto.precio }}" required>
                            </div>
                            <div class="col-md-4 mb-3">
                                <label class="form-label">Stock</label>
                                <input type="number" class="form-control" name="stock" value="{{ producto.stock }}" required>
                            </div>
                            <div class="col-md-4 mb-3">
                                <label class="form-label">Talla</label>
                                <input type="text" class="form-control" name="talla" value="{{ producto.talla }}" required>
                            </div>
                        </div>
                        
                        <div class="mb-3">
                            <label class="form-label">Color</label>
                            <input type="text" class="form-control" name="color" value="{{ producto.color }}" required>
                        </div>
                        
                        <div class="d-grid gap-2">
                            <button type="submit" class="btn btn-warning">Actualizar Producto</button>
                            <a href="{% url 'ver_productos' %}" class="btn btn-secondary">Cancelar</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

**app_Ventaropa/templates/producto/borrar_producto.html:**
```html
{% extends 'base.html' %}

{% block content %}
<div class="container mt-4">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <div class="card">
                <div class="card-header bg-danger text-white">
                    <h4 class="mb-0">🗑️ Eliminar Producto</h4>
                </div>
                <div class="card-body text-center">
                    <h5>¿Estás seguro de que deseas eliminar este producto?</h5>
                    <p class="text-muted">{{ producto.nombre }} ({{ producto.talla }}, {{ producto.color }})</p>
                    
                    <form method="POST">
                        {% csrf_token %}
                        <div class="d-grid gap-2">
                            <button type="submit" class="btn btn-danger">Sí, Eliminar</button>
                            <a href="{% url 'ver_productos' %}" class="btn btn-secondary">Cancelar</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

## 16. Configurar URLs

**app_Ventaropa/urls.py:**
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_Ventaropa, name='inicio'),
    path('agregar-producto/', views.agregar_producto, name='agregar_producto'),
    path('ver-productos/', views.ver_productos, name='ver_productos'),
    path('actualizar-producto/<int:producto_id>/', views.actualizar_producto, name='actualizar_producto'),
    path('realizar-actualizacion/<int:producto_id>/', views.realizar_actualizacion_producto, name='realizar_actualizacion_producto'),
    path('borrar-producto/<int:producto_id>/', views.borrar_producto, name='borrar_producto'),
]
```

## 17. Configurar settings.py

**backend_Ventaropa/settings.py:**
```python
# Agregar la aplicación
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_Ventaropa',  # Agregar esta línea
]

# Configurar templates
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

## 18. Configurar URLs principales

**backend_Ventaropa/urls.py:**
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_Ventaropa.urls')),
]
```

## 19. Registrar modelo en admin

**app_Ventaropa/admin.py:**
```python
from django.contrib import admin
from .models import Producto

@admin.register(Producto)
class ProductoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'categoria', 'precio', 'stock', 'talla', 'color')
    list_filter = ('categoria', 'talla', 'color')
    search_fields = ('nombre', 'descripcion')
```

## 20. Realizar migraciones finales
```bash
python manage.py makemigrations
python manage.py migrate
```

## 21. Ejecutar servidor
```bash
python manage.py runserver 8036
```

## 22. Acceder al sistema
Abrir navegador y visitar: `http://127.0.0.1:8036`

## Comandos finales de verificación
```bash
# Verificar que el servidor esté corriendo
python manage.py runserver 8036

# Crear superusuario para admin (opcional)
python manage.py createsuperuser
```

El proyecto estará completamente funcional con todas las operaciones CRUD para productos, interfaz moderna con colores suaves, y estructura organizada según los requerimientos especificados.
