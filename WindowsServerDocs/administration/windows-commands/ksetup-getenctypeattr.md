---
title: 'ksetup: getenctypeattr'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60de138ac73140c69e9a863083e01a51c0e13ca3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841538"
---
# <a name="ksetupgetenctypeattr"></a>ksetup: getenctypeattr



Recupera el atributo de tipo de cifrado del dominio. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName >|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso.|

## <a name="remarks"></a>Comentarios

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el comando **klist** y vea la salida.

Si el comando se ejecuta correctamente o produce un error, se muestra un mensaje de estado cuando se completa correctamente o con errores.

Para establecer el dominio al que desea conectarse y usar, ejecute el comando **ksetup/domain \<nombredominio >** .

## <a name="examples"></a><a name=BKMK_Examples></a>Example

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