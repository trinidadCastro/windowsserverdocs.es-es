---
title: Usar el subcomando add-AllDriverPackages
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8290a95dd53718b200d10b6804d312abe95e257
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363891"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>Usar el subcomando add-AllDriverPackages



Agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Parámetros

|          Parámetro           |                                                              Descripción                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath: \<Folder ruta de acceso >  |                      Especifica la ruta de acceso completa a la carpeta que contiene los archivos. inf para los paquetes de controladores.                      |
|   [/Server: \<Server nombre >]   | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
|     [/Architecture: {x86      |                                                                 64                                                                  |
| [/DriverGroup: \<Group nombre de la >] |                             Especifica el nombre del grupo de controladores al que se deben agregar los paquetes.                             |

## <a name="BKMK_examples"></a>Example

Para agregar paquetes de controladores, escriba uno de los siguientes:
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)