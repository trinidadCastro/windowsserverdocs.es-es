---
title: Agregar información de socio de registro del contrato de socio del servicio en línea de Microsoft
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7a297ed077f4c1457bd1e59fc0ea22feedd5de0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817590"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Agregar información de socio de registro del contrato de socio del servicio en línea de Microsoft

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Si es socio del acuerdo de socio de servicio en línea de Microsoft (MOSPA) para Office 365, para asegurarse de que tiene una compensación correcta cuando una solicitud de suscripción se originó en Windows Server Essentials a través del módulo de integración de Office 365, debe crear un clave del registro que contiene la identificación de su asociado de registro (ID.). La siguiente información se lee y se pasa al proveedor de servicios a través de las direcciones URL de suscripción a Office 365.  
  
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
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

