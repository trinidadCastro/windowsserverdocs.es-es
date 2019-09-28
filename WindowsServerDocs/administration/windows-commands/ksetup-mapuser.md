---
title: 'ksetup: mapuser'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b80538999c364e9ed10ca0ed43387f603ac9ad3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374981"
---
# <a name="ksetupmapuser"></a>ksetup: mapuser



Asigna el nombre de una entidad de seguridad de Kerberos a una cuenta. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Parámetros

|  Parámetro   |                                                   Descripción                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| @no__t 0Principal > |              El nombre de dominio completo de cualquier entidad de seguridad; por ejemplo, mike@corp.CONTOSO.COM.              |
|  @no__t 0Account >  | Cualquier cuenta o nombre de grupo de seguridad que exista en este equipo, como invitado, usuarios del dominio o administrador. |

## <a name="remarks"></a>Comentarios

Una cuenta se puede identificar específicamente, como los invitados del dominio. O bien, puede usar el carácter comodín (*) para incluir todas las cuentas.

Si se omite un nombre de cuenta, se elimina la asignación de la entidad de seguridad especificada.

El equipo solo autenticará las entidades de seguridad del dominio Kerberos dado si presentan vales de Kerberos válidos.

Use **ksetup** sin parámetros ni argumentos para ver la configuración asignada actual y el dominio Kerberos predeterminado.

Siempre que se realicen cambios en el Centro de distribución de claves externo (KDC) y la configuración de dominio Kerberos, es necesario reiniciar el equipo en el que se cambió la configuración.

## <a name="BKMK_Examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)