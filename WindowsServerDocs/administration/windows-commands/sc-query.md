---
title: Consulta SC. exe
description: Obtenga información acerca de cómo obtener información acerca de los servicios, los controladores, el tipo de servicios o el tipo de controladores mediante la utilidad SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86aabbbc42c965b72f317a3bfaa99acc99c46f3b
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850046"
---
# <a name="scexe-query"></a>Consulta SC. exe

Obtiene y muestra información sobre el servicio, el controlador, el tipo de servicio o el tipo de controlador especificados.

## <a name="syntax"></a>Sintaxis

```
sc.exe [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

### <a name="parameters"></a>Parámetros

|       Parámetro        |                                                                                                                          Descripción                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<NombreDeServidor>      |                       Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por \\ \\ejemplo, mi Server). Para ejecutar SC. exe localmente, omita este parámetro.                        |
|     \<ServiceName>     |                                      Especifica el nombre de servicio devuelto por la operación **getkeyname** . Este parámetro de **consulta** no se utiliza junto con otros parámetros de **consulta** (excepto *ServerName*).                                      |
|     tipo = {controlador      |                                                                                                                            service                                                                                                                            |
|       tipo = {propietario       |                                                                                                                             Compartir                                                                                                                             |
|     estado = {activo     |                                                                                                                           inactivo                                                                                                                            |
| bufsize = \<BufferSize> |                     Especifica el tamaño (en bytes) del búfer de enumeración. El tamaño de búfer predeterminado es de 1.024 bytes. Debe aumentar el tamaño del búfer de enumeración cuando la presentación resultante de una consulta supera los 1.024 bytes.                      |
|   RI = \<ResumeIndex>   | Especifica el número de índice en el que se va a iniciar o reanudar la enumeración. El valor predeterminado es **0** (cero). Utilice este parámetro junto con el parámetro **bufsize =** cuando una consulta devuelva más información de la que puede mostrar el búfer predeterminado. |
|  Grupo = \<Nombredegrupo>   |                                                                             Especifica el grupo de servicio que se va a enumerar. De forma predeterminada, se enumeran todos los grupos (* * grupo = * *).                                                                              |
|           /?           |                                                                                                             Muestra la ayuda en el símbolo del sistema.                                                                                                              |

## <a name="remarks"></a>Observaciones

- Sin un espacio entre un parámetro y su valor (es decir, **Type = Own**, no **Type = Own**), se producirá un error en la operación.
- La operación de **consulta** muestra la siguiente información sobre un servicio: SERVICE_NAME (nombre de la subclave del registro del servicio), tipo, estado (así como Estados que no están disponibles), WIN32_EXIT_B, SERVICE_EXIT_B, punto de control y WAIT_HINT.
- El parámetro **Type =** puede usarse dos veces en algunos casos. La primera aparición del parámetro **Type =** especifica si se consultan los servicios, los controladores o ambos (**todos**). La segunda apariencia del parámetro **Type =** especifica un tipo de la operación de **creación** para restringir aún más el ámbito de una consulta.
- Cuando la pantalla resultante de un comando de **consulta** supera el tamaño del búfer de enumeración, se muestra un mensaje similar al siguiente:  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  Para mostrar la información de **consulta** restante, vuelva a ejecutar la **consulta**, estableciendo **bufsize =** para que sea el número de bytes y establezca **RI =** en el índice especificado. Por ejemplo, la salida restante se mostraría escribiendo lo siguiente en el símbolo del sistema:  
  ```
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
Para mostrar información del servicio WUAUSERV, escriba:
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
Para mostrar información de los controladores en el grupo de especificación de interfaz de controlador de red (NDIS), escriba:
```
sc.exe query type= driver group= ndis
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
