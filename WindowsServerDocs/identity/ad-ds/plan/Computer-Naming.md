---
ms.assetid: f7002265-60fa-40b8-9dd7-4bf131d9320a
title: Nomenclatura de equipos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae2483571d67b4cdb32c2a547b924b1315573da0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842166"
---
# <a name="computer-naming"></a>Nomenclatura de equipos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando un equipo que ejecuta el sistema operativo Windows 2000, Windows XP, Windows Server 2003, Windows Server 2008 o Windows Vista se une a un dominio, de forma predeterminada, el equipo asigna a sí mismo un nombre. El nombre asigna a sí mismo comprende el nombre de host del equipo (es decir, el nombre de equipo en las propiedades del sistema) y el nombre del sistema de nombres de dominio (DNS) del dominio de Active Directory que unió el equipo (es decir, sufijo DNS principal en las propiedades del sistema). La concatenación del nombre de host y el nombre DNS del dominio se conoce como el nombre de dominio completo (FQDN). Por ejemplo, si el nombre de un equipo con host Server1 combinaciones al dominio corp.contoso.com, el FQDN del equipo es server1.corp.contoso.com.  
  
Si un equipo ya tiene un nombre de dominio DNS diferentes que se ha introducido en una zona DNS o registrado por un servicio de servidor de protocolo de configuración de Host (DHCP) / dinámicas de DNS integrado de estáticamente, el FQDN del equipo es distinto del nombre que se registró anteriormente. El equipo puede hacer referencia a cualquier nombre.  
  
Para obtener más información sobre las convenciones de nomenclatura en Active Directory Domain Services (AD DS), vea las convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas ([https://go.microsoft.com/fwlink/?LinkID=106629](https://go.microsoft.com/fwlink/?LinkID=106629)).  
  


