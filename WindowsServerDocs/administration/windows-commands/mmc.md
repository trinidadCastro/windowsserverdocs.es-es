---
title: mmc
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bf9efe257e9e2b6cf20c28c1e6c0cf27230a6bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373603"
---
# <a name="mmc"></a>mmc

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Con las opciones de la línea de comandos de MMC, puede abrir una consola **MMC** específica, abrir **MMC** en modo de autor o especificar que se abra la versión de 32 bits o 64 bits de **MMC** .
## <a name="syntax"></a>Sintaxis
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parámetros

|       Parámetro        |                                                                                                 Descripción                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path> @ no__t-1<filename>.msc |        inicia **MMC** y abre una consola guardada. Debe especificar la ruta de acceso completa y el nombre de archivo para el archivo de consola guardado. Si no especifica un archivo de consola, **MMC** abrirá una nueva consola.         |
|           /a           |                                                               Abre una consola guardada en el modo de autor.  Se usa para realizar cambios en las consolas guardadas.                                                                |
|          /64           |                         Abre la versión de 64 bits de **MMC** (mmc64). Use esta opción solo si está ejecutando un sistema operativo de Microsoft 64 bits y desea usar un complemento de 64 bits.                          |
|          /32           | Abre la versión de 32 bits de **MMC** (mmc32). Al ejecutar un sistema operativo de Microsoft 64 bits, puede ejecutar complementos de 32 bits si abre MMC con esta opción de línea de comandos cuando tiene solo complementos de 32 bits. |
|           /?           |                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                     |

## <a name="remarks"></a>Comentarios
- Con la opción de línea de comandos <path> **\\** <filename> **. msc** puede usar variables de entorno para crear líneas de comandos o accesos directos que no dependan de la ubicación explícita de los archivos de consola. Por ejemplo, si la ruta de acceso a un archivo de consola está en la carpeta del sistema (por ejemplo, **MMC c:\winnt\system32\console_name.msc**), puede usar la cadena de datos expansible **% systemroot%** para especificar la ubicación (**mmc% systemroot% \ system32 \ console_ nombre. msc**). Esto puede resultar útil si está delegando tareas a personas de la organización que trabajan en equipos diferentes.
- Con la opción de línea de comandos **/A** cuando las consolas se abren con esta opción, se abren en el modo de autor, independientemente de su modo predeterminado. Esto no cambia de forma permanente la configuración de modo predeterminado de los archivos; Cuando se omite esta opción, MMC abre los archivos de consola según su configuración de modo predeterminado.
- Después de abrir **MMC** o un archivo de consola en el modo de autor, puede abrir cualquier consola existente haciendo clic en **abrir** en el menú de la **consola** .
- Puede usar la línea de comandos para crear accesos directos para abrir **MMC** y consolas guardadas. Un comando de línea de comandos funciona con el comando **Ejecutar** del menú **Inicio** , en cualquier ventana del símbolo del sistema, en accesos directos o en cualquier archivo o programa por lotes que llame al comando.
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

