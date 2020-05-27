---
title: ksetup mapuser
description: Tema de referencia del comando ksetup mapuser, que asigna el nombre de una entidad de seguridad de Kerberos a una cuenta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ac2f3e30b3057ceea4376d7ffe8286875d5301d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817675"
---
# <a name="ksetup-mapuser"></a>ksetup mapuser

Asigna el nombre de una entidad de seguridad de Kerberos a una cuenta.

## <a name="syntax"></a>Sintaxis

```
ksetup /mapuser <principal> <account>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<principal>` | Especifica el nombre de dominio completo de cualquier usuario principal. Por ejemplo, mike@corp.CONTOSO.COM. Si no especifica un parámetro de cuenta, se elimina la asignación de la entidad de seguridad especificada. |
| `<account>` | Especifica cualquier cuenta o nombre de grupo de seguridad que exista en este equipo, como **invitado**, **usuarios del dominio**o **Administrador**. Si se omite este parámetro, se elimina la asignación de la entidad de seguridad especificada. |

#### <a name="remarks"></a>Observaciones

- Una cuenta se puede identificar específicamente, como los **invitados del dominio**, o puede usar un carácter comodín (*) para incluir todas las cuentas.

- El equipo solo autentica las entidades de seguridad del territorio determinado si presentan vales de Kerberos válidos.

- Siempre que se realicen cambios en el Centro de distribución de claves externo (KDC) y la configuración de dominio Kerberos, es necesario reiniciar el equipo en el que se cambió la configuración.

### <a name="examples"></a>Ejemplos

Para ver la configuración asignada actual y el dominio Kerberos predeterminado, escriba:

```
ksetup
```

Para asignar la cuenta de Mike Danseglio en el dominio Kerberos de Kerberos a la cuenta de invitado de este equipo y concederle todos los privilegios de un miembro de la cuenta de invitado integrada sin tener que autenticarse en este equipo, escriba:

```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```

Para quitar la asignación de la cuenta de Mike Danseglio a la cuenta invitado en este equipo para evitar que se autentique en este equipo con sus credenciales de CONTOSO, escriba:

```
ksetup /mapuser mike@corp.CONTOSO.COM
```

Para asignar la cuenta de Mike Danseglio en el dominio Kerberos de CONTOSO a cualquier cuenta existente en este equipo, escriba:

```
ksetup /mapuser mike@corp.CONTOSO.COM *
```

> [!NOTE]
> Si solo las cuentas de usuario estándar e invitado están activas en este equipo, los privilegios de Mike se establecen en ellos.

Para asignar todas las cuentas del dominio Kerberos de CONTOSO a cualquier cuenta existente del mismo nombre en este equipo, escriba:

```
ksetup /mapuser * *
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)
