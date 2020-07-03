---
title: ksetup removerealm
description: Artículo de referencia para el comando ksetup removerealm, que elimina toda la información del territorio especificado del registro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0330f7b5f9121da2fce99985fe116be46eb1c9d9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933662"
---
# <a name="ksetup-removerealm"></a>ksetup removerealm

Elimina toda la información del dominio Kerberos especificado del registro.

El nombre de dominio Kerberos se almacena en el registro en `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001` y `\CurrentControlSet\Control\Lsa\Kerberos` . Esta entrada no existe en el registro de forma predeterminada. Puede usar el comando [ksetup addrealmflags](ksetup-addrealmflags.md) para rellenar el registro.

> [!IMPORTANT]
> No se puede quitar el nombre de dominio Kerberos predeterminado del controlador de dominio porque esta opción restablece su información de DNS y, por lo tanto, es posible que el controlador de dominio no se pueda usar.

## <a name="syntax"></a>Sintaxis

```
ksetup /removerealm <realmname>
```
### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el **dominio Kerberos** predeterminados = cuando se ejecuta **ksetup** . |

### <a name="examples"></a>Ejemplos

Para quitar un nombre de dominio Kerberos erróneo (. Con en lugar de. COM) desde el equipo local, escriba:
```
ksetup /removerealm CORP.CONTOSO.CON
```

Para comprobar la eliminación, puede ejecutar el comando **ksetup** y revisar la salida.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup setrealm, comando](ksetup-setrealm.md)
