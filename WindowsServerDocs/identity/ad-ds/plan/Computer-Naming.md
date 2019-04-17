---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nombres de equipo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d0dbe2da76a1cf3d1a4dd74183b5dd7106ef17b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="computer-naming"></a>Nombres de equipo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando un equipo que ejecute el sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista se une a un dominio, de forma predeterminada, el equipo asigna a sí mismo un nombre. El nombre asigna a sí mismo incluye el nombre de host del equipo (es decir, el nombre de equipo en las propiedades del sistema) y el nombre del sistema de nombres de dominio (DNS) del dominio de Active Directory que el equipo unido (es decir, sufijo DNS principal en Propiedades del sistema). La concatenación del nombre de host y el nombre DNS del dominio se conoce como el nombre de dominio completo (FQDN). Por ejemplo, si un equipo con el nombre de host servidor 1 unirse al dominio corp.contoso.com, el nombre completo del equipo es server1.corp.contoso.com.  
  
Si un equipo ya tiene un nombre de dominio DNS diferentes estáticamente introducido en una zona DNS o registrado por un servicio de servidor de protocolo de configuración de DNS o dinámicos Host (DHCP) integrado, el nombre completo del equipo es distinto del nombre que se registró anteriormente. El equipo puede hacer referencia a cualquier nombre.  
  
Para obtener más información acerca de las convenciones de nomenclatura en los servicios de dominio de Active Directory (AD DS), consulta las convenciones de nomenclatura de Active Directory para equipos, dominios, sitios y unidades organizativas ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


