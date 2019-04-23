---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Mejoras de auditorías para AD FS en Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880236"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Mejoras de auditorías para AD FS en Windows Server 2016

>Se aplica a: Windows Server 2016

Actualmente, en AD FS para Windows Server 2012 R2 existe son numerosos eventos de auditoría generados para una única solicitud y la información relevante acerca de un registro o actividad de emisión de tokens está ausente (en algunas versiones de AD FS) o se extiende en varios eventos de auditoría. De forma predeterminada, AD FS están desactivados eventos de auditoría debido a su naturaleza detallado.  
    Con el lanzamiento de AD FS en Windows Server 2016, la auditoría se ha convertido en más sencilla y menos detallado.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Niveles de auditoría de AD FS para Windows Server 2016  
De forma predeterminada, AD FS en Windows Server 2016 tiene habilitada la auditoría básica.  Con la auditoría básica, los administradores verán 5 o menos eventos para una única solicitud.  Esto marca una disminución notable en el número de eventos que los administradores tienen que mirar, para ver una sola solicitud.   El nivel de auditoría se puede generar o bajado usando el cmdlet de PowerShell:  Set-AdfsProperties -AuditLevel.  La siguiente tabla explica los niveles de auditoría disponibles.  
  
||||  
|-|-|-|  
|Nivel de auditoría|Sintaxis de PowerShell|Descripción|  
|Ninguno|Set-AdfsProperties - AuditLevel ninguno|La auditoría está deshabilitada y no se registrará ningún evento.|  
|Básico (predeterminado)|Set-AdfsProperties - AuditLevel Basic|No más de 5 eventos se registrarán para una única solicitud|  
|Verbose|Set-AdfsProperties - AuditLevel detallado|Todos los eventos se registrarán.  Esto registrará una cantidad significativa de información por solicitud.|  
  
Para ver el nivel de auditoría actual, puede usar el cmdlet de PowerShell:  Get-AdfsProperties.  
  
![mejoras de auditoría](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
El nivel de auditoría se puede generar o bajado usando el cmdlet de PowerShell:  Set-AdfsProperties -AuditLevel.  
  
![mejoras de auditoría](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Tipos de eventos de auditoría  
Eventos de auditoría de AD FS puede ser de tipos diferentes, según los distintos tipos de solicitudes procesadas por AD FS. Cada tipo de evento de auditoría tiene datos específicos asociados con él.  El tipo de eventos de auditoría se puede diferenciar entre las solicitudes de inicio de sesión (es decir, las solicitudes de token) frente a las solicitudes del sistema (llamadas a servidores incluidos capturando información de configuración).    
  La siguiente tabla describe los tipos de eventos de auditoría básicos.  
  
||||  
|-|-|-|  
|Tipo de evento de auditoría|Id. de evento|Descripción|  
|Nueva credencial validación correcta|1202|Una solicitud donde se validan correctamente las credenciales nuevas por el servicio de federación. Esto incluye WS-Trust, WS-Federation, SAML-P (primer segmento para generar el inicio de sesión único) y puntos de conexión de autorización de OAuth.|  
|Error de validación de credencial nuevo|1203|Una solicitud donde error de validación de la nueva credencial en el servicio de federación. Esto incluye WS-Trust, WS-Fed, SAML-P (primer segmento para generar el inicio de sesión único) y puntos de conexión de autorización de OAuth.|  
|Aplicación correcta de Token|1200|Una solicitud que se emite correctamente un token de seguridad mediante el servicio de federación. Para WS-Federation, SAML-P se registra cuando se procesa la solicitud con el artefacto SSO. (por ejemplo, la cookie de inicio de sesión único).|  
|Error de símbolo (token) de la aplicación|1201|Error de una solicitud de emisión de tokens de seguridad de que en el servicio de federación. Para WS-Federation, SAML-P se registra cuando se procesó la solicitud con el artefacto SSO. (por ejemplo, la cookie de inicio de sesión único).|  
|Solicitud de cambio de contraseña correcto|1204|Una transacción de solicitud de cambio de la contraseña de donde se ha procesado correctamente por el servicio de federación.|  
|Error de solicitud de cambio de contraseña|1205|Una transacción de solicitud de cambio de la contraseña de donde no se pudo ser procesados por el servicio de federación.| 
|Cerrar sesión correcto|1206|Describe una solicitud de cierre de sesión correcta.|  
|Error de cerrar la sesión|1207|Describe una solicitud de cierre de sesión con error.|  

  


