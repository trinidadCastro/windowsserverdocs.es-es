---
title: 'ksetup: setrealm'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 453977ac39dd3a52b4f5a3104995f944e4a48392
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724555"
---
# <a name="ksetupsetrealm"></a>ksetup: setrealm



Establece el nombre de un dominio Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NombreDominioDns>|El nombre de dominio DNS puede tener el formato de un nombre de dominio completo o un nombre de dominio simple.|

## <a name="remarks"></a>Observaciones

El parámetro de nombre de dominio DNS debe escribirse en mayúsculas. De lo contrario, el comando **ksetup** solicitará la comprobación para continuar.

No se admite el establecimiento del dominio Kerberos en un controlador de dominio. Si intenta hacerlo, se producirá una advertencia y un error de comando.

## <a name="examples"></a>Ejemplos

Establezca el dominio Kerberos para este equipo en un nombre de dominio específico para restringir el acceso por parte de un controlador que no sea de dominio al dominio Kerberos de CONTOSO:
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)