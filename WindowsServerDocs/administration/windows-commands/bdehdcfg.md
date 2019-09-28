---
title: bdehdcfg
description: 'Temas de comandos de Windows para **bdehdcfg** : prepara una unidad de disco duro con las particiones necesarias para cifrado de unidad BitLocker.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acfe2582905402f7be149148520784be6ad47453
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382195"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prepara una unidad de disco duro con las particiones necesarias para Cifrado de unidad BitLocker. La mayoría de las instalaciones de Windows 7 no tendrán que usar esta herramienta, ya que el programa de instalación de BitLocker incluye la capacidad de preparar y volver a particionar las unidades según sea necesario.

> [!WARNING]
> Existe un conflicto conocido con la configuración de directiva de grupo **Denegar el acceso de escritura a unidades fijas no protegidas por BitLocker** ubicada en **Configuración del equipo\Plantillas administrativas\Componentes de Windows\Cifrado de unidad BitLocker\Unidades de datos fijas**.</br>> Si se ejecuta Bdehdcfg en un equipo cuando esta configuración de directiva está habilitada, pueden producirse los siguientes problemas:</br>>: Si intenta reducir la unidad y crear la unidad del sistema, el tamaño de la unidad se reducirá correctamente y se creará una partición sin formato. Sin embargo, no se dará formato a esa partición. Aparecerá el siguiente mensaje de error: "No se puede formatear la nueva unidad activa. Es posible que deba preparar la unidad manualmente para BitLocker".</br>>: Si intentó usar espacio sin asignar para crear la unidad del sistema, se creará una partición sin formato. Sin embargo, no se dará formato a esa partición. Aparecerá el siguiente mensaje de error: "No se puede formatear la nueva unidad activa. Es posible que deba preparar la unidad manualmente para BitLocker".</br>>: Si intentó fusionar mediante combinación una unidad existente en la unidad del sistema, la herramienta no podrá copiar el archivo de arranque necesario en la unidad de destino para crear la unidad del sistema. Aparecerá el siguiente mensaje de error: "El programa de instalación de BitLocker no pudo copiar los archivos de arranque. Es posible que deba preparar la unidad manualmente para BitLocker".</br>> Si se aplica esta configuración de Directiva, no se puede volver a particionar una unidad de disco duro porque la unidad está protegida. Si está actualizando los equipos de su organización desde una versión anterior de Windows y esos equipos se configuraron con una sola partición, debe crear la partición de sistema de BitLocker necesaria antes de aplicar la configuración de directiva a los equipos.

Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Bdehdcfg: DriveInfo](bdehdcfg-driveinfo.md)|Muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de partición de las particiones en la unidad especificada. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas.|
|[Bdehdcfg: destino](bdehdcfg-target.md)|Define la parte de una unidad que se va a usar como unidad del sistema y hace que la parte esté activa.|
|[Bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|Asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema.|
|[Bdehdcfg: tamaño](bdehdcfg-size.md)|Determina el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema.|
|[Bdehdcfg: Quiet](bdehdcfg-quiet.md)|Impide que se muestren todas las acciones y los errores en la interfaz de la línea de comandos y dirige Bdehdcfg para que use la respuesta "Yes" a los mensajes sí/no que se puedan producir durante la preparación de la unidad subsiguiente.|
|[Bdehdcfg: reinicio](bdehdcfg-restart.md)|Dirige el equipo para que se reinicie una vez finalizada la preparación de la unidad.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra cómo se usa Bdehdcfg con la unidad predeterminada para crear una partición del sistema de 500 MB. Dado que no se especifica ninguna letra de unidad, la nueva partición del sistema no tendrá una letra de unidad.
```
bdehdcfg -target default -size 500
```
En el ejemplo siguiente se muestra cómo se usa Bdehdcfg con la unidad predeterminada para crear una partición del sistema (P:) del tamaño predeterminado de 300 MB de espacio sin asignar en la unidad. La herramienta no pedirá al usuario ninguna entrada adicional ni mostrará ningún error. Una vez creada la unidad del sistema, el equipo se reiniciará automáticamente.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)