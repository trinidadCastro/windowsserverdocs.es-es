---
title: ksetup delhosttorealmmap
description: Artículo de referencia para el comando ksetup delhosttorealmmap, que quita una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos.
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 953a8d33a65bb9c5aafd4d549f762772bc059ba4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888001"
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
