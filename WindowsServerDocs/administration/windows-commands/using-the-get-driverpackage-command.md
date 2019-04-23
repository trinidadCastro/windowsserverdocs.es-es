---
title: Mediante el comando get-DriverPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6574177f1f0a8ead0cc2fa596380eb2c980f8d1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888526"
---
# <a name="using-the-get-driverpackage-command"></a>Mediante el comando get-DriverPackage



Muestra información sobre un paquete de controladores en el servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|[/DriverPackage:\<Name>]|Especifica el nombre del paquete de controladores para mostrar.|
|[/ PackageId:\<Id. >]|Especifica el identificador de servicios de implementación de Windows del paquete de controladores para mostrar. Debe especificar el Id. Si el paquete de controladores no se identifica por nombre.|
|[/ Mostrar: {controladores | Archivos | All}]|Indica qué información se va a mostrar (si se especifica). Si **/mostrar** no se especifica, el valor predeterminado es devolver solo el controlador de metadatos del paquete. **Controladores** muestra todos los controladores en el paquete. **Archivos** muestra la lista de archivos en el paquete. **Todos los** muestra los controladores, archivos y metadatos.|

## <a name="BKMK_examples"></a>Ejemplos

Para ver información acerca de un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)