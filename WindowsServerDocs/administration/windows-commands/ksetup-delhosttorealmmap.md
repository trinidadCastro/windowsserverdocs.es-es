---
title: 'ksetup: delhosttorealmmap'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9f6b4a9f1c9050ed843f3837a2bd87aaf6eae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841738"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup: delhosttorealmmap



Quita una asignación de nombre de entidad de seguridad de servicio (SPN) entre el host indicado y el dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<HostName >|El nombre de host es el nombre del equipo y se puede indicar como el nombre de dominio completo del equipo.|
|\<RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentarios

Cuando existe una asignación de host a dominio Kerberos (o varios hosts a dominio Kerberos), este comando quita esa asignación.

La asignación se registra en el registro en **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**. Debe comprobar la asignación en el registro después de usar este comando.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Modificando la configuración del dominio Kerberos CONTOSO, elimina la asignación del equipo host IPops897 al dominio Kerberos:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Después de ejecutar este comando, puede comprobar en el registro que la asignación es la prevista.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)