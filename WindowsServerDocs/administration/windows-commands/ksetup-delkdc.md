---
title: ksetup:delkdc
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e2fa065ca60338b04cc8718e199805cc494c908
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814566"
---
# <a name="ksetupdelkdc"></a>ksetup:delkdc



Elimina las instancias de los nombres de centro de distribución de claves (KDC) para el dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM y aparece como el dominio Kerberos predeterminado cuando **ksetup** se ejecuta. Es este campo desde el que está intentando eliminar el KDC de otro.|
|\<KDCName>|El nombre KDC se define como un nombre de dominio completo entre mayúsculas y minúsculas, como mitkdc.contoso.com.|

## <a name="remarks"></a>Comentarios

Estas asignaciones se almacenan en el registro en **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Para quitar datos de configuración de dominio Kerberos de varios equipos, use la distribución de complemento y la directiva de plantilla de configuración de seguridad en lugar de usar **ksetup** explícitamente en equipos individuales.

En equipos que ejecutan Windows 2000 Server con Service Pack 1 (SP1) y versiones anteriores, el equipo debe reiniciarse antes de que se usará la configuración modificada del dominio Kerberos.

Para comprobar el nombre de dominio Kerberos predeterminado para el equipo, o para comprobar que este comando funciona según lo previsto, ejecute **ksetup** en el símbolo del sistema y compruebe que el KDC que se ha quitado no existe en la lista.

## <a name="BKMK_Examples"></a>Ejemplos

Por lo que se debe quitar el vínculo entre el dominio de Windows y el dominio Kerberos que no sean Windows, han cambiado los requisitos de seguridad para este equipo. En primer lugar, determine qué asociación para quitar y generar la salida de las asociaciones existentes:
```
ksetup
```
Quitar la asociación con el comando siguiente:
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)