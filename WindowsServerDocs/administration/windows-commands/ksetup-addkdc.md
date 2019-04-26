---
title: ksetup:addkdc
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d3e0d38bdec11618561ee4acaa32ffdd06695fab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868536"
---
# <a name="ksetupaddkdc"></a>ksetup:addkdc



Agrega una dirección del centro de distribución de claves (KDC) para el dominio Kerberos especificado. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM y aparece como el dominio Kerberos predeterminado cuando **ksetup** se ejecuta. Es este dominio Kerberos que está intentando agregar otro KDC.|
|\<KDCName>|El nombre KDC se define como un nombre de dominio completo entre mayúsculas y minúsculas, como mitkdc.microsoft.com. Si se omite el nombre KDC, DNS buscará los KDC.|

## <a name="remarks"></a>Comentarios

Estas asignaciones se almacenan en el registro bajo **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Para implementar datos de configuración de dominio Kerberos de Kerberos en varios equipos, utilice la distribución de complemento y la directiva de plantilla de configuración de seguridad en lugar de usar **ksetup** explícitamente en equipos individuales.

El equipo debe reiniciarse antes de que se usará la configuración del dominio.

Para comprobar el nombre de dominio Kerberos predeterminado para el equipo, o para comprobar que este comando funciona según lo previsto, ejecute **ksetup** en el símbolo del sistema y compruebe la salida para el KDC se ha agregado.

## <a name="BKMK_Examples"></a>Ejemplos

Configurar un servidor que no sean Windows KDC y el territorio que debe usar la estación de trabajo:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Ejecutar la herramienta de Ksetup en la línea de comandos del mismo equipo como en el comando anterior para establecer la contraseña de cuenta de equipo local "p@sswrd1%?. A continuación, reinicie el equipo.
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)