---
title: 'ksetup: addkdc'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bb31cbc8ba7920c4ba609f86202e2e62a705078
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841838"
---
# <a name="ksetupaddkdc"></a>ksetup: addkdc



Agrega una dirección Centro de distribución de claves (KDC) para el dominio Kerberos determinado. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos predeterminado cuando se ejecuta **ksetup** . Es el dominio Kerberos que está intentando agregar el otro KDC.|
|\<KDCName >|El nombre del KDC se indica como un nombre de dominio completo sin distinción de mayúsculas y minúsculas, como mitkdc.microsoft.com. Si se omite el nombre del KDC, DNS buscará KDC.|

## <a name="remarks"></a>Comentarios

Estas asignaciones se almacenan en el registro en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Para implementar datos de configuración de dominio Kerberos en varios equipos, use el complemento de plantilla de configuración de seguridad y la distribución de directivas en lugar de usar **ksetup** explícitamente en equipos individuales.

El equipo debe reiniciarse antes de que se use la nueva configuración de dominio Kerberos.

Para comprobar el nombre de dominio Kerberos predeterminado para el equipo, o para comprobar que este comando funcionó según lo previsto, ejecute **ksetup** en el símbolo del sistema y Compruebe la salida del KDC agregado.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Configure un servidor KDC que no sea de Windows y el dominio Kerberos que debe usar la estación de trabajo:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Ejecute la herramienta Ksetup en la línea de comandos del mismo equipo que en el comando anterior para establecer la contraseña de la cuenta del equipo local en p@sswrd1%. A continuación, reinicie el equipo.
```
Ksetup /setcomputerpassword p@sswrd1%
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)