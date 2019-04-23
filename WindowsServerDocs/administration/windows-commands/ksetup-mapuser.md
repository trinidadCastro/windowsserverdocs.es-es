---
title: ksetup:mapuser
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2828f92b20cafcb571c81c8ceae28c741fbe025a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872866"
---
# <a name="ksetupmapuser"></a>ksetup:mapuser



El nombre de una entidad de seguridad de Kerberos se asigna a una cuenta. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Principal>|El nombre de dominio completo de cualquier entidad de seguridad; Por ejemplo, mike@corp.CONTOSO.COM.|
|\<Cuenta >|Cualquier nombre de grupo de seguridad o la cuenta de que existe en este equipo, como invitado, los usuarios del dominio o administrador.|

## <a name="remarks"></a>Comentarios

Una cuenta puede ser específicamente identificada, como invitados del dominio. O bien, puede usar el carácter comodín (*) para incluir todas las cuentas.

Si se omite un nombre de cuenta, se elimina la asignación para la entidad de seguridad especificada.

El equipo autenticará solo las entidades de seguridad del dominio Kerberos especificado si se presentan los vales de Kerberos válidos.

Use **ksetup** sin los parámetros o los argumentos para ver asignadas a la actual configuración y el dominio Kerberos predeterminado.

Cada vez que se realizan cambios en el centro de distribución de claves (KDC) externa y la configuración del dominio, se requiere un reinicio del equipo donde se ha cambiado la configuración.

## <a name="BKMK_Examples"></a>Ejemplos

Asignar cuenta Danseglio en el dominio Kerberos CONTOSO a la cuenta de invitado en este equipo, le concede todos los privilegios de un miembro de la cuenta de invitado sin tener que autenticarse en este equipo:
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Quitar la asignación de cuenta Danseglio a la cuenta de invitado en el equipo para evitar que puede autenticar este equipo con sus credenciales de CONTOSO:
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Cuenta Danseglio en el dominio Kerberos de CONTOSO se asignan a cualquier cuenta existente en este equipo. (si solo el usuario estándar y las cuentas de invitado están activas en este equipo, privilegios de Mike se establecerá en aquellos):
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Todas las cuentas en el dominio Kerberos de CONTOSO se asignan a cualquier cuenta existente del mismo nombre en este equipo:
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)