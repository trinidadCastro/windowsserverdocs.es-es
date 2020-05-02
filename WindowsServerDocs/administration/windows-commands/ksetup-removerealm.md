---
title: 'ksetup: removerealm'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb7bf4663594a6c164d6495a9ba4cd81942afb79
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724603"
---
# <a name="ksetupremoverealm"></a>ksetup: removerealm



Elimina toda la información del dominio Kerberos especificado del registro.

## <a name="syntax"></a>Sintaxis

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> RealmName|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos predeterminado cuando se ejecuta **ksetup** .|

## <a name="remarks"></a>Observaciones

El nombre de dominio Kerberos se almacena en dos lugares del registro: **HKEY_LOCAL_MACHINE \system\controlset001** y **\CurrentControlSet\Control\Lsa\Kerberos**.

No se puede quitar el nombre de dominio Kerberos predeterminado del controlador de dominio, ya que se restablecerá la información de DNS y se podrá quitar el controlador de dominio.

## <a name="examples"></a>Ejemplos

El nombre de dominio Kerberos se estableció erróneamente. COM en el equipo local en CORP. Senda. TIMADO
```
ksetup /setrealm CORP.CONTOSO.CON
```
Quite el nombre de dominio Kerberos erróneo del equipo local:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Compruebe la eliminación ejecutando **ksetup** y revise la salida.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)