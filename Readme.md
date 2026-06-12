## Ventajas de un solo repositorio

Más fácil de aprender

Tendrás:

DRF en http://localhost:8000
React en http://localhost:5173

y podrás concentrarte en:

    Fetch
    Axios
    JSON
    CORS
    Hooks (useState, useEffect)

sin complicarte con varios repositorios.x

Simula una arquitectura real

Muchos proyectos usan:

    Repositorio
    ├── backend/
    └── frontend/

aunque luego se desplieguen por separado.



    django-drf-demo/
    ├── backend/
    │   ├── manage.py
    │   ├── config/
    │   ├── users/
    │   ├── db.sqlite3
    │   └── requirements.txt
    │
    └── frontend/
        ├── package.json
        ├── vite.config.js
        ├── src/
        └── node_modules/




http://localhost:8000/api/users/