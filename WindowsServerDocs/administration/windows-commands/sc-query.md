---
title: Sc.exe consulta
description: Artículo de referencia del comando de consulta de sc.exe, que obtiene y muestra información sobre el servicio, el controlador, el tipo de servicio o el tipo de controlador especificados.
ms.topic: reference
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d5994c54914165cb79ba0f505f1a44b2d7f019a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388218"
---
# <a name="scexe-query"></a>Sc.exe consulta

Obtiene y muestra información sobre el servicio, el controlador, el tipo de servicio o el tipo de controlador especificados.

## <a name="syntax"></a>Sintaxis

```
sc.exe [<servername>] query [<servicename>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <Buffersize>] [ri= <Resumeindex>] [group= <groupname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<servername>` | Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, mi \\ Server). Para ejecutar SC.exe localmente, no utilice este parámetro. |
| `<servicename>` | Especifica el nombre de servicio devuelto por la operación **getkeyname** . Este parámetro de **consulta** no se utiliza junto con otros parámetros de **consulta** (excepto *ServerName*). |
| `type= {driver | service | all}` | Especifica lo que se va a enumerar. Entre estas opciones se incluyen:<ul><li>**driver** : especifica que solo se enumeran los controladores.</li><li>**servicio** : especifica que solo se enumeran los servicios. Este es el valor predeterminado.</li><li>**All** : especifica que se enumeran los controladores y los servicios.</li></ul> |
| `type= {own | share | interact | kernel | filesys | rec | adapt}` | Especifica el tipo de servicios o el tipo de controladores que se van a enumerar. Entre estas opciones se incluyen:<ul><li>**propietario** : especifica un servicio que se ejecuta en su propio proceso. No comparte un archivo ejecutable con otros servicios. Este es el valor predeterminado.</li><li>**compartir** : especifica un servicio que se ejecuta como un proceso compartido. Comparte un archivo ejecutable con otros servicios.</li><li>**kernel** : especifica un controlador.</li><li>**files** : especifica un controlador del sistema de archivos.</li><li>**rec** : especifica un controlador reconocido por el sistema de archivos que identifica los sistemas de archivos usados en el equipo.</li><li>**interactuar** : especifica un servicio que puede interactuar con el escritorio y recibir la entrada de los usuarios. Los servicios interactivos deben ejecutarse con la cuenta LocalSystem. Este tipo debe usarse junto con **Type = Own** o **Type = Shared** (por ejemplo, **Type = interactúe** **Type = Own**). El uso de **Type = interactúe** por sí mismo generará un error.</li></ul> |
| `state= {active | inactive | all}` | Especifica el estado Iniciado del servicio que se va a enumerar. Entre estas opciones se incluyen:<ul><li>**activo** : especifica todos los servicios activos. Este es el valor predeterminado.</li><li>**inactivo** : especifica todos los servicios en pausa o detenidos.</li><li>**All** : especifica todos los servicios.</li></ul> |
| `bufsize= <Buffersize>` | Especifica el tamaño (en bytes) del búfer de enumeración. El tamaño de búfer predeterminado es de 1.024 bytes. Debe aumentar el tamaño del búfer cuando la presentación resultante de una consulta supere los 1.024 bytes. |
| `ri= <Resumeindex>` | Especifica el número de índice en el que se va a iniciar o reanudar la enumeración. El valor predeterminado es 0 (cero). Si se devuelve más información de la que puede mostrar el búfer predeterminado, use este parámetro con el `bufsize=` parámetro. |
| `group= <Groupname>` | Especifica el grupo de servicio que se va a enumerar. De forma predeterminada, se enumeran todos los grupos. De forma predeterminada, se enumeran todos los grupos (* * grupo = * *). |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Cada opción de línea de comandos (parámetro) debe incluir el signo igual como parte del nombre de la opción.

- Se requiere un espacio entre una opción y su valor (por ejemplo, **Type = Own**. Si se omite el espacio, se produce un error en la operación.

- La operación de **consulta** muestra la siguiente información sobre un servicio: SERVICE_NAME (nombre de la subclave del registro del servicio), tipo, estado (así como Estados que no están disponibles), WIN32_EXIT_B, SERVICE_EXIT_B, punto de control y WAIT_HINT.

- El parámetro **Type =** puede usarse dos veces en algunos casos. La primera aparición del parámetro **Type =** especifica si se consultan los servicios, los controladores o ambos (**todos**). La segunda apariencia del parámetro **Type =** especifica un tipo de la operación de **creación** para restringir aún más el ámbito de una consulta.

- Cuando los resultados de la presentación de un comando de **consulta** superan el tamaño del búfer de enumeración, se muestra un mensaje similar al siguiente:

  ```
  Enum: more data, need 1822 bytes start resume at index 79

  To display the remaining **query** information, rerun **query**, setting **bufsize=** to be the number of bytes and setting **ri=** to the specified index. For example, the remaining output would be displayed by typing the following at the command prompt:

  sc.exe query bufsize= 1822 ri= 79
  ```

## <a name="examples"></a>Ejemplos

Para mostrar información solo para los servicios activos, escriba cualquiera de los siguientes comandos:

```
sc.exe query
sc.exe query type= service
```

Para mostrar información de los servicios activos y especificar un tamaño de búfer de 2.000 bytes, escriba:

```
sc.exe query type= all bufsize= 2000
```

Para mostrar información del servicio *wuauserv* , escriba:

```
sc.exe query wuauserv
```

Para mostrar información de todos los servicios (activo e inactivo), escriba:

```
sc.exe query state= all
```

Para mostrar información de todos los servicios (activos e inactivos), a partir de la línea 56, escriba:

```
sc.exe query state= all ri= 56
```

Para mostrar información de los servicios interactivos, escriba:

```
sc.exe query type= service type= interact
```

Para mostrar información solo para los controladores, escriba:

```
sc.exe query type= driver
```

Para mostrar información de los controladores en el *grupo de especificación de interfaz de controlador de red (NDIS)*, escriba:

```
sc.exe query type= driver group= NDIS
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
