---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: "Mejoras de auditoría de AD FS en Windows Server 2016"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Mejoras de auditoría de AD FS en Windows Server 2016

>Se aplica a: Windows Server 2016

Actualmente, en AD FS de Windows Server 2012 R2 allí se numerosos eventos de auditoría generados para una sola solicitud y la información relevante acerca de un registro o actividad de emisión de token está ausente (en algunas versiones de AD FS) o que se extienda a través de varios eventos de auditoría. De manera predeterminada la AD FS eventos de auditoría están desactivados debido a su naturaleza detallada.  
    Con el lanzamiento de AD FS en Windows Server 2016, auditoría ha vuelto más fluida y menos detallado.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Niveles de auditoría en AD FS de Windows Server 2016  
De manera predeterminada, AD FS en Windows Server 2016 tiene que habilitar la auditoría básica.  Con la auditoría básica, los administradores verán eventos 5 o menos para una sola solicitud.  Esto marca una reducción significativa en el número de eventos, los administradores tienen mirando, para ver una sola solicitud.   El nivel de auditoría pueden provocar o rebajado usando el cmdlt PowerShell: conjunto AdfsProperties - AuditLevel.  La siguiente tabla explica los niveles de auditoría disponibles.  
  
||||  
|-|-|-|  
|Nivel de auditoría|Sintaxis de PowerShell|Descripción|  
|Ninguno|Set-AdfsProperties - AuditLevel ninguno|Auditoría está deshabilitada y no hay eventos se registrarán.|  
|Básico (predeterminado)|Set-AdfsProperties - AuditLevel Basic|No más de 5 eventos se registrarán para una sola solicitud|  
|Detallado|Set-AdfsProperties - AuditLevel detallado|Todos los eventos se registrarán.  Esta opción registran una gran cantidad de información por solicitud.|  
  
Para ver el nivel de auditoría actual, puedes usar la cmdlt PowerShell: Get-AdfsProperties.  
  
![mejoras de auditoría](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
El nivel de auditoría pueden provocar o rebajado usando el cmdlt PowerShell: conjunto AdfsProperties - AuditLevel.  
  
![mejoras de auditoría](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipos de eventos de auditoría  
Eventos de auditoría de AD FS puede ser de diferentes tipos, en función de los diferentes tipos de solicitudes procesadas por AD FS. Cada tipo de evento de auditoría tiene datos específicos asociados a él.  El tipo de eventos de auditoría puede diferenciar entre solicitudes de inicio de sesión (es decir, solicitudes de token) frente a las solicitudes del sistema (incluyendo la obtención de información de configuración de llamadas de servidor).    
  La siguiente tabla describe los tipos de eventos de auditoría básicos.  
  
||||  
|-|-|-|  
|Tipo de evento de auditoría|Identificador de evento|Descripción|  
|Éxito de validación de credenciales desde cero|1202|Donde frescas valida las credenciales correctamente los servicios de federación de solicitud. Esto incluye WS-Trust, WS-federación SAML-P (el primer segmento para generar SSO) y OAuth autorizar extremos.|  
|Error de validación de credenciales desde cero|1203|Una solicitud donde error de validación de credenciales desde cero en el servicio de federación. Esto incluye WS-Trust, WS-Federal de tecnología, SAML P (el primer segmento para generar SSO) y OAuth autorizar extremos.|  
|Éxito de Token de aplicación|1200|Una solicitud de donde se emite correctamente un token de seguridad por el servicio de federación. Para WS-federación, esto se registra cuando se procesa la solicitud con el artefacto SSO SAML-P. (por ejemplo, la cookie SSO).|  
|Error de Token de la aplicación|1201|Una solicitud que no se pudo emisión de token de seguridad en el servicio de federación. Para WS-federación, esto se registra cuando se procesó la solicitud con el artefacto SSO SAML-P. (por ejemplo, la cookie SSO).|  
|Éxito de solicitud de cambio de contraseña|1204|Una transacción de solicitud de cambio de la contraseña de donde se ha procesado correctamente el servicio de federación.|  
|Error de solicitud de cambio de contraseña|1205|No se pudo ser procesado por el servicio de federación una transacción donde solicitud de cambio de la contraseña.| 
|Cerrar sesión correcto|1206|Describe una solicitud de cierre de sesión correcta.|  
|Cerrar sesión Error|1207|Describe una solicitud de cierre de sesión erróneas.|  

  


