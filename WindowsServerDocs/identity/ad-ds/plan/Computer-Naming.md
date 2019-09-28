---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nomenclatura de equipos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: dd666270ea19e69d88e8dc69941ab086bcd788f5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402746"
---
# <a name="computer-naming"></a>Nomenclatura de equipos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando un equipo que ejecuta el sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista se une a un dominio, de forma predeterminada el equipo se asigna a sí mismo un nombre. El nombre que asigna se compone del nombre de host del equipo (es decir, el nombre del equipo en las propiedades del sistema) y el nombre del sistema de nombres de dominio (DNS) del dominio Active Directory que el equipo ha unido (es decir, el sufijo DNS principal en las propiedades del sistema). La concatenación del nombre de host y el nombre DNS del dominio se conoce como el nombre de dominio completo (FQDN). Por ejemplo, si un equipo con el nombre de host server1 se une al dominio corp.contoso.com, el FQDN del equipo es server1.corp.contoso.com.  
  
Si un equipo ya tiene un nombre de dominio DNS diferente que se especificó estáticamente en una zona DNS o lo registró un servicio de servidor DNS o protocolo de configuración dinámica de host (DHCP) integrado, el FQDN del equipo es distinto del nombre que se registró. visitado. Se puede hacer referencia al equipo por cualquier nombre.  
  
Para obtener más información acerca de las convenciones de nomenclatura en Active Directory Domain Services (AD DS), consulte convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


