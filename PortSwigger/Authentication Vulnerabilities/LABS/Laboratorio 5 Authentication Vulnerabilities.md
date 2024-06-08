
## Objetivo:

![[Pasted image 20240602173300.png]]

## Solución:

Enviamos la peticion de Login al Intruder

![[Pasted image 20240602173538.png]]

Crearemos una lista de "usernames" de la siguiente manera... Osea escribir 3 veces (asumiendo que el bloqueo se produce a los 4 intentos fallidos) cada usuario. Para esto tambien hice un script para facilitarnos esta tarea.
![[Pasted image 20240602180014.png]]

Ahora metemos como variables la password y el username y configuramos el ataque a Tipo "PitchFork"

![[Pasted image 20240602180324.png]]


En la lista numero #1 metemos la lista de usuarios y en la #2 las contraseñas. En este caso las contraseñas son aleatorias que las agregue yo mismo, nada mas porque de momento NO son relevantes, simplemente quiero hacer enumeracion de usuarios, por eso puse contraseñas sin sentido...

![[Pasted image 20240602180428.png]]

Le metemos un Grep en "Invalid username or password."

![[Pasted image 20240602180623.png]]


Y Comenzar Ataque
![[Pasted image 20240602183137.png]]

Una vez que encontramos un "usuario bloqueado" entonces ya sabemos que dicho usuario existe... Ahora aplicamos fuerza bruta a la contraseña y seleccionamos el ataque de tipo sniper
![[Pasted image 20240602183423.png]]

Y metemos los payloads con la lista de posibles contraseñas
![[Pasted image 20240602183457.png]]

Y le metemos un Grep a "Incorrect username or password"

![[Pasted image 20240602183556.png]]

Y Comenzar Ataque

![[Pasted image 20240602184620.png]]


Esperemos al menos 1 minuto, porque fijo estamos bloqueados por tantos intentos del intruder

![[Pasted image 20240602184821.png]]

![[Pasted image 20240602184838.png]]



### Scrips para listar usuarios
![[Pasted image 20240602184934.png]]

```
archivo="users.txt"

  

if [ ! -f "$archivo" ]; then

echo "El archivo $archivo no existe"

exit 1

fi

  
  

while IFS= read -r linea; do

echo "$linea" >> usersOk.txt

echo "$linea" >> usersOk.txt

echo "$linea" >> usersOk.txt

echo "$linea" >> usersOk.txt

done < "$archivo"

  
  

echo "Finalizado con exito!"
```

