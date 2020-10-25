---
title: Copy-drivergroup
description: Artículo de referencia para el comando copy-drivergroup, que duplica un grupo de controladores existente en el servidor, incluidos los filtros, los paquetes de controladores y el estado habilitado o deshabilitado.
ms.topic: reference
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 924f6181e424dad88a33f1421098e658275d1fa3
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524920"
---
# <a name="copy-drivergroup"></a>Copy-drivergroup

Duplica un grupo de controladores existente en el servidor, incluidos los filtros, los paquetes de controladores y el estado habilitado o deshabilitado.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Groupname> /GroupName:<New Groupname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| Servidor`<Servername>` | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| /DriverGroup:`<Source Groupname>` | Especifica el nombre del grupo de controladores de origen. |
| GroupName`<New Groupname>` | Especifica el nombre del nuevo grupo de controladores. |

## <a name="examples"></a>Ejemplos

Para copiar un grupo de controladores, escriba:

```
wdsutil /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```

```
wdsutil /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
