---
title: ksetup:addkpasswd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2a85eb6dfe30c33126504744a7659fe2cc573087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856826"
---
# <a name="ksetupaddkpasswd"></a>ksetup:addkpasswd



Agrega una dirección de servidor de la contraseña (Kpasswd) de Kerberos para un dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Parámetros

Si el dominio Kerberos que la estación de trabajo van a autenticar a admite Kerberos para cambiar el protocolo de contraseña, puede configurar un equipo cliente que ejecutan el sistema operativo de Windows para usar un servidor de la contraseña de Kerberos. Esta opción se configura en el lado del dominio.

|Parámetro|Descripción|
|---------|-----------|
|\<RealmName>|El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM y aparece como el valor predeterminado territorio o un territorio = cuando **ksetup** se ejecuta.|
|\<KpasswdName>|El nombre de KDC que se usará como el servidor de la contraseña de Kerberos se define como un nombre de dominio completo entre mayúsculas y minúsculas, como mitkdc.microsoft.com. Si se omite el nombre KDC, DNS podría utilizarse para buscar los KDC.|

## <a name="remarks"></a>Comentarios

Si el dominio Kerberos que la estación de trabajo van a autenticar a admite Kerberos para cambiar el protocolo de contraseña, puede configurar un equipo cliente que ejecutan el sistema operativo de Windows para usar un servidor de la contraseña de Kerberos.

Ejecute el comando **ksetup** para comprobar el nombre KDC. Si **kpasswd =** no aparece en la salida, no se ha configurado la asignación.

Puede agregar otros nombres de KDC uno a la vez.

## <a name="BKMK_Examples"></a>Ejemplos

Configurar el dominio Kerberos, CORP. CONTOSO.COM, para que use el servidor que no sean Windows KDC, mitkdc.contoso.com, como el servidor de la contraseña:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Esto da como resultado un servidor de contraseña de Kerberos que no son de Windows que controla todas las contraseñas para la autenticación entre ella y el dominio Kerberos.

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)