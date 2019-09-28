---
title: Paso 2 planear la implementación del servidor RADIUS
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a991b312a0938a3809acd2b94c00aa678f5b41da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404400"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Paso 2 planear la implementación del servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de implementar un solo servidor de acceso remoto, planee el servidor de autenticación de contraseña de un solo tiempo (OTP).  
  
|Tarea|Descripción|  
|----|--------|  
|2,1 planear el servidor RADIUS|Para el servidor de autenticación OTP, el acceso remoto en Windows Server 2016 y Windows Server 2012 admite cualquier servidor OTP habilitado para RADIUS que admita el protocolo de autenticación de contraseña (PAP).|  
  
## <a name="BKMK_1.1"></a>2,1 planear el servidor RADIUS  
Tenga en cuenta lo siguiente al planear un servidor RADIUS para la autenticación de OTP:  
  
-   Para la mayoría de los tipos de implementaciones de OTP, debe configurar el servidor de acceso remoto como agente RADIUS. Para obtener más información, consulte la documentación del proveedor de OTP.  
  
-   Para todas las implementaciones de OTP, debe sincronizar los usuarios de Active Directory con el servidor RADIUS.  
  
-   No es necesario que el servidor RADIUS sea un miembro del dominio.  
  
-   Al implementar el servidor RADIUS, se configura un secreto compartido y el número de puerto para el tráfico RADIUS. Tome nota de estos detalles; son necesarios cuando se configura el servidor de acceso remoto.  
  
Puede ver una guía del laboratorio de pruebas de ejemplo que configura la autenticación de OTP con un servidor RSA SecurID en @no__t guía de laboratorio de 0Test: Demostración de DirectAccess con autenticación OTP y RSA SecurID @ no__t-0.  
  
  
  


