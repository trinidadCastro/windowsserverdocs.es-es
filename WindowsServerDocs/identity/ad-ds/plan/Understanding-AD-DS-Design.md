---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Descripción del diseño de AD LDS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f46cad23e13edcef57bf00113e601069c02cfd4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402457"
---
# <a name="understanding-ad-ds-design"></a>Descripción del diseño de AD LDS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las organizaciones pueden usar Active Directory Domain Services (AD DS) en Windows Server para simplificar la administración de usuarios y recursos al crear infraestructuras escalables, seguras y administrables. Puede usar AD DS para administrar la infraestructura de red, incluidos los entornos de sucursales, Microsoft Exchange Server y varios bosques.  
  
Un proyecto de implementación de AD DS implica tres fases: una fase de diseño, una fase de implementación y una fase de operaciones. Durante la fase de diseño, el equipo de diseño crea un diseño para el AD DS estructura lógica que mejor se adapte a las necesidades de cada división de la organización que utilizará el servicio de directorio. Una vez aprobado el diseño, el equipo de implementación prueba el diseño en un entorno de laboratorio y, a continuación, implementa el diseño en el entorno de producción. Dado que el equipo de implementación realiza las pruebas y esto afecta potencialmente a la fase de diseño, se trata de una actividad provisional que se superpone tanto al diseño como a la implementación. Una vez completada la implementación, el equipo de operaciones es responsable del mantenimiento del servicio de directorio.  
  
Aunque las estrategias de diseño e implementación de Windows Server AD DS que se presentan en esta guía se basan en las pruebas extensas de laboratorio y programa piloto, y en la implementación correcta en los entornos de los clientes, es posible que tenga que personalizar el diseño del AD DS y implementación para adaptarse mejor a entornos específicos y complejos.
  
- Para obtener más información acerca de la implementación de AD DS en un entorno de sucursal, vea la [Guía de planeación de la sucursal del controlador de dominio de solo lectura (RODC)](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Para obtener más información acerca de la implementación de AD DS en un entorno de Exchange, consulte el artículo [exchange 2007-Planning Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Para obtener más información sobre la implementación de AD DS en un entorno de varios bosques, vea el artículo [consideraciones sobre el bosque en windows 2000 y Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
