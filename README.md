# 1.-Putty  
https://github.com/MARIAEL/PROCESSWIRE-PUTTY#descargar-putty  
Para windows usar putty [video](https://youtu.be/lxKQ3Sq47mc)  

#2.- Terminal crear web.  
Preparación del entorno, esto se hace únicamente la primera vez.   
```sh
# Debemos asegurarnos de estar en la ruta correcta (/home/bitnami), vamos a comprobarlo
$ cd   
$ pwd          #debemos estar en /home/bitnami

# Descargamos una sola vez el script para crear la estructura de la web y BD
$ wget https://raw.githubusercontent.com/manviny/EC2/master/PwScripts.sh && sudo chmod +x PwScripts.sh  && ./PwScripts.sh
```
###Estos son los pasos que seguimos cada vez que queramos crear una web.
```sh
# Ahora podemos crear una nueva web con: sudo ./creaPW.sh seguido de nombreWeb y DBpass (dbpass es mi contraseña de bitnami)
$ sudo ./creaPW.sh miweb dbpass  
```  

#3.- Configurar desde el navegador  

Ahora tenemos la web disponible para configurar  
1. Ir al navegador y ponemos la url: usuario.bitnamiapp.com/miweb    
2. Rellenamos los datos de la base de datos **DB Name**, **DB USer** y **DB Pass** con los datos generados en el script anterior (están en la pantalla de putty).  
3. **Default Time Zone** seleccionamos Europe/Madrid  
4. Pasamos a la siguiente pantalla y en **Admin Login URL** ponemos **admin**  
5. En **User**, **Password**,  **mail**, ponemos nuestros datos para poder acceder al administrador   
6. (Pe. User: minombre; Pass:exclusive; Email: el de gmail)  
6. Volver al terminal y escribir ```sudo ./finalizaPW.sh miweb```   


**Para BORRAR una web** ```sudo ./borraPW.sh miweb``` (USAR ESTE COMANDO CON PRECAUCIÓN porque nos borraría toda la web con sus archivos, fotos, etc.  )   

### Si nos aparece el aviso en rojo "This request ... to be forged"
```sh
# Abrir config.php  

$ cd ./apps/miweb/htdocs/site
$ sudo nano config.php  

# al final del texto poner

  $config->protectCSRF = false;  
  
# Para guardar Ctrl+X y luego Y+enter  

```

###Instalar Angular con un HTML mínimo  
  
1. Módulos > nuevo > nombre de la clase > Pages2JSON     
2. Pegar este código (**Pages2JSON**) en nuestra web  
3. En cada plantilla (pe. home.php) debemos editarla y en la última pestaña (Pages2JSON) indicarle que campos serán visibles  
 
Editar las siguientes páginas como sigue:  

**_init.php**  

```php
<html>
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular-route.min.js"></script>

    <script>var app=angular.module('myApp',['ngRoute'])</script>
  </head>
  <body ng-app="myApp">
```
**home.php**  

```php
<script>
  app.controller('HomeCtrl',function($scope){
      $scope.page=<?php echo $page->toJSON() ?>;
  });
</script>

<div ng-controller="HomeCtrl">
	<h1>hi there {{page.title}}</h1>
</div>
```
**_main.php**  

```php
  </body>
</html>
```  


## Conectar sublime text con servidor creado con bitnami [video](https://youtu.be/mAgvZ-dyPWQ)


## Script para instalar web en servidor creado con bitnami [enlace](https://processwire.com/talk/topic/9858-script-to-create-new-pw-in-bitnami-stack/)

## Preparar sublime text para acceder a los ficheros de nuestra web en AWS
1. Descarga la llave .ppk desde nuestra consola de bitnami (servidor > manage > connect)

## Arrastrar a sublime text los ficheros que necesitamos para nuestra nueva web.
1. Arrastramos la carpeta templates hasta sublime text  C:/bitnami/processwire/apps/processwire/htdocs/site/templates   

## Preparamos la cabecera y pie compartido en todas las páginas
1. Borramos el contenido de _init.php, _main.php y home.php   
2. Vamos a instalar una plantilla, para ello abrimos en sublime text cualquier html de la plantilla.  
3. Copiamos en _init.php la parte de la cabecera que compartiran todas las páginas de nuestra web. Si abrimos el inspector veremos muchas lineas en rojo, pues no encuentra los ficheros css, volveremos sobre este aspecto luego.
4. Copiamos la parte del pie que se repetirá en todas las páginas en _main.php


## Instalar todos los ficheros CSS y JS necesarios para esta plantilla
1. Arrastramos todos los css a nuestra carpeta de styles y los js a scripts ( por SFTP hacer upload folder)
2. Hacer lo mismo con font e images
3. Ahora en todas las lineas, tanto de css como de js sustituiremos de la siguiente forma
```php
    # donde ponga por ejemplo: ccs/bootstrap.min.css  =>  
    <?php echo $config->urls->templates?>css/bootstrap.min.css  
    # 
```

## logo y fuentes
1. Si tenemos un logo o fuentes debemos seguir los mismos pasos que en el caso de css y js  

## Páginas
Ahora podemos ya empezar a crear cada página de nuestra web.  
La página principal es home.php (inicio, portada)  


## Cambiar el idioma [video](https://youtu.be/lWXvyRH2tpw)
1. En el menú principal: Modules > Core y activar "Languages Support"
2. Ahora desde el menu principal ir a Setup y Languages, add new
3. tanto es title como name poner **es**
4. En la nueva ventana que se ha abierto, pulsar el texto rojo a mitad de pantalla "language packs"
5. Pinchar en Spanish(es-ES) v.2 y al final de la página darle a "Download this module"
6. Arrastramos el fichero descargado a  Site Translation Files y en pocos segundo podemos pinchar en Save
7. Ahora necesitamos decirle que nuestro idioma preferido es el español
8. Vamos al menu Access > users y pinchamos en el nuestro
9. Al final seleccionamos es y ya tenemos el idioma preferido.

## Instalar angular
 ```bash
# Necesitamos instalar el módulo Pages2JSON
# Crear carpeta PwAngular en C:/bitnami/processwire/apps/MIWEB/htdocs/site/modules
$ cd /home/bitnami/apps/MIWEB/htdocs/site/modules
$ mkdir PwAngular
$ cd PwAngular
$ wget https://github.com/manviny/processgular/archive/master.zip
$ unzip master.zip
Poner el contenido de https://github.com/manviny/processgular/tree/master
```
 ## Vamos a probarlo en home.php
 ```php
 
    ## Pegar este código
<script>
    app.controller('HomeCtrl', function ($scope, PW) {

     	$scope.saluda = function(){
     		toastr.info( 'Bienvenido '  ,{timeOut:4000});
     	}

    });
</script>

<div class="container" ng-controller="HomeCtrl">
	<button ng-click="saluda()">saluda</button>
</div>
 ```

#4.- Crear campos (instalar módulos)  
#5.- Crear plantilla  
#6.- Crear páginas  
#7.- Conectar Sublime text  
#8.- Crear código plantilla    
