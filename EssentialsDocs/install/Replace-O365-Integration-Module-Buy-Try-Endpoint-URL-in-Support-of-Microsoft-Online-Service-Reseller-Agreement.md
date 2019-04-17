---
title: "Reemplazar O365 integración módulo comprar Try dirección URL de extremo para admitir el contrato de revendedores de servicio en línea de Microsoft"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Reemplazar O365 integración módulo comprar Try dirección URL de extremo para admitir el contrato de revendedores de servicio en línea de Microsoft

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 Si eres un Microsoft Online Services revendedor contrato (mosra), para garantizar que las transacciones de clientes suscritos se procesan a través de tu portal, tendrás que reemplaza las direcciones URL de extremo utilizadas por el módulo de integración de Office 365 de Windows Server Essentials.  
  
 El módulo de integración usa las direcciones URL de extremo cuatro siguientes:  
  
1.  Un extremo de compra de suscripción de Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nombre de clave = MOSRASTDBUY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL compra de suscripción de empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Un extremo de prueba de suscripción de Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nombre de clave = MOSRASTDTRY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL compra de suscripción de empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Un extremo de compra de suscripción de Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nombre de clave = MOSRALITEBUY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL compra de suscripción de empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Un extremo de prueba de suscripción de Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nombre de clave = MOSRALITETRY  
  
    -   Valor = *xxxxx*, donde xxxxx es la dirección URL compra de suscripción de empresa. Por ejemplo, valor = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Para agregar una dirección URL de extremo al registro  
  
1.  En el equipo de referencia, haz clic en **inicio**, tipo **regedit**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**y, a continuación, expanda **MSO**.  
  
3.  Si MSO no existe, haz clic en **Windows Server**, elija **nueva**, haz clic en **clave**y, a continuación, escribe **MSO** para el nombre de la clave.  
  
4.  Haz clic en MSO y, a continuación, haz clic en **valor de cadena**. Escribe uno de los siguientes nombres de cadena de extremo como nombre de la cadena:  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Haz clic en la nueva cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  
6.  Escribe la nueva dirección URL de extremo en el **información del valor** cuadro de texto y, a continuación, haz clic en **Aceptar**.  
  
7.  Repite los pasos 4-6 para cada nombre de cadena enumerado en el paso 4.  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)[crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

