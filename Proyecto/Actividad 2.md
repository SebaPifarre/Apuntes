
1)
a) Está función responde a la ruta del bp + /perfil/editar aceptando el método http post.
Recibe de la http request el email y el nombre los cuales los guarda en varibales locales.
Busca de la sesión el user_id que guarda en una variable y lo utiliza para hacer una consulta a la base de datos. Modifica el email y el nombre del objeto obtenido en de la base de datos y realiza un db commit para que se efectuen los cambios. Por último retorna la página perfil.html, el usuario modificado y un mensaje de éxito
b) Los dos problemas tienen que ver con
* Asume que hay una sesión activa con un user_id válido. No verifica si el usuario sigue en la base de datos ni verifica si no existe una sesión. Tampoco verifica si el usuario tiene los permisos necesarios para realizar esta operación. Representa un fallo porque si el usuario fue eliminado de db o no tiene permisos o no tiene una sesión activa, se realiza la busqueda sobre la bd de todas maneras. Si cualquiera es verdadera la consulta va a fallar y no se está haciendo un manejo adecuado. La solución es hacer uso de escepciones, verificar los datos correctamente y si surge algún inconveniente, levantar una excepción para no seguir con el flujo nomral de ejecución.
c) Si el usuario ingresa un email que ya existe y el campo email tiene la restricción de UNIQUE va a ocurrir un error de integridad , la sesión de la base de datos quedará inhabilitada hasta que ocurra un rollback(). Como en este caso no se maneja ese problema en particular, la aplicación va a devolver un error y no va a permitir que siga su ejecución.

a) Está función es una ruta de acceso a la aplicación en bp + /admin/eliminar_usuario/<ind:user_id> que acepta el método post.
