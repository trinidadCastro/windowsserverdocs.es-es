---
title: mmc
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c168a9f4d91422f3877fd0c210c76248d1172b00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723968"
---
# <a name="mmc"></a>mmc

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Con las opciones de la línea de comandos de MMC, puede abrir una consola **MMC** específica, abrir **MMC** en modo de autor o especificar que se abra la versión de 32 bits o 64 bits de **MMC** .
## <a name="syntax"></a>Sintaxis
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
#### <a name="parameters"></a>Parámetros

|       Parámetro        |                                                                                                 Descripción                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>. msc |        inicia **MMC** y abre una consola guardada. Debe especificar la ruta de acceso completa y el nombre del archivo de consola guardado. Si no especifica un archivo de consola, **MMC** abrirá una nueva consola.         |
|           /a           |                                                               Abre una consola guardada en el modo de autor.  Se usa para realizar cambios en las consolas guardadas.                                                                |
|          /64           |                         Abre la versión de 64 bits de **MMC** (mmc64). Utilice esta opción sólo si ejecuta un sistema operativo de Microsoft de 64 bits y desea utilizar un complemento de 64 bits.                          |
|          /32           | Abre la versión de 32 bits de **MMC** (mmc32). Al ejecutar un sistema operativo de Microsoft 64 bits, puede ejecutar complementos de 32 bits si abre MMC con esta opción de línea de comandos cuando tiene solo complementos de 32 bits. |
|           /?           |                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                     |

## <a name="remarks"></a>Observaciones
- Mediante la <path> **\\** <filename>opción de línea de comandos **. msc** puede usar variables de entorno para crear líneas de comandos o accesos directos que no dependan de la ubicación explícita de los archivos de consola. Por ejemplo, si la ruta de acceso a un archivo de consola está en la carpeta del sistema (por ejemplo, **MMC c:\winnt\system32\ console_name. msc**), puede usar la cadena de datos expansibles **% systemroot%** para especificar la ubicación (**mmc% systemroot% \ system32 \ console_name. msc**). Esto puede resultar útil si delega tareas en usuarios de la organización que trabajan en equipos distintos.
- Con la opción de línea de comandos **/A** cuando las consolas se abren con esta opción, se abren en el modo de autor, independientemente de su modo predeterminado. Esto no cambia de forma permanente la configuración de modo predeterminado de los archivos; Cuando se omite esta opción, MMC abre los archivos de consola según su configuración de modo predeterminado.
- Después de abrir **MMC** o un archivo de consola en el modo de autor, puede abrir cualquier consola existente haciendo clic en **abrir** en el menú de la **consola** .
- Puede usar la línea de comandos para crear accesos directos para abrir **MMC** y consolas guardadas. Un comando de línea de comandos funciona con el comando **Ejecutar** del menú **Inicio** , en cualquier ventana del símbolo del sistema, en accesos directos o en cualquier archivo o programa por lotes que llame al comando.
  ## <a name="additional-references"></a>Referencias adicionales
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

