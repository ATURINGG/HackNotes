# 🆔 ¿Qué carajos es BOLA y por qué OWASP la considera la mayor amenaza para las APIs?
> Cuando estar autenticado no significa estar autorizado

Muchas personas al entregar los requerimientos de seguridad para el desarrollo de APIs se centran mas en la autenticacion que en la autorizacion, por eso desde que tenga JWT u OUATH 2.0 implementado ya estamos cumplimiento y protegiendo nuestra API, pero la realidad es otra. ¿Que sucede si el usuario que se autentica a tus servicios pregunta por los datos de otro usuario?.
Eso es BOLA (Broken Object Level Autorization) es una amenaza que permite a los atacantes realizas peticiones no autorizadas dentro de una misma sesion.

inserta imagen de usuario a consulta datos de usuario a, usuario b y usuario c.

Normalmente esta vulnerabilidad se da por falta de autorizacion en las peticiones, esto quiere decir que si el usuario está autenticado no necesariamente tiene permitido ver la informacion de otros usuarios, pero remitamonos un poco a los conceptos, que es autenticación y que es autorización. **La autenticación** es el proceso de verificación que tu si eres tu, en este proceso es donde verificamos tu usuario, tu contraseña y tu multiplefactor a la hora de acceder a un sistema. Aqui, se usa una estrategia de identificación con 3 componentes, algo que tienes, algo que eres y algo que sabes. con estos 3 componentes los sistemas se pueden asegurar que en realidad una autenticacion en un sistema es legitima, si lo trasladamos al mundo de las APIs podria ser una autenticacion por medio de JWT que me permite por medio de usuario y clave generar un token de acceso a algun servicio. hasta este punto no sabemos si este usuario que es legitimo tiene acceso a toda la informacion del sistema o solo a parte de la informacion, aqui es donde econtramos en concepto de **autorizacion** este proceso lo que nos permite es definir y restringir las acciones y los datos a visualizar en el sistema que te acabas de autenticar, por ejemplo. un usuario de entregas de paquetes, solo tendrá permitido ver los pedidos asignados y la ruta para entregar los pedidos, pero en cambio, el administrador de paquetes podrá acceder a todos los paquetes que está en bodega, despachados y entregados. permitiendole asignar pedidos a los repoartidores disponibles en el sistema. eso es tener una autorización definida en un sistema. como defino y aplico restricciones a los datos a suministrar en mi sistema.

insertar imagen donde se va una consulta autenticada con jwt y que despues se vea un json que sea vulnerable a BOLA con el ejemplo de los pedidos del parrafo anterior

### Como mitigarlo?
- usar los scopes cuando se haga oauth 2.0
- realizar modelados de roles y perfiles a las apis a desplegar
- limitar todos los endpoints de las apis con su respectiva autorizacion






¿Qué significa BOLA?
¿Por qué es una de las vulnerabilidades más críticas en APIs?
¿Cuál es la diferencia entre autenticación y autorización?
¿Qué es un “objeto” en una API?
¿Cómo un atacante puede acceder a información de otro usuario cambiando un ID?
¿Qué impacto puede tener en datos personales, cuentas, transacciones o recursos internos?
¿Cómo se puede mitigar desde diseño, desarrollo y pruebas?
