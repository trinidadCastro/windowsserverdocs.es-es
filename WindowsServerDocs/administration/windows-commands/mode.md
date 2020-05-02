---
title: mode
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bd7875687aa112c500a966e0ebbcb5af142eb83
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723958"
---
# <a name="mode"></a>mode



Muestra el estado del sistema, cambia la configuración del sistema o vuelve a configurar puertos o dispositivos. Si se usa sin parámetros, el **modo** muestra todos los atributos controlables de la consola de y los dispositivos com disponibles.

Puede usar el **modo** para realizar las siguientes tareas: cada tarea usa una sintaxis diferente:
-   [Para configurar un puerto de comunicaciones serie](#BKMK_1)
-   [Para mostrar el estado de todos los dispositivos o de un solo dispositivo](#BKMK_2)
-   [Para redirigir la salida de un puerto paralelo a un puerto de comunicaciones serie](#BKMK_3)
-   [Para seleccionar, actualizar o mostrar los números de las páginas de códigos de la consola](#BKMK_4)
-   [Para cambiar el tamaño del búfer de pantalla del símbolo del sistema](#BKMK_5)
-   [Para establecer la velocidad de teclado](#BKMK_6)

## <a name="to-configure-a-serial-communications-port"></a><a name=BKMK_1></a>Para configurar un puerto de comunicaciones serie

### <a name="syntax"></a>Sintaxis

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>Parámetros

|  Parámetro  |                                                                                                                                                                                     Descripción                                                                                                                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| >\<com M [:]  |                                                                                                                                                      Especifica el número del puerto de comunicaciones de Async prncnfg. vbshronous.                                                                                                                                                      |
|  Baud =\<B>  | Especifica la velocidad de transmisión en bits por segundo. En la tabla siguiente se enumeran las abreviaturas válidas para *B* y sus tarifas relacionadas.</br>-   **11** = 110 baudios</br>-   **15** = 150 baudios</br>-   **30** = 300 baudios</br>-   **60** = 600 baudios</br>-   **12** = 1200 baudios</br>-   **24** = 2400 baudios</br>-   **48** = 4800 baudios</br>-   **96** = 9600 baudios</br>-   **19** = 19.200 baudios |
| Parity =\<P> |                              Especifica cómo el sistema utiliza el bit de paridad para comprobar los errores de transmisión. En la tabla siguiente se enumeran los valores válidos para *P*. El valor predeterminado es **e**. No todos los equipos admiten los valores **m** y **s**.</br>-   **n** = ninguno</br>-   **e** = par</br>-   **o** = impar</br>-   **m** = marca</br>-   **s** = espacio                              |
|  Data =\<D>  |                                                                                                    Especifica el número de bits de datos en un carácter. Los valores válidos para **d** están en el intervalo comprendido entre 5 y 8. El valor predeterminado es 7. No todos los equipos admiten los valores 5 y 6.                                                                                                     |
|  detener =\<S>  |                                                                                  Especifica el número de bits de parada que definen el final de un carácter: 1, 1,5 o 2. Si la velocidad en baudios es 110, el valor predeterminado es 2. De lo contrario, el valor predeterminado es 1. No todos los equipos admiten el valor 1,5.                                                                                   |
|   en = {activado    |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|   Xon = {activado   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|  ODSR = {activado   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|  PTU = {activado   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|   DTR = {activado   |                                                                                                                                                                                         apagado                                                                                                                                                                                         |
|   RTS = {activado   |                                                                                                                                                                                         apagado                                                                                                                                                                                         |
|  IDSR = {activado   |                                                                                                                                                                                        off}                                                                                                                                                                                         |
|     /?      |                                                                                                                                                                        Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                         |

## <a name="to-display-the-status-of-all-devices-or-of-a-single-device"></a><a name=BKMK_2></a>Para mostrar el estado de todos los dispositivos o de un solo dispositivo

### <a name="syntax"></a>Sintaxis

```
mode [<Device>] [/status]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> del dispositivo|Especifica el nombre del dispositivo para el que desea mostrar el estado.|
|/status|Solicita el estado de las impresoras paralelas redirigidas. Puede abreviar la opción de línea de comandos **/status** como **/STA**.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Observaciones

Si se usa sin parámetros, el **modo** muestra el estado de todos los dispositivos instalados en el sistema.

## <a name="to-redirect-output-from-a-parallel-port-to-a-serial-communications-port"></a><a name=BKMK_3></a>Para redirigir la salida de un puerto paralelo a un puerto de comunicaciones serie

### <a name="syntax"></a>Sintaxis

```
mode lpt<N>[:]=com<M>[:]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|LPT\<N> [:]|Necesario. Especifica el puerto paralelo. Los valores válidos para *N* están en el intervalo comprendido entre 1 y 3.|
|>\<com M [:]|Necesario. Especifica el puerto serie. Los valores válidos para *M* están en el intervalo comprendido entre 1 y 4.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Observaciones

Debe ser miembro del grupo administradores para redirigir la impresión.

### <a name="examples"></a>Ejemplos

Para configurar el sistema de modo que envíe la salida de la impresora en paralelo a una impresora serie, debe utilizar el comando **modo** dos veces. La primera vez, use el **modo** para configurar el puerto serie. La segunda vez, utilice el **modo** para redirigir la salida de la impresora paralela al puerto serie especificado en el primer comando de **modo** .

Por ejemplo, si la impresora serie funciona a 4800 baudios con paridad uniforme y está conectada al puerto COM1 (la primera conexión serie del equipo), escriba:
```
mode com1 48,e,,,b
mode lpt1=com1
```
Si redirige la salida de la impresora paralela de LPT1 a COM1, pero después decide que desea imprimir un archivo con LPT1, escriba el siguiente comando antes de imprimir el archivo:
```
mode lpt1
```
Este comando impide el redireccionamiento del archivo de LPT1 a COM1.

## <a name="to-select-refresh-or-display-the-numbers-of-the-code-pages-for-the-console"></a><a name=BKMK_4></a>Para seleccionar, actualizar o mostrar los números de las páginas de códigos de la consola

### <a name="syntax"></a>Sintaxis

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> del dispositivo|Necesario. Especifica el dispositivo para el que desea seleccionar una página de códigos. Con es el único nombre válido para un dispositivo.|
|Página de códigos Select =|Necesario. Especifica la página de códigos que se va a usar con el dispositivo especificado. Puede abreviar la **Página de códigos** **de la selección** como **CP** **SEL**.|
|\<YYY>|Necesario. Especifica el número de la página de códigos que se va a seleccionar. En la siguiente lista se muestran todas las páginas de códigos admitidas y su país o región o idioma.</br>437: Estados Unidos</br>850: multilingüe (Latín I)</br>852: Eslava (Latín II)</br>855: cirílico (Ruso)</br>857: Turco</br>860: Portugués</br>861: Islandés</br>863: Canadá-francés</br>865: Nordic</br>866: Ruso</br>869: Griego moderno|
|codepage|Necesario. Muestra los números de las páginas de códigos (si existen) que se han seleccionado para el dispositivo especificado.|
|/status|Muestra los números de las páginas de códigos actuales seleccionadas para el dispositivo especificado. Puede abreviar **/status** para **/STA**. Independientemente de si se especifica **/status**, el **modo CodePage** muestra los números de las páginas de códigos que se seleccionan para el dispositivo especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="to-change-the-size-of-the-command-prompt-screen-buffer"></a><a name=BKMK_5></a>Para cambiar el tamaño del búfer de pantalla del símbolo del sistema

### <a name="syntax"></a>Sintaxis

```
mode con[:] [cols=<C>] [lines=<N>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|con [:]|Necesario. Indica que el cambio se aplica a la ventana del símbolo del sistema.|
|cols =\<C>|Especifica el número de columnas en el búfer de pantalla del símbolo del sistema.|
|líneas =\<N>|Especifica el número de líneas en el búfer de pantalla del símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="to-set-the-keyboard-typematic-rate"></a><a name=BKMK_6></a>Para establecer la velocidad de teclado

### <a name="syntax"></a>Sintaxis

```
mode con[:] [rate=<R> delay=<D>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|con [:]|Necesario. Hace referencia al teclado.|
|rate =\<R>|Especifica la velocidad a la que se repite un carácter en la pantalla cuando se mantiene presionada una tecla.|
|Delay =\<D>|Especifica la cantidad de tiempo que transcurrirá después de presionar y mantener presionada una tecla antes de que se repita la salida de caracteres.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Observaciones

- La velocidad del teclado es la velocidad a la que se repite un carácter cuando se mantiene presionada la tecla para ese carácter. La velocidad de los intermitencias tiene dos componentes: la velocidad y el retraso. Algunos teclados no reconocen este comando.
- Usar **Rate =**<em>R</em>

  Los valores válidos están en el intervalo comprendido entre 1 y 32. Estos valores son aproximadamente de 2 a 30 caracteres por segundo. El valor predeterminado es 20 para los teclados compatibles con IBM y 21 para los teclados compatibles con IBM PS/2. Si establece la velocidad, también debe establecer el retraso.
- Usar **retraso**=*D*

  Los valores válidos para *D* son 1, 2, 3 y 4 (que representan 0,25, 0,50, 0,75 y 1 segundo). El valor predeterminado es 2. Si establece el retraso, también debe establecer la velocidad.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)