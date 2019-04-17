---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Asignar el DNS para el rol de AD DS propietario
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e393cbf32aa5a13ff22044eabb8c575508baaf79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Asignar el DNS para el rol de AD DS propietario

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El propietario de bosque asigna un sistema de nombre de dominio (DNS) para el propietario de los servicios de dominio de Active Directory (AD DS) para el bosque. El DNS de propietario de AD DS del bosque es una persona (o grupo de personas) quién es responsable de supervisar la implementación de DNS para la infraestructura de AD DS y para asegurarse de que (si procede) se registran los nombres de dominio con las autoridades de Internet adecuadas.  
  
El DNS de propietario de AD DS es responsable de DNS para un diseño el bosque de AD DS. Si tu organización está funcionando un servicio de servidor DNS, el diseñador DNS para el servicio de servidor DNS existente funciona con el DNS de propietario de AD DS delegar el nombre DNS raíz del bosque a servidores DNS que se ejecuten en controladores de dominio.  
  
El DNS de propietario de AD DS para el bosque también mantiene el contacto con el grupo de protocolo de configuración dinámica de Host (DHCP) y el grupo DNS de la organización y coordenadas de los planes de los propietarios DNS individuales de cada dominio del bosque (si existe) con estos grupos. El propietario DNS para el bosque garantiza que los grupos DHCP y DNS intervienen en DNS para el proceso de diseño de AD DS para que cada grupo tiene constancia del plan de diseño DNS y puede proporcionar datos de entrada desde el principio.  
  


