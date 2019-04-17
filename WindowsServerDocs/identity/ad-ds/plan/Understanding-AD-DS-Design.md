---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: "Descripción de diseño de AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 09afe3d19add87327d05bfafba6e0492a278b968
ms.sourcegitcommit: 1c3e6375b2e8eb01cfd299d0e9478fee46905c99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-ad-ds-design"></a>Descripción de diseño de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las organizaciones pueden usar los servicios de dominio de Active Directory (AD DS) en Windows Server para simplificar la administración de usuarios y recursos durante la creación de infraestructuras escalables, seguras y administrables. Puedes usar AD DS para administrar la infraestructura de red, incluidos sucursal, Microsoft Exchange Server y varios entornos de bosque.  
  
Un proyecto de implementación de AD DS implica tres fases: una fase de diseño, una fase de implementación y operaciones. Durante la fase de diseño, el equipo de diseño crea un diseño de la estructura lógica de AD DS que mejor se adapte a las necesidades de cada división de la organización que usará el servicio de directorio. Después de aprobar el diseño, el equipo de distribución comprueba el diseño en un entorno de laboratorio y, a continuación, implementa el diseño en el entorno de producción. Dado que las pruebas se realizan por el equipo de implementación y potencialmente afecta a la fase de diseño, es una actividad intermedia que se superpone diseño e implementación. Una vez completada la implementación, el equipo de operaciones es responsable de mantener el servicio de directorio.  
  
Aunque las estrategias de diseño e implementación de Windows Server AD DS que se presentan en esta guía se basan en el laboratorio amplia y pruebas de programa piloto y una implementación correcta en entornos de clientes, es posible que debas personalizar el diseño de AD DS y implementación para adaptarse mejor a entornos complejos específicos.  
  
-   Para obtener más información acerca de la implementación de AD DS en un entorno de sucursales, consulta el Read-Only Guía de planificación de Office de controlador de dominio (RODC) rama ([https://go.microsoft.com/fwlink/?LinkId=100207](https://go.microsoft.com/fwlink/?LinkId=100207)).  
  
-   Para obtener más información acerca de la implementación de AD DS en un entorno de Exchange, consulta Exchange 2007 - planear Active Directory ([https://go.microsoft.com/fwlink/?LinkId=88904](https://go.microsoft.com/fwlink/?LinkId=88904)).  
  
-   Para obtener más información acerca de la implementación de AD DS en un entorno de bosques múltiples, consulta varias consideraciones de bosque en Windows 2000 y Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkId=88905](https://go.microsoft.com/fwlink/?LinkId=88905)).  
  


