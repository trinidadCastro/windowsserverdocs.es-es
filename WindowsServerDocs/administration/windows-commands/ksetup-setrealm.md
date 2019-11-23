---
title: 'ksetup: setrealm'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1bbe5c000b7e84066c19511639fe3d92d7e4b558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374902"
---
# <a name="ksetupsetrealm"></a>ksetup: setrealm



Establece el nombre de un dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<NombreDominioDns >|El nombre de dominio DNS puede tener el formato de un nombre de dominio completo o un nombre de dominio simple.|

## <a name="remarks"></a>Observaciones

El parámetro de nombre de dominio DNS debe escribirse en mayúsculas. De lo contrario, el comando **ksetup** solicitará la comprobación para continuar.

No se admite el establecimiento del dominio Kerberos en un controlador de dominio. Si intenta hacerlo, se producirá una advertencia y un error de comando.

## <a name="BKMK_Examples"></a>Example

Establezca el dominio Kerberos para este equipo en un nombre de dominio específico para restringir el acceso por parte de un controlador que no sea de dominio al dominio Kerberos de CONTOSO:
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)