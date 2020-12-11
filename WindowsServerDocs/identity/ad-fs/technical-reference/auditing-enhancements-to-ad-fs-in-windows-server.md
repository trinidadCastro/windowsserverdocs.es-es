---
description: Más información acerca de las mejoras de auditoría en AD FS en Windows Server 2016
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Mejoras de auditorías para AD FS en Windows Server 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.openlocfilehash: 1281ce03b291748b093ea491f54e6e7924e03cde
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050403"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Mejoras de auditorías para AD FS en Windows Server 2016

Actualmente, en AD FS para Windows Server 2012 R2, se generan numerosos eventos de auditoría para una única solicitud y la información relevante sobre una actividad de inicio de sesión o de emisión de tokens está ausente (en algunas versiones de AD FS) o se puede distribuir entre varios eventos de auditoría. De forma predeterminada, se desactivan los eventos de auditoría de AD FS debido a su naturaleza detallada.

Con el lanzamiento de AD FS en Windows Server 2016, la auditoría se ha vuelto más optimizada y menos detallada.

## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Niveles de auditoría en AD FS para Windows Server 2016
De forma predeterminada, AD FS en Windows Server 2016 tiene habilitada la auditoría básica.  Con la auditoría básica, los administradores verán 5 eventos o menos para una única solicitud.  Esto marca una disminución significativa en el número de eventos que los administradores tienen que consultar para ver una única solicitud.   El nivel de auditoría se puede aumentar o reducir mediante el cmdlt de PowerShell: Set-AdfsProperties-AuditLevel.  En la tabla siguiente se explican los niveles de auditoría disponibles.

| Nivel de auditoría | Sintaxis de PowerShell | Descripción |
|--|--|--|
| None | Set-AdfsProperties-AuditLevel ninguno | La auditoría está deshabilitada y no se registrará ningún evento. |
| Básico (predeterminado) | Set-AdfsProperties AuditLevel Basic | No se registrarán más de 5 eventos para una única solicitud. |
| Verbose | Set-AdfsProperties-AuditLevel detallado | Se registrarán todos los eventos.  Esto registrará una cantidad significativa de información por solicitud. |

Para ver el nivel de auditoría actual, puede usar el cmdlt de PowerShell: get-AdfsProperties.

![mejoras de auditoría](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)

El nivel de auditoría se puede aumentar o reducir mediante el cmdlt de PowerShell: Set-AdfsProperties-AuditLevel.

![mejoras de auditoría](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)

## <a name="types-of-audit-events"></a>Tipos de eventos de auditoría
AD FS los eventos de auditoría pueden ser de tipos diferentes, en función de los diferentes tipos de solicitudes procesadas por AD FS. Cada tipo de evento de auditoría tiene datos específicos asociados.  El tipo de eventos de auditoría se puede diferenciar entre las solicitudes de inicio de sesión (es decir, las solicitudes de token) frente a las solicitudes del sistema (llamadas servidor-servidor, incluida la captura de información de configuración).

En la tabla siguiente se describen los tipos básicos de eventos de auditoría.

| Tipo de evento de auditoría | Id. de evento | Descripción |
|--|--|--|
| Validación de nueva credencial correcta | 1202 | Una solicitud en la que el Servicio de federación valida correctamente las credenciales nuevas. Esto incluye los puntos de conexión de WS-Trust, WS-Federation, SAML-P (primer segmento para generar SSO) y OAuth Authorize. |
| Error de validación de credenciales nuevas | 1203 | Una solicitud en la que se produjo un error en la validación de credenciales nuevas en el Servicio de federación. Esto incluye los puntos de conexión de WS-Trust, WS-FED, SAML-P (primer segmento para generar SSO) y OAuth Authorize. |
| Éxito del token de aplicación | 1200 | Una solicitud en la que el Servicio de federación emite correctamente un token de seguridad. En el caso de WS-Federation, SAML-P se registra cuando la solicitud se procesa con el artefacto de SSO. (como la cookie de SSO). |
| Error de token de aplicación | 1201 | Solicitud en la que se produjo un error en la emisión de tokens de seguridad en el Servicio de federación. En el caso de WS-Federation, SAML-P se registra cuando la solicitud se procesó con el artefacto de SSO. (como la cookie de SSO). |
| Solicitud de cambio de contraseña correcta | 1204 | Una transacción en la que el Servicio de federación procesó correctamente la solicitud de cambio de contraseña. |
| Error de solicitud de cambio de contraseña | 1205 | Una transacción en la que el Servicio de federación no pudo procesar la solicitud de cambio de contraseña. |
| Cerrar sesión correctamente | 1206 | Describe una solicitud de cierre de sesión correcta. |
| Cerrar sesión de error | 1207 | Describe una solicitud de cierre de sesión con error. |
