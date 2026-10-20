
## Ejercicio 1

Crea un usuario para las bases de datos usando el nombre ‘appt_user’. Asigne a
estos todos los permisos sobre sus respectivas tablas. Habiendo creado este
usuario evitaremos el uso de ‘root’ para el resto del trabajo práctico.
Adicionalmente, con respecto a esta base de datos:
a. Cree un usuario sólo con permisos para realizar consultas de selección, es
decir que no puedan realizar cambios en la base (ni estructural ni de datos).
Use el nombre ‘appt_select’.
b. Cree un usuario que pueda realizar consultas de selección, inserción,
actualización y eliminación a nivel de filas, pero que no puedan modificar el
esquema. Use el nombre ‘appt_update’.
c. Cree un usuario que tenga los permisos de los anteriores, pero que además
pueda modificar el esquema de la base de datos. Use el nombre
'appt_schema’.

```sql
ALTER USER 'appt_user'@'%'   IDENTIFIED BY 'appt_user';
ALTER USER 'appt_select'@'%' IDENTIFIED BY 'appt_select';
ALTER USER 'appt_update'@'%' IDENTIFIED BY 'appt_update';
ALTER USER 'appt_schema'@'%' IDENTIFIED BY 'appt_schema';
```

```sql
GRANT ALL PRIVILEGES ON mi_base.* TO 'appt_user'@'%';
GRANT SELECT ON mi_base.* TO 'appt_select'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON mi_base.* TO 'appt_update'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, DROP, INDEX, REFERENCES, CREATE VIEW
  ON mi_base.* TO 'appt_schema'@'%';
```

## Ejercicio 2

Hallar aquellos pacientes que para todas sus consultas médicas siempre hayan
dejado su número de teléfono primario (nunca el teléfono secundario).

```sql
SELECT * FROM patient p
WHERE p.primary_phone
= all
(
	Select a.contact_phone from appointment a
	where p.patient_id = a.patient_id
)
and exists (select 1 from appointment a 
	where a.patient_id = a.patient_id)
```

se agrega el and exists porque si la subconsulta no devuelve filas, all da verdadero.

## Ejercicio 3

Crear una vista llamada ‘doctors_per_patients’ que muestre los id de los pacientes y
los id de doctores de la ciudad donde vive el paciente.

```sql
create view mi_base.doctors_per_patients
as (
	select p.patient_id, d.doctor_id
	from patient p, doctor d
	where p.patient_city = d.doctor_city
	)
```

## Ejercicio 4

Utiliza la vista generada en el ejercicio anterior para resolver las siguientes
consultas:
a. Obtener la cantidad de doctores por cada paciente que tiene disponible en
su ciudad
b. Obtener los nombres de los pacientes sin doctores en su ciudad
c. Obtener los doctores que comparten ciudad con más de cinco pacientes.

```

```