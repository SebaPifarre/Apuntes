
## Ejercicio 1

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