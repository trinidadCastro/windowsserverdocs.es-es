---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Asignar el DNS al rol de propietario de AD DS
description: Información sobre cómo asignar el DNS para AD DS rol de propietario.
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 56097e1a7db947be2d4b7dcbf83798f324346fc0
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068237"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Asignar el DNS al rol de propietario de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El propietario del bosque asigna un sistema de nombres de dominio (DNS) para el propietario de Active Directory Domain Services (AD DS) para el bosque. El DNS de AD DS propietario del bosque es una persona (o grupo de personas) responsable de la supervisión de la implementación del DNS para la infraestructura de AD DS y para asegurarse de que los nombres de dominio (si es necesario) estén registrados con las autoridades de Internet adecuadas.

El DNS de AD DS propietario es responsable del diseño de AD DS para el bosque. Si su organización está realizando actualmente un servicio de servidor DNS, el diseñador DNS para el servicio servidor DNS existente funciona con el DNS de AD DS propietario para delegar el nombre DNS raíz del bosque a los servidores DNS que se ejecutan en los controladores de dominio.

El DNS de AD DS propietario del bosque también mantiene el contacto con el grupo de protocolo de configuración dinámica de host (DHCP) y el grupo de DNS de la organización, y coordina los planes de los propietarios DNS individuales de cada dominio del bosque (si existe) con estos grupos. El propietario DNS del bosque garantiza que los grupos DHCP y DNS están implicados en el DNS para AD DS proceso de diseño, de modo que cada grupo sea consciente del plan de diseño de DNS y pueda proporcionar la entrada al principio.
