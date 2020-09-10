---
title: ksetup addkpasswd
description: Artículo de referencia para el comando ksetup addkpasswd, que agrega una dirección de servidor de contraseña Kerberos (Kpasswd) para un dominio Kerberos.
ms.topic: reference
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 21cbbfeeedccc54dc81121502a858e9418bec07d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639732"
---
# <a name="ksetup-addkpasswd"></a>ksetup addkpasswd

Agrega una dirección de servidor de contraseña Kerberos (Kpasswd) para un dominio Kerberos.

## <a name="syntax"></a>Sintaxis

```
ksetup /addkpasswd <realmname> [<kpasswdname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM, y aparece como el dominio Kerberos o el **dominio Kerberos** predeterminados = cuando se ejecuta **ksetup** . |
| `<kpasswdname>` | Especifica el servidor de contraseñas de Kerberos. Se indica como un nombre de dominio completo que no distingue entre mayúsculas y minúsculas, como mitkdc.contoso.com. Si se omite el nombre del KDC, se podría usar DNS para buscar KDC. |

#### <a name="remarks"></a>Observaciones

- Si el dominio Kerberos con el que se va a autenticar la estación de trabajo es compatible con el protocolo de cambio de contraseña de Kerberos, puede configurar un equipo cliente que ejecute el sistema operativo Windows para que use un servidor de contraseñas de Kerberos.

- Puede Agregar nombres KDC adicionales de uno en uno.

### <a name="examples"></a>Ejemplos

Para configurar CORP. CONTOSO.COM dominio Kerberos para usar el servidor KDC que no es de Windows, mitkdc.contoso.com, como servidor de contraseñas, escriba:

```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

Para comprobar que se ha establecido el nombre del KDC, escriba `ksetup` y, a continuación, vea la salida, buscando el texto **Kpasswd =**. Si no ve el texto, significa que la asignación no se ha configurado.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup delkpasswd, comando](ksetup-delkpasswd.md)
