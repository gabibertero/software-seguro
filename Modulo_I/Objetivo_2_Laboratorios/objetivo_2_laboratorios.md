# Turnero:



1. Nos logueamos con el user y password que nos da el enunciado
2. Abro el devtools y voy a la pestaña network y filtro por peticiones XHR
3. Cancelo uno de los turnos para ver que nuevas peticiones genera
4. Una de las peticiones GET es "api/1/appointments/" donde asumo que el 1 es un userid
5. Pruebo con distintos números id
6. El id 49, me dio que el user es analia, no es el que busco asique sigo probando
7. El id 101 me dio el user que busco, xdalvik
8. En el paso 3 hice un delete "api/appointments/1" donde asumo que el 1 es el id del turno
9. Realizo los delete para los turnos de xdalvik remplazando el id del turno "api/appointments/$id$" por los id de los turnos del usuario que son: 10,11,12,13
10. Recargo la page y me da el MD5 del lab d27fa3f8fc14ea101603d09436e28bf6


Presupuesto:
===



1. Nos logueamos con el user y password que nos da el enunciado
2. Abro el devtools y voy a la pestaña network y filtro por peticiones XHR
3. La petición GET a "/api/gastos" en la response nos da todo el JSON
4. Toco en el botón revisar para ver que sucede, me da un POST a "/api/gastos/1/editar/" donde asumo que el 1 es el id del gasto
5. Reenvio diversas peticiones con POST con el siguiente formato {"monto": "1500.00", "revisado": true} cambiando siempre el id del gasto y haciendo cuentas para que me den los resultados que pide el enunciado

PRESUPUESTO - MONTOS A CARGAR


ID  | Titulo                            | Categoria   | Monto nuevo | revisado

\----|-----------------------------------|-------------|-------------|---------

1   | Flete mercaderia                  | transporte  | 1500.00     | true

2   | Alquiler local                    | esenciales  | 10000.00    | true

3   | Luz                               | esenciales  | 5000.00     | true

4   | Agua                              | alimentos   | 500.00      | true

5   | Internet                          | esenciales  | 500.00      | true   (minimo)

6   | Nomina empleados                  | esenciales  | 50000.00    | true   (maximo)

7   | Marketing                         | varios      | 6000.00     | true

8   | Publicidad                        | varios      | 6000.00     | true

9   | Reuniones                         | varios      | 6000.00     | true

10  | Reparacion de equipo informatico  | varios      | 6000.00     | true

11  | Seguros                           | impuestos   | 500.00      | true

12  | Licencias de software             | varios      | 6000.00     | true

13  | Formacion de empleados            | varios      | 6000.00     | true



VERIFICACION:

\- Esenciales (2,3,5,6): 10000+5000+500+50000 = 65500 / 4 = 16375  OK

\- Varios (7,8,9,10,12,13): 6000 x 6 = 36000 / 6 = 6000  OK

\- Total: 104000 / 13 = 8000  OK

\- Minimo = 500 (id 4, 5, 11)  |  Maximo = 50000 (id 6)  OK



6\. Una vez cargada todas las nuevas peticiones recargo la page y me da el MD5  bf58371373e52613ae270d5acf832bad


# 

# Gran rifa 2019



1. Nos logueamos con el user y password que da el enunciado
2. Abro el devtools, voy a la pestaña network y filtro por peticiones XHR
3. Toco el botón editar para sacar mas información
4. Me dio un petición POST a "api/numeros/2/editar/" donde asumo que el 2 es el user id, la response me dio estado:"OK"
5. Envio una petición POST a "api/numeros/4/editar/" donde el userid de Jhon Backus es 4, con el cuerpo de "{"esta\_pago": true}"
6. Recargo la page y me da el MD5 ed20b8f11252a75b30d594af897c3aad





# Ventas:

1. Al entrar en la page del lab ponemos la ruta de /ventas
2. Vamos a buscar cada venta por su id y nos damos cuenta que algunas existen poniendo /ventas/?id=2 por ejemplo
3. Ejecutamos un Intruder Attack por medio de burp que sea tipo Number que va desde el id 0 al 3000
4. Seleccionamos todos los resultados y nos da que hay 1641 ventas
5. Al pasar el 1641 a MD5 nos da el code del lab 10c272d06794d3e5785d5e7c5356e9ff

