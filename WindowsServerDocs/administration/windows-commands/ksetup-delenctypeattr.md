---
title: 'ksetup: delenctypeattr'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c650b973ac34e28394d5b6ec38142a058ad76338
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841768"
---
# <a name="ksetupdelenctypeattr"></a>ksetup: delenctypeattr



Quita el atributo de tipo de cifrado del dominio. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<DomainName >|Nombre del dominio en el que desea establecer una conexión. Use el nombre de dominio completo o una forma sencilla del nombre, como corp.contoso.com o contoso.|

## <a name="remarks"></a>Comentarios

Para ver el tipo de cifrado del vale de concesión de vales (TGT) de Kerberos y la clave de sesión, ejecute el comando **klist** y vea la salida.

Se muestra un mensaje de estado cuando se completa correctamente o con errores.

Para establecer el dominio al que desea conectarse y usar, ejecute el comando **ksetup/domain \<nombredominio >** .

## <a name="examples"></a><a name=BKMK_Examples></a>Example

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