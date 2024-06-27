## Objetivo:

![[Pasted image 20240622160653.png]]

## Solución:

Entramos a una categoria

![[Pasted image 20240622160818.png]]

Suponiendo que esta de esta manera internamente
```
SELECT * FROM categories FROM category 'prueba';
```

Volveremos a hacer lo mismo **ESTO NO MOSTRARE FOTOS NI REPETIRE ESTOS PASOS DE AHORA EN ADELANTE POSIBLEMENTE**:
1. Identificar la cantidad de columnas.
2. Ver tipo de dato de cada columna.
3. Una vez identificado, crear el payload correspondiente a la explotación que queremos realizar.

En este caso esta en la segunda columna

![[Pasted image 20240622161221.png]]

Una vez identificado en que columna se muestra el tipo de dato, inyectaremos el Payload... Pero antes para saber ante que tipo de Base de Datos estamos podemos mostrar la version de esta misma en la segunda columna y así de esta manera podriamos identificar que tipo de syntaxis usaremos para hacer dicha concadenacion de usuario y contraseña para mostrarlos ambos juntos en una misma columna (columna #2)..

![[Pasted image 20240622161440.png]]

Vemos que estamos ante un Postgres, en todo casi si hubieramos puesto @@version u otras formas de saber la version capaz y nos devolvera "Internal Server Error" ya que no reconocería dicho comando...

Entonces Ahora meteremos el payload para recuperar lo datos de los usuarios basando la syntaxis de concadenacion en Postgres:

Usare este payload.
```
' UNION SELECT NULL, username||' >> '||password FROM users--
```

Y por detras la consulta quedaría asi:
```
SELECT * FROM categories FROM category '' UNION SELECT NULL, username||' >> '||password FROM users--';
```

![[Pasted image 20240622161759.png]]

Ahora iniciamos sesion con el usuario administrador

![[Pasted image 20240622161825.png]]

