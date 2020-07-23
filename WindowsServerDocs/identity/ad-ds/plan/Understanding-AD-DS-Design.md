---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Descripción del diseño de AD LDS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 72abc2b5b45ec1eefbff3ca82b619649ffd0fda9
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959767"
---
# <a name="understanding-ad-ds-design"></a>Descripción del diseño de AD LDS

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las organizaciones pueden usar Active Directory Domain Services (AD DS) en Windows Server para simplificar la administración de usuarios y recursos al crear infraestructuras escalables, seguras y administrables. Puede usar AD DS para administrar la infraestructura de red, incluidos los entornos de sucursales, Microsoft Exchange Server y varios bosques.

Un proyecto de implementación de AD DS implica tres fases: una fase de diseño, una fase de implementación y una fase de operaciones. Durante la fase de diseño, el equipo de diseño crea un diseño para el AD DS estructura lógica que mejor se adapte a las necesidades de cada división de la organización que utilizará el servicio de directorio. Una vez aprobado el diseño, el equipo de implementación prueba el diseño en un entorno de laboratorio y, a continuación, implementa el diseño en el entorno de producción. Dado que el equipo de implementación realiza las pruebas y esto afecta potencialmente a la fase de diseño, se trata de una actividad provisional que se superpone tanto al diseño como a la implementación. Una vez completada la implementación, el equipo de operaciones es responsable del mantenimiento del servicio de directorio.

Aunque las estrategias de diseño e implementación de Windows Server AD DS que se presentan en esta guía se basan en pruebas exhaustivas de laboratorios y programas piloto y en una implementación correcta en los entornos de los clientes, es posible que tenga que personalizar el diseño y la implementación de AD DS para adaptarse mejor a entornos específicos y complejos.

- Para obtener más información acerca de la implementación de AD DS en un entorno de sucursal, vea la [Guía de planeación de la sucursal del controlador de dominio de solo lectura (RODC)](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd734758(v=ws.10)).
- Para obtener más información acerca de la implementación de AD DS en un entorno de Exchange, consulte el artículo [Active Directory en organizaciones de Exchange Server](/exchange/plan-and-deploy/active-directory/active-directory).
- Para obtener más información sobre la implementación de AD DS en un entorno de varios bosques, vea el artículo [consideraciones sobre el bosque en windows 2000 y Windows Server 2003](/previous-versions/windows/it-pro/windows-server-2003/cc739395(v=ws.10)).
