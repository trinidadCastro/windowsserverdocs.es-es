---
title: Paso 4 de Plan para OTP en el servidor de acceso remoto
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 470eebc82177a21985afb8d0bf143427a33d65fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825446"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Paso 4 de Plan para OTP en el servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la configuración de certificado y una contraseña de servidor RADIUS (OTP), el último paso para planear una implementación de OTP de acceso remoto es planear la configuración de OTP de cliente en el servidor de acceso remoto.  
  
|Tarea|Descripción|  
|----|--------|  
|[4.1 planear exenciones de OTP cliente](#bkmk_4_1_Exemptions)|Planear exenciones para los usuarios que no requieren autenticación con OTP.|  
|[4.2 planear para los clientes de Windows 7](#bkmk_4_2_Win7)|Plan implementar la conectividad Asistente DirectAccess (DCA) 2.0 en los equipos cliente de Windows 7.|  
|[4.3 plan para tarjetas inteligentes](#BKMK_smartcard)|Planear el uso de tarjetas inteligentes para la autorización adicional.|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 planear exenciones de OTP cliente  
Cuando se habilita la autenticación de OTP, de forma predeterminada todos los usuarios deben autenticarse mediante una combinación de nombre de usuario y contraseña y las credenciales de OTP. Sin embargo, puede permitir que los usuarios seleccionados se autentiquen mediante un nombre de usuario y una contraseña solo, sin OTP. Para ello, cree un grupo de seguridad y agregar los usuarios que desee que quedarán exentos de la autenticación de OTP.  
  
> [!NOTE]  
> Solo los equipos cliente de un único bosque pueden eximirse debido al hecho ese grupo de solo seguridad puede seleccionarse para exenciones de cliente.  
  
## <a name="bkmk_4_2_Win7"></a>4.2 planear para los clientes de Windows 7  
De forma predeterminada, los equipos cliente de Windows 7 no pueden autenticar con OTP.  Los equipos cliente de Windows 7 requieren DCA 2.0 autenticar con OTP en una implementación de acceso remoto de Windows Server 2012. Para obtener más información acerca de DCA 2.0, consulte [2.0 Asistente de conectividad de DirectAccess](https://go.microsoft.com/fwlink/?LinkId=253699) en Microsoft Download Center.  
  
## <a name="BKMK_smartcard"></a>4.3 plan para tarjetas inteligentes  
Cuando se habilita la autenticación de OTP, la opción para habilitar el uso de tarjetas inteligentes para la autorización adicional está disponible. Cree un grupo de seguridad para permitir el acceso temporal en caso de que una tarjeta inteligente de usuario no es funcional.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Configurar DirectAccess con autenticación OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


