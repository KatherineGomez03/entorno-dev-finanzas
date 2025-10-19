# Documentación de la API

## URL Base
```
http://localhost:3000
```

## Autenticación
Todos los endpoints protegidos requieren un token Bearer en el encabezado de Autorización:
```
Authorization: Bearer <tu_token_jwt>
```

## Endpoints

### Módulo de Autenticación

#### 1. Iniciar Sesión
- **URL**: `/auth/login`
- **Método**: `POST`
- **Autenticación Requerida**: No
- **Cuerpo**:
  ```json
  {
    "email": "usuario@ejemplo.com",
    "password": "contraseña123"
  }
  ```
- **Respuesta Exitosa**:
  - **Código**: 200
  - **Contenido**:
    ```json
    {
      "statusCode": 200,
      "message": "Inicio de sesión exitoso",
      "access_token": "token_jwt_aqui",
      "user": {
        "id": "id_usuario",
        "username": "nombre_usuario",
        "email": "usuario@ejemplo.com",
        "level": 1,
        "health": 100,
        "maxHealth": 100,
        "experience": 0,
        "maxExperience": 100,
        "attack": 10,
        "defense": 5,
        "coins": 0
      }
    }
    ```
- **Respuesta de Error**:
  - **Código**: 401
  - **Contenido**: `{ "message": "Credenciales inválidas." }`

#### 2. Registro
- **URL**: `/auth/register`
- **Método**: `POST`
- **Autenticación Requerida**: No
- **Cuerpo**:
  ```json
  {
    "username": "jugador123",
    "email": "usuario@ejemplo.com",
    "password": "MiC0ntr@seña123",
    "firstName": "Juan",
    "lastName": "Pérez"
  }
  ```
- **Requisitos de Contraseña**:
  - Mínimo 8 caracteres
  - Al menos una letra mayúscula
  - Al menos una letra minúscula
  - Al menos un número
  - Al menos un carácter especial (@$!%*?&)
- **Respuesta Exitosa**:
  - **Código**: 201
  - **Contenido**: Objeto usuario con token
- **Respuesta de Error**:
  - **Código**: 409
  - **Contenido**: `{ "message": "Ya existe un usuario con este correo." }`

### Módulo de Usuarios (Rutas Protegidas)

Todos los endpoints en este módulo requieren autenticación (token Bearer).

#### 1. Crear Usuario
- **URL**: `/users`
- **Método**: `POST`
- **Autenticación Requerida**: Sí
- **Cuerpo**: Igual que el endpoint de Registro
- **Respuesta Exitosa**:
  - **Código**: 201
  - **Contenido**: Objeto usuario creado
- **Respuesta de Error**:
  - **Código**: 409
  - **Contenido**: `{ "message": "Ya existe un usuario con este correo o nombre de usuario." }`

#### 2. Obtener Todos los Usuarios
- **URL**: `/users`
- **Método**: `GET`
- **Autenticación Requerida**: Sí
- **Respuesta Exitosa**:
  - **Código**: 200
  - **Contenido**: Array de objetos usuario

#### 3. Obtener Usuario por ID
- **URL**: `/users/:id`
- **Método**: `GET`
- **Autenticación Requerida**: Sí
- **Parámetros URL**: `id=[string]`
- **Respuesta Exitosa**:
  - **Código**: 200
  - **Contenido**: Objeto usuario
- **Respuesta de Error**:
  - **Código**: 404
  - **Contenido**: `{ "message": "Usuario no encontrado." }`

#### 4. Actualizar Usuario
- **URL**: `/users/:id`
- **Método**: `PATCH`
- **Autenticación Requerida**: Sí
- **Parámetros URL**: `id=[string]`
- **Cuerpo**: Campos disponibles para actualizar
  ```json
  {
    "health": 100,           // (0-100)
    "maxHealth": 100,        // (>0)
    "experience": 50,        // (≥0)
    "maxExperience": 100,    // (>0)
    "level": 1,             // (≥1)
    "attack": 10,           // (≥0)
    "defense": 5,           // (≥0)
    "coins": 100            // (≥0)
  }
  ```
- **Notas**: 
  - Todos los campos son opcionales
  - Los valores deben estar dentro de los rangos especificados
  - No se pueden actualizar campos de autenticación (email, password)
- **Respuesta Exitosa**:
  - **Código**: 200
  - **Contenido**: Objeto usuario actualizado

#### 5. Eliminar Usuario
- **URL**: `/users/:id`
- **Método**: `DELETE`
- **Autenticación Requerida**: Sí
- **Parámetros URL**: `id=[string]`
- **Respuesta Exitosa**:
  - **Código**: 200
  - **Contenido**: `{ "message": "Usuario eliminado exitosamente" }`

## Configuración del Entorno

La API requiere las siguientes variables de entorno:

```env
# MongoDB
MONGO_USER=usuario
MONGO_PASSWORD=contraseña
MONGO_DB=finance_db

# Backend
NODE_ENV=development
BACKEND_PORT=3000
```

## Configuración de Docker

La API está containerizada y puede ejecutarse usando Docker Compose. Los servicios son:

1. MongoDB (Puerto: 27017)
2. Backend (NestJS) (Puerto: 3000)
3. Frontend (NextJS) (Puerto: 3001)

Para iniciar los servicios:
```bash
docker-compose up
```

## Notas para el Desarrollo Frontend

1. La URL base para las peticiones API en el frontend debe configurarse como:
   ```
   NEXT_PUBLIC_API_URL=http://app_backend:3000
   ```

2. Todas las peticiones autenticadas deben incluir el token JWT en el encabezado de Autorización:
   ```typescript
   const headers = {
     'Authorization': `Bearer ${token}`,
     'Content-Type': 'application/json'
   };
   ```

3. Manejar los códigos de estado HTTP apropiadamente:
   - 200: Éxito
   - 201: Creado
   - 400: Solicitud Incorrecta
   - 401: No Autorizado
   - 403: Prohibido
   - 404: No Encontrado
   - 409: Conflicto
   - 500: Error Interno del Servidor