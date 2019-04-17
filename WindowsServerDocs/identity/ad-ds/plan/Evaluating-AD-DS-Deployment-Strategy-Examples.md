---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: "Evaluar los ejemplos de estrategia de implementación de AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cac1e344cfed078927f2945048807d686deebd1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Evaluar los ejemplos de estrategia de implementación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considera el siguiente ejemplo de la empresa ficticia Contoso farmacéuticas, que es implementar los servicios de dominio de Active Directory (AD DS) en su entorno. El entorno de Contoso consta de cuatro dominios. El nivel funcional del bosque es Windows Server 2003. La siguiente ilustración muestra la estructura de dominio actual para la organización Contoso.  
  
![Estrategia de implementación de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Después de revisar es entorno existente e identificar los objetivos de implementación, Contoso estableció la estrategia de implementación de AD DS siguiente:  
  
-   Actualizar los dominios de Windows Server 2003a dominios de Windows Server 2008.  
  
-   Habilitar características avanzadas de AD DS elevar los niveles funcionales de bosque y dominio a Windows Server 2008.  
  
-   Reestructurar el dominio africa.concorp.contoso.com en el bosque para consolidar ese dominio con el dominio emea.concorp.contoso.con.  
  
Aumentar el nivel funcional del bosque a Windows Server 2008 te permitirá Contoso aprovechar al máximo de las nuevas características de AD DS. Reestructurar los dominios del bosque, como se muestra en la siguiente ilustración, se reducirá la cantidad de administración que es necesario para administrar los dominios.  
  
![Estrategia de implementación de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


