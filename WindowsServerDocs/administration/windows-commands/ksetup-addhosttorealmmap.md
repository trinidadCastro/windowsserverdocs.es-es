---
title: 'ksetup: addhosttorealmmap'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ee8f434482b0658194daed46b62f6f7f70abae1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841848"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup: addhosttorealmmap



Agrega una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<HostName >|El nombre de host es el nombre del equipo y se puede indicar como el nombre de dominio completo del equipo.|
|\<RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentarios

Este comando permite asignar un host o varios hosts que comparten el mismo sufijo DNS en el dominio Kerberos.

La asignación se registra en el registro en **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Como parte de la configuración del territorio CONTOSO, asigne el equipo host IPops897 al dominio Kerberos:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Compruebe en el registro que la asignación es la prevista.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)