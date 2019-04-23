---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Evaluación de ejemplos de estrategia de implementación de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 169d0a55f9fb167390c13ac1c89f8d68427f318d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842116"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Evaluación de ejemplos de estrategia de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere el siguiente ejemplo de una compañía ficticia, Contoso Pharmaceuticals, que está implementando servicios de dominio de Active Directory (AD DS) en su entorno. El entorno de Contoso está formado por cuatro dominios. El nivel funcional del bosque es Windows Server 2003. La siguiente ilustración muestra la estructura de dominios actual para la organización de Contoso.  
  
![Estrategia de implementación de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Después de revisar el entorno existente e identificar sus objetivos de implementación, Contoso estableció la estrategia de implementación de AD DS siguiente:  
  
-   Actualizar los dominios de Windows Server 2003 a dominios de Windows Server 2008.  
  
-   Habilitar características avanzadas de AD DS al elevar los niveles funcionales de dominio y bosque a Windows Server 2008.  
  
-   Reestructurar el dominio africa.concorp.contoso.com en el bosque para consolidarlo ese dominio con el dominio emea.concorp.contoso.con.  
  
Elevar el nivel funcional del bosque a Windows Server 2008 permitirá Contoso aprovechar al máximo las nuevas características de AD DS. Reestructurar los dominios del bosque, como se muestra en la siguiente ilustración, se reducirá la cantidad de administración que es necesario para administrar los dominios.  
  
![Estrategia de implementación de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


