---
title: red
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 970a5d6403804db7585bf3895dbb61eead286c37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384383"
---
# <a name="san"></a>red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra o establece la Directiva de red de área de almacenamiento (San) para el sistema operativo.
> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Parámetros

|                          Parámetro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| directiva = { &#124; offlineAll &#124; offlineShared}] | Establece la Directiva de San para el sistema operativo que se está iniciando actualmente. La Directiva San determina si un disco recién detectado se pone en línea o permanece sin conexión y si se convierte en lectura/escritura o permanece de solo lectura. Cuando un disco está sin conexión, se puede leer el diseño del disco, pero no se muestran los dispositivos de volumen a través de Plug and Play. Esto significa que no se puede montar ningún sistema de archivos en el disco. Cuando un disco está en línea, se instalan uno o varios dispositivos de volumen para el disco. La siguiente es una explicación de cada parámetro:<br /><br />-    en**lineal**. Especifica que todos los discos recién detectados se conectarán y se convertirán en lectura/escritura. **AÚN**     La especificación **de un** servidor que comparte discos podría provocar daños en los datos. Por lo tanto, no debe establecer esta Directiva si los discos se comparten entre servidores a menos que el servidor forme parte de un clúster.<br />-   **offlineAll**. Especifica que todos los discos recién detectados, excepto el disco de inicio, estarán sin conexión de forma predeterminada ANDREAD-only.<br />-   **offlineShared**. Especifica que todos los discos recién detectados que no residen en un bus compartido (como SCSI e iSCSI) se ponen en línea y se convierten en de lectura y escritura. Los discos que quedan sin conexión serán de solo lectura de forma predeterminada.<br /><br />para obtener más información, vea [enumeración VDS_san_POLICY](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            Noerr                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Se usa solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentarios
- Si el comando se proporciona sin parámetros, se muestra la Directiva de San actual.
  ## <a name="BKMK_Examples"></a>Example
  Para ver la Directiva actual, escriba:
  ```
  san
  ```
  Para que todos los discos recién detectados, excepto el disco de inicio, sin conexión y de solo lectura de forma predeterminada, escriba:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
