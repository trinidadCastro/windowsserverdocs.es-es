---
title: 'ksetup: delkdc'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63b264da227d51b6f47f982c66828536bd677920
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841698"
---
# <a name="ksetupdelkdc"></a>ksetup: delkdc



Elimina instancias de nombres de Centro de distribución de claves (KDC) para el dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /delkdc <RealmName> <KDCName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos predeterminado cuando se ejecuta **ksetup** . Es el dominio Kerberos desde el que está intentando eliminar el otro KDC.|
|\<KDCName >|El nombre del KDC se indica como un nombre de dominio completo que no distingue entre mayúsculas y minúsculas, como mitkdc.contoso.com.|

## <a name="remarks"></a>Comentarios

Estas asignaciones se almacenan en el registro en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Para quitar los datos de configuración de dominio Kerberos de varios equipos, use el complemento de plantilla de configuración de seguridad y la distribución de directivas en lugar de usar **ksetup** explícitamente en equipos individuales.

En los equipos que ejecutan Windows 2000 Server con Service Pack 1 (SP1) y versiones anteriores, el equipo debe reiniciarse antes de que se use la configuración de configuración de dominio Kerberos modificada.

Para comprobar el nombre de dominio Kerberos predeterminado del equipo, o para comprobar que este comando funcionó como se esperaba, ejecute **ksetup** en el símbolo del sistema y compruebe que el KDC que se quitó no existe en la lista.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Los requisitos de seguridad de este equipo han cambiado, por lo que debe quitarse el vínculo entre el dominio Kerberos de Windows y el dominio que no es de Windows. En primer lugar, determine qué asociación desea quitar y generar la salida de las asociaciones existentes:
```
ksetup
```
Quite la asociación mediante el comando siguiente:
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)