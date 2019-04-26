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
ms.openlocfilehash: 3f62208d6576890529be80b1c6cb3cc073a2b4e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853366"
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

Establece el nombre de dominio Kerberos equivocadamente por error ortográfico ". ¿COM? en el equipo local para CORP. CONTOSO. CON
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