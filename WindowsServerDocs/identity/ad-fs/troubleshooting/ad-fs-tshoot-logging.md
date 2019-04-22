---
title: 'AD FS solución de problemas: eventos de auditoría y registro'
description: Este documento describe cómo usar los distintos registros de AD FS para solucionar problemas
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 20e2d0747b98e7c7728230d0768506261f5b0d50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825126"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Solución de AD FS - eventos y registro
AD FS proporciona dos registros principales que pueden usarse para solucionar problemas.  Estas sobrecargas son:

- el inicio de sesión de administrador
- el registro de seguimiento  
 
Cada uno de estos registros se explicará más adelante.

## <a name="admin-log"></a>Registro de administración
El inicio de sesión de administrador proporciona información de alto nivel sobre los problemas que se producen y está habilitados de forma predeterminada.

### <a name="to-view-the-admin-log"></a>Para ver el registro de administración
1.  Abre el Visor de eventos
2.  Expanda **aplicaciones y servicios Registro**.
3.  Expanda **AD FS**.
4.  Haga clic en **Admin**.

![mejoras de auditoría](media/ad-fs-tshoot-logging/event1.PNG)  

## <a name="trace-log"></a>Registro de seguimiento
El registro de seguimiento es donde se registran los mensajes detallados y será el registro más útil para solucionar el problema. Puesto que se puede generar una gran cantidad de información de registro de seguimiento en un breve período de tiempo, lo que puede afectar al rendimiento del sistema, los registros de seguimiento están deshabilitados de forma predeterminada. 

### <a name="to-enable-and-view-the-trace-log"></a>Para habilitar y ver el registro de seguimiento
1.  Abre el Visor de eventos
2.  Haga doble clic en **aplicaciones y servicios de registro** y seleccione Ver y haga clic en **mostrar registros analíticos y depuración**.  Esto mostrará los nodos adicionales a la izquierda.
![mejoras de auditoría](media/ad-fs-tshoot-logging/event2.PNG)  
3.  Expanda el seguimiento de AD FS
4.  Haga doble clic en Depurar y seleccione **Habilitar registro**.
![mejoras de auditoría](media/ad-fs-tshoot-logging/event3.PNG)  


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Información de auditoría de eventos de AD FS en Windows Server 2016  
De forma predeterminada, AD FS en Windows Server 2016 tiene un nivel básico de está habilitada la auditoría.  Con la auditoría básica, los administradores verán 5 o menos eventos para una única solicitud.  Esto marca una disminución notable en el número de eventos que los administradores tienen que mirar, para ver una sola solicitud.   El nivel de auditoría se puede generar o bajado usando el cmdlet de PowerShell:  

```PowerShell
Set-AdfsProperties -AuditLevel 
```

La siguiente tabla explica los niveles de auditoría disponibles.  

|Nivel de auditoría|Sintaxis de PowerShell|Descripción|  
|----- | ----- | ----- |
|Ninguno|Set-AdfsProperties - AuditLevel ninguno|La auditoría está deshabilitada y no se registrará ningún evento.|  
|Básico (predeterminado)|Set-AdfsProperties - AuditLevel Basic|No más de 5 eventos se registrarán para una única solicitud|  
|Verbose|Set-AdfsProperties - AuditLevel detallado|Todos los eventos se registrarán.  Esto registrará una cantidad significativa de información por solicitud.|  
  
Para ver el nivel de auditoría actual, puede usar el cmdlet de PowerShell:  Get-AdfsProperties.  
  
![mejoras de auditoría](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)  
  
El nivel de auditoría se puede generar o bajado usando el cmdlet de PowerShell:  Set-AdfsProperties -AuditLevel.  
  
![mejoras de auditoría](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)  
  
## <a name="types-of-events"></a>Tipos de eventos  
Los eventos de AD FS pueden ser de tipos diferentes, según los distintos tipos de solicitudes procesadas por AD FS. Cada tipo de evento tiene datos específicos asociados con él.  El tipo de eventos se puede diferenciar entre las solicitudes de inicio de sesión (es decir, las solicitudes de token) frente a las solicitudes del sistema (llamadas a servidores incluidos capturando información de configuración).    

La siguiente tabla describe los tipos básicos de eventos.  
  
|Tipo de evento|Id. de evento|Descripción| 
|----- | ----- | ----- | 
|Nueva credencial validación correcta|1202|Una solicitud donde se validan correctamente las credenciales nuevas por el servicio de federación. Esto incluye WS-Trust, WS-Federation, SAML-P (primer segmento para generar el inicio de sesión único) y puntos de conexión de autorización de OAuth.|  
|Error de validación de credencial nuevo|1203|Una solicitud donde error de validación de la nueva credencial en el servicio de federación. Esto incluye WS-Trust, WS-Fed, SAML-P (primer segmento para generar el inicio de sesión único) y puntos de conexión de autorización de OAuth.|  
|Aplicación correcta de Token|1200|Una solicitud que se emite correctamente un token de seguridad mediante el servicio de federación. Para WS-Federation, SAML-P se registra cuando se procesa la solicitud con el artefacto SSO. (por ejemplo, la cookie de inicio de sesión único).|  
|Error de símbolo (token) de la aplicación|1201|Error de una solicitud de emisión de tokens de seguridad de que en el servicio de federación. Para WS-Federation, SAML-P se registra cuando se procesó la solicitud con el artefacto SSO. (por ejemplo, la cookie de inicio de sesión único).|  
|Solicitud de cambio de contraseña correcto|1204|Una transacción de solicitud de cambio de la contraseña de donde se ha procesado correctamente por el servicio de federación.|  
|Error de solicitud de cambio de contraseña|1205|Una transacción de solicitud de cambio de la contraseña de donde no se pudo ser procesados por el servicio de federación.| 
|Cerrar sesión correcto|1206|Describe una solicitud de cierre de sesión correcta.|  
|Error de cerrar la sesión|1207|Describe una solicitud de cierre de sesión con error.|  

## <a name="security-auditing"></a>Auditoría de seguridad
Auditoría de seguridad de la cuenta de servicio de AD FS a veces puede ayudar a solucionar los problemas con las actualizaciones de contraseña, registro de solicitud/respuesta, encabezados de contexto de solicitud y los resultados del registro de dispositivo.  Auditoría de la cuenta de servicio de AD FS está deshabilitada de forma predeterminada.

### <a name="to-enable-security-auditing"></a>Para habilitar la auditoría de seguridad
1.       Haga clic en Inicio, seleccione **programas**, apunte a **herramientas administrativas**y, a continuación, haga clic en **directiva de seguridad Local**.
2.       Navegue a la carpeta **Configuración de seguridad\Directivas locales\Administración de permisos del usuario** y haga doble clic en **Generar auditorías de seguridad**.
3.       En el **configuración de seguridad Local** , compruebe que aparece la cuenta de servicio de AD FS. Si no está presente, haga clic en Agregar usuario o grupo y agregarlo a la lista y, a continuación, haga clic en Aceptar.
4.       Abra un símbolo del sistema con privilegios elevados y ejecute el siguiente comando para habilitar la auditoría auditpol.exe /set/SubCategory: "Aplicación generado" /failure:enable /success:enable 5.       Cerrar **directiva de seguridad Local**y, a continuación, abra el complemento de administración de AD FS.
 
Para abrir el complemento de administración de AD FS, haga clic en Inicio, elija programas, herramientas administrativas y, a continuación, haga clic en administración de AD FS.
 
6.       En el panel Acciones, haga clic en Editar Federation Service propiedades 7.       En el cuadro de diálogo Propiedades del servicio de federación, haga clic en la ficha de eventos. 8.       Seleccione el **auditorías de aciertos** y **auditorías de errores** casillas de verificación.
9.       Haga clic en Aceptar.

![mejoras de auditoría](media/ad-fs-tshoot-logging/event4.PNG)  
 
>[!NOTE]
>Las instrucciones anteriores sirven solamente cuando AD FS se encuentra en un servidor miembro independiente.  Si AD FS se ejecuta en un controlador de dominio, en lugar de la directiva de seguridad Local, use el **directiva predeterminada de controladores de dominio** ubicado en **controladores de administración de dominio o el bosque/dominios/directiva de grupo**.  Haga clic en Editar y vaya a **equipo Configuración del equipo\Directivas\Configuración de Windows\Configuración seguridad\Directivas locales\Asignación Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Mensajes de Windows Communication Foundation y Windows Identity Foundation
Además de registro de seguimiento, en ocasiones, deberá ver los mensajes de Windows Communication Foundation (WCF) y Windows Identity Foundation (WIF) con el fin de solucionar un problema. Esto puede hacerse mediante la modificación de la **Microsoft.IdentityServer.ServiceHost.Exe.Config** archivo en el servidor de AD FS. 

Este archivo se encuentra en **\Windows\ADFS < % system raíz % >** y está en formato XML. Las partes relevantes del archivo se muestran a continuación: 
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Después de aplicar estos cambios, guarde la configuración y reinicie el servicio AD FS. Después de habilitar estos seguimientos estableciendo los modificadores correspondientes, aparecerá en el registro de seguimiento de AD FS en el Visor de eventos de Windows.

## <a name="correlating-events"></a>Correlación de eventos
Uno de los aspectos más difíciles de solucionar es los problemas de acceso que generan una gran cantidad de errores o eventos de depuración.

Para ello, AD FS establece una correlación entre todos los eventos que se registran en el Visor de eventos, en el administrador y los registros de depuración, que corresponden a una solicitud concreta mediante el uso de un único único identificador global (GUID) llamado identificador de la actividad. Este identificador se genera cuando la solicitud de emisión de tokens se presenta inicialmente a la aplicación web (para aplicaciones mediante el passive requestor profile) o las solicitudes enviadas directamente en el proveedor de notificaciones (para aplicaciones de uso de WS-Trust). 

![ActivityID](media/ad-fs-tshoot-logging/activityid1.png)

Este Id. de actividad sigue siendo el mismo para toda la duración de la solicitud y se registra como parte de todos los eventos registrados en el evento Visor para esa solicitud. Esto significa que:
 - filtrar o buscar el Visor de eventos con este identificador puede ayudar a realizar un seguimiento de todos los eventos relacionados que corresponden a la solicitud de token de actividad
 - el mismo identificador de actividad se registra en las distintas máquinas que permite a la solución de problemas de una solicitud de usuario entre varios equipos, como el proxy de servidor de federación (FSP)
 - el identificador de actividad también aparecerán en el explorador del usuario si se produce un error en la solicitud de AD FS en absoluto, lo que permite al usuario comunicarse este identificador al servicio de asistencia o soporte técnico de TI.

![ActivityID](media/ad-fs-tshoot-logging/activityid2.png)

Para facilitar el proceso de solución de problemas, AD FS también registra el evento Id. de autor de llamada cada vez que se produce un error en el proceso de emisión de tokens en un servidor de AD FS. Este evento contiene el tipo de notificación y el valor de uno de los siguientes tipos de notificación, suponiendo que esta información se pasó al servicio de federación como parte de una solicitud de token:
- http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- http://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress 
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- http://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier 

El evento Id. de autor de llamada también registra el identificador de actividad para que pueda usar dicho identificador de actividad para filtrar o buscar los registros de eventos para una solicitud determinada.




## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
