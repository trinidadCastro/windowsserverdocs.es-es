---
title: start
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7388633681120442544adf4ee0e337d8599854
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441226"
---
# <a name="start"></a>start



Inicia una ventana de símbolo del sistema independiente para ejecutar un programa o comando especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|"\<Title >"|Especifica el título que se mostrará en la barra de título de la ventana de símbolo del sistema.|
|/d \<Path>|Especifica el directorio de inicio.|
|/i|El entorno de inicio de Cmd.exe se pasa a la nueva ventana de símbolo del sistema. Si **/i** no se especifica, se usa el entorno actual.|
|/min  \| /máx.|Especifica que se debe minimizar ( **/min**) o maximizar ( **/máx.** ) la nueva ventana de símbolo del sistema.|
|/ separar \| / shared|Inicia programas de 16 bits en un espacio de memoria independiente ( **/separar**) o compartir el espacio de memoria ( **/ shared**). Estas opciones no se admiten en plataformas de 64 bits.|
|/low \| /normal \| /high \| /realtime \| /abovenormal \| /belownormal|Inicia la aplicación en la clase de prioridad especificado. Los valores de clase de prioridad válido son **/baja**, **/normal**, **/alto**, **/realtime**, **/abovenormal**, y **/belownormal**.|
|/affinity \<HexAffinity >|Se aplica la máscara de afinidad de procesador especificado (expresada como un número hexadecimal) a la nueva aplicación.|
|/wait|Inicia la aplicación y espera a que finalice.|
|/b|Inicia la aplicación sin tener que abrir una nueva ventana de símbolo del sistema. Control de CTRL + C se omite a menos que la aplicación habilita el procesamiento de CTRL + C. Utilice CTRL+INTER para interrumpir la aplicación.|
|/b \<comando > \| \<programa >|Especifica el comando o el programa para iniciar.|
|\<Parameters>|Especifica los parámetros que se pasan al programa o comando.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Puede ejecutar archivos no ejecutables a través de su asociación de archivo, escriba el nombre del archivo como un comando.
- Al ejecutar un comando que contiene la cadena "CMD" como el primer token sin un calificador de extensión o ruta de acceso, "CMD" se reemplaza por el valor de la variable COMSPEC. Esto impide que los usuarios recoger **cmd** desde el directorio actual.
- Al ejecutar una aplicación de 32 bits gráfica de usuario (GUI) de la interfaz, **cmd** no espera la aplicación se cierre antes de volver a la línea de comandos. Este comportamiento no se produce si ejecuta la aplicación desde una secuencia de comandos.
- Al ejecutar un comando que usa un primer token que no tiene una extensión, Cmd.exe utiliza el valor de la variable de entorno PATHEXT para determinar qué extensiones se buscarán y en qué orden. El valor predeterminado de la variable PATHEXT es:  
  ```
  .COM;.EXE;.BAT;.CMD 
  ```  
  Tenga en cuenta que la sintaxis es igual que la variable de ruta de acceso con punto y coma separa cada extensión.
- Cuando busca un archivo ejecutable, si no hay ninguna coincidencia en cualquier extensión, **iniciar** comprueba si el nombre coincide con un nombre de directorio. Si es así, **iniciar** abre Explorer.exe en esa ruta de acceso.

## <a name="BKMK_examples"></a>Ejemplos

Para iniciar el programa Myapp en el símbolo del sistema y mantener el uso de la ventana de símbolo del sistema actual, escriba:
```
start myapp 
```
Para ver el **iniciar** maximizada de tema de Ayuda de línea de comandos en otra ventana del símbolo del sistema, escriba:
```
start /max start /?
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
