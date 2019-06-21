---
title: Paso 2 configurar el servidor RADIUS
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c111ce52f2cca0cc37ea4d5b873c5fde12bce18
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282444"
---
# <a name="step-2-configure-the-radius-server"></a>Paso 2 configurar el servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de configurar el servidor de acceso remoto para admitir admite DirectAccess con OTP, configure el servidor RADIUS.  
  
|Tarea|Descripción|  
|----|--------|  
|[2.1. Configurar los tokens de distribución de software RADIUS](#BKMK_1.1)|En el servidor RADIUS, configure los tokens de distribución de software.|  
|[2.2. Configurar la información de seguridad RADIUS](#BKMK_1.2)|En el servidor RADIUS, configure los puertos y el secreto compartido que se usará.|  
|[2.3 agregando la cuenta de usuario para el sondeo de OTP](#BKMK_Probe)|En el servidor RADIUS, cree una nueva cuenta de usuario para el sondeo de OTP.|  
|[2.4 sincronizar con Active Directory](#BKMK_Active)|En el servidor RADIUS, cree las cuentas de usuario sincronizadas con las cuentas de Active Directory.|  
|[2.5 configurar al agente de autenticación RADIUS](#BKMK_AuthAgent)|Configurar el servidor de acceso remoto como un agente de autenticación RADIUS.|  
  
## <a name="BKMK_1.1"></a>2.1 configurar los tokens de distribución de software RADIUS  
El servidor RADIUS debe configurarse con la licencia necesaria y los tokens de hardware o software de distribución que va a usar DirectAccess con OTP. Este proceso debe ser específico para cada implementación de proveedor RADIUS.  
  
## <a name="BKMK_1.2"></a>2.2 configura la información de seguridad RADIUS  
El servidor RADIUS usa puertos UDP para fines de comunicación, y cada proveedor RADIUS tiene sus propios puertos UDP predeterminada para la comunicación entrante y saliente. Para que el servidor RADIUS trabajar con el servidor de acceso remoto, asegúrese de todos los firewalls en el entorno se configuran para permitir el tráfico UDP entre los servidores de DirectAccess y OTP a través de los puertos necesarios según sea necesario.  
  
El servidor RADIUS usa un secreto compartido para la autenticación. Configurar el servidor RADIUS con una contraseña segura para el secreto compartido y tenga en cuenta que se usará al configurar la configuración del equipo de cliente del servidor de DirectAccess para su uso con DirectAccess con OTP.  
  
## <a name="BKMK_Probe"></a>2.3 agregando la cuenta de usuario para el sondeo de OTP  
En el servidor RADIUS, cree una nueva cuenta de usuario denominada **DAProbeUser** y asígnele la contraseña **DAProbePass**.  
  
## <a name="BKMK_Active"></a>2.4 sincronizar con Active Directory  
El servidor RADIUS debe tener cuentas de usuario que corresponden a los usuarios de Active Directory que se va a utilizar DirectAccess con OTP.  
  
#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Para sincronizar los usuarios RADIUS y Active Directory  
  
1.  Registre la información de usuario de Active Directory para DirectAccess todas con usuarios OTP.  
  
2.  Utilice el procedimiento específico del proveedor para crear un usuario idéntico **dominio\nombre de usuario** cuentas en el servidor RADIUS que se registraron.  
  
## <a name="BKMK_AuthAgent"></a>2.5 configurar al agente de autenticación RADIUS  
El servidor de acceso remoto debe configurarse como un agente de autenticación de RADIUS para el cliente de DirectAccess con la implementación de OTP. Siga las instrucciones del proveedor RADIUS para configurar el servidor de acceso remoto como un agente de autenticación RADIUS.  
  


