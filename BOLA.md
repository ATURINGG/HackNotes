# 🆔 ¿Qué carajos es BOLA y por qué OWASP la considera la mayor amenaza para las APIs?
> Cuando estar autenticado no significa estar autorizado

Una vulnerabilidad BOLA ocurre por una falla en la autorización de las peticiones. Esto significa que un usuario puede estar correctamente autenticado en una aplicación, pero eso no quiere decir que tenga permiso para consultar o modificar información que pertenece a otros usuarios.Para entenderlo mejor, primero debemos diferenciar dos conceptos clave: **autenticación y autorización.**

**La autenticación** responde a la pregunta: ¿quién eres? Es el proceso mediante el cual un sistema verifica la identidad de un usuario. Por ejemplo, cuando una persona ingresa su usuario, contraseña y un segundo factor de autenticación, el sistema valida que realmente sea quien dice ser. Esta verificación puede apoyarse en tres tipos de factores: algo que sabes, como una contraseña; algo que tienes, como un código 2FA o un token; y algo que eres, como una huella o reconocimiento facial.

En el mundo de las APIs, un ejemplo común de autenticación es el uso de JWT. El usuario inicia sesión con sus credenciales y, si son correctas, la aplicación genera un token de acceso. A partir de ese momento, cada petición enviada a la API puede incluir ese token para demostrar que el usuario está autenticado.Sin embargo, estar autenticado no significa tener acceso a todos los datos del sistema. Aquí entra el concepto de **autorización**, que responde a la pregunta: ¿qué puedes ver o hacer?
La autorización define las acciones permitidas y los datos a los que un usuario puede acceder según su rol, permisos o relación con el recurso solicitado.Por ejemplo, en una aplicación de pedidos llamada deliveryhack, un repartidor autenticado debería poder consultar únicamente los pedidos que tiene asignados y la ruta para entregarlos. En cambio, un administrador podría tener permisos más amplios, como ver pedidos en bodega, pedidos despachados, pedidos entregados y asignar entregas a los repartidores disponibles.

![bola](assets/bola.png)

El problema aparece cuando la API solo valida que el usuario tenga un JWT válido, pero no verifica si ese usuario tiene permiso sobre el pedido consultado. Por ejemplo, si el repartidor con ID 123 consulta el endpoint:
```http
GET /api/pedidos/456
Authorization: Bearer <jwt_valido>
```
y la API responde con información de un pedido asociado a otro usuario, entonces estamos frente a una vulnerabilidad BOLA. En este caso, la autenticación funcionó correctamente, porque el usuario tenía un token válido, pero la autorización falló, porque el sistema no comprobó si ese pedido realmente le pertenecía o estaba asignado a él.

### En resumen
BOLA no ocurre porque el usuario no esté autenticado, sino porque la API no valida correctamente si ese usuario está autorizado para acceder al objeto solicitado. Por eso, cada petición a recursos como pedidos, cuentas, facturas, perfiles o documentos debe validar no solo la identidad del usuario, sino también su permiso específico sobre el recurso consultado.



## Mitigación  de BOLA en APIs: diseño, desarrollo y pruebas

Mitigar **BOLA** no se logra “poniendo JWT” ni “activando OAuth 2.0”. Eso solo resuelve parte de la **autenticación**. La solución de raíz es implementar un modelo fuerte de **autorización a nivel de objeto**, donde cada petición valide si el usuario tiene permiso específico sobre el recurso que está intentando consultar, modificar o eliminar.

---

## 1. Desde el diseño: definir el modelo de autorización antes de construir la API

Antes de desarrollar los endpoints, se debe definir claramente **quién puede acceder a qué recurso, bajo qué condición y para qué acción**.

No basta con decir:

> “El usuario está autenticado.”

La pregunta correcta es:

> “¿Este usuario puede ejecutar esta acción sobre este objeto específico?”

Por ejemplo, en una app como **deliveryhack**:

| Rol | Recurso | Acción permitida |
|---|---|---|
| Repartidor | Pedido asignado | Consultar ruta y estado |
| Repartidor | Pedido de otro repartidor | Denegado |
| Cliente | Pedido propio | Consultar estado |
| Cliente | Pedido de otro cliente | Denegado |
| Administrador | Todos los pedidos | Consultar, asignar y gestionar |

Desde diseño se deben definir controles como:

- **RBAC**: autorización basada en roles.
- **ABAC**: autorización basada en atributos.
- **ReBAC**: autorización basada en relaciones entre usuario y recurso.
- **Scopes OAuth 2.0**: permisos funcionales sobre acciones.
- **Claims en JWT**: atributos del usuario, rol, tenant, permisos o contexto.
- **CIAM/IAM**: plataforma centralizada para identidad, autenticación y gobierno de acceso.

Ejemplo de scopes:

```text
pedidos:read
pedidos:update
pedidos:assign
pedidos:admin
```
