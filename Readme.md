# Proyecto Django DRF + React Axios Vite

**Objetivo**: Construiruna aplicación **Django** +** React moderna** (Aprender React de la forma en que hoy se utiliza en proyectos reales **(2025-2026)**, evitando patrones antiguos que todavía aparecen en muchos tutoriales.)


**Ejercicio Final:**

    DRF → listar usuarios tabla user
    React + Axios → consumir usuarios.
    Crear usuario desde React usando DRF.
    Autenticación con JWT usando Simple JWT.
    Dockerizar frontend y backend.



Django = API REST
React = interfaz de usuario
Comunicación = HTTP (JSON)


### Backend

    cd /Users/desarrollador/development/2026/django/django-drf-demo
    source venv/bin/activate
    which python 

### frontend

    cd /Users/desarrollador/development/2026/django/django-drf-demo/frontend
    npm run dev


## Proyecto en un solo repositorio github


Simula una arquitectura real, muchos proyectos usan:

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


## FrontEnd

Para un proyecto moderno con React, tecnologías recomendadas actualmente:

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


### React

1. Crear el proyecto React con Vite

	    cd django-drf-demo
	    npm create vite@latest frontend -- --template react
	    cd frontend
	    npm install
	    npm install axios
	    npm run dev
	    Local: http://localhost:5173/

Este flujo es el corazón de React:

	    React
	    ↓
	    useEffect()
	    ↓
	    Axios GET
	    ↓
	    Django DRF
	    ↓
	    JSON
	    ↓
	    setUsers()
	    ↓
	    React vuelve a renderizar

* El navegador nunca recarga la página; 
* React actualiza el DOM automáticamente cuando cambia el estado (users).


### Flujo REACT: Entendiendo los pasos

~~~javascript
import { useEffect, useState } from "react";
import axios from "axios";

function App() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    axios
      .get("http://127.0.0.1:8000/api/users/")
      .then((response) => {
        setUsers(response.data);
      });
  }, []);

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.username}</li>
      ))}
    </ul>
  );
}
~~~

#### 1. useState(): React crea una variable de estado.

    const [users, setUsers] = useState([]);

Puedes leerlo como:

    users     = valor actual
    setUsers  = función para cambiar ese valor
    []        = valor inicial

Al comenzar: **users = []** Es un arreglo vacío.


#### 2. useEffect()

    useEffect(() => {
    ...
    }, []);

useEffect permite ejecutar código cuando ocurre algo en el ciclo de vida del componente.

    Con: []

React interpreta:

* Ejecuta esto una sola vez
* cuando la página se carga.

Es parecido a:

    # pseudocódigo

    al_iniciar_la_pagina():
        consultar_api()


#### 3. Axios GET

    axios.get("http://127.0.0.1:8000/api/users/")

Axios es una librería para hacer peticiones HTTP.

Equivale a:

    Navegador
    |
    | GET
    v
    /api/users/

La petición sale desde React hacia Django.


#### 4. Django DRF: ViewSet recibe la petición.

    class UserViewSet(viewsets.ReadOnlyModelViewSet):
        queryset = User.objects.all()
        serializer_class = UserSerializer

Internamente sucede:

    React
    |
    | GET /api/users/
    |
    Django
    |
    | consulta BD
    |
    SQLite

Supongamos que existen:

    admin
    juan
    maria


#### 5. Serializador: DRF transforma objetos **Python a JSON**.

Antes:

    [
        User(id=1, username='admin'),
        User(id=2, username='juan')
    ]

Después:

    [
	    {
	        "id": 1,
	        "username": "admin"
	    },
	    {
	        "id": 2,
	        "username": "juan"
	    }
    ]

Este JSON viaja de vuelta al navegador.

#### 6. response

Axios recibe la respuesta.

    .then((response) => {

response es un objeto grande.

Algo parecido a:

    {
    data: [
        {
	        id: 1,
	        username: "admin"
        },
        {
	        id: 2,
	        username: "juan"
        }
    ],
    status: 200,
    headers: {...}
    }

#### 7. response.data

Contiene únicamente el JSON.

Resultado:

    [
        {
            id: 1,
            username: "admin"
        },
        {
            id: 2,
            username: "juan"
        }
    ]

#### 8. setUsers()

    setUsers(response.data);

Aquí ocurre lo más importante.

Tomamos el JSON recibido y lo guardamos en el estado de React.

Antes:

    users = []

Después:

    users = [
    {
        id: 1,
        username: "admin"
    },
    {
        id: 2,
        username: "juan"
    }
    ]


Por eso una buena definición sería:

> **setUsers()** actualiza el estado users con los datos JSON obtenidos desde la API. Cuando se ejecuta, React detecta el cambio y vuelve a renderizar la interfaz.

#### 9. React detecta el cambio

Cuando haces:

    setUsers(...)

React piensa:

    El estado cambió.
    Debo volver a ejecutar App().

No recarga la página.

Simplemente vuelve a ejecutar:

    function App() {
    ...
    }

con el nuevo valor de users.

#### 10. users.map()

Ahora:

    users.map((user) => (
    <li key={user.id}>
        {user.username}
    </li>
    ))

users ya contiene:

    [
        {
            id: 1,
            username: "admin"
        },
        {
            id: 2,
            username: "juan"
        }
    ]

Entonces React recorre cada elemento.

Primera vuelta:

    user = {
    id: 1,
    username: "admin"
    }

Genera:

    <li>admin</li>


y asi sucesivamente.

#### Flujo Completo

      1. React carga App()

      users = []

      2. useEffect se ejecuta

      3. axios.get()
            |
            v
      4. Django DRF

      5. Django consulta SQLite

      6. DRF genera JSON

      [
        {id:1, username:"admin"},
        {id:2, username:"juan"}
      ]

      7. Axios recibe respuesta

      response.data

      8. setUsers(response.data)

      users =
      [
        {id:1, username:"admin"},
        {id:2, username:"juan"}
      ]

      9. React detecta cambio

      10. React vuelve a ejecutar App()

      11. users.map()

      12. Se muestran los usuarios en pantalla


Si vienes de Django, una comparación útil es:

    usuarios = User.objects.all()
    return render(request, "usuarios.html", {
        "usuarios": usuarios
    })

En React sería conceptualmente:

    const usuarios = response.data;
    setUsers(usuarios);

> La diferencia es que en Django el render ocurre en el servidor, mientras que en React el render ocurre en el navegador del usuario.


### FUTURO


7. Primer objetivo de aprendizaje

Antes de agregar Router, TanStack Query o JWT, te sugiero verificar que entiendes estos tres conceptos:

    useState()
    useEffect()
    axios.get()

Porque prácticamente cualquier aplicación React que consuma una API se construye sobre esos fundamentos.

Si te funciona, el siguiente paso natural sería refactorizar este ejemplo para crear:

    src/
    ├── api/
    ├── services/
    ├── pages/
    └── components/
    
    


y podrás concentrarte en:

    Fetch
    Axios
    JSON
    CORS
    Hooks (useState, useEffect)

sin complicarte con varios repositorios.

