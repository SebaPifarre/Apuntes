### ¿Qué protocolos se utilizan para el envío de mails entre el cliente y su servidor de correo? ¿Y entre servidores de correo?

Para ambos se utiliza el protocolo SMTP
* Simple Mail Transport Protocol
* Protocolo Cliente - Servidor
* Utiliza formato ASCII 7 bits en 8 NVT
* Usa TCP puerto servidor: 25
* Los LMTA (Local Mail Transport Agent) de los MUA (Mail User Agent) hablan SMTP con su servidor SMTP saliente.

### ¿Qué protocolos se utilizan para la recepción de mails? Enumere y explique características y diferencias entre las alternativas posibles.

* <mark style="background: #D2B3FFA6;">POP3 (Post Office Protocol, RFC-1939: POP v3):</mark>

Extremadamente simple protocolo de acceso al mail. Al ser tan simple, su funcionalidad está limitada. POP3 empieza con el user agent abriendo una conexión TCP con el servidor de mail en el puerto 110. Con la conexión establecida, POP3 progresa por tres fases: autorización, transacción y actualización. Durante la primera fase, el user agent envía un usuario y contraseña para autenticar al usuario. Durante la segunda fase, el user agent recupera sus mensajes y puede marcarlos para borrarlos y obtener estadísticas. La tercera fase ocurre cuando el cliente usa el comando quit, terminando la sesión POP3, en este momento el servidor de mail elimina los mensajes que fueron marcados.
Se puede confi