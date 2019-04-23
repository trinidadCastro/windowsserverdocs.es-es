---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Asignar el DNS al rol de propietario de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dde9ed6035b30ba5b5b96b7132d25a1f8a1c0b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884026"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Asignar el DNS al rol de propietario de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El propietario del bosque, se asigna un sistema de nombres de dominio (DNS) para el propietario de los servicios de dominio de Active Directory (AD DS) para el bosque. El DNS para el propietario de AD DS del bosque es una persona (o grupo de personas) que es responsable de supervisar la implementación del DNS para la infraestructura de AD DS y para asegurarse de que (si es necesario) los nombres de dominio se registran con las autoridades de Internet adecuadas.  
  
El DNS para el propietario de AD DS es responsable de DNS para el diseño de AD DS para el bosque. Si su organización está funcionando actualmente un servicio de servidor DNS, el diseñador DNS para el servicio servidor DNS existente funciona con el DNS para el propietario de AD DS delegar el nombre DNS raíz del bosque para servidores DNS que se ejecutan en controladores de dominio.  
  
El DNS para el propietario de AD DS para el bosque también mantiene el contacto con el grupo de protocolo de configuración dinámica de Host (DHCP) y el grupo DNS de la organización y coordina los planes de los propietarios individuales de DNS de cada dominio del bosque (si existe) con estos grupos. El propietario del DNS para el bosque, se garantiza que los grupos de DHCP y DNS están implicados en el DNS para el proceso de diseño de AD DS para que cada grupo es compatible con el plan de diseño DNS y puede proporcionar la entrada al principio.  
  


