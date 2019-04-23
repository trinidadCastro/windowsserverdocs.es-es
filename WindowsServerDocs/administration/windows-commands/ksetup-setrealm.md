---
title: ksetup:setrealm
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa6b2a21904ec4dae1e60def5bd36647291b1af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877406"
---
# <a name="ksetupsetrealm"></a>ksetup:setrealm



Establece el nombre de un dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DNSDomainName>|El nombre de dominio DNS puede ser en forma de un nombre de dominio completo o el nombre de dominio simple.|

## <a name="remarks"></a>Comentarios

El parámetro de nombre de dominio DNS debe escribirse en mayúsculas. En caso contrario, el **ksetup** comando le solicitará de verificación continuar.

No se admite establecer el dominio Kerberos en un controlador de dominio. Al intentar hacerlo, se producirá una advertencia y un error de comando.

## <a name="BKMK_Examples"></a>Ejemplos

Establezca el dominio para este equipo a un nombre de dominio específico para restringir el acceso mediante un controlador de dominio que no es solo para el dominio Kerberos de CONTOSO:
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)