---
title: start
description: Artículo de referencia del comando START, que inicia una ventana de símbolo del sistema independiente para ejecutar un programa o un comando especificado.
ms.topic: reference
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6c154cd0c3cab4520f64c77d2ecc920dfc8bdcc9
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718272"
---
# <a name="start"></a>start

Inicia una ventana de símbolo del sistema independiente para ejecutar un programa o un comando especificado.

## <a name="syntax"></a>Sintaxis

```
start [<title>] [/d <path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <hexaffinity>] [/wait] [/elevate] [/b] [<command> [<parameter>... ] | <program> [<parameter>... ]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<title>` | Especifica el título que se va a mostrar en la barra de título de la ventana **del símbolo del sistema** . |
| /d. `<path>` | Especifica el directorio de inicio. |
| /i | Pasa el Cmd.exe entorno de inicio a la nueva ventana **del símbolo del sistema** . Si no se especifica **/i** , se utiliza el entorno actual. |
| `{/min | /max}` | Especifica la minimización (**/min**) o la maximización (**/Max**) de la nueva ventana del **símbolo del sistema** . |
| `{/separate | /shared}` | Inicia programas de 16 bits en un espacio de memoria independiente (**/separate**) o en un espacio de memoria compartido (**/Shared**). Estas opciones no se admiten en las plataformas de 64 bits. |
| `{/low | /normal | /high | /realtime | /abovenormal | belownormal}` | Inicia una aplicación en la clase de prioridad especificada. |
| /affinity `<hexaffinity>` | Aplica la máscara de afinidad de procesador especificada (expresada como un número hexadecimal) a la nueva aplicación. |
| /Wait | Inicia una aplicación y espera a que finalice. |
| /elevate | Ejecuta la aplicación como administrador. |
| /b | Inicia una aplicación sin abrir una nueva ventana **del símbolo del sistema** . El control de CTRL + C se omite a menos que la aplicación habilite el procesamiento de CTRL + C. Use CTRL + INTER para interrumpir la aplicación. |
| `[<command> [<parameter>... ] | <program> [<parameter>... ]]` | Especifica el comando o el programa que se va a iniciar. |
| `<parameter>` | Especifica los parámetros que se van a pasar al comando o al programa. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Puede ejecutar archivos no ejecutables a través de su asociación de archivo escribiendo el nombre del archivo como un comando.

- Si ejecuta un comando que contiene la cadena CMD como el primer token sin un calificador de extensión o ruta de acceso, CMD se reemplaza por el valor de la variable comspec. Esto impide a los usuarios seleccionar **cmd** desde el directorio actual.

- Si ejecuta una aplicación de interfaz gráfica de usuario (GUI) de 32 bits, **cmd** no espera a que la aplicación se cierre antes de volver al símbolo del sistema. Este comportamiento no se produce si ejecuta la aplicación desde un script de comandos.

- Si ejecuta un comando que usa un primer token que no contiene una extensión, Cmd.exe usa el valor de la variable de entorno PATHEXT para determinar qué extensiones se deben buscar y en qué orden. El valor predeterminado de la variable PATHEXT es:

  ```
  .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
  ```

  Tenga en cuenta que la sintaxis es la misma que la variable PATH, con punto y coma (;) separar cada extensión.

- Al buscar un archivo ejecutable, si no hay ninguna coincidencia en ninguna extensión, **inicie** comprobaciones para ver si el nombre coincide con un nombre de directorio. Si lo hace, **iniciar** abre Explorer.exe en esa ruta de acceso.

## <a name="examples"></a>Ejemplos

Para iniciar el programa *MyApp* en el símbolo del sistema y conservar el uso de la ventana del **símbolo del sistema** actual, escriba:

```
start Myapp
```

Para ver el tema de ayuda de la línea de comandos de **Inicio** en una ventana **del símbolo del sistema** maximizada independiente, escriba:

```
start /max start /?
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
