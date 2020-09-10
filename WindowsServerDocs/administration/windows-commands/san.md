---
title: san
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d0104bad1cf93e4ec33db23ec088e41eb6348d07
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637167"
---
# <a name="san"></a>san

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra o establece la Directiva de red de área de almacenamiento (San) para el sistema operativo.
> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
#### <a name="parameters"></a>Parámetros

|                          Parámetro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Directiva = &#124; {offlineAll &#124; offlineShared}] | Establece la Directiva de San para el sistema operativo que se está iniciando actualmente. La Directiva San determina si un disco recién detectado se pone en línea o permanece sin conexión y si se convierte en lectura/escritura o permanece de solo lectura. Cuando un disco está sin conexión, se puede leer el diseño del disco, pero no se muestran los dispositivos de volumen a través de Plug and Play. Esto significa que no se puede montar ningún sistema de archivos en el disco. Cuando un disco está en línea, se instalan uno o varios dispositivos de volumen para el disco. La siguiente es una explicación de cada parámetro:<p>-   **lineal**. Especifica que todos los discos recién detectados se conectarán y se convertirán en lectura/escritura. **Importante:**     La especificación **de un** servidor que comparte discos podría provocar daños en los datos. Por lo tanto, no debe establecer esta Directiva si los discos se comparten entre servidores a menos que el servidor forme parte de un clúster.<br />-   **offlineAll**. Especifica que todos los discos recién detectados, excepto el disco de inicio, estarán sin conexión de forma predeterminada ANDREAD-only.<br />-   **offlineShared**. Especifica que todos los discos recién detectados que no residen en un bus compartido (como SCSI e iSCSI) se ponen en línea y se convierten en de lectura y escritura. Los discos que quedan sin conexión serán de solo lectura de forma predeterminada.<p>para obtener más información, vea [enumeración de VDS_san_POLICY](https://go.microsoft.com/fwlink/?LinkId=203815) ( <https://go.microsoft.com/fwlink/?LinkId=203815> ). |
|                            noerr                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Observaciones
- Si el comando se proporciona sin parámetros, se muestra la Directiva de San actual.
  ## <a name="examples"></a>Ejemplos
  Para ver la Directiva actual, escriba:
  ```
  san
  ```
  Para que todos los discos recién detectados, excepto el disco de inicio, sin conexión y de solo lectura de forma predeterminada, escriba:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
