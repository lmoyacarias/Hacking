``
## Objetivo:

![[Pasted image 20240602150131.png]]

## Solucion:

Vemos que si intento meter el usuario "carlos" con una contraseña random, este me devolvera "Incorrect password".

![[Pasted image 20240602150425.png]]


Despues de 3 intentos, vemos que me aparece que estoy bloqueado por 1 minuto por intentar muchas veces iniciar sesion

![[Pasted image 20240602150559.png]]

Aunque ingrese con un usuario con credenciales correctas vemos que sigo bloqueado
![[Pasted image 20240602150657.png]]

Entonces tocara esperar un minuto y haremos una prueba ingresando 2 veces el usuario carlos para una vez el usuario wiener (del cual si tenemos la contraseña) y luego asi sucesivamente... Y notaremos que al ingresar las credenciales correctas de un usuario, se resetean los intentos de bloqueo, entonces ya con esto podemos crear nuestro diccionario de la siguiente manera. 

Tenemos que intercalar cada 2 intentos fallidos un usuario con credenciales correctas... Y tenemos que asegurarnos de que el orden de las contraseñas coincidan con el orden de usuarios de manera ***usuarioCorrecto:contraseñaCorrecta y usarioAProbar:posibleContraseña***
Esto se puede hacer manualmente con un editor de texto, pero de igual manera dejare un script que hice para automatizar esta lista de contraseña de manera intercalada.

![[Pasted image 20240602152942.png]]

Enviamos la peticion de Login al Intruder y metemos las variables tanto en el usuario como contraseña y seleccionamos el ataque de Tipo "PitchFork"


![[Pasted image 20240602153152.png]]

En la Lista de Payload #1, metemos la lista de usuarios y en la #2 la lista de contraseñas
![[Pasted image 20240602153257.png]]

![[Pasted image 20240602153311.png]]


Le metemos un Grep en "Incorrect Password", aunque igual si iniciamos sesión con el usuario "carlos" nos devolvera una redirección (status code 302)
![[Pasted image 20240602153441.png]]

Y Comenzar Ataque

![[Pasted image 20240602153630.png]]


![[Pasted image 20240602153647.png]]

![[Pasted image 20240602153700.png]]



## Scripts para ordenar contraseñas

##### Como usarlo:

Tener en cuenta que hay que tener una archivo llamado "passwords.txt" con la lista de contraseñas que ofrece Burp Suite, tener tambien un archivo llamado passwordsOk.txt (aqui se almacenaran las contraseñas en el orden esperado)

![[Pasted image 20240602154038.png]]

```
archivo="passwords.txt"

  

if [ ! -f "$archivo" ]; then

echo "El archivo $archivo no existe"

exit 1

fi

  
  

contador=0

  

while IFS= read -r linea; do

echo "$linea" >> passwordsOk.txt

contador=$((contador + 1))

  

if [ $contador -eq 2 ]; then

contador=0

echo "peter" >> passwordsOk.txt

fi

done < "$archivo"

  

# Descomenta lo de abajo si sabes como usarlo :)

# cat passwordsOk.txt | xclip -selection clipboard
```
