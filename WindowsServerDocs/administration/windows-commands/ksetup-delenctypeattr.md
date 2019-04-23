---
title: ksetup:delenctypeattr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c2cc96e8156cafd3846422596abe62513e275b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838136"
---
# <a name="ksetupdelenctypeattr"></a>ksetup:delenctypeattr



Quita el atributo de tipo de cifrado para el dominio. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio al que va a establecer una conexión. Utilice el nombre de dominio completo o un formulario simple del nombre, por ejemplo, corp.contoso.com o contoso.|

## <a name="remarks"></a>Comentarios

Para ver el tipo de cifrado para el vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el **klist** de comandos y ver la salida.

Se muestra un mensaje de estado tras la finalización correcta o errónea.

Para establecer el dominio que desea conectarse y usar, ejecute el **/Domain ksetup \<DomainName >** comando.

## <a name="BKMK_Examples"></a>Ejemplos

Determinar los tipos de cifrado actuales que se establecen en este equipo:
```
klist
```
Establecer el dominio a mit.contoso.com:
```
ksetup /domain mit.contoso.com
```
Compruebe lo que es el atributo de tipo de cifrado para el dominio:
```
ksetup /getenctypeattr mit.contoso.com
```
Quite el atributo de tipo de conjunto de cifrado para el dominio de mit.contoso.com:
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)