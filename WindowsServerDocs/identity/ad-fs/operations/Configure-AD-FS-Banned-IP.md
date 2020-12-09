---
title: Configuración de AD FS IP prohibidas direcciones
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.openlocfilehash: 5aa751f29eb83777370dbd350cdbabf9ae3226d3
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866294"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS y direcciones IP prohibidas


En junio de 2018, AD FS en Windows Server 2016 presentó **IP prohibidas** con la AD FS actualización del 2018 de junio.  Esta actualización le permite configurar un conjunto de direcciones IP globalmente en AD FS, de modo que las solicitudes procedentes de esas direcciones IP, o que tengan esas direcciones IP en los encabezados **x-forwarded-for** o **x-MS-forwarded-Client-IP** , se bloquearán mediante AD FS.

## <a name="adding-banned-ips"></a>Agregar direcciones IP prohibidas
Para agregar direcciones IP prohibidas a la lista global, use el siguiente cmdlet de PowerShell:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formatos permitidos

1.  IPv4
2.  IPv6
3.  Formato CIDR con IPv4 o V6

Hay un límite de 300 entradas para las direcciones IP prohibidas. Puede usar CIDR o el formato de intervalo para denegar un bloque grande de entradas con una sola entrada.

## <a name="removing-banned-ips"></a>Quitar direcciones IP prohibidas
Para quitar direcciones IP prohibidas de la lista global, use el siguiente cmdlet de PowerShell:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Leer direcciones IP prohibidas
Para leer el conjunto actual de direcciones IP prohibidas, use el siguiente cmdlet de PowerShell:

``` powershell
PS C:\ >Get-AdfsProperties
```

Salida de ejemplo:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Referencias adicionales
[Prácticas recomendadas para proteger Servicios de federación de Active Directory (AD FS)](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](/powershell/module/adfs/set-adfsproperties)

[Operaciones de AD FS](../ad-fs-operations.md)
