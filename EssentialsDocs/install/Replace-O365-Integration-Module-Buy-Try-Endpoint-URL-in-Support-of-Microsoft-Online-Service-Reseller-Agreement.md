---
title: Sustitución de la dirección URL de extremo de compra/prueba del módulo de integración de O365 en el soporte del contrato de revendedor de servicio en línea de Microsoft
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b690cedd2f692cc6d11af6e05dd0cd4b4ea5a1d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833106"
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Sustitución de la dirección URL de extremo de compra/prueba del módulo de integración de O365 en el soporte del contrato de revendedor de servicio en línea de Microsoft

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 Si es un asociado de acuerdo de revendedor de servicio en línea de Microsoft (MOSRA), para asegurarse de que se procesan las transacciones de la suscripción de cliente a través de su portal, deberá reemplazar las direcciones URL de punto de conexión usadas por el módulo de integración de Office 365 de Windows Server Essentials.  
  
 El módulo de integración utiliza las cuatro direcciones URL de extremo siguientes:  
  
1.  Un extremo de adquisición de suscripciones de Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG SZ  
  
    -   Nombre de clave = MOSRASTDBUY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Un extremo de prueba de suscripciones de Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG SZ  
  
    -   Nombre de clave = MOSRASTDTRY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Un extremo de compra de suscripciones de Office 365 pequeña empresa Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG SZ  
  
    -   Nombre de clave = MOSRALITEBUY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Un extremo de evaluación de suscripciones de Office 365 pequeña empresa Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG SZ  
  
    -   Nombre de clave = MOSRALITETRY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL de compra de suscripciones de la empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Para agregar una clave de dirección URL de extremo al Registro  
  
1.  En el equipo de referencia, haga clic en **Inicio**, escriba **regedit** y después presione ENTRAR.  
  
2.  En el panel izquierdo, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** y, finalmente, **MSO**.  
  
3.  Si MSO no existe, haga clic con el botón secundario en **Windows Server**, seleccione **Nuevo**, haga clic en **Clave** y después escriba **MSO** como nombre de la clave.  
  
4.  Haga clic con el botón secundario en MSO y, a continuación, haga clic en **Valor de cadena**. Escriba uno de los siguientes nombres de cadena de extremo como nombre de la cadena:  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Haga clic con el botón secundario sobre la nueva cadena en el panel derecho y a continuación haga clic en **Modificar**.  
  
6.  Escriba la nueva dirección URL de extremo en el cuadro de texto **Información del valor** y haga clic en **Aceptar**.  
  
7.  Repita los pasos 4 a 6 para cada nombre de cadena que se enumera en el paso 4.  
  
## <a name="see-also"></a>Vea también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md) [crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparar la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

