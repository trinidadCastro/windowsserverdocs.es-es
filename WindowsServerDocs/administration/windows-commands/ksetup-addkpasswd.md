---
title: 'ksetup: addkpasswd'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72c27cb6b068dc46cd58e753b4b08d68b39bfb20
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375195"
---
# <a name="ksetupaddkpasswd"></a>ksetup: addkpasswd



Agrega una dirección de servidor de contraseña Kerberos (Kpasswd) para un dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Parámetros

Si el dominio Kerberos con el que se va a autenticar la estación de trabajo es compatible con el protocolo de cambio de contraseña de Kerberos, puede configurar un equipo cliente que ejecute el sistema operativo Windows para que use un servidor de contraseñas de Kerberos. Esta opción está configurada en el lado del dominio Kerberos.

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0RealmName >|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el dominio Kerberos predeterminados = cuando se ejecuta **ksetup** .|
|@no__t 0KpasswdName >|El nombre del KDC que se va a usar como servidor de contraseñas de Kerberos se indica como un nombre de dominio completo que no distingue mayúsculas de minúsculas, como mitkdc.microsoft.com. Si se omite el nombre del KDC, se podría usar DNS para buscar KDC.|

## <a name="remarks"></a>Comentarios

Si el dominio Kerberos con el que se va a autenticar la estación de trabajo es compatible con el protocolo de cambio de contraseña de Kerberos, puede configurar un equipo cliente que ejecute el sistema operativo Windows para que use un servidor de contraseñas de Kerberos.

Ejecute el comando **ksetup** para comprobar el nombre del KDC. Si **Kpasswd =** no aparece en la salida, la asignación no se ha configurado.

Puede Agregar nombres KDC adicionales de uno en uno.

## <a name="BKMK_Examples"></a>Example

Configure el dominio Kerberos, CORP. CONTOSO.COM, para que use el servidor KDC que no sea de Windows, mitkdc.contoso.com, como servidor de contraseña:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Esto da como resultado un servidor de contraseñas de Kerberos que no es de Windows y que controla todas las contraseñas para la autenticación entre él y el dominio Kerberos.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)