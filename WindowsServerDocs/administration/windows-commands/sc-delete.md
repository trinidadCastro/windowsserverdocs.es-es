---
title: sc.exe eliminar
description: Artículo de referencia para el comando sc.exe Delete, que elimina una subclave de servicio del registro.
ms.topic: reference
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12c09bd72259a8e4b58f134ca19f1fc825f2f1bb
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388466"
---
# <a name="scexe-delete"></a>sc.exe eliminar

Elimina una subclave de servicio del registro. Si el servicio se está ejecutando o si otro proceso tiene un identificador abierto para el servicio, el servicio se marca para su eliminación.

> [!NOTE]
> No se recomienda usar este comando para eliminar servicios de sistema operativo integrados, como DHCP, DNS o Internet Information Services. Para instalar, quitar o volver a configurar roles de sistema operativo, servicios y componentes, consulte [instalación o desinstalación de roles, servicios de rol o características](/WindowsServerDocs/administration/server-manager/install-or-uninstall-roles-role-services-or-features.md) .

## <a name="syntax"></a>Sintaxis

```
sc.exe [<servername>] delete [<servicename>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<servername>` | Especifica el nombre del servidor remoto en el que se encuentra el servicio. El nombre debe usar el formato de Convención de nomenclatura universal (UNC) (por ejemplo, mi \\ Server). Para ejecutar SC.exe localmente, no utilice este parámetro. |
| `<servicename>` | Especifica el nombre de servicio devuelto por la operación **getkeyname** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para eliminar la subclave de servicio **NewServ** del registro en el equipo local, escriba:

```
sc.exe delete NewServ
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
