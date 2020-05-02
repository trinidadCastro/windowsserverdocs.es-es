---
title: 'ksetup: mapuser'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: daa1b8d2c6d0ce2801191b953a533a63bcd8f4ab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724615"
---
# <a name="ksetupmapuser"></a>ksetup: mapuser



Asigna el nombre de una entidad de seguridad de Kerberos a una cuenta.

## <a name="syntax"></a>Sintaxis

```
ksetup /mapuser <Principal> <Account>
```

#### <a name="parameters"></a>Parámetros

|  Parámetro   |                                                   Descripción                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| \<> principal |              El nombre de dominio completo de cualquier entidad de seguridad; por ejemplo, mike@corp.CONTOSO.COM.              |
|  \<> de cuenta  | Cualquier cuenta o nombre de grupo de seguridad que exista en este equipo, como invitado, usuarios del dominio o administrador. |

## <a name="remarks"></a>Observaciones

Una cuenta se puede identificar específicamente, como los invitados del dominio. O bien, puede usar el carácter comodín (*) para incluir todas las cuentas.

Si se omite un nombre de cuenta, se elimina la asignación de la entidad de seguridad especificada.

El equipo solo autenticará las entidades de seguridad del dominio Kerberos dado si presentan vales de Kerberos válidos.

Use **ksetup** sin parámetros ni argumentos para ver la configuración asignada actual y el dominio Kerberos predeterminado.

Siempre que se realicen cambios en el Centro de distribución de claves externo (KDC) y la configuración de dominio Kerberos, es necesario reiniciar el equipo en el que se cambió la configuración.

## <a name="examples"></a>Ejemplos

Asigne la cuenta de Mike Danseglio en el dominio Kerberos de Kerberos a la cuenta de invitado de este equipo, y concédale todos los privilegios de un miembro de la cuenta de invitado integrada sin tener que autenticarse en este equipo:
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Quite la asignación de la cuenta de Mike Danseglio a la cuenta invitado en este equipo para evitar que se autentique en este equipo con sus credenciales de CONTOSO:
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Asigne la cuenta de Mike Danseglio en el dominio Kerberos de CONTOSO a cualquier cuenta existente en este equipo. (si solo las cuentas de usuario estándar e invitado están activas en este equipo, los privilegios de Mike se establecerán en ellos):
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Asignar todas las cuentas del dominio Kerberos de CONTOSO a cualquier cuenta existente del mismo nombre en este equipo:
```
ksetup /mapuser * *
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)