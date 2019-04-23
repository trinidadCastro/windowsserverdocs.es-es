---
title: Configurar AD FS no permitidas de direcciones IP
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8ff87a1043b589e83faa875467ddced536291b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867516"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS y las direcciones IP no permitidas

>Se aplica a: Windows Server 2016

En junio de 2018, AD FS en Windows Server 2016 introdujo **prohibidas** con AD FS actualización de junio de 2018.  Esta actualización le permite configurar un conjunto de direcciones IP global en AD FS, para que las solicitudes procedentes de esas direcciones IP, o que tengan esas direcciones IP el **x-forwarded-for** o **x-ms-reenviados-client-ip** encabezados, se bloqueará por AD FS.

## <a name="adding-banned-ips"></a>Adición de direcciones IP no permitidas
Para agregar direcciones IP no permitidas en la lista global, use el siguiente cmdlet de Powershell:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formatos permitidos

1.  IPv4
2.  IPv6
3.  Formato CIDR con IPv4 o v6

Hay un límite de 300 entradas de direcciones IP prohibidas. Puede usar el formato CIDR o intervalo de denegar a un bloque grande de entradas con una sola entrada.

## <a name="removing-banned-ips"></a>Eliminación de direcciones IP no permitidas
Para quitar direcciones IP no permitidas en la lista global, use el siguiente cmdlet de Powershell:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Leer direcciones IP no permitidas
Para leer el conjunto actual de direcciones IP no permitidas, use el siguiente cmdlet de Powershell:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Ejemplo de resultado:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Referencias adicionales  
[Procedimientos recomendados para proteger los servicios de federación de Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
