---
title: 'ksetup: getenctypeattr'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8363113d4fbb310d98b40d852b36a00f20320e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724634"
---
# <a name="ksetupgetenctypeattr"></a>ksetup: getenctypeattr



Recupera el atributo de tipo de cifrado del dominio.

## <a name="syntax"></a>Sintaxis

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso.|

## <a name="remarks"></a>Observaciones

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el comando **klist** y vea la salida.

Si el comando se ejecuta correctamente o produce un error, se muestra un mensaje de estado cuando se completa correctamente o con errores.

Para establecer el dominio al que desea conectarse y usar, ejecute el comando **ksetup/domain \<nombredominio>** .

## <a name="examples"></a>Ejemplos

Compruebe el atributo de tipo de cifrado del dominio:
```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)