---
title: Problemas conocidos de DirectAccess
description: En este tema se proporciona un vínculo a los documentos de soporte técnico de Microsoft para DirectAccess en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 3511a91f-1d5d-45a0-97f2-3fc0d6f079b4
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 117e598542ff0140c1cacc533e26d85174f0fcc6
ms.sourcegitcommit: eb995fa887ffe1408b9f67caf743c66107173666
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98666574"
---
# <a name="directaccess-known-issues"></a>Problemas conocidos de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2019

## <a name="dns-registration-of-directaccess-client-ipv6-addresses"></a>Registro DNS de las direcciones IPv6 del cliente de DirectAccess

A partir de la actualización 2020 de Windows 10, un cliente ya no registra sus direcciones IP en los servidores DNS configurados en una tabla de directivas de resolución de nombres (NRPT).
Si se necesita el registro de DNS, por ejemplo, la **Administración fuera**, se puede habilitar explícitamente con esta clave del registro en el cliente:

Ruta de acceso: `HKLM\System\CurrentControlSet\Services\Dnscache\Parameters`<br/>
Tipo: `DWORD`<br/>
Nombre de valor: `DisableNRPTForAdapterRegistration`<br/>
Valores:<br/>
`1` -Registro DNS deshabilitado (valor predeterminado desde la actualización de Windows 10 2020 de mayo)<br/>
`0` -Registro DNS habilitado

## <a name="recommended-hotfixes-and-updates-for-windows-server-2012-directaccess"></a>Revisiones y actualizaciones recomendadas para Windows Server 2012 DirectAccess
En el vínculo siguiente se enumeran los documentos de soporte técnico de Microsoft para DirectAccess que debe revisar y aplicar antes de comenzar la implementación para evitar una configuración inutilizable.

-   [Revisiones y actualizaciones recomendadas para Windows Server 2012 DirectAccess](https://support.microsoft.com/kb/2883952)


