## Proyecto

Django = API REST
React = interfaz de usuario
Comunicación = HTTP (JSON)


Backend

    cd /Users/desarrollador/development/2026/django/django-drf-demo
    source venv/bin/activate                                                                                                    ✔  10219  08:47:39
    which python 

frontend

    cd /Users/desarrollador/development/2026/django/django-drf-demo/frontend
    npm run dev



## Ventajas de un solo repositorio




Simula una arquitectura real

Muchos proyectos usan:

    Repositorio
    ├── backend/
    └── frontend/


http://localhost:8000/api/users/
DRF en http://localhost:8000
React en http://localhost:5173


Aunque luego se desplieguen por separado.

    django-drf-demo/
    ├── backend/
    │   ├── manage.py
    │   ├── config/
    │   ├── users/
    │   ├── db.sqlite3
    │   └── requirements.txt
    │
    └── frontend/
        ├── src/
        ├── public/
        ├── package.json
        └── vite.config.js


### FrontEnd

Aprender React de la forma en que hoy se utiliza en proyectos reales (2025-2026),




Tecnologías recomendadas actualmente

Para un proyecto moderno:

**Base**

* React 19
* Vite
* React Router
* Axios

**Manejo de datos**

* TanStack Query (antes React Query)

**Formularios**

* React Hook Form

**Estilos**

* Bootstrap (si vienes de Django y quieres avanzar rápido)
* o Tailwind CSS (muy demandado actualmente)

**Calidad**

* ESLint
* Prettier


### Como ya tienes el endpoint DRF funcionando, hagamos el ejemplo más simple posible primero y después lo iremos profesionalizando.

1. Crear el proyecto React con Vite

    # Desde la raíz del repositorio:
    cd django-drf-demo
    npm create vite@latest frontend -- --template react
    # Entrar al proyecto:
    cd frontend
    # Instalar dependencias:
    npm install
    npm install axios

    # Iniciar React:
    npm run dev

    # Deberías ver algo como:
    Local: http://localhost:5173/











y podrás concentrarte en:

    Fetch
    Axios
    JSON
    CORS
    Hooks (useState, useEffect)

sin complicarte con varios repositorios.


