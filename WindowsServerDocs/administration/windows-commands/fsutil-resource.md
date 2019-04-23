---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: recursos de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b55063c3c5ea41b43573e6322b5efb36d2dad90e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828336"
---
# <a name="fsutil-resource"></a>recursos de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Crea un administrador de recursos secundario transaccional, se inicia o detiene un administrador de recursos transaccional, o muestra información acerca de un administrador de recursos transaccional y modifica el comportamiento siguiente:

-   Si el valor predeterminado es administrador de recursos transaccional limpiará sus metadatos en el siguiente montaje transaccional

-   El Administrador de recursos transaccional especificado para preferir coherencia a través de disponibilidad

-   El Administrador de recursos de transacción especificada para preferir disponibilidad a través de coherencia

-   Las características de una ejecución transaccional Resource Manager

Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples) .

## <a name="syntax"></a>Sintaxis

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>

```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|crear|Crea un administrador de recursos secundario transaccional.|
|<RmRootPathname>|Especifica la ruta de acceso completa a un directorio raíz de administrador de recursos transaccional.|
|info|Muestra información de la especificada transaccional del Administrador de recursos.|
|setautoreset|Especifica si un administrador de recursos transaccional predeterminado limpiará los metadatos en el siguiente montaje transaccional.<br /><br />-Establecer la **setautoreset** parámetro **true** para especificar que el Administrador de recursos de transacción limpiará los metadatos transaccional en el siguiente montaje, de forma predeterminada.<br />-Establecer la **setautoreset** parámetro **false** para especificar que el Administrador de recursos de transacción no limpiará los metadatos transaccional en el siguiente montaje, de forma predeterminada.|
|<DefaultRmRootPathname>|Especifica el nombre de la unidad seguido de dos puntos.|
|setavailable|Especifica que un administrador de recursos transaccional preferirá disponibilidad a través de coherencia.|
|setconsistent|Especifica que un administrador de recursos transaccional preferirán coherencia a través de disponibilidad.|
|setlog|Cambia las características de un administrador de recursos transaccional que ya se está ejecutando.|
|Crecimiento|Especifica la cantidad por la que puede alcanzar el registro del Administrador de recursos transaccional.<br /><br />El parámetro de crecimiento se puede especificar como sigue:<br /><br />-El número de contenedores con el formato: *Contenedores ***contenedores**<br />: porcentaje con el formato: *Por ciento *** por ciento**|
|<containers>|Especifica los objetos de datos que se usan por el Administrador de recursos transaccionales.|
|maxextent|Especifica el número máximo de contenedores para el Administrador de recursos transaccional especificado.|
|minextent|Especifica el número mínimo de contenedores para el Administrador de recursos transaccional especificado.|
|modo {completa&#124;deshacer}|Especifica si se registran todas las transacciones ( **completa**) o solo revierte se registran los eventos (**deshacer**).|
|rename|Cambia el GUID para el Administrador de recursos transaccionales.|
|shrink|Especifica el porcentaje por el que puede reducir automáticamente el registro del Administrador de recursos transaccional.|
|size|Especifica el tamaño del Administrador de recursos transaccional como un número especificado de *contenedores*.|
|start|Se inicia el Administrador de recursos transaccional especificado.|
|stop|Detiene el Administrador de recursos transaccional especificado.|

### <a name="BKMK_examples"></a>Ejemplos
Para establecer el registro para el Administrador de recursos transaccional especificado por c:\test, tener un crecimiento automático de cinco contenedores, escriba:

```
fsutil resource setlog growth 5 containers c:test
```

Para establecer el registro para el Administrador de recursos transaccional que se especifica mediante c:\test, tener un crecimiento automático de dos por ciento, escriba:

```
fsutil resource setlog growth 2 percent c:test
```

Para especificar que el valor predeterminado de administrador de recursos transaccional limpiará los metadatos transaccional en el siguiente montaje en la unidad C, escriba:

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS transaccional](https://go.microsoft.com/fwlink/?LinkID=165402)


