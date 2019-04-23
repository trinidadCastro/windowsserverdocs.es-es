---
title: ksetup:getenctypeattr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 302f94616f98eb350332b08ad37a58305a0a0be1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841996"
---
# <a name="ksetupgetenctypeattr"></a>ksetup:getenctypeattr



Recupera el atributo de tipo de cifrado para el dominio. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName>|Nombre del dominio al que va a establecer una conexión. Utilice el nombre de dominio completo o un formulario simple del nombre, por ejemplo, corp.contoso.com o contoso.|

## <a name="remarks"></a>Comentarios

Para ver el tipo de cifrado para el vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el **klist** de comandos y ver la salida.

Si el comando se ejecuta correctamente o produce un error, se muestra un mensaje de estado tras la finalización correcta o errónea.

Para establecer el dominio que desea conectarse y usar, ejecute el **/Domain ksetup \<DomainName >** comando.

## <a name="BKMK_Examples"></a>Ejemplos

Compruebe que el atributo de tipo de cifrado para el dominio:
```
ksetup /getenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)