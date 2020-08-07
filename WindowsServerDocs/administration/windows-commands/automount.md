---
title: automount
description: Artículo de referencia para el comando automount, que habilita o deshabilita la característica de montaje automático.
ms.topic: article
ms.assetid: 4635fc91-a477-4f17-8dcc-aa08854bfe45
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a31e1569f1db4f47c63aae2e859587f5088dcbc4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895195"
---
# <a name="automount"></a>automount

Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

> [!IMPORTANT]
> En las configuraciones de red de área de almacenamiento (SAN), al deshabilitar el montaje automático se impide que Windows Monte automáticamente o asigne Letras de unidad a los nuevos volúmenes básicos que estén visibles para el sistema.

## <a name="syntax"></a>Sintaxis

montaje automático [{Enable | Disable | Scrub}] [Noerr]

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| enable | Permite a Windows montar automáticamente nuevos volúmenes básicos y dinámicos que se agregan al sistema y asignarles letras de unidad. |
| disable | Impide que Windows Monte automáticamente los nuevos volúmenes básicos y dinámicos que se agreguen al sistema.<p>**Nota**: al deshabilitar el montaje automático se puede producir un error en los clústeres de conmutación por error de la parte de almacenamiento del Asistente para validar una configuración. |
| scrub | Quita los directorios del punto de montaje de volumen y la configuración del Registro de aquellos volúmenes que ya no se encuentran en el sistema. Así se impide que los volúmenes que se encontraban previamente en el sistema se monten automáticamente y reciban los puntos de montaje de volumen anteriores cuando se vuelven a agregar al sistema. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para ver si la característica de montaje automático está habilitada, escriba los siguientes comandos en el comando DiskPart:

```
automount
```

Para habilitar la característica de montaje automático, escriba:

```
automount enable
```

Para deshabilitar la característica de montaje automático, escriba:

```
automount disable
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comandos Diskpart](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc770877(v%3dws.11))
