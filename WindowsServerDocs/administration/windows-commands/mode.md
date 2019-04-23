---
title: mode
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde0e9786f72823f446202f1c87ad8e9e181d29c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848206"
---
# <a name="mode"></a>mode



Muestra el estado del sistema, cambia la configuración del sistema o vuelve a configurar puertos o dispositivos. Si se utiliza sin parámetros, **modo** muestra todos los atributos controlables de la consola y los dispositivos de COM disponibles.

Puede usar **modo** para realizar las tareas siguientes, cada tarea utiliza una sintaxis diferente:
-   [Para configurar un puerto de comunicaciones en serie](#BKMK_1)
-   [Para mostrar el estado de todos los dispositivos o de un solo dispositivo](#BKMK_2)
-   [Para redirigir la salida de un puerto paralelo a un puerto de comunicaciones en serie](#BKMK_3)
-   [Para seleccionar, actualizar o mostrar los números de las páginas de códigos para la consola](#BKMK_4)
-   [Para cambiar el tamaño del búfer de pantalla de símbolo del sistema](#BKMK_5)
-   [Para establecer la velocidad del teclado](#BKMK_6)

## <a name="BKMK_1"></a>Para configurar un puerto de comunicaciones en serie

### <a name="syntax"></a>Sintaxis

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Com\<M > [:]|Especifica el número de puerto de comunicaciones Prncnfg.vbshronous async.|
|velocidad en baudios =\<B >|Especifica la velocidad de transmisión en bits por segundo. En la tabla siguiente se enumera las abreviaturas válidas para *B* y sus tipos relacionados.</br>-   **11** = 110 baudios</br>-   **15** = 150 baudios</br>-   **30** = 300 baudios</br>-   **60** = 600 baudios</br>-   **12** = 1200 baudios</br>-   **24** = 2400 baudios</br>-   **48** = 4800 baudios</br>-   **96** = 9600 baud</br>-   **19** = 19.200 baudios|
|parity=\<P>|Especifica cómo el sistema usa el bit de paridad para comprobar los errores de transmisión. La tabla siguiente enumeran los valores válidos para *P*. El valor predeterminado es **e**. No todos los equipos admiten los valores **m** y **s**.</br>-   **n** = ninguno</br>-   **e** = even</br>-   **o** = odd</br>-   **m** = mark</br>-   **s** = espacio|
|data=\<D>|Especifica el número de bits de datos en un carácter. Los valores válidos para **d.** están en el intervalo de 5 a 8. El valor predeterminado es 7. No todos los equipos admiten los valores 5 y 6.|
|stop=\<S>|Especifica el número de bits de parada que define el final de un carácter: 1, 1,5 ó 2. Si la velocidad en baudios es 110, el valor predeterminado es 2. En caso contrario, el valor predeterminado es 1. No todos los equipos admiten el valor 1.5.|
|a = {on | off}|Especifica si el procesamiento de tiempo de espera infinito está activado o desactivado. El valor predeterminado es.|
|xon={on | off}|Especifica si el protocolo xon o xoff para el control de flujo de datos está activado o desactivado.|
|odsr={on | off}|Especifica si el protocolo de enlace de salida que utiliza el circuito de conjunto de datos preparado (DSR) está activado o desactivado.|
|octs={on | off}|Especifica si el protocolo de enlace de salida que utiliza el circuito Listo para enviar (CTS) está activado o desactivado.|
|dtr={on | Desactivado | hs}|Especifica si el circuito de Terminal de datos preparado (DTR) está activada o desactivada o establecido en el protocolo de enlace.|
|rts={on | Desactivado | hs | tg}|Especifica si el circuito de la solicitud de envío (RTS) está establecido en on, off, protocolo de enlace o botón de alternancia.|
|idsr={on | off}|Especifica si la sensibilidad DSR circuito está activado o desactivado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_2"></a>Para mostrar el estado de todos los dispositivos o de un solo dispositivo

### <a name="syntax"></a>Sintaxis

```
mode [<Device>] [/status]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Dispositivo >|Especifica el nombre del dispositivo para el que desea mostrar el estado.|
|/status|Solicita el estado del puerto serie. Se puede abreviar el **/Status** la opción de línea de comandos como **/sta**.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios

Si se utiliza sin parámetros, **modo** muestra el estado de todos los dispositivos que están instalados en el sistema.

## <a name="BKMK_3"></a>Para redirigir la salida de un puerto paralelo a un puerto de comunicaciones en serie

### <a name="syntax"></a>Sintaxis

```
mode lpt<N>[:]=com<M>[:]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|LPT\<N > [:]|Obligatorio. Especifica el puerto paralelo. Los valores válidos para *N* están comprendidos entre 1 y 3.|
|com\<M > [:]|Obligatorio. Especifica el puerto serie. Los valores válidos para *M* están en el intervalo de 1 a 4.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios

Debe ser miembro del grupo Administradores para redirigir la impresión.

### <a name="examples"></a>Ejemplos

Para configurar el sistema para que lo envía los resultados de la impresora en paralelo a una impresora de la serie, se debe utilizar el **modo** dos veces el comando. La primera vez, utilice **modo** para configurar el puerto serie. La segunda vez, utilice **modo** redirigir la salida de la impresora en paralelo al puerto serie que especificó en la primera **modo** comando.

Por ejemplo, si la impresora serie funciona a 4800 baudios con paridad par y está conectado al puerto COM1 (la primera conexión serie del equipo), escriba:
```
mode com1 48,e,,,b
mode lpt1=com1
```
Si se redirige la salida de la impresora en paralelo de LPT1 a COM1, pero, a continuación, decide desea imprimir un archivo mediante el uso de LPT1, escriba el siguiente comando antes de imprimir el archivo:
```
mode lpt1
```
Este comando impide la redirección del archivo de LPT1 a COM1.

## <a name="BKMK_4"></a>Para seleccionar, actualizar o mostrar los números de las páginas de códigos para la consola

### <a name="syntax"></a>Sintaxis

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Dispositivo >|Obligatorio. Especifica el dispositivo para el que desea seleccionar una página de códigos. CON es el nombre solo es válido para un dispositivo.|
|Seleccione la página de códigos =|Obligatorio. Especifica qué página de códigos para usar con el dispositivo especificado. Se puede abreviar **codepage** **seleccione** como **cp** **sel**.|
|\<YYY &GT;|Obligatorio. Especifica el número de página de códigos que seleccione. La siguiente lista muestra cada código página que se admite y su país o región o idioma.</br>437: Estados Unidos</br>850: Multilingüe (Latín I)</br>852: Eslavo (Latín II)</br>855: Cirílico (ruso)</br>857: Turco</br>860: Portugués</br>861: Islandés</br>863: Y el francés canadiense</br>865: Nordic</br>866: Ruso</br>869: Griego moderno|
|página de códigos|Obligatorio. Muestra los números del código de páginas (si existe) que están seleccionados para el dispositivo especificado.|
|/status|Muestra los números de las páginas de códigos actual seleccionados para el dispositivo especificado. Se puede abreviar **/Status** a **/sta**. Si especifica **/Status**, **modo codepage** muestra los números de las páginas de códigos que están seleccionados para el dispositivo especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_5"></a>Para cambiar el tamaño del búfer de pantalla de símbolo del sistema

### <a name="syntax"></a>Sintaxis

```
mode con[:] [cols=<C>] [lines=<N>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|con [:]|Obligatorio. Indica que el cambio se aplica a la ventana de símbolo del sistema.|
|cols=\<C>|Especifica el número de columnas en el búfer de pantalla de símbolo del sistema.|
|lines=\<N>|Especifica el número de líneas en el búfer de pantalla de símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_6"></a>Para establecer la velocidad del teclado

### <a name="syntax"></a>Sintaxis

```
mode con[:] [rate=<R> delay=<D>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|con [:]|Obligatorio. Hace referencia al teclado.|
|rate=\<R>|Especifica la velocidad a la que se repite un carácter en la pantalla cuando se mantiene presionada una tecla.|
|delay=\<D>|Especifica la cantidad de tiempo que debe transcurrir después de presionar y mantener presionada una tecla antes de que se repite la salida de caracteres.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios

-   La velocidad del teclado es la velocidad a la que se repite un carácter cuando se mantiene presionada la tecla correspondiente a ese carácter. La velocidad del teclado tiene dos componentes, la velocidad y el retraso. Algunos teclados no reconocen este comando.
-   Uso de **tasa = *** R*

    Los valores válidos están comprendidos entre 1 y 32. Estos valores son iguales a aproximadamente 2 y 30 caracteres por segundo. El valor predeterminado es 20 para teclados compatibles con IBM AT y 21 para teclados compatibles con PS/2 de IBM. Si establece la velocidad, también debe establecer el retraso.
-   Uso de **retraso**=*d.*

    Los valores válidos para *d.* son 1, 2, 3 y 4 (que representan 0,25, 0,50, 0,75 y 1 segundo). El valor predeterminado es 2. Si establece el retraso, también debe establecer la velocidad.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)