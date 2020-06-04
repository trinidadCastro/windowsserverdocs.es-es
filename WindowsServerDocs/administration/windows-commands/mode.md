---
title: mode
description: Tema de referencia del comando modo, que muestra el estado del sistema, cambia la configuración del sistema o vuelve a configurar puertos o dispositivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4c895c59bb527b8bfb6973a72d0d4e163cb2ace
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354585"
---
# <a name="mode"></a>mode

Muestra el estado del sistema, cambia la configuración del sistema o vuelve a configurar puertos o dispositivos. Si se usa sin parámetros, el **modo** muestra todos los atributos controlables de la consola de y los dispositivos com disponibles.

## <a name="serial-port"></a>Puerto serie

Configura un puerto de comunicaciones serie y establece el protocolo de enlace de salida.

### <a name="syntax"></a>Sintaxis

```
mode com<m>[:] [baud=<b>] [parity=<p>] [data=<d>] [stop=<s>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>Parámetros

| Parámetro  | Descripción |
| ---------- | ----------- |
| `com<m>[:]` | Especifica el número del puerto de comunicaciones de Async prncnfg. vbshronous. |
| `baud=<b>`  | Especifica la velocidad de transmisión en bits por segundo. Los valores válidos son:<ul><li>**11** -110 baudios</li><li>**15** a 150 baudios</li><li>**30** a 300 baudios</li><li>**60** -600 baudios</li><li>**12** a 1200 baudios</li><li>**24** a 2400 baudios</li><li>**48** -4800 baudios</li><li>**96** -9600 baudios</li><li>**19** a 19.200 baudios</li></ul> |
| `parity=<p>` | Especifica cómo el sistema utiliza el bit de paridad para comprobar los errores de transmisión. Los valores válidos son:<ul><li>**n** -ninguno</li><li>**e** -Even (valor predeterminado)</li><li>**o** -impar</li><li>marca **m**</li><li>espacio de **s**</li></ul>No todos los dispositivos admiten el uso de los parámetros **m** o **s** . |
| `data=<d>` | Especifica el número de bits de datos en un carácter. Los valores válidos oscilan entre **5** y **8**. El valor predeterminado es **7**. No todos los dispositivos admiten los valores **5** y **6**. |
| `stop=<s>` | Especifica el número de bits de parada que definen el final de un carácter: **1**, **1,5**o **2**. Si la velocidad en baudios es **110**, el valor predeterminado es **2**. De lo contrario, el valor predeterminado es **1**. No todos los dispositivos admiten el valor **1,5**. |
| `to={on | off}` | Especifica si el dispositivo utiliza el procesamiento de tiempo de espera infinito. El valor predeterminado es **OFF**. La activación **de** esta opción significa que el dispositivo nunca dejará de esperar para recibir una respuesta de un host o equipo cliente. |
| `xon={on | off}` | Especifica si el sistema permite el protocolo XON/XOFF. Este protocolo proporciona control de flujo para las comunicaciones en serie, mejorando la confiabilidad, pero reduciendo el rendimiento. |
| `odsr={on | off}` | Especifica si el sistema activa el protocolo de enlace de salida de conjunto de datos preparado (DSR). |
| `octs={on | off}` | Especifica si el sistema activa el protocolo de enlace de salida Clear to send (CTS). |
| `dtr={on | off | hs}` | Especifica si el sistema activa el protocolo de enlace de salida de terminal de datos preparado (DTR). Al establecer este valor en el modo **on** , se proporciona una señal constante para mostrar que el terminal está listo para enviar datos. Al establecer este valor en el modo **HS** se proporciona una señal de protocolo de enlace entre los dos terminales. |
| `rts={on | off | hs | tg}` | Especifica si el sistema activa el protocolo de enlace de salida de la solicitud de envío (RTS). Al establecer este valor en el modo **on** , se proporciona una señal constante para mostrar que el terminal está listo para enviar datos. Al establecer este valor en el modo **HS** se proporciona una señal de protocolo de enlace entre los dos terminales. Establecer este valor en el modo **TG** proporciona una manera de alternar entre los Estados listo y no listo. |
| `idsr={on | off}` | Especifica si el sistema activa la sensibilidad de DSR. Debe activar esta opción para utilizar el protocolo de enlace DSR.
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="device-status"></a>Estado del dispositivo

Muestra el estado de un dispositivo especificado. Si se usa sin parámetros, el **modo** muestra el estado de todos los dispositivos instalados en el sistema.

### <a name="syntax"></a>Sintaxis

```
mode [<device>] [/status]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<device>` | Especifica el nombre del dispositivo para el que desea mostrar el estado. Los nombres estándar incluyen, LPT1: a LPT3:, COM1: a COM9: y con. |
| /status | Solicita el estado de las impresoras paralelas redirigidas. También puede usar **/STA** como una versión abreviada de este comando. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="redirect-printing"></a>Impresión de redireccionamiento

Redirige la salida de la impresora. Debe ser miembro del grupo administradores para redirigir la impresión.

> [!NOTE]
Para configurar el sistema de modo que envíe la salida de la impresora en paralelo a una impresora serie, debe utilizar el comando **modo** dos veces. La primera vez, debe usar el **modo** para configurar el puerto serie. La segunda vez, debe usar el **modo** para redirigir la salida de la impresora paralela al puerto serie especificado en el primer comando de **modo** .

### <a name="syntax"></a>Sintaxis

```
mode LPT<n>[:]=COM<m>[:]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| LPT `<n>` [:] | Especifica el número de LPT que se va a configurar. Normalmente, esto significa proporcionar un valor de **LTP1: a LTP3:**, a menos que el sistema incluya compatibilidad especial con el puerto paralelo. Este parámetro es obligatorio.|
| COM `<m>` [:] | Especifica el puerto COM que se va a configurar. Normalmente, esto significa proporcionar un valor de **COM1: a COM9:**, a menos que el sistema tenga hardware especial para puertos com adicionales. Este parámetro es obligatorio. |
| /? | Muestra la ayuda en el símbolo del sistema.|

#### <a name="examples"></a>Ejemplos

Para redirigir una impresora serie que funcione a 4800 baudios con paridad uniforme y que esté conectada al puerto COM1 (la primera conexión serie del equipo), escriba:

```
mode com1 48,e,,,b
mode lpt1=com1
```

Para redirigir la salida de la impresora paralela de LPT1 a COM1 y, a continuación, imprimir un archivo con LPT1, escriba el siguiente comando antes de imprimir el archivo:

```
mode lpt1
```

Este comando impide el redireccionamiento del archivo de LPT1 a COM1.

## <a name="select-code-page"></a>Seleccionar página de códigos

Configura o consulta la información de la página de códigos para un dispositivo seleccionado.

### <a name="syntax"></a>Sintaxis

```
mode <device> codepage select=<yyy>
mode <device> codepage [/status]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<device>` |Especifica el dispositivo para el que desea seleccionar una página de códigos. Con es el único nombre válido para un dispositivo. Este parámetro es obligatorio. |
| codepage | Especifica la página de códigos que se va a usar con el dispositivo especificado. También puede usar **CP** como una versión abreviada de este comando. Este parámetro es obligatorio. |
| seleccionar =`<yyy>` | Especifica el número de la página de códigos que se va a usar con el dispositivo. Las páginas de códigos admitidas, por país o región o idioma, incluyen:<ul><li>**437:** Estados Unidos</li><li>**850:** Multilingüe (Latín I)</li><li>**852:** Eslava (Latín II)</li><li>**855:** Cirílico (Ruso)</li><li>**857:** Turco</li><li>**860:** Portugués</li><li>**861:** Islandés</li><li>**863:** Canadá (Francés)</li><li>**865:** Guantes</li><li>**866:** Ruso</li><li>**869:** Griego moderno</li></ul> Este parámetro es obligatorio. |
| /status | Muestra los números de las páginas de códigos actuales seleccionadas para el dispositivo especificado. También puede usar **/STA** como una versión abreviada de este comando. Independientemente de si especifica **/status**, el comando **modo CodePage** mostrará los números de las páginas de códigos seleccionadas para el dispositivo especificado. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="display-mode"></a>Modo de pantalla

Cambia el tamaño del búfer de pantalla del símbolo del sistema

### <a name="syntax"></a>Sintaxis

```
mode con[:] [cols=<c>] [lines=<n>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| con [:] | Indica que el cambio se aplica a la ventana del símbolo del sistema. Este parámetro es obligatorio. |
| columnas =`<c>` | Especifica el número de columnas en el búfer de pantalla del símbolo del sistema. El valor predeterminado es 80 columnas, pero puede establecerlo en cualquier valor. Si no usa el valor predeterminado, los valores típicos son 40 y 135 columnas. El uso de valores no estándar puede dar lugar a problemas en la aplicación del símbolo del sistema. |
| líneas =`<n>` | Especifica el número de líneas en el búfer de pantalla del símbolo del sistema. El valor predeterminado es 25, pero puede establecerlo en cualquier valor. Si no usa el valor predeterminado, el otro valor típico es 50 líneas. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="typematic-rate"></a>Velocidad de los intermitencias

Establece la velocidad del teclado. La velocidad de intermitencia es la velocidad a la que Windows puede repetir un carácter al presionar la tecla en un teclado.

> [!NOTE]
> Algunos teclados no reconocen este comando.

### <a name="syntax"></a>Sintaxis

```
mode con[:] [rate=<r> delay=<d>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| con [:] | Especifica el teclado. Este parámetro es obligatorio. |
| tasa =`<r>` | Especifica la velocidad a la que se repite un carácter en la pantalla cuando se mantiene presionada una tecla. El valor predeterminado es 20 caracteres por segundo para los teclados compatibles de IBM y 21 para los teclados compatibles con IBM PS/2, pero puede usar cualquier valor entre 1 y 32. Si establece este parámetro, también debe establecer el parámetro **Delay** .|
| retraso =`<d>` | Especifica la cantidad de tiempo que transcurrirá después de presionar y mantener presionada una tecla antes de que se repita la salida de caracteres. El valor predeterminado es 2 (. 50 segundos), pero también puede usar 1 (. 25 segundos), 3 (. 75 segundos) o 4 (1 segundo). Si establece este parámetro, también debe establecer el parámetro **Rate** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)