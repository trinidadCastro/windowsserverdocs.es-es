---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Evaluación de ejemplos de estrategia de implementación de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3d04530c53150a3222b609a80938d7fdfcdfeff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402585"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Evaluación de ejemplos de estrategia de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere el ejemplo siguiente de una empresa ficticia, contoso Pharmaceuticals, que implementa Active Directory Domain Services (AD DS) en su entorno. El entorno de Contoso consta de cuatro dominios. El nivel funcional del bosque es Windows Server 2003. En la ilustración siguiente se muestra la estructura de dominio actual de la organización de contoso.  
  
![Estrategia de implementación de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Después de revisar su entorno existente e identificar sus objetivos de implementación, contoso ha establecido lo siguiente AD DS estrategia de implementación:  
  
-   Actualice los dominios de Windows Server 2003 a dominios de Windows Server 2008.  
  
-   Habilite las características de AD DS avanzadas elevando los niveles funcionales de dominio y bosque a Windows Server 2008.  
  
-   Reestructure el dominio africa.concorp.contoso.com en el bosque para consolidar ese dominio con el dominio EMEA. concorp. contoso. con.  
  
Elevar el nivel funcional del bosque a Windows Server 2008 permitirá a contoso sacar el máximo partido de las nuevas características de AD DS. Reestructurar los dominios del bosque, tal como se muestra en la siguiente ilustración, reducirá la cantidad de administración necesaria para administrar los dominios.  
  
![Estrategia de implementación de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


