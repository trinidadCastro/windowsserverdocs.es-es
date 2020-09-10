---
title: ksetup delkpasswd
description: Artículo de referencia para el comando ksetup delkpasswd, que quita un servidor de contraseña de Kerberos (Kpasswd) para un dominio Kerberos.
ms.topic: reference
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 52624252953770ebe131a7c31618058232a21d05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639674"
---
# <a name="ksetup-delkpasswd"></a>ksetup delkpasswd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita un servidor de contraseñas de Kerberos (Kpasswd) para un dominio Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /delkpasswd <realmname> <kpasswdname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` |  Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el **dominio Kerberos** predeterminados = cuando se ejecuta **ksetup** . |
| `<kpasswdname>` | Especifica el servidor de contraseñas de Kerberos. Se indica como un nombre de dominio completo que no distingue entre mayúsculas y minúsculas, como mitkdc.contoso.com. Si se omite el nombre del KDC, se podría usar DNS para buscar KDC. |

### <a name="examples"></a>Ejemplos

Para asegurarse de que el dominio Kerberos CORP. CONTOSO.COM usa el servidor KDC que no es de Windows mitkdc.contoso.com como el servidor de contraseñas; escriba:

```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

Para asegurarse de que el dominio Kerberos CORP. CONTOSO.COM no está asignado a un servidor de contraseñas de Kerberos (el nombre de KDC), escriba `ksetup` en el equipo de Windows y, a continuación, vea la salida.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup delkpasswd, comando](ksetup-delkpasswd.md)
