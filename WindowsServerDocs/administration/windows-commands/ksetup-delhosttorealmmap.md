---
title: 'ksetup: delhosttorealmmap'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b6b14785f254a63f0e16fcd16f1cd464a2d69c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724693"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup: delhosttorealmmap



Quita una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Nombre de host>|El nombre de host es el nombre del equipo y se puede indicar como el nombre de dominio completo del equipo.|
|\<> RealmName|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Observaciones

Cuando existe una asignación de host a dominio Kerberos (o varios hosts a dominio Kerberos), este comando quita esa asignación.

La asignación se registra en el registro en **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**. Debe comprobar la asignación en el registro después de usar este comando.

## <a name="examples"></a>Ejemplos

Modificando la configuración del dominio Kerberos CONTOSO, elimina la asignación del equipo host IPops897 al dominio Kerberos:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Después de ejecutar este comando, puede comprobar en el registro que la asignación es la prevista.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)