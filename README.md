**66.48 SEMINARIO DE SISTEMAS EMBEBIDOS - FIUBA**
- Darío Capucchio 85119
- Cristian García 89040
# TP1
**Objetivo**
- **Uso del IDE** edición, compilación y depuración de programas.
- **Uso de GPIO** manejo de Salidas y Entradas Digitales.
- **Documentación**
# 1 IDE
Se descargó LPCXpreso 8.2.0 y se insatalaron los complementos de OpenOCD, eGit y Yakindu StateChart Tools siguiendo los pasos de la [hoja de ayuda](https://campus.fi.uba.ar/pluginfile.php/307047/mod_resource/content/5/Sistemas_Embebidos-2019_2doC-Instalacion_de_Herramientas-Cruz.pdf) de la materia.
![Imagen 00 Activación](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/00_Instalacion_Activacion.png)

![Imagen 01 Complementos](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/01_Complementos.png)

## 1.1 Firmware
Utilizando git se descargó el repositorio *firmware_v2* y se copió el archivo *project.mk.template* a *project.mk* utilizando los siguientes comandos:
```sh
$ git clone https://github.com/ciaa/firmware_v2.git
$ cd firmware_v2
$ git status -s
$ git checkout master
$ cp project.mk.template project.mk
```

Luego se ejecutó el LPCXpreso y se creó un workspace con el nombre *workspace-TP1* y se agregó el *firmware_v2*. Nos dirigimos a *File--New--Other--C/C++--Makefile Project with Existing Code...*
- En **Existing Code Location Browse** se colocó la ubicación de la carpeta *firmware_v2*.
- Se destildó el casillero **C++**.
- Se seleccionó la opción **NPX MCU Tools**.

El archivo project.mk se configuró de la siguiente manera:
- PROJECT = sapi_examples/edu-ciaa-nxp/bare_metal/gpio/gpio_02_blinky
- TARGET = lpc4337_m4
- BOARD = edu_ciaa_nxp

![Imagen 02 arbol](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/02_firmware_v2.png)

## 1.2 Debug
Antes de realizar el debug, se realizó la operación **Build**. Se hizo click derecho sobre la carpeta *firmware_v2* y en el menú se seleccionó la opción **Build**.
Para realizar el **Debug** se hizo click derecho sobre la carpeta *firmware_v2* y en el menú se seleccionó *Debug As--Debug Configurrations...*.

![Imagen 03 debug config](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/03_debug_config_1.png)
![Imagen 04 debug config](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/04_debug_config_2.png)

Para realizar acciones en el modo debug utilizamos los botones de la barra de tareas, o con los atajos:
 > F5 Step Into
 > F6 Step Over

![Imagen 05 debug acciones](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/05_debug_acciones.png)

Para modificar el código se cambia a la vista de modo debug a modo developer.
![Imagen 06 debug vistas](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/06_debug_vistas.png)

## 1.3 Migrar blinky a TP1
Se copiaron los archivos del ejemplo blinky a la carpeta *projects/TP1* y se le cambió el nombre de los archivos .c y .h a *TP1*. 

![Imagen 07 blinky](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/07_blinky_TP1.png)

Para hacer un debug primero se cambió, en la ventana de *debug configurations*, la opción *C/C++ Application*

![Imagen 08 TP1_debug](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/08_TP1_debug.png)

Haciendo *Step Into* en la función *boardConfig()* se abre el archivo *sapi_board.c*.

Función *gpioWrite(gpioMap_t,bool_t)*
```sh
bool_t gpioWrite(gpioMap_t pin, bool_t value){ // La función recibe el pin y el estado
    
    bool_t ret_val     = 1;   // Valor de retorno

   int8_t pinNamePort = 0;    // Inicializa todas las variables en cero
   int8_t pinNamePin  = 0;

   int8_t func        = 0;

   int8_t gpioPort    = 0;
   int8_t gpioPin     = 0;

   gpioObtainPinConfig( pin, &pinNamePort, &pinNamePin, &func, &gpioPort, &gpioPin );

   Chip_GPIO_SetPinState( LPC_GPIO_PORT, gpioPort, gpioPin, value);

   return ret_val;
}
```

## 1.4 Repositorio
![Imagen 09 repositorio](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/09_repositorio.png)

# 2 Switches leds

# 3 tickHook

# 4 Portabilidad

# 5 Mensajes de depuración por puerto serie

# 6 Sensado de Push Buttons
