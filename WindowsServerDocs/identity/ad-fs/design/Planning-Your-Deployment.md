---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: "Planear la implementación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c7cec9ad92605f3dc98f8ce8fb7853a7ae61299
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="planning-your-deployment"></a>Planear la implementación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Al planear para la colaboración de \(federation\-based\) cross\ organizativa mediante los servicios de federación de Active Directory \(AD FS\), determinar si la organización hospedará un recurso Web para tener acceso a otras organizaciones a través de Internet o si vas a proporcionar acceso al recurso Web para los empleados de la organización. Esta decisión afecta a cómo implementar AD FS y es fundamental en el diseño de la infraestructura de AD FS.  
  
> [!NOTE]  
> Asegúrate de que el papel que desempeña en la organización en el acuerdo de federación claramente se entiende por todas partes.  
  
Para la [federados Web SSO diseño](Federated-Web-SSO-Design.md), AD FS utiliza términos como *asociado de cuenta* \ (también conocida como *proveedor de identidad* en la administración de AD FS snap\-in\) y *asociado de recurso* \ (también denominados *usuario de confianza* en la administración de AD FS snap\-in\) para ayudar a diferenciar la organización que hospeda las cuentas \(the account partner\) de la organización que hospeda los recursos basados en Web\ \(the resource partner\).  
  
En la [Web SSO diseño](Web-SSO-Design.md), la organización actúa en ambas las cuenta partner y recursos partner funciones porque proporciona a los usuarios con acceso a sus aplicaciones.  
  
Los temas siguientes explican que algunos de la AD FS conceptos de la organización de partner. También contienen vínculos a temas de la Guía de implementación de AD FS que contienen información sobre cómo instalar y configurar las organizaciones asociadas de cuenta y las organizaciones asociadas de recursos en función de los objetivos de la implementación de AD FS.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Los procedimientos recomendados para el diseño seguro y la implementación de AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planear para la interoperabilidad con AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Cuándo usar la delegación de identidad](When-to-Use-Identity-Delegation.md)  
  
-   [Implementación de AD FS en la organización de Partner de cuenta](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Implementación de AD FS en la organización de Partner de recurso](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


