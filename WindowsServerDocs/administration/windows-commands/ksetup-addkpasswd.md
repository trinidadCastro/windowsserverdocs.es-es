---
title: 'ksetup: addkpasswd'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c260c711ae87f88be8b9466e73afaf3fe1c83a1e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724736"
---
# <a name="ksetupaddkpasswd"></a>ksetup: addkpasswd



Agrega una dirección de servidor de contraseña Kerberos (Kpasswd) para un dominio Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

#### <a name="parameters"></a>Parámetros

Si el dominio Kerberos con el que se va a autenticar la estación de trabajo es compatible con el protocolo de cambio de contraseña de Kerberos, puede configurar un equipo cliente que ejecute el sistema operativo Windows para que use un servidor de contraseñas de Kerberos. Esta opción está configurada en el lado del dominio Kerberos.

|Parámetro|Descripción|
|---------|-----------|
|\<> RealmName|El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el dominio Kerberos predeterminados = cuando se ejecuta **ksetup** .|
|\<> KpasswdName|El nombre del KDC que se va a usar como servidor de contraseñas de Kerberos se indica como un nombre de dominio completo que no distingue mayúsculas de minúsculas, como mitkdc.microsoft.com. Si se omite el nombre del KDC, se podría usar DNS para buscar KDC.|

## <a name="remarks"></a>Observaciones

Si el dominio Kerberos con el que se va a autenticar la estación de trabajo es compatible con el protocolo de cambio de contraseña de Kerberos, puede configurar un equipo cliente que ejecute el sistema operativo Windows para que use un servidor de contraseñas de Kerberos.

Ejecute el comando **ksetup** para comprobar el nombre del KDC. Si **Kpasswd =** no aparece en la salida, la asignación no se ha configurado.

Puede Agregar nombres KDC adicionales de uno en uno.

## <a name="examples"></a>Ejemplos

Configure el dominio Kerberos, CORP. CONTOSO.COM, para que use el servidor KDC que no sea de Windows, mitkdc.contoso.com, como servidor de contraseña:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Esto da como resultado un servidor de contraseñas de Kerberos que no es de Windows y que controla todas las contraseñas para la autenticación entre él y el dominio Kerberos.

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)