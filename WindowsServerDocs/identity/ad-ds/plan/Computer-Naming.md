---
description: 'Más información acerca de: nomenclatura de equipos'
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nomenclatura de equipos
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ecc0bc20a25adfb265a47c0be6ac05719c2aff73
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050013"
---
# <a name="computer-naming"></a>Nomenclatura de equipos

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando un equipo que ejecuta el sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista se une a un dominio, de forma predeterminada el equipo se asigna a sí mismo un nombre. El nombre que asigna se compone del nombre de host del equipo (es decir, el nombre del equipo en las propiedades del sistema) y el nombre del sistema de nombres de dominio (DNS) del dominio Active Directory que el equipo ha unido (es decir, el sufijo DNS principal en las propiedades del sistema). La concatenación del nombre de host y el nombre DNS del dominio se conoce como el nombre de dominio completo (FQDN). Por ejemplo, si un equipo con el nombre de host server1 se une al dominio corp.contoso.com, el FQDN del equipo es server1.corp.contoso.com.

Si un equipo ya tiene un nombre de dominio DNS diferente que se especificó estáticamente en una zona DNS o lo registró un servicio de servidor de protocolo de configuración dinámica de host (DHCP) de DNS o integrado, el FQDN del equipo es distinto del nombre que se registró anteriormente. Se puede hacer referencia al equipo por cualquier nombre.

Para obtener más información acerca de las convenciones de nomenclatura en Active Directory Domain Services (AD DS), consulte [convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/help/909264/).
