---
title: bdehdcfg
description: Tema de los comandos de Windows para **bdehdcfg** -prepara una unidad de disco duro con las particiones necesarias para BitLocker Drive Encryption.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a4fda562f4aa6b8caa885fcd70155b7150c91d12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869706"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prepara una unidad de disco duro con las particiones necesarias para BitLocker Drive Encryption. La mayoría de las instalaciones de Windows 7 no necesitará utilizar esta herramienta porque el programa de instalación de BitLocker incluye la capacidad para preparar y volver a particionar discos según sea necesario.

> [!WARNING]
> Existe un conflicto conocido con la configuración de directiva de grupo **Denegar el acceso de escritura a unidades fijas no protegidas por BitLocker** ubicada en **Configuración del equipo\Plantillas administrativas\Componentes de Windows\Cifrado de unidad BitLocker\Unidades de datos fijas**.</br>> Si se ejecuta Bdehdcfg en un equipo cuando se habilita esta configuración de directiva, se pueden producir los problemas siguientes:</br>>: Si ha intentado reducir la unidad y crear la unidad del sistema, el tamaño de la unidad se reducirá correctamente y se creará una partición sin formato. Sin embargo, no se dará formato a esa partición. Aparecerá el siguiente mensaje de error: "No se puede formatear la nueva unidad activa. Es posible que deba preparar la unidad manualmente para BitLocker".</br>>: Si ha intentado usar espacio sin asignar para crear la unidad del sistema, se creará una partición sin formato. Sin embargo, no se dará formato a esa partición. Aparecerá el siguiente mensaje de error: "No se puede formatear la nueva unidad activa. Es posible que deba preparar la unidad manualmente para BitLocker".</br>>: Si ha intentado combinar una unidad existente en la unidad del sistema, se producirá un error en la herramienta Copiar el archivo de arranque necesaria en la unidad de destino para crear la unidad del sistema. Aparecerá el siguiente mensaje de error: "El programa de instalación de BitLocker no pudo copiar los archivos de arranque. Es posible que deba preparar la unidad manualmente para BitLocker".</br>> Si se está aplicando esta configuración de directiva, una unidad de disco duro no se han repartido porque la unidad está protegida. Si está actualizando los equipos de su organización desde una versión anterior de Windows y esos equipos se configuraron con una sola partición, debe crear la partición de sistema de BitLocker necesaria antes de aplicar la configuración de directiva a los equipos.

Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[Bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|Muestra la letra de unidad, el tamaño total, el espacio máximo disponible y las características de particiones de las particiones en la unidad especificada. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas.|
|[Bdehdcfg: target](bdehdcfg-target.md)|Define qué parte de una unidad que se usará como la unidad del sistema y hace que la parte activa.|
|[Bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|Asigna una letra de unidad nueva a la parte de una unidad utilizada como unidad del sistema.|
|[Bdehdcfg: size](bdehdcfg-size.md)|Determina el tamaño de la partición del sistema cuando se crea una nueva unidad de sistema.|
|[bdehdcfg: silencioso](bdehdcfg-quiet.md)|Impide que la visualización de todas las acciones y errores en la interfaz de línea de comandos y dirige Bdehdcfg para usar la respuesta "Yes" en cualquier sí/preparación de unidad sin mensajes que pueden producirse durante las posteriores.|
|[Bdehdcfg: restart](bdehdcfg-restart.md)|Dirige el equipo se reinicie una vez finalizada la preparación de la unidad.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="BKMK_Examples"></a>Ejemplos

El siguiente ejemplo muestra Bdehdcfg que se usa con la unidad predeterminada para crear una partición de sistema de 500 MB. Como no se especifica ninguna letra de unidad, la nueva partición del sistema no tendrán una letra de unidad.
```
bdehdcfg -target default -size 500
```
En el ejemplo siguiente se ilustra Bdehdcfg que se usa con la unidad predeterminada para crear una partición del sistema (p) del tamaño predeterminado de 300 MB de espacio sin asignar en la unidad. La herramienta no solicitará al usuario ninguna entrada adicional ni se mostrarán los errores. Una vez creada la unidad del sistema, el equipo se reiniciará automáticamente.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)