---
title: ksetup:removerealm
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579b0772e4642389b90aa370dad80a3eebea9d34
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564715"
---
# <a name="ksetupremoverealm"></a>ksetup:removerealm



Elimina toda la información para el dominio Kerberos especificado desde el registro. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM y aparece como el dominio Kerberos predeterminado cuando **ksetup** se ejecuta.|

## <a name="remarks"></a>Comentarios

El nombre de dominio Kerberos se almacena en dos lugares en el registro: **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** and **\CurrentControlSet\Control\Lsa\Kerberos**.

No se puede quitar el nombre de dominio Kerberos predeterminado del controlador de dominio porque esta acción restablecerá la información de DNS y quitarlo podría inutilizar el controlador de dominio.

## <a name="BKMK_Examples"></a>Ejemplos

Establecer por error el nombre de dominio Kerberos escribiendo mal ".COM" en el equipo local para CORP. CONTOSO. CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
Quitar ese nombre de dominio Kerberos erróneos desde el equipo local:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Comprobar la eliminación mediante la ejecución de **ksetup** y revisar la salida.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)