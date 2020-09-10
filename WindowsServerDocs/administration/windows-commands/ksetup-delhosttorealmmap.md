---
title: ksetup delhosttorealmmap
description: Artículo de referencia para el comando ksetup delhosttorealmmap, que quita una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos.
ms.topic: reference
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 321d468169005d5b183e3b3d4149872a731b8cfa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639695"
---
# <a name="ksetup-delhosttorealmmap"></a>ksetup delhosttorealmmap

Quita una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos. Este comando también quita cualquier asignación entre un host y un dominio (o varios hosts).

La asignación se almacena en el registro, en `HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm` . Después de ejecutar este comando, se recomienda asegurarse de que la asignación aparece en el registro.

## <a name="syntax"></a>Sintaxis

```
ksetup /delhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<hostname>` | Especifica el nombre de dominio completo del equipo. |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM. |

### <a name="examples"></a>Ejemplos

Para cambiar la configuración del territorio CONTOSO y eliminar la asignación del equipo host IPops897 al dominio Kerberos, escriba:

```
ksetup /delhosttorealmmap IPops897 CONTOSO
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup addhosttorealmmap, comando](ksetup-addhosttorealmmap.md)
