---
title: Con el comando add-DriverPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7e96391ee8dce0b77f00f51d7cb78ff9b8bf242
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564659"
---
# <a name="using-the-add-driverpackage-command"></a>Con el comando add-DriverPackage



Agrega un paquete de controladores al servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ArchivoInf:\<ruta de acceso de archivo Inf >|Especifica la ruta de acceso completa del archivo .inf para agregar.|
|/ Server:\<nombre del servidor >|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|/ Arquitectura: {x86 | ia64 | x64}|Especifica la arquitectura del paquete de controladores.|
|[/ DriverGroup:\<nombre de grupo >]|Especifica el nombre del grupo de controladores a la que se debe agregar el paquete.|
|[/ Name:\<Nombre_descriptivo >]|Indica el nombre descriptivo para el paquete de controladores.|

## <a name="BKMK_examples"></a>Ejemplos

Para agregar un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

