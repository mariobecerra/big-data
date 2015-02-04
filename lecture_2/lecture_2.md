Big data :: Lecture 2
========================================================
author: Adolfo De Unánue T.
date: 21 de Enero, 2015
font-import: http://fonts.googleapis.com/css?family=Risque
font-family: 'Risque'
css: lecture_2.css

Ciencia de datos
=======================================================
type: sub-section


Recordando
========================================================
Masson y Wiggins (*A taxonomy of Data Science* Mason and Wiggins, 2010) definen los cinco pasos que realiza
un científico de datos:

- Obtener datos
- Manipular datos
- Explorar datos
- Modelar datos
- Intepretar datos

Esta clase es sobre los primeros dos y cuando hay muchos datos...

Obtener datos
========================================================

- Descargar datos desde otro lado
- Consultar una base de datos
- Consultar una API
- Extraer datos de un archivo
- Generar tus propios datos

Este paso incluye el ¿Dónde lo guardo de manera _efectiva_?

Manipular datos
========================================================

- Filtrar líneas
- Extraer columnas
- Reemplazar valores
- Manejar los valores
- Convertir los datos de un tipo al otro

Este paso incluye el ¿Cómo lo manipulo/limpio de manera _efectiva_?

Preguntas
========================================================
- ¿Sus datos son estáticos o dinámicos?
- ¿Dónde los voy a almacenar?
  - Base de datos, web, etc
  - Infraestructura como servicio (IaaS)
- ¿En que formato?
- ¿Cómo presentamos los datos?
  - Visualización, descarga, acceso, etc.
- ¿Estoy en **big data**?
  - Memoria.
  - Columnas, no sólo renglones.

IaaS
=======================================================
type: sub-section


IaaS
========================================================

- Ventajas
  - Por el costo de un *hardware* caro, es posible de tener mucho más poder en la nube.
  - Es posible sólo pagar por el uso.
  - Escala muy bien, mucho mejor que comprar cada vez más espacio, CPU, etc.

- Desventajas
  - La red es *muy lenta*.
  - De verdad, es **muy lenta**.
  - `25 Gb a 5.3 Mb/s -> 11h ` `:(`


Formatos
=======================================================
type: sub-section


CSV
========================================================
- *Comma separated values*
- Sirve para datos que se pueden acomodar en una tabla.
- Es excelente para acceso *secuencial* a los datos.
  - Cuando el archivo es muy grande, considerar procesar *bulk* y no línea por línea.
- Se puede partir en pedacitos y sigue funcionando.
- No sirve para datos que no estén en formato rectangular.
- No se describe así misma...
  - Y no es estándar su creación.
- Separadores, *headers*, `encoding`, escapes raros, etc...

CSV
========================================================

- Yo recomiendo usar como separador el *pipe*: `|`.
  - Terminación `psv`.
  - El problema es que las personas en Office en Mac, probablemente sufran...
- Poner siempre el `header`.

CSV
========================================================
- En `R`


```r
  write.table(datos, file="algun_archivo.psv", quote=TRUE, na="NA", dec=".", row.names=FALSE, col.names=TRUE, sep="|")
```

CSV
========================================================

- En `python`

<small>
```{python, eval=FALSE}
def write_psv(file_name, dic, header):
    csv.register_dialect('psv', delimiter='|', quoting=csv.QUOTE_ALL) # Dialecto PSV
    with open(os.path.relpath('../data/'+ file_name), 'a') as csv_output:
        csvWriter = csv.DictWriter(f=csv_output, extrasaction='raise', fieldnames=header, dialect='psv')
        csvWriter.writerow(dic)
```
</small>

CSV
========================================================
- En `PostgreSQL` (desde `psql`)

```{sql}
\copy (select * from foo) to '/algun_archivo.psv' with header csv delimiter '|'
```

Lo veremos luego...

XML
========================================================
- *eXtensible Markup Languague*
- Los datos se validan contra la estructura.
- La descripción de los datos es parte de los datos.
  - i.e. tienen un *esquema*.
- Súper *verbose*.
- Es posible convertir a cualquier tipo de archivo desde aquí.
- Recorrerlo puede ser doloroso.
  - Ni siquiera voy a poner un ejemplo
- Usar como **fuente de la verdad**.
  - Interoperación

JSON
============================================================
- *JavaScript Object Notation*
- Es Javascript (en realidad es **Javascript**)
- Súper popular entre los desarrolladores.
- Más rápido que *parsear* que un `XML`.
- Se puede accesar secuencialmente.
- *Verbose*
- No tiene validación.
- Fácil de *subir* a bases de datos **no-relacionales**.


Ejemplos
==============================================================

- CSV

<small>
`nombre, apellido, fecha`

`Adolfo, De Unanue, 04/02/1978`
</small>

Ejemplos
=========================================================
- XML

<small>
```{xml}
<xml>
<profesor>
  <nombre>
    <nombre> Adolfo </nombre>
    <apellidos>
      <paterno>De Unánue</paterno>
      <materno>Tiscareño</materno>
    </apellidos>
  </nombre>
  <nacimiento>
    <fecha>04/02/1978</fecha>
    <lugar></lugar>
  </nacimiento>
</profesor>
</xml>
```
</small>

Ejemplo
============================================================

- JSON

<small>
```{json}
  {
  "nombre": Adolfo De Unánue
  "fecha_nacimiento": 04/02/1978
  }
```
</small>

Serialización
=========================================================
- Moverlos por la red es terrible si son varios `Gb`
  - Comprimirlos también puede ser costoso en tiempo...
- **Apache Thrift**
  - Original de Facebook.
  - Archivo de metadatos -> Generador de código -> Servidor que se encarga de la serialización.
  - Otro proyecto similar es *Google Protocol Buffer*
- **Apache Avro**
  - Autodescriptivo como `JSON`, pero no incorpora la descripción en cada elemento.
  - Soporta nativamente la compresión.


Otros consejos
=========================================================
- Encoding: **UTF-8**
  - Verifica tu editor de texto, proceso, etc.
- Transformación de archivos:
  - **Apache Hadoop**


Herramientas
=======================================================
type: sub-section

Consola
=======================================================
type: sub-section

Bash / Zsh
=======================================================
- En Ubuntu / Debian etc.

```
sudo apt-get install zsh
```

- Instalar `Oh-my-zsh`

```
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

- Establecer `zsh` como default

```
chsh -s $(which zsh)
```

**Nota:** Ya está instalado en `docker`.

Navegar
=======================================================

- `Ctrl + a` Inicio de la línea
- `Ctrl + e` Fin de la línea
- `Ctrl + r` Buscar hacia atrás
  - Elimina el *flechita arriba*
- `Ctrl + b` / `Alt + b`
- `Ctrl + f` / `Alt + f`
- `Ctrl + k` - Elimina el resto de la línea
- `Ctrl + d` - Cierra la terminal
- `Ctrl + z` - Manda a *background*
- `Ctrl + c` - Intenta cancelar



Git & Github
=======================================================
type: sub-section


¿Por qué un control de versiones?
=======================================================
- Sincronizar cambios con tus compañeros de trabajo.
- Respaldos regulares (muy regulares) de tu trabajo.
  - Enviar correros electrónicos no está bien, no escala, no es *cool*, punto.
- *Configuration Management*  líneas de desarrollo en paralelo.
  - Eso no lo veremos en este curso, pero vale la pena averiguar.

Configuración básica
========================================================

```
$ git config --global user.name "Nombre Apellido"
$ git config --global user.email "correo@buzon.com"

```

**Nota:** Hacer esto en `docker`

Primero: Fork
=======================================================
type: prompt



**DEMO EN EL NAVEGADOR**


Flujo
========================================================

![flujo](images/simple-daily-git-workflow.jpg)


Ejemplo
============================================================
Luego de hacer el `fork`
```
git clone https://github.com/ITAM-DS/big-data.git
cd big-data
touch uno.txt
touch dos.txt
git add .
git status
git commit -m "Archivos iniciales"
echo "Hola" > uno.txt
echo "Adios" > dos.txt
git diff uno.txt
git status
git add uno.txt
git commit -m "Agregamos un hola"
```

Ejemplo (Continuación)
=========================================================
```
git status
git add dos.txt
git commit -m "Agregamos un adios"
git status
git push
```

Luego hacemos un `pull request` en el navegador (si es que ya vamos a entregar la tarea).

Ejemplo (Continuación)
==========================================================

Por último, para sincronizar con los cambios en el `fork` principal
```{bash}
> git remote -v # Vemos que remotes tenemos ligados

# Agregamos el repo de la clase

> git remote add clase https://github.com/ITAM-DS/big-data.git

> git remote -v

> git pull clase master # Para "jalar" los cambios del fork principal

> git push # Para subir los cambios a su fork
```

Unix tools para big data
=======================================================
type: sub-section

Outline
============================================================
- Pipes y flujos
- `seq`
- `wc`
- `head`, `tail`
- `split`, `cat`
- `cut`
- `uniq`, `sort`
- Expresiones regulares `regex`
- `grep`
- `awk`, `sed`
- `GNU parallel`

¿Por qué?
==================================================================

En muchas ocasiones, se verán en la necesidad de responder muy rápido y
en una etapa muy temprana del proceso de *big data* . Las peticiones
regularmente serán cosas muy sencillas, como estadística univariable y es
aquí donde es posible responder con las herramientas mágicas de UNIX.


Lego: Idea poderosa
===================================================================
![lego](images/tools.png)



Datos para jugar
==================================================================
![ufo](images/flying-disk-ufo.jpg)

Datos para jugar
==================================================================
- Para los siguientes ejemplos trabajaremos con los archivos
encontrados en **The National UFO Reporting Center Online Database**.

- Estos datos representan los *avistamientos* de OVNIS en EUA.

- Usaremos como ejemplo la descarga el mes de Diciembre de 2014
  - `http://www.nuforc.org/webreports/ndxe201412.html`

- Se encuentra en la carpeta `data`.


seq
==============================================================

Genera secuencias de números...

```
~ seq 5

~ seq 3 10

~ seq -s '|' 10

~ seq -w 1 10
```

tr
========================================================

Cambia, reemplaza o borra caracteres del `stdin` al `stdout`

```
~ man tr

~ echo "Hola mi nombre es Adolfo De Unánue"  | tr '[:upper]' '[:lower]'

~ echo "Hola mi nombre es Adolfo De Unánue"  | tr -d ' '

~ echo "Hola mi nombre es Adolfo De Unánue"  | tr -s ' ' '_'

```

Pipes y flujos
===============================================================

- `|` (pipe) “Entuba” la salida de un comando al siguiente

```
echo 'Me gusta estar en la línea de comandos!' | cowsay
```

```
# grep "busca y selecciona" cadenas o patrones (lo veremos al rato)
~ seq 50 | grep 3
```

- `>`,`>>`, Redirecciona la salida de los comandos a un sumidero.

```
~ seq 10 > numeros.txt
```

```
~ ls >> prueba.dat
```

Ejercicio
===============================================================
type: exclaim

Transforma el archivo de `data` de tabuladores a `|`, cambia el nombre con terminación `.psv`.


Pipes y flujos
===============================================================

- `<` Redirecciona desde el archivo

```
sort < prueba.dat # A la línea de comandos acomoda con sort,
sort < prueba.dat > prueba_sort.dat # Guardar el sort a un archivo.
```

En el siguiente ejemplo redireccionamos al `stdin` el archivo como entrada del `wc -l`
sin generar un nuevo proceso

```
< numeros.txt wc -l
```

Condicionales
===============================================================

- `&&` es un AND, sólo ejecuta el comando que sigue a `&&` si el
primero es exitoso.

```
> ls && echo "Hola"
> lss && echo "Hola"
```

curl
===============================================================

- Obtener datos desde el internet...


```
# The Time Machine de H.G. Wells, desde el Proyecto Gutenberg
~ curl http://www.gutenberg.org/cache/epub/35/pg35.txt
```

```
# Sin el progress bar (útil cuando vamos a hacer un pipe a otro comando)
~ curl -s http://www.gutenberg.org/cache/epub/35/pg35.txt
```

- La opción `-u` por si lo piden usuario y password

```
~ curl -u username:password ...
```

- La opción `-L` por si hay redirecciones, por ejemplo si el `URL` empieza con  `http://bit.ly/...`

wc
================================================================

- `wc` significa *word count*
  - Cuenta palabras,renglones, bytes, etc.
- Es un buen momento para aprender que existe un manual.
  - `man wc`
- En nuestro caso nos interesa la bandera `-l` la cual sirve para contar líneas.

```
~ seq 30 | grep 3 | wc -l
```

wc
====================================================================

```
> cd data

> wc -l UFO-Dic-2014.tsv
498 UFO-Dic-2014.tsv

> wc -l *
498 UFO-Dic-2014.tsv
498 UFO-Dic-2014.psv
996 total
```

head, tail
===================================================================
- `head` y `tail` sirven para explorar visualmente las primeras diez
(default) o las últimas diez (default) renglones del archivo,
respectivamente.


```
> cd data

> head UFO-Dic-2014.tsv

> tail -3 UFO-Dic-2014.tsv
```


split, cat
===================================================================
- `cat` concatena archivos y/o imprime al `stdout`

```
> echo 'Hola mundo' >> test

> echo 'Adios mundo cruel' >> test

> cat test

...

> rm test

> cd data

> cat UFO-Nov-2014.tsv UFO-Dic-2014.tsv  > UFO-Nov-Dic-2014.tsv

> wc -l UFO-Nov-Dic-2014.tsv
```

split, cat
===================================================================

- `split` hace la función contraria, divide archivos.
- Puede hacerlo por tamaño (bytes, `-b`) o por líneas (`-l`).

```
> split -l 500 UFO-Nov-Dic-2014.tsv
> wc -l UFO-Nov-Dic-2014.tsv
```

cut
===================================================================
- Con `cut` podemos dividir el archivo pero por columnas.
- Donde columnas puede estar definido como campo (`-f`), en
conjunción con (`-d`), carácter (`-c`) o bytes (`-b`).
- En este curso nos interesa partir por campo.

**NOTA**: Para partir por tabulador usa `-d$'\t'`

```
$ echo "Adolfo|1978|Físico" >> prueba.psv
echo "Patty|1984|Abogada" >> prueba.psv
cut -d’|’ -f1 prueba.psv
cut -d’|’ -f1,3 prueba.psv
cut -d’|’ -f1-3 prueba.psv
```

Pregunta
==================================================================
type: prompt

Pregunta: ¿Qué pasa con los datos de avistamiento? Quisiera
las columnas 2, 4, 6 ó si quiero las columnas Fecha, Posted, Duración y Tipo (en ese orden).

```
> head UFO-Nov-Dic-2014.tsv | cut -d$'\t' -f2,4,6
...
> head UFO-Nov-Dic-2014.tsv | cut  -f4,1,5,2  # \t es el default
...
```

¿Notaste el problema? Para solucionarlo requeriremos comandos más poderosos...

Si checas la documentación (`man cut`), ¿Puedes ver la razón del problema?


uniq y sort
===============================================================

- `uniq` Identifica aquellos renglones consecutivos que son iguales.
- `uniq` puede contar (`-c`), eliminar (`-u`), imprimir sólo las duplicadas
(`-d`), etc.
- `sort` Ordena el archivo, es muy poderoso, puede ordenar por
columnas (`-k`), usar ordenamiento numérico (`-g`, `-h`, `-n`), mes
(`-M`), random (`-r`) etc.

```
sort -t "," -k 2  UFO-Nov-Dic-2014.tsv
```

uniq y sort
===============================================================

- Combinados podemos tener un `group by`:

```
# Group by por timestamp y estado
 cat UFO-Dic-2014.tsv \
      | cut -d$'\t' -f1,3 \
      | sort -t $'\t' -k 2 -k 1 \
      | uniq -c | head
```

Otro problema ¿Puedes ver cuál es? En lo que se arregla:

**Ejercicio**: ¿Cuál es el top 5 de estados por avistamiento?


Expresiones regulares
================================================================
*In computing, regular expressions provide a concise and flexible means for
identifying strings of text of interest, such as particular characters, words,
or patterns of characters. Regular expressions (abbreviated as regex or
regexp, with plural forms regexes, regexps, or regexen) are written in a
formal language that can be interpreted by a regular expression processor,
a program that either serves as a parser generator or examines text and
identifies parts that match the provided specification.*

`Wikipedia: Regular Expressions`

Regexp: Básicos
===================================================================
- Hay varios tipos POSIX, Perl, PHP, GNU/Emacs, etc.
  -Aquí veremos `POSIX`.

- Pensar en patrones (*patterns*).
- Operadores básicos
  - OR, `gato|gata` hará match con gato o gata.
  - Agrupamiento o precedencia de operadores, `gat(a|o)` tiene el mismo
significado que `gato|gata`.
  - Cuantificadores, `?` 0 o 1, `+` uno o más, `*` cero o más.


Regexp: Básicos
========================================================================

- Expresiones básicas
  - `.` Cualquier carácter.
  - `[ ]` Cualquier carácter incluido en los corchetes, e.g. `[xyz]`, `[a-zA-Z0-9-]`.
  - `[^ ]` Cualquier car ́acter individual que n esté en los corchetes, e.g.
`[^abc]`. También puede indicar inicio de líınea (fuera de los corchetes.).

Regexp: Básicos
========================================================================

- `\( \)` ó `( )` crea una subexpresión que luego puede ser invocada con
`\n` donde `n` es el número de la subexpresión.

  -`{m,n}` Repite lo anterior un número de al menos `m` veces pero no mayor
a `n` veces.
  - `\b` representa el límite de palabra.


Regexp: Ejemplos
=================================================================

- username: `[a-z0-9 -]{3,16}`
- contraseña: `[a-z0-9 -]{6,18}`

- IP address:

```
( 25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?_)3(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)
```

Regexp: Ejercicios
=========================================================

- fecha (dd/mm/yyyy): ???
- email (adolfo@itam.edu) : ???
- URL (http://gmail.com): ???


Regexp: Expresiones de caracteres
========================================================
- `[:digit:]` Dígitos del 0 al 9.
- `[:alnum:]` Cualquier caracter alfanumérico 0 al 9 OR A a la Z OR a
a la z.
- `[:alpha:]` Caracter alfabético A a la Z OR a a la z.
- `[:blank:]` Espacio  o TAB únicamente.


Ejercicio
========================================================
type: alert


- ¿Cómo reconocemos los avistamientos en otro país?
- ¿Cuántos hay?
- ¿Cuántos avistamientos no tienen forma de esferoide?

Regexp: ¿Quieres saber más?
========================================================

- [Learn Regular Expressions in 20 minutes](http://tutorialzine.com/2014/12/learn-regular-expressions-in-20-minutes/)
- [The 30 minute regex tutorial](http://www.codeproject.com/Articles/9099/The-Minute-Regex-Tutorial)



grep
========================================================

`grep` nos permite buscar líneas que tengan un patrón específico

```
> grep "CA" UFO-Nov-Dic-2014.tsv
> grep "HOAX" UFO-Nov-Dic-2014.tsv
> grep -v "18:" UFO-Nov-Dic-2014.tsv
> grep -E "18:|19:|20:" UFO-Nov-Dic-2014.tsv
> grep -E "[B|b]lue|[O|o]range" UFO-Nov-Dic-2014.tsv \
        | grep -v "Orangebug"
> grep -i -E "[blue|orange]" UFO-Dic-2014.tsv
> grep -c -o -E "[B|b]lue|[O|o]range" UFO-Nov-Dic-2014.tsv  # Ejecuta una bandera a la vez
> grep -o -E "[B|b]lue|[O|o]range" UFO-Nov-Dic-2014.tsv | sort | uniq -c
> grep "\/[0-9]\{1,2\}\/" UFO-Dic-2014.tsv # Seleccionamos días
> grep -v "\/[0-9]\{4\}" UFO-Dic-2014.tsv  # Año mal formateado
> grep -E "([aeiou]).*\1" names.txt # Vocal caracteres Misma vocal
> echo "Hola grupo ¿Cómo están?" > grep -oE '\w+'
```



Ejercicio
===============================================================

Usando los archivos de la carpeta `grep`
- Selecciona las líneas que tienen exactamente cinco dígitos.
- Selecciona las que tienen más de 6 dígitos.
- Cuenta cuantos javier, romina o andrea hay.


Ejercicio
===============================================================
type:exclaim

Descarga el libro de **The Time Machine** de *H. G. Wells*, convertirlo a minúsculas, extraer las palabras,
ordenarlas, eliminar duplicados y contarlos, ordenar de mayor a menor y luego mostrar el top 10.


awk
==========================================================
- `awk` es un lenguaje de programación muy completo, orientado a archivos de texto que vengan en columnas.
- Un programa de `awk` consiste en una secuencia de enunciados del tipo patrón-acción:

```
  pattern { action statement }
```
- Si hay varios se separan con `;`

```
[pattern] {[command1]; [command2]; [command3]}
```

awk
==========================================================
- Keywords: `BEGIN , END, [op1] ~` `[regular expression]`, operadores booleanos como en `C`.

- Variables especiales:
    - `$1, $2, $3, ...` – Valores de las columnas
    - `$0` – toda la línea
    - `FS` – separador de entrada
      data.
    - `OFS` – separador de salida
    - `NR` – número de la línea actual
    - `NF` – número de campos en la línea (record)

awk: sintáxis
=========================================================
type:exclaim

```
awk '/search pattern1/ {Actions}
     /search pattern2/ {Actions}' file
```

awk: Ejemplos
=========================================================
```
> awk 'END { print NR }'  UFO-Nov-Dic-2014.tsv  # Lo mismo que wc -l
# Número de columnas
> awk 'BEGIN{FS = "\t" }; { print NF }' UFO-Nov-Dic-2014.tsv \
      | sort -n | uniq
> awk -F"[\t]" '{ print NF }' UFO-Nov-Dic-2014.tsv \
      | sort -n | uniq
> awk 'BEGIN{ FS = "\t" }; { if(NF != 7){ print >> "UFO_fixme.csv"} \
else { print >> "UFO_OK.csv" } }' UFO-Nov-Dic-2014.tsv
# Limpia el archivo con columnas de más
> awk '{ print NF ":" $0 }' UFO-Nov-Dic-2014.tsv
> awk '{ $2 = ""; print }' UFO-Nov-Dic-2014.tsv
> awk 'BEGIN{ FS = "," }; {sum += $2} END {print sum}' numbers.dat
# Suma de una columna (aunque aquí no tiene mucho sentido)
> awk -F, '{sum1+=$1; sum2+=$2;mul+=$2*$3} END {print sum1/NR,sum2/NR,mul/NR}' numbers.dat
# Promedios de varias columnas
```


awk: Ejemplos
=========================================================
```
> awk '/CA/ { n++ }; END { print n+0 }' UFO-Nov-Dic-2014.tsv
> awk -F, '$1 > max { max=$1; maxline=$0 }; END { print max, maxline }' numbers.dat
> awk '{ sub(/FL/,"Florida"); print }' UFO-Nov-Dic-2014.tsv
> awk '{ gsub(/foo/,"bar"); print }' UFO-Nov-Dic-2014.tsv
> awk '/baz/ { gsub(/foo/, "bar") }; { print }' UFO-Nov-Dic-2014.tsv
> awk '!/baz/ { gsub(/foo/, "bar") }; { print }' UFO-Nov-Dic-2014.tsv
> awk 'a != $0; { a = $0 }' # Como uniq
> awk '!a[$0]++' # Remueve duplicados que no sean consecutivos
> awk -F"[\t]" '$4 ~/Circle/' UFO-Nov-Dic-2014.tsv
> awk -F"[\t]" 'BEGIN { conteo=0;} \
        $4 ~/Circle/ { conteo++; }
END { print "Número de avistamientos circulares en el dataset =",conteo;}' UFO-Nov-Dic-2014.tsv
```

awk: Más sintaxis
=========================================================
type:exclaim

```
awk '
BEGIN { Actions}
{ACTION} # Action for everyline in a file
END { Actions }
# is for comments in Awk
' file
```

awk: mean, max, min
=========================================================

```
> awk '{FS="|"}{if(min==""){min=max=$1}; if($1>max) {max=$1};if($1<min) {min=$1}; total+=$1; count+=1} END \
{print "mean = " total/count,"min = " min, "max = " max}' data.txt
```

awk: Mediana
==========================================================

```
function mediana(c,v,j) {
    asort(v,j); ## Ordena y asigna enteros a la posición
    if (c % 2) {
        return j[(c+1)/2];
    } else {
        return (j[c/2+1]+j[c/2])/2.0;
    }
}
{
    print "1st", count
    count++;
    print "2nd", count
    values[count]=$1;
} END {
    print  "mediana = " mediana(count,values);
}

```

**NOTA**: Este archivo se encuentra en `mediana.awk`.

```
$ < data.txt awk -f mediana.awk
```


awk: Desviación estándar
=========================================================

```
awk '{sum+=$1; sumsq+=$1*$1;}
END {print "stdev = " sqrt(sumsq/NR - (sum/NR)**2);}'
data.txt
```

awk
=========================================================

¿Qué pasa cuando hay muchas columnas?

```
awk -F "|" '
{ for (i=1; i<=NF; ++i) sum[i] += $i; j=NF }
END { for (i=1; i <= j; ++i) printf "%s ", sum[i]; printf "\n"; }
' data2.txt
```

awk: RTFM
=========================================================


Para saber más consulta el manual:

- [**GNU awk** _Effective AWK Programming_](https://www.gnu.org/software/gawk/manual/)

sed
=========================================================

- `sed` significa *stream editor* . Permite editar archivos de manera automática.
- El comando tiene cuatro *espacios*
  - Flujo de entrada
  - Patrón
  - Búfer
  - Flujo de salida
- Entonces, `sed` lee el *flujo de entrada* hasta que encuentra `\n`. Lo copia al *espacio patrón*, y es ahí donde se realizan las operaciones con los datos. El *búfer* está para su uso, pero es opcional, es un búfer, vamos. Y finalmente copia al *flujo de salida*.


sed
==========================================================
```
> sed 's/foo/bar/' data3.txt   # Sustituye foo por bar
> sed -n 's/foo/bar/' data3.txt # Lo mismo pero no imprime a la salida
> sed -n 's/foo/bar/; p' data3.txt # Lo mismo pero el comando "p", imprime
> sed -n 's/foo/bar/' -e p data3.txt # Si no queremos separar por espacios
> sed '3s/foo/bar/'  data3.txt # Sólo la tercera linea
> sed '3!s/foo/bar/'  data3.txt # Excluye la tercera línea
> sed '2,3s/foo/bar/' data3.txt # Con rango
> sed -n '2,3p' data3.txt  # Imprime sólo las líneas de la 2 a la 3
> sed -n '$p' # Imprime la última línea
> sed '/abc/,/-foo-/d' data3.txt # Elimina todas las líneas entre "abc" y "-foo-"
> sed '/123/s/foo/bar/g'  data3.txt
# Sustituye globalmente "foo" por "bar" en las líneas que tengan 123
> sed 1d data2.txt # Elimina la primera línea del archivo
> sed -i 1d data2.txt  # Elimina la primera línea del archivo de manera interactiva
```


Ejercicio
==========================================================
type:exclaim

- Elimina los `headers` repetidos con sed en los archivos `UFO`.

- Describe estadísicamente los tiempos de observación (tendrás que usar: , `cut`,`grep`,`sed`, `awk`, etc.)

Otros comandos útiles
==========================================================

- `file -i` Provee información sobre el archivo en cuestion

```
> file -i UFO-Nov-Dic-2014.tsv
UFO-Nov-Dic-2014.tsv : text/plain; charset=utf-8
```

- `iconv` Convierte entre encodings, charsets etc.

```
> iconv -f iso-8859-1 -t utf-8 UFO-Nov-Dic-2014.tsv  > UFO-Nov-Dic_utf8.csv
```

Otros comandos útiles
==========================================================
`od` Muestra el archivo en octal y otros formatos, en particular la bandera `-bc`
lo muestra en octal seguido con su representación ascii. Esto sirve para identificar separadores raros.

```
> od -bc UFO-Nov-Dic-2014.tsv   | head -4
0000000 104 141 164 145 040 057 040 124 151 155 145 011 103 151 164 171
          D   a   t   e       /       T   i   m   e  \t   C   i   t   y
0000020 011 123 164 141 164 145 011 123 150 141 160 145 011 104 165 162
         \t   S   t   a   t   e  \t   S   h   a   p   e  \t   D   u   r
```

Utilerías
==========================================================
- `screen` o `tmux` para tener varias sesiones abiertas en una terminal.
  - En particular muy útiles para dejar corriendo procesos en el servidor.

- `bg` ó  `&` para mandar procesos al *background*

```{python}
python -m SimpleHTTPServer 8008 &
# Ejecuto un servidor HTTP
```

- `jobs` para saber cuales están ejecutandose.

- `fg` para traerlos a la vida de nuevo.


Programando
=======================================================
type: sub-section

Bash programming
========================================================
- *loops*

```
for var in `comando`
do
instrucción
instrucción
...
done
```

- Condicionales

```
if TEST-COMMANDS; then CONSEQUENT-COMMANDS; fi
```




Bash programming
========================================================
- Al final hay que poner esto en un archivo, ponerlo a correr e irnos
  a pensar...
- Para cualquier programa *script* es importante que la primera línea del archivo le diga a bash que comando usar para ejecutarlo.

- También hay dar permisos de ejecución al archivo

```
~ chmod u+x ejemplo.py
```

Bash programming
========================================================

- A la primera línea se conoce como *shebang* y se representa por `#!` seguido de la ruta al ejecutable. eg. `#!/usr/bin/python` cambia la ejecución de

```
> python ejemplo.py
```
a
```
> ./ejemplo.py
```

Python programming
=========================================================

Usando el truco del `shebang` podemos hacer el siguiente programa:

Recursividad:

```{python}
#!/usr/bin/env python

def fibonacci(x):
  if n == 0:
    return 0
  elif n == 1:
    return 1
  else
    return fibonacci(n-1) + fibonacci(n-2)

if __name__ = "__main__":
  import sys
  x = int(sys.argv[1]) # Hay maneras mas elegantes
  print fibonacci(x)
```


Python programming
=========================================================

Usando el truco del `shebang` podemos hacer el siguiente programa (otro archivo)

Generadores:

```{python}
#!/usr/bin/env python

def fibonacci(x):
  a,b = 0,1
  while True:
    yield a
    a, b = b, a + b


if __name__ = "__main__":
  import sys
  x = int(sys.argv[1]) # Hay maneras mas elegantes
  print fibonacci(x)
```

R programming
=========================================================

El `shebang` para `R` sería

```{R}
#!/usr/bin/env Rscript
...
```


Python y R, leyendo de stdin
========================================================

- **Python**

```{python}
#!/usr/bin/env python

import re
import sys

n = int(sys.argv[1]) # Leemos un entero como argumento
texto = sys.stdin.read()  # Leemos texto desde el stdin
...

```

- **R**

```{R}
#!/usr/bin/env Rscript
n <- as.integer(commandArgs(trailingOnly = TRUE)) # Leemos un entero como argumento
f <- file("stdin")
lines <- readLines(f)
...

```



Tarea
==========================================================
type: exclaim

Reproducir el ejercicio con **Python** y **R** usando redirección y ejecutables.

Procesando en Serie
=======================================================
type: sub-section

Serie
========================================================

Podemos hacer _loops_ sobre varias cosas:

- Sobre números

```
for i in (0..100..2)
do
echo "$i"
done
```

- Sobre líneas

```
curl -s http://www.gutenberg.org/cache/epub/35/pg35.txt > time_machine.txt

while read line
do
echo "Línea: ${line}"
done < time_machine.txt
```

Serie
========================================================

- Y sobre archivos

```
for archivo in awk_sed\*.txt
do
echo "El archivo es ${archivo}"
done
```

Esto último tiene muchos problemas (no maneja espacios o caracteres raros, por ejemplo).

Una mejor alternativa es `find`

```
find awk_sed -name '*.txt' -exec echo "El archivo es {}" \;
```

Además `find` permite buscar por fecha, tamaño, fecha de acceso, permisos, etc.


Procesando en Paralelo
=======================================================
type: sub-section

GNU parallel
==========================================================

- Todo esto está muy bien, pero ...
  - ¿Cómo aprovecho todos los `cores` (o `procesadores` si son afortunados) de mi máquina?

- Instalación

```
curl -s \
http://ftp.jaist.ac.jp/pub/GNU/parallel/parallel-latest.tar.bz2 \
        | tar -xjv > extraccion.log
cd $(head -n 1 extraccion.log)
./configure --prefix=$HOME && make && make install
```


GNU parallel
==========================================================

- Limpiando

```
cd ..
rm -R $(head -n 1 extraccion.log)
rm parallel-latest.tar.bz2 extraccion.log
```

- Verificando

```
parallel --version
```

GNU parallel: Ejemplos sencillos
==========================================================
- Pasando parámetros

```
seq 10 | parallel echo {}
```

- Si el archivo `archivos_a_procesar` contiene una lista de archivos

```
ls *.txt >> archivos.txt
cat archivos.txt
parallel -a archivo.txt gzip
```


GNU parallel: Ejemplos sencillos
==========================================================

- También puede usar el `STDIN`

```
> ls *.gz | parallel echo
```

- Y reproduciendo el ejemplo de `find`

```
find awk_sed --name '*.txt' -print0 | \
      parallel -0 echo "Archivo {}"
# print0 y -0 son usados por si hay espacios u otros caracteres.
```

GNU parallel: Ejemplos sencillos
==========================================================
- Vayamos a  la carpeta  `data` y comprimamos los archivos

```
> ls *.txt | parallel gzip -1
```


**NOTA**: Si quisieras descomprimir

```
> ls *.gz | parallel gunzip -1
```

- Convirtamos a `bz2`

```
> ls *.gz | parallel -j0 --eta 'zcat {} | bzip2 -9 > {.}.bz2'
```

`-j0` creará cuantos `jobs` en paralelo pueda., `-j1` ejecuta las cosas en serie.


GNU parallel: Ejemplos sencillos
==========================================================

Si tienes dudas sobre tu comando, siempre puedes usar el argumento

```
--dryrun
```

El cual, en lugar de ejecutar, imprimirá las líneas sin hacer nada.

GNU parallel: Logging
==========================================================

- Usa la opción `--results` la cual guarda la salida de cada `job` en un archivo separado.

```
seq 5 | parallel --results log "echo Soy el número {}"
```


GNU parallel: Progreso
==========================================================

`> parallel --progress sleep ::: 10 3 2 2 1 3 3 2 10`

`> parallel --eta sleep ::: 10 3 2 2 1 3 3 2 10`


GNU parallel: Controlando la red
=========================================================

```
ls *.gz |  time /usr/local/bin/parallel -j+0 --eta -S192.168.0.101,: --transfer --return {.}.bz2 --cleanup 'zcat {} | bzip2 -9 >{.}.bz2'
```

- `-S` lista de servidores (`:` es `localhost`).
  - require que las máquinas tengan configurado ssh sin password).

- `--transfer` mover los archivos al servidor.

- `--return` regresar los archivos a la máquina que está ejecutando.

- `--cleanup` eliminar los archivos generados de las máquinas remotas.

GNU parallel: Archivotes
========================================================
- Carpeta `parallel`, el archivo `1000000.txt` tiene `cat 1000000.txt | wc -l` líneas.

- Podemos procesarlo por pedazos (será muy útil a la hora de cargar en PostgreSQL)

`> cat 1000000.txt | parallel --pipe wc -l`

- Podemos cambiar el `blocksize`

`> cat 1000000.txt | parallel --pipe --block 3M wc -l`

(por default corta los bloques en `\n`)




GNU parallel: RTFM
=========================================================

Siempre es bueno tener esto a la mano:


[GNU Parallel Tutorial](http://www.gnu.org/software/parallel/parallel_tutorial.html)


Distribuido: AWS
=======================================================
type: sub-section


AWS CLI
=======================================================

- En su contenedor está instalado el comando `aws`

```
$ aws
usage: aws [options] <command> <subcommand> [parameters]
aws: error: too few arguments
```

- Obtengan sus llaves de Amazon (creen una cuenta, etc.) y luego configuren su `aws` con

```
$ aws configure
AWS Access Key ID [None]:
...
```

AWS CLI
=======================================================

Agrega lo siguiente al final del archivo `~/.ssh/config`

```
Host *.amazonaws.com
  IdentityFile ~/.ssh/mi_llave_aws.pem
  User ubuntu
```

- Esto es para que puedas hacer `login` sin que te pida la contraseña.

- El usuario que estamos usando (`ubuntu`) presupone que tus instancias son de tipo **Ubuntu**.


AWS CLI
=======================================================

- Guarda en un archivo llamado `instancias` la dirección de las instancias que tengas corriendo.

- Verifiquemos que podemos conectarnos:

```
parallel --nonall -slf instancias hostname
```

- `slf` = `--sshloginfile`
- `--nonall` significa que se ejecute el mismo comando en todas las máquinas remotas sin parámetros.


AWS CLI
=======================================================

- Para usar todos los `cores` de la máquina remota debes de tener instalado `parallel`

```
parallel --nonall -slf instancias "sudo apt-get install -y parallel"
```


AWS CLI: Distribuyendo archivos
=======================================================

Ejemplo de juguete:

```
seq 50000 | parallel -N5000  --pipe --slf instancias "(hostname; wc -l) | paste -sd:"
```

¿Cómo lo modificarías para poder usar archivos? (y no una secuencia)

AWS CLI: Archivos remotos
=======================================================


AWS CLI: RTFM
=======================================================

- [**Interfaz de línea de comandos de AWS**](http://aws.amazon.com/es/cli/)



A modo de cierre
=======================================================
type: sub-section

Unos trucos (vía @climagic)
=========================================================
type:exclaim

```
git log --author=$USER --format="- %B" --since=-7days --reverse | mail -s "What I've done this week" adolfo.deunanue@itam.mx
```

```
cat /dev/urandom | hexdump -C | grep "ca fe"
```

```
for t in "Wake up" "The Matrix has you" "Follow the white rabbit" "Knock, knock";do pv -qL10 <<<$'\e[2J'$'\e[32m'$t$'\e[37m';sleep 5;done
```



Finalmente...
=========================================================
type:exclaim
```
Computers are supposed to do the work for us. If you're doing most of the work for the computer,
then you've lost your way.

@climagic
```

Tarea
==========================================================
type: alert

- Modificar los ejemplos de `awk` de promedios, máximo, mínimo y desviación estándar para que se calculen con varias columnas

- UFO dataset:
  - ¿Cuántos estados, ciudades diferentes hay?
  - ¿Cuál es el número de avistamientos por mes? ¿Por día de la semana?
  - Generar un reporte con estadística univariante de las columnas: Conteos (para categóricas), promedios, máximos, mínimos, faltantes, errores del archivo.

