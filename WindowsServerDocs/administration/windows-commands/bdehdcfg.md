---
title: bdehdcfg
description: Artículo de referencia para el comando bdehdcfg, que prepara una unidad de disco duro con las particiones necesarias para Cifrado de unidad BitLocker.
ms.topic: reference
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea7806fc75d01e3b261296ff6fd462473ca5683b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031463"
---
# <a name="bdehdcfg"></a>bdehdcfg

Prepara una unidad de disco duro con las particiones necesarias para Cifrado de unidad BitLocker. La mayoría de las instalaciones de Windows 7 no tendrán que usar esta herramienta, ya que el programa de instalación de BitLocker incluye la capacidad de preparar y volver a particionar las unidades según sea necesario.

> [!WARNING]
> Existe un conflicto conocido con la configuración de directiva de grupo **Denegar el acceso de escritura a unidades fijas no protegidas por BitLocker** ubicada en **Configuración del equipo\Plantillas administrativas\Componentes de Windows\Cifrado de unidad BitLocker\Unidades de datos fijas**.
>
>Si se ejecuta bdehdcfg en un equipo cuando esta configuración de directiva está habilitada, pueden producirse los siguientes problemas:
>
>- Si ha intentado reducir la unidad y crear la unidad de sistema, el tamaño de la unidad se reducirá correctamente y se creará una partición sin formato. Sin embargo, no se dará formato a esa partición. Se muestra el siguiente mensaje de error: no se puede dar formato a la nueva unidad activa. Es posible que tenga que preparar manualmente la unidad para BitLocker.
>
>- Si ha intentado usar espacio sin asignar para crear la unidad de sistema, se creará una partición sin formato. Sin embargo, no se dará formato a esa partición. Se muestra el siguiente mensaje de error: no se puede dar formato a la nueva unidad activa. Es posible que tenga que preparar manualmente la unidad para BitLocker.
>
>- Si ha intentado combinar una unidad existente con la unidad de sistema, la herramienta no podrá copiar el archivo de arranque necesario en la unidad de destino para crear la unidad de sistema. Aparece el siguiente mensaje de error: el programa de instalación de BitLocker no pudo copiar los archivos de arranque. Es posible que tenga que preparar manualmente la unidad para BitLocker.
>
>- Si se aplica esta configuración de directiva, no se pueden volver a crear particiones porque la unidad está protegida. Si está actualizando los equipos de su organización desde una versión anterior de Windows y esos equipos se configuraron con una sola partición, debe crear la partición de sistema de BitLocker necesaria antes de aplicar la configuración de directiva a los equipos.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg [–driveinfo <drive_letter>] [-target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}] [–newdriveletter] [–size <size_in_mb>] [-quiet]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |----------- |
| [bdehdcfg: DriveInfo](bdehdcfg-driveinfo.md) | Muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de partición de las particiones en la unidad especificada. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas. |
| [bdehdcfg: destino](bdehdcfg-target.md) | Define la parte de una unidad que se va a usar como unidad del sistema y hace que la parte esté activa. |
| [bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md) | Asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema. |
| [bdehdcfg: tamaño](bdehdcfg-size.md) | Determina el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema. |
| [bdehdcfg: Quiet](bdehdcfg-quiet.md) | Impide que se muestren todas las acciones y los errores en la interfaz de la línea de comandos y dirige a bdehdcfg que use la respuesta Yes a cualquier mensaje sí/no que se pueda producir durante la preparación de la unidad posterior. |
| [bdehdcfg: reinicio](bdehdcfg-restart.md) | Dirige el equipo para que se reinicie una vez finalizada la preparación de la unidad. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
