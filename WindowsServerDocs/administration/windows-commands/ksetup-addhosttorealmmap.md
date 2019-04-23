---
title: ksetup:addhosttorealmmap
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25cf258309c94f0efde980018dd5dcf3c7df4d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837506"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup:addhosttorealmmap



Agrega una asignación de nombre de entidad de seguridad (SPN) de servicio entre el host indicado y el dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<HostName>|El nombre de host es el nombre del equipo y se puede establecer como nombre de dominio completo del equipo.|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentarios

Este comando permite asignar un host o varios hosts que comparten el mismo sufijo DNS para el dominio Kerberos.

La asignación se registra en el registro en **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**.

## <a name="BKMK_Examples"></a>Ejemplos

Como parte de la configuración del dominio CONTOSO, asigne el equipo host IPops897 al dominio Kerberos:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
En el registro, compruebe que la asignación es según lo previsto.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)