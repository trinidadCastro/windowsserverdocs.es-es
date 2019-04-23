---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Descripción del diseño de AD LDS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c94f6ddd19e3178243545b0cc71f6f4c7bb4dbec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828796"
---
# <a name="understanding-ad-ds-design"></a>Descripción del diseño de AD LDS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las organizaciones pueden usar los servicios de dominio de Active Directory (AD DS) en Windows Server para simplificar la administración de usuarios y recursos durante la creación de infraestructuras escalables, seguras y administrables. Puede usar AD DS para administrar la infraestructura de red, incluidas la sucursal, Microsoft Exchange Server y varios entornos de bosques.  
  
Un proyecto de implementación de AD DS implica tres fases: una fase de diseño, una fase de implementación y operaciones. Durante la fase de diseño, el equipo de diseño crea un diseño de la estructura lógica de AD DS que mejor satisfaga las necesidades de cada división de la organización que se va a usar el servicio de directorio. Una vez que se aprueba el diseño, el equipo de implementación comprueba el diseño en un entorno de laboratorio y, a continuación, implementa el diseño del entorno de producción. Dado que las pruebas se realizaron mediante el equipo de implementación y posiblemente afecta a la fase de diseño, es una actividad intermedia que abarca diseño e implementación. Una vez completada la implementación, el equipo de operaciones es responsable de mantener el servicio de directorio.  
  
Aunque las estrategias de diseño e implementación de Windows Server AD DS que se presentan en esta guía se basan en extensas de laboratorio y prueba del programa piloto y una implementación correcta en entornos de cliente, es posible que deba personalizar su diseño de AD DS y implementación para adaptarse mejor a los entornos complejos específicos.
  
- Para obtener más información sobre la implementación de AD DS en un entorno de sucursal, consulte el [Guía de planeación de oficinas sucursales de controlador de dominio de sólo lectura (RODC)](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Para obtener más información sobre la implementación de AD DS en un entorno de Exchange, consulte el artículo [Exchange 2007: planeación de Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Para obtener más información sobre la implementación de AD DS en un entorno de varios bosques, consulte el artículo [consideraciones sobre varios bosques en Windows 2000 y Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
