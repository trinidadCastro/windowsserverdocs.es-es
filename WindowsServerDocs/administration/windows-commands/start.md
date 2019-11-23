---
title: start
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1fb0875c972f8259b47f48ef84ed486fc678d8b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370891"
---
# <a name="start"></a>start



Inicia una ventana de símbolo del sistema independiente para ejecutar un programa o un comando especificado.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|"título de\<>"|Especifica el título que se va a mostrar en la barra de título de la ventana del símbolo del sistema.|
|/d \<ruta de acceso >|Especifica el directorio de inicio.|
|/i|Pasa el entorno de inicio de cmd. exe a la nueva ventana del símbolo del sistema. Si no se especifica **/i** , se utiliza el entorno actual.|
|/min \|/Max|Especifica la minimización ( **/min**) o la maximización ( **/Max**) de la nueva ventana del símbolo del sistema.|
|/Separate \|/Shared|Inicia programas de 16 bits en un espacio de memoria independiente ( **/separate**) o en un espacio de memoria compartido ( **/Shared**). Estas opciones no se admiten en las plataformas de 64 bits.|
|/Low \|/normal \|/High \|/Realtime \|/abovenormal \|/BelowNormal|Inicia una aplicación en la clase de prioridad especificada. Los valores válidos de la clase Priority son **/Low**, **/normal**, **/High**, **/Realtime**, **/abovenormal**y **/BelowNormal**.|
|/Affinity \<HexAffinity >|Aplica la máscara de afinidad de procesador especificada (expresada como un número hexadecimal) a la nueva aplicación.|
|/Wait|Inicia una aplicación y espera a que finalice.|
|b|Inicia una aplicación sin abrir una nueva ventana del símbolo del sistema. El control de CTRL + C se omite a menos que la aplicación habilite el procesamiento de CTRL + C. Use CTRL + INTER para interrumpir la aplicación.|
|/b \<> de comandos \| \<programa >|Especifica el comando o el programa que se va a iniciar.|
|\<parámetros >|Especifica los parámetros que se van a pasar al comando o programa.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- Puede ejecutar archivos no ejecutables a través de su asociación de archivo escribiendo el nombre del archivo como un comando.
- Cuando se ejecuta un comando que contiene la cadena "CMD" como primer token sin un calificador de extensión o ruta de acceso, "CMD" se reemplaza por el valor de la variable comspec. Esto impide a los usuarios seleccionar **cmd** desde el directorio actual.
- Al ejecutar una aplicación de interfaz gráfica de usuario (GUI) de 32 bits, **cmd** no espera a que la aplicación se cierre antes de volver al símbolo del sistema. Este comportamiento no se produce si ejecuta la aplicación desde un script de comandos.
- Cuando se ejecuta un comando que usa un primer token que no contiene una extensión, cmd. exe usa el valor de la variable de entorno PATHEXT para determinar qué extensiones se deben buscar y en qué orden. El valor predeterminado de la variable PATHEXT es:  
  ```
  .COM;.EXE;.BAT;.CMD 
  ```  
  Tenga en cuenta que la sintaxis es la misma que la variable PATH, con puntos y comas que separan cada extensión.
- Cuando busca un archivo ejecutable, si no hay ninguna coincidencia en ninguna extensión, **inicie** comprobaciones para ver si el nombre coincide con un nombre de directorio. Si lo hace, **iniciar** abre Explorer. exe en esa ruta de acceso.

## <a name="BKMK_examples"></a>Example

Para iniciar el programa MyApp en el símbolo del sistema y conservar el uso de la ventana del símbolo del sistema actual, escriba:
```
start myapp 
```
Para ver el tema de ayuda de la línea de comandos de **Inicio** en una ventana del símbolo del sistema maximizada independiente, escriba:
```
start /max start /?
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
