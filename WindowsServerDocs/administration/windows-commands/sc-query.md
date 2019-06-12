---
title: Consulta de SC
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b6e945c4b2944f97d40cbc27694acc2915c615
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441624"
---
# <a name="sc-query"></a>Consulta de SC



Obtiene y muestra información sobre el servicio especificado, controlador, tipo de servicio o tipo de controlador.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>Parámetros

|       Parámetro        |                                                                                                                          Descripción                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName>      |                       Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe tener el formato de convención de nomenclatura Universal (UNC) (por ejemplo, \\ \\myserver). Para ejecutar SC.exe localmente, omita este parámetro.                        |
|     \<ServiceName>     |                                      Especifica el nombre del servicio devolviendo por la **getkeyname** operación. Esto **consulta** parámetro no se utiliza junto con otras **consulta** parámetros (distintos de *ServerName*).                                      |
|     tipo = {driver      |                                                                                                                            servicio                                                                                                                            |
|       type= {own       |                                                                                                                             Compartir                                                                                                                             |
|     estado = {activa     |                                                                                                                           inactivo                                                                                                                            |
| bufsize = \<BufferSize > |                     Especifica el tamaño (en bytes) del búfer de enumeración. El tamaño de búfer predeterminado es 1.024 bytes. Debe aumentar el tamaño del búfer de enumeración cuando la presentación resultante de una consulta supera los 1024 bytes.                      |
|   RI = \<índiceReanudación >   | Especifica el número de índice en el que la enumeración es iniciar o reanudar. El valor predeterminado es **0** (cero). Utilice este parámetro junto con el **bufsize =** parámetro cuando una consulta devuelve más información que puede mostrar el búfer de forma predeterminada. |
|  grupo = \<GroupName >   |                                                                             Especifica el grupo de servicio que hay que enumerar. De forma predeterminada, se enumeran todos los grupos (**grupo = ""** ).                                                                              |
|           /?           |                                                                                                             Muestra la ayuda en el símbolo del sistema.                                                                                                              |

## <a name="remarks"></a>Comentarios

- Sin un espacio entre el parámetro y su valor (es decir, **tipo = propio**, no **tipo = propio**), se producirá un error en la operación.
- El **consulta** operación muestra la siguiente información acerca de un servicio: SERVICE_NAME (nombre de la subclave del servicio en el registro), TYPE, STATE (así como los Estados que no están disponibles), WIN32_EXIT_B, SERVICE_EXIT_B, CHECKPOINT y WAIT_HINT.
- El **tipo =** parámetro se puede utilizar dos veces en algunos casos. La primera aparición de la **tipo =** parámetro especifica si se debe consultar los servicios, controladores o ambos (**todas**). El segundo aspecto de la **tipo =** parámetro especifica un tipo de la **crear** operación para restringir aún más el ámbito de la consulta.
- Cuando la presentación resultante de un **consulta** comando supera el tamaño del búfer de enumeración, se muestra un mensaje similar al siguiente:  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  Para mostrar los restantes **consulta** información, vuelva a ejecutar **consulta**, estableciendo **bufsize =** al número de bytes y configuración **ri =** a la índice especificado. Por ejemplo, la salida restante se mostraría escribiendo lo siguiente en el símbolo del sistema:  
  ```
  sc query bufsize= 1822 ri= 79
  ```

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar información de servicios activos solo, escriba cualquiera de los siguientes comandos:
```
sc query
sc query type= service
```
Para mostrar la información de servicios activos y para especificar un tamaño de búfer de 2.000 bytes, escriba:
```
sc query type= all bufsize= 2000
```
Para mostrar información para el servicio WUAUSERV, escriba:
```
sc query wuauserv
```
Para mostrar información para todos los servicios (activos e inactivos), escriba:
```
sc query state= all
```
Para mostrar información para todos los servicios (activos e inactivos), a partir de la línea de 56, escriba:
```
sc query state= all ri= 56
```
Para mostrar información de servicios interactivos, escriba:
```
sc query type= service type= interact
```
Para mostrar información de sólo con controladores, escriba:
```
sc query type= driver
```
Para mostrar la información de controladores en el grupo de especificación de interfaz de controlador de red (NDIS), escriba:
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)