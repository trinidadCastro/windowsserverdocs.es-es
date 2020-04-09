---
title: 'ksetup: removerealm'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1465ce08c0cf45de828683324b29fb2df8d0e893
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841458"
---
# <a name="ksetupremoverealm"></a>ksetup: removerealm



Elimina toda la información del dominio Kerberos especificado del registro. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos predeterminado cuando se ejecuta **ksetup** .|

## <a name="remarks"></a>Comentarios

El nombre de dominio Kerberos se almacena en dos lugares del registro: **HKEY_LOCAL_MACHINE \system\controlset001** y **\CurrentControlSet\Control\Lsa\Kerberos**.

No se puede quitar el nombre de dominio Kerberos predeterminado del controlador de dominio, ya que se restablecerá la información de DNS y se podrá quitar el controlador de dominio.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

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