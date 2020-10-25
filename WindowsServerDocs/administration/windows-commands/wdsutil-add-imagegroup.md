---
title: WDSUtil Add-imagegroup
description: Artículo de referencia del comando WDSUtil Add-imagegroup, que agrega un grupo de imágenes a un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3bffb562ba019bb55c783541b78c906dd4a08dc7
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524470"
---
# <a name="wdsutil-add-imagegroup"></a>WDSUtil Add-imagegroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega un grupo de imágenes a un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /add-ImageGroup imageGroup:<Imagegroupname> [/Server:<Server name>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| imageGroup: `<Imagegroupname>` ] | Especifica el nombre de la imagen que se va a agregar. |
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se utiliza el servidor local. |

## <a name="examples"></a>Ejemplos

Para agregar un grupo de imágenes, escriba:

```
wdsutil /add-ImageGroup imageGroup:ImageGroup2
```

```
wdsutil /verbose /add-Imagegroup imageGroup:My Image Group /Server:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Get-allimagegroups (comando)](wdsutil-get-allimagegroups.md)

- [WDSUtil Get-imagegroup (comando)](wdsutil-get-imagegroup.md)

- [WDSUtil Remove-imagegroup (comando)](wdsutil-remove-imagegroup.md)

- [WDSUtil Set-imagegroup (comando)](wdsutil-set-imagegroup.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
