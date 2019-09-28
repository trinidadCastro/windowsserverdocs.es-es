---
title: bootcfg query
description: 'El tema de comandos de Windows para la **consulta Bootcfg** : consulta y muestra las entradas de la sección [cargador de arranque] y [sistemas operativos] de boot. ini.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae82357cfe178343872448c2ebd46c49a797b5a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379914"
---
# <a name="bootcfg-query"></a>bootcfg query

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta y muestra las entradas de la sección [cargador de arranque] y [sistemas operativos] de boot. ini.

## <a name="syntax"></a>Sintaxis
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parámetros

|        Término         |                                                                                             Definición                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain> @ no__t-1 @ no__t-2 | Ejecuta el comando con los permisos de cuenta del usuario especificado por <User>or <Domain> @ no__t-2 @ no__t-3. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
|    /p <Password>    |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                        |
|         /?          |                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                 |

##### <a name="remarks"></a>Comentarios
- A continuación se muestra un ejemplo de la salida de **bootcfg/Query** :
  ```
  Boot Loader Settings
  ----------
  timeout: 30
  default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  Boot Entries
  ------
  Boot entry ID:   1
  Friendly Name:   ""
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- La parte de configuración del cargador de arranque del resultado de la **consulta Bootcfg** muestra cada entrada de la sección [boot loader] de boot. ini.
- La parte de entradas de arranque del resultado de la **consulta Bootcfg** muestra los detalles siguientes para cada entrada del sistema operativo en la sección [operating systems] de boot. ini: IDENTIFICADOR de la entrada de arranque, nombre descriptivo, ruta de acceso y opciones de carga del sistema operativo.
  ## <a name="BKMK_examples"></a>Example
  En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/Query** :
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
