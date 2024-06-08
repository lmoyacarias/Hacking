
## Objetivo:

![[Pasted image 20240605204549.png]]

## Solucion:

Iniciamos sesión con nuestra cuenta y marcamos la opción "Permanecer Conectado" y lo enviamos al Repeater

![[Pasted image 20240605204918.png]]

Vemos que nos devuelve una cookie llamada "stay-logged-in"

![[Pasted image 20240605205720.png]]

La decodificamos en Base64

![[Pasted image 20240605205831.png]]

Vemos que luego de los dos puntos (:) nos devuelve algo y es un hash en "md5" entonces intentamos romper este hash por fuerza bruta.

Y como vemos resulta que es nuestra contraseña... Ya con esto podemos decir que tenemos bien estudiada la forma en como se crea dicha cookie. En este caso es ***"nombreUsuario:contraseñaEnHashMd5" y luego todo eso convertido en Base64***

![[Pasted image 20240605210041.png]]

Intentaremos crear esto para estar seguros... Y como vemos al seguir este patron llegamos al mismo valor de la cookie "d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw"

![[Pasted image 20240605210432.png]]


Ahora solo faltaria crear muchas cookies pero esta vez con el usuario "carlos" siguiente éste patron pero utilizando la lista de contraseñas de un diccionario... Pero para automatizar esto hice un script que copiara la lista de cookies en el clipboard, pero esto lo usaremos mas adelante

![[Pasted image 20240605214823.png]]

```
archivo="pass.txt"

cookies="listaCookies.txt"

  

if [ ! -f "$archivo" ]; then

echo "El archivo $archivo no existe"

exit 1

fi

  

if [ ! -f "$cookies" ]; then

echo "[*] Creando archivo $cookies"

touch $cookies

echo "[!] Archivo $cookies creado!"

fi

  
  

while IFS= read -r linea; do

pass=$(echo -n "$linea" | md5sum | cut -d "-" -f 1 | cut -d ' ' -f 1)

pass=$(echo "carlos:$pass")

pass=$(echo -n "$pass" |base64)

echo $pass >> listaCookies.txt

done < "$archivo"

  

echo "[*] Copiando listado de Cookies desde $cookies"

cat $cookies | xclip -selection clipboard

echo "[!] Cookies copiadas con exito"
```


Entramos a "My account" y lo enviamos al Intruder

![[Pasted image 20240605215004.png]]

Ponemos el Tipo de Ataque a "Sniper", agregamos como varaible el valor de la cookie "stay-logged-in" y cambiamos el "id" del param de la ruta a "carlos"... Cerramos la sesión del usuario "wiener" (nuestro usuario)

![[Pasted image 20240605215134.png]]

Y lanzamos el ataque

En este caso vemos que nos devolvio un status 200, igualmente yo le aplique un Grep pero no es necesario... Ahora solo tendriamo que dar click derecho y en la peticion y luego en "Request in browser" y luego en "In original session", copiamos y pegamos eso en el el navegador...

![[Pasted image 20240605220928.png]]

![[Pasted image 20240605221203.png]]

