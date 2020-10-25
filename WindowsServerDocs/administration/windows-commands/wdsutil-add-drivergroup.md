---
title: WDSUtil Add-drivergroup
description: Artículo de referencia del comando WDSUtil Add-drivergroup, que agrega un grupo de controladores al servidor.
ms.topic: reference
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 68583ba35c141df03cbc5e351da6c4dc8ea4e161
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524620"
---
# <a name="wdsutil-add-drivergroup"></a>WDSUtil Add-drivergroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega un grupo de controladores al servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /add-DriverGroup /DriverGroup:<Groupname>\n\ [/Server:<Servername>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filtertype> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica el nombre del nuevo grupo de controladores. |
| Servidor`<Servername>` | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| Activó`{Yes|No}` | Habilita o deshabilita el paquete. |
| Identificado`{Matched|All}` | Especifica los paquetes que se van a instalar si se cumplen los criterios de filtro. **Matched** significa instalar solo los paquetes de controladores que coinciden con el hardware del cliente. **Todo** significa instalar todos los paquetes en los clientes, independientemente de su hardware. |
| FilterType`<Filtertype>` | Especifica el tipo de filtro que se va a agregar al grupo. Puede especificar varios tipos de filtro en un solo comando. Cada tipo de filtro debe ir seguido de **/Policy** y al menos un **/Value**. Los valores válidos son:<ul><li>BiosVendor</li><li>Biosversion</li><li>Chassistype</li><li>Fabricante</li><li>Uuid</li><li>OSVersion</li><li>Osedition</li><li>OsLanguage</li></ul> Para obtener información acerca de cómo obtener valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |
| [/Policy: `{Include|Exclude}` ] | Especifica la Directiva que se va a establecer en el filtro. Si **/Policy** se establece en **include**, los equipos cliente que coincidan con el filtro podrán instalar los controladores en este grupo. Si **/Policy** está establecido en **excluir**, los equipos cliente que coincidan con el filtro no podrán instalar los controladores de este grupo. |
| [/Value: `<Value>` ] | Especifica el valor de cliente que corresponde a **/FilterType**. Puede especificar varios valores para un único tipo. Para obtener información acerca de los valores de tipo de filtro aceptables, consulte [filtros de grupo de controladores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |

## <a name="examples"></a>Ejemplos

Para agregar un grupo de controladores, escriba:

```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```

```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Add-drivergrouppackage (comando)](wdsutil-add-drivergrouppackage.md)

- [WDSUtil Add-drivergrouppackages (comando)](wdsutil-add-drivergrouppackages.md)

- [WDSUtil Add-drivergroupfilter (comando)](wdsutil-add-drivergroupfilter.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
