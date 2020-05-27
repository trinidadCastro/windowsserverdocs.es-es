---
title: ksetup delkpasswd
description: Tema de referencia del comando ksetup delkpasswd, que quita un servidor de contraseña de Kerberos (Kpasswd) para un dominio Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84a70158ad707fb36d1ca8a4879a93fe0b7df06
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817845"
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
