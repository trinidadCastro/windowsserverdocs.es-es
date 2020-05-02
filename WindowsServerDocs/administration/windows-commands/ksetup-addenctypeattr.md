---
title: 'ksetup: addenctypeattr'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32cc87d7-b9e1-4d14-9eb7-3b439c55aa3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26441f739979cde31715e5fb06b5f3ab59845d97
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724750"
---
# <a name="ksetupaddenctypeattr"></a>ksetup: addenctypeattr



Agrega el atributo de tipo de cifrado a la lista de posibles tipos para el dominio.

## <a name="syntax"></a>Sintaxis

```
ksetup /addenctypeattr <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso.|
|Tipo de cifrado|Debe ser uno de los siguientes tipos de cifrado admitidos:</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Observaciones

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el comando **klist** y vea la salida.

Puede establecer o agregar varios tipos de cifrado separando los tipos de cifrado en el comando con un espacio. Sin embargo, solo puede hacerlo para un dominio a la vez.

Si el comando se ejecuta correctamente o produce un error, se muestra un mensaje de estado.

Para establecer el dominio al que desea conectarse y usar, ejecute el comando **ksetup/domain \<nombredominio>** .

## <a name="examples"></a>Ejemplos

Determinar los tipos de cifrado actuales que se establecen en este equipo:
```
klist
```
Establezca el dominio en corp.contoso.com:
```
ksetup /domain corp.contoso.com
```
Agregue el tipo de cifrado AES-256-CTS-HMAC-SHA1-96 a la lista de posibles tipos para el dominio corp.contoso.com:
```
ksetup /addenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Establezca el atributo de tipo de cifrado en AES-256-CTS-HMAC-SHA1-96 para el dominio corp.contoso.com:
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Compruebe que el atributo de tipo de cifrado se ha establecido como previsto para el dominio:
```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)