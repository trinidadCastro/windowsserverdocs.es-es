---
title: "Agregar información de Partner de registro del acuerdo de Partner de Microsoft servicio en línea"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 39ce43228cd7392bcc86de4a410c52676ce15047
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Agregar información de Partner de registro del acuerdo de Partner de Microsoft servicio en línea

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Si eres un partner de Microsoft Online Service Partner contrato (MOSPA) para Office 365, para asegurarte de que recibes una compensación correcta cuando una solicitud de suscripción se origina desde Windows Server Essentials a través del módulo de integración de Office 365, debes crear una clave del registro que contenga tu identificación de socio de registro (POR ID). La siguiente información se lee y se pasa al proveedor de servicio a través de las direcciones URL de suscripción de Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows server\mso\  
  
-   Tipo = valor de cadena  
  
-   Nombre de clave = Partner  
  
-   Valor = xxxxx, donde xxxxx es tu POR ID  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Para agregar la clave de POR ID al registro  
  
1.  En el equipo de referencia, haz clic en **inicio**, tipo **regedit**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**y después expande **Windows Server**.  
  
3.  Haz clic en **Windows Server**, elija **nueva**y, a continuación, haz clic en **clave**.  
  
4.  Tipo **MSO** para el nombre de la clave.  
  
5.  Haz clic en la clave que has creado y, a continuación, haz clic en **valor de cadena**.  
  
6.  Tipo **Partner** el nombre de la cadena y presiona ENTRAR.  
  
7.  Haz clic en el nuevo **Partner** de cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  
8.  Escribe tu POR ID en el **información del valor** cuadro de texto y, a continuación, haz clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

