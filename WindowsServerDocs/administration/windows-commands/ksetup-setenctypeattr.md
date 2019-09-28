---
title: 'ksetup: setenctypeattr'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bcc268ecb591e3008d7274f6c9f6d6017c86f99d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374934"
---
# <a name="ksetupsetenctypeattr"></a>ksetup: setenctypeattr



Establece el atributo de tipo de cifrado para el dominio. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0DomainName >|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso.|
|Tipo de cifrado|Debe ser uno de los siguientes tipos de cifrado admitidos:</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Comentarios

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el comando **klist** y vea la salida.

Puede establecer o agregar varios tipos de cifrado separando los tipos de cifrado en el comando con un espacio. Sin embargo, solo puede hacerlo para un dominio a la vez.

Si el comando se ejecuta correctamente o produce un error, se muestra un mensaje de estado.

Para establecer el dominio al que desea conectarse y usar, ejecute el comando **ksetup/domain \<DomainName >** .

## <a name="BKMK_Examples"></a>Example

Determinar los tipos de cifrado actuales que se establecen en este equipo:
```
klist
```
Establezca el dominio en corp.contoso.com:
```
ksetup /domain corp.contoso.com
```
Establezca el atributo de tipo de cifrado en AES-256-CTS-HMAC-SHA1-96 para el dominio corp.contoso.com:
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Compruebe que el atributo de tipo de cifrado se ha establecido como previsto para el dominio:
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)