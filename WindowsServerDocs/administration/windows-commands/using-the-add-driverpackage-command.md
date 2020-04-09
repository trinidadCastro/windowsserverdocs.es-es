---
title: Add-DriverPackage
description: Tema de comandos de Windows para Add-DriverPackage, que agrega un paquete de controladores al servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2ccdfcddd2f605eccd9cd32fed7b8c6921297fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832018"
---
# <a name="add-driverpackage"></a>Add-DriverPackage

agrega un paquete de controladores al servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Parámetros

|          Parámetro           |                                                              Descripción                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<ruta de acceso del archivo INF >   |                                           Especifica la ruta de acceso completa del archivo. inf que se va a agregar.                                            |
|    /Server:\<nombre de servidor >    | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
|      /Architecture: {x86      |                                                                 64                                                                  |
| [/DriverGroup: nombre de grupo de\<>] |                             Especifica el nombre del grupo de controladores al que se debe agregar el paquete.                              |
|   [/Name:\<nombre descriptivo >]   |                                           Indica el nombre descriptivo para el paquete de controladores.                                            |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para agregar un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

