---
title: 'ksetup: delenctypeattr'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2908cc0a095a6985c11f7885766926b7f0354ab0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724701"
---
# <a name="ksetupdelenctypeattr"></a>ksetup: delenctypeattr



Quita el atributo de tipo de cifrado del dominio.

## <a name="syntax"></a>Sintaxis

```
ksetup /delenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso.|

## <a name="remarks"></a>Observaciones

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el comando **klist** y vea la salida.

Se muestra un mensaje de estado cuando se completa correctamente o con errores.

Para establecer el dominio al que desea conectarse y usar, ejecute el comando **ksetup/domain \<nombredominio>** .

## <a name="examples"></a>Ejemplos

Determinar los tipos de cifrado actuales que se establecen en este equipo:
```
klist
```
Establezca el dominio en mit.contoso.com:
```
ksetup /domain mit.contoso.com
```
Compruebe cuál es el atributo de tipo de cifrado para el dominio:
```
ksetup /getenctypeattr mit.contoso.com
```
Quite el atributo Set Encryption Type del dominio mit.contoso.com:
```
ksetup /delenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)