---
title: 'ksetup: delkpasswd'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2de1546b112041f7035a711852140e9bb34babe3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724673"
---
# <a name="ksetupdelkpasswd"></a>ksetup: delkpasswd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

quita un servidor de contraseñas de Kerberos (Kpasswd) para un dominio Kerberos.
## <a name="syntax"></a>Sintaxis
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
#### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                   Descripción                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                El nombre de dominio Kerberos se indica como un nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el dominio Kerberos predeterminados = cuando se ejecuta **ksetup** .                                |
| <KpasswdName> | El nombre del KDC que se va a usar como servidor de contraseñas de Kerberos se indica como un nombre de dominio completo sin distinción entre mayúsculas y minúsculas, como mitkdc.contoso.com. Si se omite el nombre del KDC, se podría usar DNS para buscar KDC. |

## <a name="remarks"></a>Observaciones
Ejecute el comando **ksetup** para comprobar el nombre del KDC. Si **Kpasswd =** no aparece en la salida, la asignación no se ha configurado. Si se establece, se mostrarán varias asignaciones.
## <a name="examples"></a>Ejemplos
Compruebe el dominio de la empresa. CONTOSO.COM usa el servidor KDC que no es de Windows mitkdc.contoso.com como el servidor de contraseñas:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Para comprobar que el comando ha funcionado según lo previsto, ejecute **ksetup** en el equipo Windows para comprobar el dominio de trabajo. CONTOSO.COM no está asignado a un servidor de contraseñas de Kerberos (el nombre del KDC).
## <a name="additional-references"></a>Referencias adicionales
-   [ksetup](ksetup.md)
-   [ksetup: delkpasswd](ksetup-delkpasswd.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
