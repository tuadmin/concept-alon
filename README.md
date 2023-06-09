# concept-alon
ANY LINES OBJECT NOTATION (formato de notacion de cualquier objeto por lineas)
## descripcion
este repositorio es para poner a pruebas el concepto de un nuevo tipo de formato de archivos denominado ALON (ANY LINES OBJECT NOTATION) debido a la necesidad de tener una presentacion mas potente de la que ofrece CSV y JSON. el cual podria simplificar en medida el uso de dichos archivos para listados gigantes sin sacrificar rendiemiento ni RAM. 

# objetivo
El objetivo  sería desarrollar y evaluar el formato de archivo ALON (ANY LINES OBJECT NOTATION) como una alternativa más potente y eficiente para el almacenamiento y manipulación de listados de datos grandes en comparación con los formatos existentes, como CSV y JSON. El objetivo sería demostrar que ALON puede simplificar el uso de archivos para listados gigantes sin comprometer el rendimiento ni el consumo de memoria. El repositorio serviría como un lugar para probar y validar el concepto del nuevo formato ALON.

# concepto de formato
El formato debe permitir solo inserciones y evitar actualizaciones estilo blockchain. Sin embargo, en el caso de un archivo de 2 TB que requiera edición, se deberá dividir en tres secciones: la sección anterior a la fila que se va a editar y la sección posterior a dicha fila. Esto permitirá realizar la edición en la fila.
```alon
alon:checksum=adler32:type=json
31:87ba088b:{"prop1":"any","prop2":12321}:31
```
con cheksum propio que consta solo de 2 bytes
```alon
alon:10|16|32:checksum:type=json
31:??:{"prop1":"any","prop2":12321}:31
```
## simbolos especiales
* ":" doble punto: fuera de la data se usa para separar 
* "=" fuera de la data se usa para indicar el valor del parametro 
* "\n" salto de linea: si bien el formato de salto de linea es el de linux. podria tambien contener "\r\n"
## filas
la primera fila sera representada por el formato de **cabecera**
la fila de cuerpo sera representada por la formato de **fila dato**
```alon
{ cabecera }
{ fila datos }
{ fila datos }

```
### cabecera
La cabecera es la parte crucial ya que para ello se basara en: parametros, separador de lineas
```alon
alon:{baseSize(la base de numeracion para representar la cantidad de bytes de una fila)}:{parametros}
```
#### baseSize
representa el la base de numeracion que se usara para indicar el tamaño de una fila, podria ser base 10, base 16,base 32
#### parametro
se tiene los siguientes parametros
* checksum=(detault?|adler32|md5|other):cuando no se usa se usa el default que consta de 2bytes donde se hace oepraciones XOR sobre la fila de datos
* type=(json,any):el formato de los datos con el que estara el archivo. se aconseja usar formatos que no contengan saltos de linea para mantener mayor comodidad al lector
* sugerencia para mas metadatas

### fila dato

```alon
{size}:{checksum}:{data}:{size}
```

la fila del dato esta compuesta por
* size: que es el tamaño ocupado en bytes por la fila
* checksum: contiene una verificacion de la informacion por la data
* data: contiene la informacion se recomienda un formato legible o imprimible
