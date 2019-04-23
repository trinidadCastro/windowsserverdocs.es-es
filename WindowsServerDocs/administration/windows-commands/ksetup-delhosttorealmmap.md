---
title: ksetup:delhosttorealmmap
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf01edc4932fd5ec1cf98043de04286b3a100a34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882346"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup:delhosttorealmmap



Quita una asignación de nombre de entidad de seguridad (SPN) de servicio entre el host indicado y el dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<HostName>|El nombre de host es el nombre del equipo y se puede establecer como nombre de dominio completo del equipo.|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentarios

Cuando existe un host a la asignación del dominio (o varios hosts al dominio Kerberos), este comando quita esa asignación.

La asignación se registra en el registro en **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**. Debe comprobar la asignación en el registro después de usar este comando.

## <a name="BKMK_Examples"></a>Ejemplos

Modificar la configuración del dominio CONTOSO, elimine la asignación del equipo host IPops897 al dominio Kerberos:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Después de ejecutar este comando, puede comprobar en el registro que la asignación es según lo previsto.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)