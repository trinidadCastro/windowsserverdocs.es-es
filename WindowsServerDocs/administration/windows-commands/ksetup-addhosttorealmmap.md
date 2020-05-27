---
title: ksetup addhosttorealmmap
description: Tema de referencia del comando ksetup addhosttorealmmap, que agrega una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee2639a5bb071bdd3d6ac3f6373e881c18f3bf9a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818115"
---
# <a name="ksetup-addhosttorealmmap"></a>ksetup addhosttorealmmap

Agrega una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos. Este comando también permite asignar un host o varios hosts que comparten el mismo sufijo DNS en el dominio Kerberos.

La asignación se almacena en el registro, en **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**.

## <a name="syntax"></a>Sintaxis

```
ksetup /addhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| `<hostname>` | El nombre de host es el nombre del equipo y se puede indicar como el nombre de dominio completo del equipo. |
| `<realmname>` | El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM. |

### <a name="examples"></a>Ejemplos

Para asignar el equipo host *IPops897* al dominio Kerberos de *contoso* , escriba:

```
ksetup /addhosttorealmmap IPops897 CONTOSO
```

Compruebe el registro para asegurarse de que la asignación se produjo según lo previsto.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup delhosttorealmmap, comando](ksetup-delhosttorealmmap.md)
