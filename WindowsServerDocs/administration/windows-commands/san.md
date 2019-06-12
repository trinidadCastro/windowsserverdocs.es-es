---
title: SAN
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 90b6cec9e44ae91b21932c1e4c46a33e0b5da5e4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441677"
---
# <a name="san"></a>SAN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra o establece la directiva de red (san) del área de almacenamiento para el sistema operativo.
> [!NOTE]
> Este comando sólo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Parámetros

|                          Parámetro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Directiva = {onlineAll &#124; offlineAll &#124; offlineShared}] | Establece la directiva san del sistema operativo arrancado actualmente. La directiva san determina si un disco recién detectado se pone en línea o permanece sin conexión y, si pasa a ser de lectura/escritura o sigue siendo de solo lectura. Cuando un disco está sin conexión, se puede leer el diseño de disco, pero no hay ningún dispositivo de volumen se expone a través de Plug and Play. Esto significa que ningún sistema de archivos se puede montar en el disco. Cuando un disco está en línea, se instalan uno o varios dispositivos de volumen para el disco. El siguiente es una explicación de cada parámetro:<br /><br />-   **onlineAll**. Especifica que todos los recién detectados se desplazará discos en línea y se convierten de lectura/escritura. **IMPORTANTE:**     Especificar **onlineAll** en un servidor que comparte discos podría provocar daños en los datos. Por lo tanto, no debe establecer esta directiva si los discos se comparten entre los servidores a menos que el servidor forma parte de un clúster.<br />-   **offlineAll**. Especifica todos los recién detectados discos, salvo el disco de inicio estará sin conexión solo andread de forma predeterminada.<br />-   **offlineShared**. Especifica que todos recién detectan discos que no residen en un bus compartido (por ejemplo, iSCSI y SCSI) se ponen en línea y se realizan la lectura y escritura. Los discos que quedan sin conexión es de solo lectura de forma predeterminada.<br /><br />Para obtener más información, consulte [VDS_san_POLICY enumeración](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            noerr                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Se utiliza sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentarios
- Si no se especifica el comando sin parámetros, se muestra la directiva san actual.
  ## <a name="BKMK_Examples"></a>Ejemplos
  Para ver la directiva actual, escriba:
  ```
  san
  ```
  Para que todos los discos recién detectados, excepto el disco de inicio, sin conexión y de solo lectura de forma predeterminada, escriba:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
