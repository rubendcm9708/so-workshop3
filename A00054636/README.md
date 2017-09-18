### Taller 3  
**Universidad ICESI**    
**Curso:** Sistemas Operativos    
**Docente:** Daniel Barragán C.  
**Estudiante:** Ruben Dario Ceballos  
**Git Url:** https://github.com/rubendcm9708/so-workshop3/blob/A00054636/so-workshop3/README.md      
**Tema:** Llamadas al sistema (syscalls)  
**Correo:** daniel.barragan at correo.icesi.edu.co


### Objetivos
* Conocer y explicar la función de las llamadas al sistema en un sistema operativo

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Aplicativo strace

### Descripción
El tercer taller del curso sistemas operativos trata sobre las llamadas al sistema y su importancia para el sistema operativo

### Actividades

1. Empleando el aplicativo **strace** obtenga 5 llamadas al sistema para uno o varios comandos de linux. Explique por qué los comandos seleccionados emplean las llamadas al sistema encontradas, para ello debe emplear los manuales de Linux en Internet o del sistema operativo (comando **man**). Debe incluir la explicación de los parámetros que reciben las llamadas al sistema encontradas. Consigne capturas de pantalla donde muestre las llamadas al sistema obtenidas (sugerencia: emplear -etrace para filtrar los resultados)

  ![][1]  
Para estudiar los syscalls, usé el comando **pwd**, que me permite saber sobre que dirección esta parado el puntero de navegación de centOS7  

  ![][2]  
**-WRITE(fd, buf, nbytes)** Usamos write para escribir una cierta cantidad de bytes sobre memoria 
 **fd:** File descriptor, nos dice si el archivo es una entrada estandar (fd = 0), si es una salida estandar (fd = 1) o si es un error (fd = 2).  
 **buf:** Buffer, puntero que señala al buffer que tiene la información almacenada.  
 **nbytes:** Number of bytes, El número de bytes a escribir sobre el buffer.
 Este syscall retorna el número de bytes que se escribieron si el proceso fue exitoso, y -1 si hubo un error.  
 
  ![][3]   
**-OPEN(dir,tAcc)** Usamos open para crear un entero positivo que representará un directorio de un archivo
**dir:**  Dirección en la que se encuentra el archivo
**tAcc:** El tipo de acceso requerido sobre el archivo, puede ser lectura, escritura, adjuntar, entre otros.  
En nuestro caso, el valor usado es 3, evidenciado en los retornos al usar pwd

  ![][4]  
**-ACCESS(dir,perm)** Usamos access para comprobar los permisos que tenemos sobre un archivo  
**dir:**  Dirección en la que se encuentra el archivo  
**perm:**  bits que representan un F_OK(exist), R_OK (read), W_OK (write), X_OK (execute)  
En nuestro caso, el retorno es -1, ya que se llama access sobre un archivo que no existe  

  ![][5]  
**-CLOSE(fd)** Cierra el file descriptor que se creo al usar open, para que no se vaya a referir erroneamente a ese archivo.  
**fd:** File descriptor, referencia de la ruta de un archivo  
Retorno 0 si fue cerrado exitosamente, -1 si hubo un error  

  ![][6]  
**-READ(fd,buf,count)** Se usa read para leer los datos de un archivo  
**fd:** File descriptor, referencia de la ruta de un archivo  
**buf:** Buffer, puntero que señala al buffer que se almacenará la información.    
**count:** Counter, es el tamaño máximo de bytes que se almacenarán sobre el buffer.  
Retorna el número de bytes que se leyeron

2. Realice la compilación del código fuente adjunto y su ejecución empleando el aplicativo **strace**. Identifique las llamadas al sistema encargadas de enviar y recibir datos a través de la red. A partir de los manuales de Linux en Internet o del sistema operativo explique las llamadas al sistema encontradas y sus parámetros.

![][7]  

Para poder haber usado el comando strace, primero tuve que instalar otra librerias como gcc, que es el compilador de c usado para este ejemplo. 


**-CONNECT(sockfd,serv_addr,addrlen):** Creamos una conexión con una dirección remota  
**sockfd:** Descriptor de fichero de socket  
**serv_addr:**  Direccion IP y puerto remoto.  
**addrlen** Tamaño del serv_addr.  

**-SOCKET(domain,type,protocol):** Se abre un socket por la que se hará la comunicación  
**domain:** Dominio, conjunto de direcciones a las que se hace la conexión  
**type:** Tipo de socket al que nos conectamos.    
**protocol:** Protocolo usado para abrir el socket  

**-GETSOCKNAME(sockfd,addr,addrlen):** Retorna a la dirección a la que el sockfd apunta  
**sockfd:** Descriptor de fichero de socket.  
**addr:** Puntero del buffer al que sockfd esta ligado  
**addrlen.** Tamaño en bytes del buffer que apunta addr    
 
**-SENDTO(sockfd,msg,len,flags):** Envía un mensaje a un socket remoto  
**sockfd:** Descriptor de fichero de socket.  
**msg:** Puntero que señala los datos por enviar  
**len:** Longitud de los datos por enviar  
**flags:** Banderas del estado del mensaje    
Devuelve el número de bytes que se enviaron o -1 si hubo error en la transmisión.  

**-RECVFROM(sockfd, buf, len, flags, struct sockaddr from):** Se usa para recibir una cantidad de bytes de un host remoto  
**sockfd:** Descriptor de fichero de socket.  
**buff:** Buffer, representa el buffer donde se almacenará la información a medida que se reciba  
**len:** Tamaño máximo del buffer  
**flags:** Banderas del estado del mensaje  
**from:** Direccion IP y Socket del transmisor remoto  
Devuelve el número de bytes que se enviaron o -1 si hubo error en la transmisión.  




**Nota:** Cuando compile programas tenga en cuenta que estos pueden necesitar la instalación de librerías para su compilación y ejecución.

**Debian**
```
# apt-get install libcurl4-openssl-dev
$ gcc -o curl curl.c -lcurl
```
**CentOS**
```
# yum install libcurl-devel
$ gcc -o curl curl.c -lcurl

```

### Nota

El informe debe ser entregado en formato README.md y debe ser subido a un repositorio de github. El repositorio de github debe ser un fork de https://github.com/ICESI-Training/so-workshop3 y para la entrega deberá hacer un Pull Request (PR) respetando la estructura definida. El código fuente y la url de github deben incluirse en el informe.  

## Referencias
https://en.wikipedia.org/wiki/Write_(system_call)  
http://codewiki.wikidot.com/c:system-calls:open  
https://linux.die.net/man/2/open  
https://jvns.ca/blog/2014/09/18/you-can-be-a-kernel-hacker/  
http://man7.org/linux/man-pages/man2/access.2.html  
https://linux.die.net/man/2/close  
http://man7.org/linux/man-pages/man2/getsockname.2.html  
https://linux.die.net/man/2/sendto  
 
 [1]: imagenes/Llamados.png
 [2]: imagenes/Write.png
 [3]: imagenes/Open.png
 [4]: imagenes/Access.png
 [5]: imagenes/Close.png
 [6]: imagenes/Read.png
 [7]: imagenes/Llamados_curl.png
