---
title: san
description: Artículo de referencia del comando San, que muestra o establece la Directiva de red de área de almacenamiento (San) para el sistema operativo.
ms.topic: reference
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8881855e77f8df579cb054954d26296edf4c971f
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388374"
---
# <a name="san"></a>san

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra o establece la Directiva de red de área de almacenamiento (San) para el sistema operativo. Si se usa sin parámetros, se muestra la Directiva de San actual.

## <a name="syntax"></a>Sintaxis

```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| Directiva = {lineAll|offlineAll|offlineShared}] | Establece la Directiva de San para el sistema operativo que se está iniciando actualmente. La Directiva San determina si un disco recién detectado se pone en línea o permanece sin conexión y si se convierte en lectura/escritura o permanece de solo lectura. Cuando un disco está sin conexión, se puede leer el diseño del disco, pero no se muestran los dispositivos de volumen a través de Plug and Play. Esto significa que no se puede montar ningún sistema de archivos en el disco. Cuando un disco está en línea, se instalan uno o varios dispositivos de volumen para el disco. La siguiente es una explicación de cada parámetro:<ul><li>**lineal**. Especifica que todos los discos recién detectados se conectarán y se convertirán en lectura/escritura. **Importante:** La especificación **de un** servidor que comparte discos podría provocar daños en los datos. Por lo tanto, no debe establecer esta Directiva si los discos se comparten entre servidores a menos que el servidor forme parte de un clúster.</li><li>**offlineAll**. Especifica que todos los discos recién detectados, excepto el disco de inicio, estarán sin conexión y de solo lectura de forma predeterminada.</li><li>**offlineShared**. Especifica que todos los discos recién detectados que no residen en un bus compartido (como SCSI e iSCSI) se ponen en línea y se convierten en de lectura y escritura. Los discos que quedan sin conexión serán de solo lectura de forma predeterminada.</li></ul>Para obtener más información, vea [enumeración de VDS_san_POLICY](https://docs.microsoft.com/windows/win32/api/vds/ne-vds-vds_san_policy?redirectedfrom=MSDN). |
| noerr | Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

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

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
