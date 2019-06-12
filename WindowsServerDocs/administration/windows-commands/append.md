---
title: append
description: 'Tema de los comandos de Windows para '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe641e1336c163b5e98421a5fc32f8dbe64023b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435325"
---
# <a name="append"></a>append



Permite a los programas abrir archivos de datos en los directorios especificados como si estuvieran en el directorio actual. Si se utiliza sin parámetros, **anexar** muestra la lista de directorios agregados.

> [!NOTE]
> Este comando que no se admite en Windows 10.
>

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                 Descripción                                                                                 |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                                                                 Especifica una unidad y directorio que se anexará.                                                                  |
|       /x:on       |                                                  Aplica los directorios agregados a búsquedas de archivos y aplicaciones al iniciar.                                                  |
|      /x:off       |                                     Solo se aplica directorios agregados a las solicitudes para abrir archivos.</br>**/ x: off** es la configuración predeterminada.                                     |
|     /path:on      |                               Aplica los directorios agregados a las solicitudes de archivos que ya especifican una ruta de acceso. **/ Path: en** es la configuración predeterminada.                               |
|     /path:off     |                                                                    Desactiva el efecto de **/path: en**.                                                                    |
|        /e         | Almacena una copia de la lista de directorios agregados en una variable de entorno denominada APPEND. **/e** se puede usar solo la primera vez que use **anexar** después de iniciar el sistema. |
|         ;         |                                                                     Borra la lista de directorios agregados.                                                                     |
|        /?         |                                                                    Muestra la ayuda en el símbolo del sistema.                                                                     |

## <a name="BKMK_examples"></a>Ejemplos

Para borrar la lista de directorios agregados, escriba:
```
append ;
```
Para almacenar una copia del directorio anexado a una variable de entorno denominada APPEND, escriba:
```
append /e
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
