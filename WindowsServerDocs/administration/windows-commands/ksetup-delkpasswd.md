---
title: ksetup:delkpasswd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c701707f736fe51a1f4af70a2571e63025f281
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438042"
---
# <a name="ksetupdelkpasswd"></a>ksetup:delkpasswd

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita un servidor de la contraseña de Kerberos (Kpasswd) para un dominio Kerberos. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxis
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                   Descripción                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                El nombre de dominio Kerberos se define como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM y aparece como el valor predeterminado territorio o un territorio = cuando **ksetup** se ejecuta.                                |
| <KpasswdName> | El nombre KDC que se usará como el servidor de la contraseña de Kerberos se define como un nombre de dominio completo entre mayúsculas y minúsculas, como mitkdc.contoso.com. Si se omite el nombre KDC, DNS podría utilizarse para buscar los KDC. |

## <a name="remarks"></a>Comentarios
Ejecute el comando **ksetup** para comprobar el nombre KDC. Si **kpasswd =** no aparece en la salida, no se ha configurado la asignación. Se mostrarán varias asignaciones, si establece.
## <a name="BKMK_Examples"></a>Ejemplos
Compruebe el dominio CORP. CONTOSO.COM utiliza el mitkdc.contoso.com de KDC que no es de Windows server como servidor de contraseñas:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Para comprobar que el comando funcionó según lo previsto, ejecute **ksetup** en el equipo de Windows para comprobar el dominio CORP. CONTOSO.COM no está asignado a un servidor de la contraseña de Kerberos (el nombre KDC).
## <a name="additional-references"></a>Referencias adicionales
-   [ksetup](ksetup.md)
-   [ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
