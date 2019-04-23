---
title: Con el comando remove-DriverGroupPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82a8fb8fbe9e713c3e22c08839bc4bc22fe900db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883996"
---
# <a name="using-the-remove-drivergrouppackage-command"></a>Con el comando remove-DriverGroupPackage



Quita un paquete de controladores de un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|[/DriverPackage:\<Name>]|Especifica el nombre del paquete de controladores para quitar.|
|[/ PackageId:\<Id. >]|Especifica el identificador de servicios de implementación de Windows del paquete de controladores para quitar. Debe especificar esta opción si el paquete de controladores no se identifica por nombre.|

## <a name="BKMK_examples"></a>Ejemplos

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)