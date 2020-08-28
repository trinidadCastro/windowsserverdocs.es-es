---
title: start
description: Artículo de referencia de Start, que inicia una ventana de símbolo del sistema independiente para ejecutar un programa o un comando especificado.
ms.topic: reference
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f17b4093bbe82d869ad561dce45437389dc347e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036933"
---
# <a name="start"></a>start

Inicia una ventana de símbolo del sistema independiente para ejecutar un programa o un comando especificado.



## <a name="syntax"></a>Sintaxis

```
start [<Title>] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/elevate] [/b] [<Command> [<Parameter>... ] | <Program> [<Parameter>... ]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Title>|Especifica el título que se va a mostrar en la barra de título de la ventana del símbolo del sistema.|
|/d. \<Path>|Especifica el directorio de inicio.|
|/i|Pasa el Cmd.exe entorno de inicio a la nueva ventana del símbolo del sistema. Si no se especifica **/i** , se utiliza el entorno actual.|
|/min \| /Max|Especifica la minimización (**/min**) o la maximización (**/Max**) de la nueva ventana del símbolo del sistema.|
|/Separate \| /Shared|Inicia programas de 16 bits en un espacio de memoria independiente (**/separate**) o en un espacio de memoria compartido (**/Shared**). Estas opciones no se admiten en las plataformas de 64 bits.|
|/Low \| /normal \| /High \| /Realtime \| /abovenormal \| /BelowNormal|Inicia una aplicación en la clase de prioridad especificada. Los valores válidos de la clase Priority son **/Low**, **/normal**, **/High**, **/Realtime**, **/abovenormal**y **/BelowNormal**.|
|/affinity \<HexAffinity>|Aplica la máscara de afinidad de procesador especificada (expresada como un número hexadecimal) a la nueva aplicación.|
|/Wait|Inicia una aplicación y espera a que finalice.|
|/elevate|Ejecuta la aplicación como administrador.|
|/b|Inicia una aplicación sin abrir una nueva ventana del símbolo del sistema. El control de CTRL + C se omite a menos que la aplicación habilite el procesamiento de CTRL + C. Use CTRL + INTER para interrumpir la aplicación.|
|\<Command> \| \<Program>|Especifica el comando o el programa que se va a iniciar.|
|\<Parameter>...|Especifica los parámetros que se van a pasar al comando o al programa.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- Puede ejecutar archivos no ejecutables a través de su asociación de archivo escribiendo el nombre del archivo como un comando.
- Al ejecutar un comando que contiene la cadena CMD como el primer token sin un calificador de extensión o ruta de acceso, CMD se reemplaza por el valor de la variable comspec. Esto impide a los usuarios seleccionar **cmd** desde el directorio actual.
- Al ejecutar una aplicación de interfaz gráfica de usuario (GUI) de 32 bits, **cmd** no espera a que la aplicación se cierre antes de volver al símbolo del sistema. Este comportamiento no se produce si ejecuta la aplicación desde un script de comandos.
- Cuando se ejecuta un comando que usa un primer token que no contiene una extensión, Cmd.exe usa el valor de la variable de entorno PATHEXT para determinar qué extensiones se deben buscar y en qué orden. El valor predeterminado de la variable PATHEXT es:
  ```
  .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
  ```
  Tenga en cuenta que la sintaxis es la misma que la variable PATH, con puntos y comas que separan cada extensión.
- Cuando busca un archivo ejecutable, si no hay ninguna coincidencia en ninguna extensión, **inicie** comprobaciones para ver si el nombre coincide con un nombre de directorio. Si lo hace, **iniciar** abre Explorer.exe en esa ruta de acceso.

## <a name="examples"></a>Ejemplos

Para iniciar el programa MyApp en el símbolo del sistema y conservar el uso de la ventana del símbolo del sistema actual, escriba:
```
start myapp
```
Para ver el tema de ayuda de la línea de comandos de **Inicio** en una ventana del símbolo del sistema maximizada independiente, escriba:
```
start /max start /?
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
