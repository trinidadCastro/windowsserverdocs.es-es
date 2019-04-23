---
title: Agregar información de socio de registro del contrato de socio del servicio en línea de Microsoft
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833046"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Agregar información de socio de registro del contrato de socio del servicio en línea de Microsoft

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Si es un socio de Microsoft Online Service asociado contrato (MOSPA) para Office 365, para asegurarse de que se compensan correctamente cuando se origina una solicitud de suscripción de Windows Server Essentials a través del módulo de integración de Office 365, deberá crear un clave del registro que contenga su identificación de socio de registro (ID POR). La siguiente información se lee y se pasa al proveedor de servicios a través de las direcciones URL de suscripción a Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Tipo = Valor de cadena  
  
-   Nombre de clave = Partner  
  
-   Valor = xxxxx, donde xxxxx es su ID de POR  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Para agregar la clave ID POR al Registro  
  
1.  En el equipo de referencia, haga clic en **Inicio**, escriba **regedit** y después presione ENTRAR.  
  
2.  En el panel izquierdo, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** y finalmente **Windows Server**.  
  
3.  Haga clic con el botón secundario en **Windows Server**, seleccione **Nuevo** y a continuación haga clic en **Clave**.  
  
4.  Escriba **MSO** como nombre de la clave.  
  
5.  Haga clic con el botón secundario en la clave que acaba de crear y a continuación haga clic en **Valor de cadena**.  
  
6.  Escriba **Partner** como nombre de la cadena y después presione ENTRAR.  
  
7.  Haga clic con el botón secundario en la nueva cadena, **Partner**, en el panel derecho y, a continuación, haga clic en **Modificar**.  
  
8.  Escriba su ID POR en el cuadro de texto **Información del valor** y haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Vea también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparar la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

