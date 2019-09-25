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
Se creó un repositorio en GitHub, y se sincronizó la carpeta *TP1* del proyecto *firmware_v2* utilizando los siguientes comandos
```sh
$ git init
$ git remote add origin 
$ git status
$ git add .
$ git commit -m ""
$ git push -u origin master
```
![Imagen 09 repositorio](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/09_repositorio.png)

# 2 Switches leds

## 2.1 compilacion condicional

Para realizar la compilacion condicional de los codigos fuentes de TP_1 y TP_2 se declarararon previamente las etiquetas TEST(asignando la etiqueta correspondinte al codigo a compilar) TP_1 y TP_2.Luego se utilizaron las directivas del preprocesador #if(TEST == TP1_1) #endif para el codigo de blinky y #if(TEST == TP1_2) #endif para el codigo de blinkyswitches_leds.
En la figura se puede ver como se sombrea la estructura que no va a ser compilada(TP_1).

![Imagen Switches_leds_compilacion_condicional_2_a_1](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/Switches_leds_compilacion_condicional_2_a_1.png)

## 2.2.a funciones

Se pueden visualizar en la siguiente figura las funciones:

boardConfig(); -- configura los pines de entrada y salida de placa
gpioConfig( GPIO0, GPIO_INPUT ); -- configura el pin del primer parametro(GPIO0) en el modo ingresado como segundo parametro(GPIO_INPUT entrada)
valor = !gpioRead( TEC1 ); -- lee el valor actual del pin introducido como parametro(TEC1) y retorna FALSE el estado es 1.
gpioWrite( LEDB, valor ); -- asigna el estado valor al pin introducido en el primer parametro(LEDB)

![Imagen Switches_leds_funciones_2_b_1](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/Switches_leds_funciones_2_b_1.png)

## 2.2.b constantes,variables

Se pueden visualizar en la siguiente figura las funciones:

GPIO0        -- pin correspondiente a GPIO0
GPIO_INPUT   -- estado de entrada de un pin GPIO
GPIO1        -- pin correspondiente a GPIO0
GPIO_OUTPUT  -- estado de salida de un pin GPIO
TEC1         -- primer pulsador
LEDB         -- pin correspondiente a LED azul
valor        -- variable booleana que se utiliza para el valor de estado de cada pulsador
TEC2         -- segundo pulsador
LED1         -- pin correspondiente a LED amarillo
TEC3         -- tercer pulsador
LED2         -- pin correspondiente a LED rojo
TEC4         -- cuarto pulsador
LED3         -- pin correspondiente a LED verde



![Imagen Switches_leds_variables_constantes_2_b_2](https://raw.githubusercontent.com/DarioCapu/TP1/master/Imagenes/Switches_leds_variables_constantes_2_b_2.png)


# 3 tickHook

# 4 Portabilidad

# 5 Mensajes de depuración por puerto serie

# 6 Sensado de Push Buttons
