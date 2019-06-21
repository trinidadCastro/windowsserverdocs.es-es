---
title: Paso 2 Planear la implementación del servidor RADIUS
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f74a83c3962c7accd76fbf07307216742ada863d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280828"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Paso 2 Planear la implementación del servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de implementar un único servidor de acceso remoto, planee el servidor de autenticación de contraseña de un solo uso (OTP).  
  
|Tarea|Descripción|  
|----|--------|  
|2.1 planear el servidor RADIUS|Para el servidor de autenticación de OTP, acceso remoto en Windows Server 2016 y Windows Server 2012 es compatible con cualquier servidor compatible con RADIUS OTP que admita el protocolo de autenticación de contraseña (PAP).|  
  
## <a name="BKMK_1.1"></a>2.1 planear el servidor RADIUS  
Tenga en cuenta lo siguiente al planear un servidor RADIUS para la autenticación de OTP:  
  
-   Para la mayoría de los tipos de implementaciones de OTP, debe configurar el servidor de acceso remoto como un agente de RADIUS. Para obtener más información, consulte la documentación del proveedor OTP.  
  
-   Para todas las implementaciones de OTP, debe sincronizar los usuarios de Active Directory con el servidor RADIUS.  
  
-   El servidor RADIUS no es necesario ser miembro del dominio.  
  
-   Al implementar el servidor RADIUS, configure un secreto compartido y el número de puerto para el tráfico RADIUS. Tome nota de estos detalles; son necesarios cuando se configura el servidor de acceso remoto.  
  
Puede ver una guía del laboratorio de prueba de ejemplo que configura la autenticación de OTP con un servidor RSA SecurID en [Test Lab Guide: Demostrar DirectAccess con autenticación OTP y RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  
  
  


