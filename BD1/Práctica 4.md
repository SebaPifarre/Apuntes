
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

a)

```mysql
select dpp.patient_id, count(*)
from doctors_per_patients dpp
group by patient_id
```

b)

```mysql
select p.patient_name
from patient p
where p.patient_id not in (select patient_id from doctors_per_patients)
```

c)

```mysql
select *
from doctor d
where d.doctor_id IN
(
	select dpp.doctor_id from doctors_per_patients dpp
	group by dpp.doctor_id
	having count(*) > 5
)
```

## Ejercicio 5

Escribe y ejecute la sentencia correspondiente para crear la siguiente tabla:
APPOINTMENTS_PER_PATIENT
idApP: int(11) PK AI
patient_id: int(11)
count_appointments: int(11)
last_update: datetime
user: varchar(16)

```mysql
create table mi_base.appointments_per_patient
(
	idApP integer(11) auto_increment primary key,
	patient_id integer(11) not null,
	constraint patient_id FOREIGN key (patient_id)
	references patient(patient_id),
	count_appointments integer(11),
	last_update DATETIME,
	user varchar(16)
)
```

## Ejercicio 6

Crear un Stored Procedure que realice los siguientes pasos dentro de una
transacción:
a. Realizar la siguiente consulta: cada pacient (identificado por patient_id),
calcule la cantidad de appointments que tiene registradas. Registrar la fecha
en la que se realiza esta carga y además del usuario con el se realiza.
b. Guardar el resultado de la consulta en un cursor.
c. Iterar el cursor e insertar los valores correspondientes en la tabla
APPOINTMENTS_PER_PATIENT. Tenga en cuenta que last_update es la
fecha en que se realiza esta carga, es decir la fecha actual, mientras que
user es el usuario logueado actualmente, utilizar las correspondientes
funciones para esto.
Para el campo user, guardar únicamente el nombre de usuario, sin el host.

```mysql

```