---
title: 'ksetup: addkdc'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66efe4e56007aff39b83c92dfea2afaadcfc0210
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375210"
---
# <a name="ksetupaddkdc"></a>ksetup: addkdc



Agrega una dirección Centro de distribución de claves (KDC) para el dominio Kerberos determinado. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos predeterminado cuando se ejecuta **ksetup** . Es el dominio Kerberos que está intentando agregar el otro KDC.|
|\<KDCName >|El nombre del KDC se indica como un nombre de dominio completo sin distinción de mayúsculas y minúsculas, como mitkdc.microsoft.com. Si se omite el nombre del KDC, DNS buscará KDC.|

## <a name="remarks"></a>Observaciones

Estas asignaciones se almacenan en el registro en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Para implementar datos de configuración de dominio Kerberos en varios equipos, use el complemento de plantilla de configuración de seguridad y la distribución de directivas en lugar de usar **ksetup** explícitamente en equipos individuales.

El equipo debe reiniciarse antes de que se use la nueva configuración de dominio Kerberos.

Para comprobar el nombre de dominio Kerberos predeterminado para el equipo, o para comprobar que este comando funcionó según lo previsto, ejecute **ksetup** en el símbolo del sistema y Compruebe la salida del KDC agregado.

## <a name="BKMK_Examples"></a>Example

Configure un servidor KDC que no sea de Windows y el dominio Kerberos que debe usar la estación de trabajo:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Ejecute la herramienta Ksetup en la línea de comandos del mismo equipo que en el comando anterior para establecer la contraseña de la cuenta del equipo local en "p@sswrd1%". A continuación, reinicie el equipo.
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)