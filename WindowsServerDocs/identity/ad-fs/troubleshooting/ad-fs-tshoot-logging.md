---
title: 'AD FS solución de problemas: auditoría de eventos y registro'
description: En este documento se describe cómo usar los distintos registros de AD FS para solucionar problemas
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2018
ms.topic: article
ms.openlocfilehash: ef0adfa8b0e565981ea5273508f8c943d67e2d02
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953149"
---
# <a name="ad-fs-troubleshooting---events-and-logging"></a>Solución de problemas de AD FS: eventos y registro
AD FS proporciona dos registros principales que se pueden usar en la solución de problemas.  Son las siguientes:

- el registro de administración
- Registro de seguimiento

A continuación se explica cada uno de estos registros.

## <a name="admin-log"></a>Registro de administración
El registro de administración proporciona información de alto nivel sobre los problemas que se producen y está habilitado de forma predeterminada.

### <a name="to-view-the-admin-log"></a>Para ver el registro de administración
1.  Abra el visor de eventos.
2.  Expanda **registros de aplicaciones y servicios**.
3.  Expanda **AD FS**.
4.  Haga clic en **Admin** (Administrador).

![mejoras de auditoría](media/ad-fs-tshoot-logging/event1.PNG)

## <a name="trace-log"></a>Registro de seguimiento
El registro de seguimiento es donde se registran los mensajes detallados y será el registro más útil al solucionar problemas. Dado que se puede generar una gran cantidad de información de registro de seguimiento en un breve período de tiempo, lo que puede afectar al rendimiento del sistema, los registros de seguimiento están deshabilitados de forma predeterminada.

### <a name="to-enable-and-view-the-trace-log"></a>Para habilitar y ver el registro de seguimiento
1.  Abra el visor de eventos.
2.  Haga clic con el botón derecho en el **registro de aplicaciones y servicios** y seleccione Ver y haga clic en **Mostrar registros analíticos y de depuración**.  Esto mostrará nodos adicionales a la izquierda.
![mejoras de auditoría](media/ad-fs-tshoot-logging/event2.PNG)
3.  Expandir seguimiento de AD FS
4.  Haga clic con el botón derecho en depurar y seleccione **Habilitar registro**.
![mejoras de auditoría](media/ad-fs-tshoot-logging/event3.PNG)


## <a name="event-auditing-information-for-ad-fs-on-windows-server-2016"></a>Información de auditoría de eventos para AD FS en Windows Server 2016
De forma predeterminada, AD FS en Windows Server 2016 tiene habilitado el nivel básico de auditoría.  Con la auditoría básica, los administradores verán 5 eventos o menos para una única solicitud.  Esto marca una disminución significativa en el número de eventos que los administradores tienen que consultar para ver una única solicitud.   El nivel de auditoría se puede aumentar o reducir mediante el cmdlt de PowerShell:

```PowerShell
Set-AdfsProperties -AuditLevel
```

En la tabla siguiente se explican los niveles de auditoría disponibles.

|Nivel de auditoría|Sintaxis de PowerShell|Descripción|
|----- | ----- | ----- |
|None|Set-AdfsProperties-AuditLevel ninguno|La auditoría está deshabilitada y no se registrará ningún evento.|
|Básico (predeterminado)|Set-AdfsProperties-AuditLevel Basic|No se registrarán más de 5 eventos para una única solicitud.|
|Verbose|Set-AdfsProperties-AuditLevel verbose|Se registrarán todos los eventos.  Esto registrará una cantidad significativa de información por solicitud.|

Para ver el nivel de auditoría actual, puede usar el cmdlt de PowerShell: get-AdfsProperties.

![mejoras de auditoría](media/ad-fs-tshoot-logging/ADFS_Audit_1.PNG)

El nivel de auditoría se puede aumentar o reducir mediante el cmdlt de PowerShell: set-AdfsProperties-AuditLevel.

![mejoras de auditoría](media/ad-fs-tshoot-logging/ADFS_Audit_2.png)

## <a name="types-of-events"></a>Tipos de eventos
AD FS eventos pueden ser de tipos diferentes, en función de los diferentes tipos de solicitudes procesadas por AD FS. Cada tipo de evento tiene datos específicos asociados.  El tipo de eventos se puede diferenciar entre las solicitudes de inicio de sesión (es decir, las solicitudes de token) frente a las solicitudes del sistema (llamadas Server-Server, incluida la captura de información de configuración).

En la tabla siguiente se describen los tipos básicos de eventos.

|Tipo de evento|Id. de evento|Descripción|
|----- | ----- | ----- |
|Validación de nueva credencial correcta|1202|Una solicitud en la que el Servicio de federación valida correctamente las credenciales nuevas. Esto incluye los puntos de conexión de WS-Trust, WS-Federation, SAML-P (primer segmento para generar SSO) y OAuth Authorize.|
|Error de validación de credenciales nuevas|1203|Una solicitud en la que se produjo un error en la validación de credenciales nuevas en el Servicio de federación. Esto incluye los puntos de conexión de WS-Trust, WS-FED, SAML-P (primer segmento para generar SSO) y OAuth Authorize.|
|Éxito del token de aplicación|1200|Una solicitud en la que el Servicio de federación emite correctamente un token de seguridad. En el caso de WS-Federation, SAML-P se registra cuando la solicitud se procesa con el artefacto de SSO. (como la cookie de SSO).|
|Error de token de aplicación|1201|Solicitud en la que se produjo un error en la emisión de tokens de seguridad en el Servicio de federación. En el caso de WS-Federation, SAML-P se registra cuando la solicitud se procesó con el artefacto de SSO. (como la cookie de SSO).|
|Solicitud de cambio de contraseña correcta|1204|Una transacción en la que el Servicio de federación procesó correctamente la solicitud de cambio de contraseña.|
|Error de solicitud de cambio de contraseña|1205|Una transacción en la que el Servicio de federación no pudo procesar la solicitud de cambio de contraseña.|
|Cerrar sesión correctamente|1206|Describe una solicitud de cierre de sesión correcta.|
|Cerrar sesión de error|1207|Describe una solicitud de cierre de sesión con error.|

## <a name="security-auditing"></a>Auditoría de seguridad
En ocasiones, la auditoría de seguridad de la cuenta de servicio de AD FS puede ayudar a realizar un seguimiento de los problemas con las actualizaciones de contraseña, el registro de solicitudes/respuestas, los encabezados de contect de solicitudes y los resultados del registro de dispositivos.  La auditoría de la cuenta de servicio de AD FS está deshabilitada de forma predeterminada.

### <a name="to-enable-security-auditing"></a>Para habilitar la auditoría de seguridad
1. Haga clic en Inicio, seleccione **programas**, seleccione **herramientas administrativas**y, a continuación, haga clic en **Directiva de seguridad local**.
2. Navegue a la carpeta **Configuración de seguridad\Directivas locales\Administración de permisos del usuario** y haga doble clic en **Generar auditorías de seguridad**.
3. En la pestaña **Configuración de seguridad local**, compruebe que aparezca la cuenta de servicio de AD FS. Si no aparece, haga clic en Agregar usuario o grupo y agréguelo a la lista; después, haga clic en Aceptar.
4. Abra un símbolo del sistema con privilegios elevados y ejecute el siguiente comando para habilitar la auditoría auditpol.exe/Set/subcategory: "aplicación generada"/Failure: enable/Success: enable
5. Cierre **Directiva de seguridad local** y luego abra el complemento Administración de AD FS.

Para abrir el complemento Administración de AD FS, haga clic en Inicio, seleccione programas, seleccione Herramientas administrativas y, a continuación, haga clic en administración de AD FS.

6. En el panel acciones, haga clic en Editar Servicio de federación propiedades.
7. En el cuadro de diálogo Propiedades del servicio de federación, haga clic en la pestaña Eventos.
8. Active las casillas **Auditorías de aciertos** y **Auditorías de errores**.
9. Haga clic en Aceptar.

![mejoras de auditoría](media/ad-fs-tshoot-logging/event4.PNG)

>[!NOTE]
>Las instrucciones anteriores solo se usan cuando AD FS está en un servidor miembro independiente.  Si AD FS se está ejecutando en un controlador de dominio, en lugar de en la Directiva de seguridad local, use la **directiva predeterminada de controladores de dominio** ubicada en **Directiva de Grupo administración/bosque/Dominios/controladores de dominio**.  Haga clic en editar y navegue a **equipo \ configuración de seguridad\Directivas Seguridad\directivas \ Rights Management**

## <a name="windows-communication-foundation-and-windows-identity-foundation-messages"></a>Mensajes de Windows Communication Foundation y Windows Identity Foundation
Además del registro de seguimiento, en ocasiones es posible que necesite ver los mensajes de Windows Communication Foundation (WCF) y Windows Identity Foundation (WIF) para solucionar un problema. Para ello, modifique el archivo de **Microsoft.IdentityServer.ServiceHost.Exe.Config** en el servidor de AD FS.

Este archivo se encuentra en **<% raíz del sistema% > \windows\adfs** y está en formato XML. A continuación se muestran las partes relevantes del archivo:
```
<!-- To enable WIF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="Microsoft.IdentityModel" switchValue="Off"> … </source>

<!-- To enable WCF tracing, change the switchValue below to desired trace level - Verbose, Information, Warning, Error, Critical -->

<source name="System.ServiceModel" switchValue="Off" > … </source>
```


Después de aplicar estos cambios, guarde la configuración y reinicie el servicio AD FS. Después de habilitar estos seguimientos mediante la configuración de los modificadores adecuados, aparecerán en el registro de seguimiento de AD FS en el Visor de eventos de Windows.

## <a name="correlating-events"></a>Correlacionar eventos
Uno de los aspectos más difíciles de solucionar problemas es el acceso que genera una gran cantidad de eventos de error o de depuración.

Para ayudarlo, AD FS correlaciona todos los eventos que se registran en el Visor de eventos, tanto en el registro de administración como en el de depuración, que corresponden a una solicitud determinada mediante un identificador único global (GUID) único denominado ID. de actividad. Este identificador se genera cuando la solicitud de emisión de tokens se presenta inicialmente a la aplicación web (para aplicaciones que usan el perfil de solicitante pasivo) o solicitudes enviadas directamente al proveedor de notificaciones (para aplicaciones que usan WS-Trust).

![ActivityId](media/ad-fs-tshoot-logging/activityid1.png)

Este ID. de actividad sigue siendo el mismo para toda la duración de la solicitud y se registra como parte de todos los eventos registrados en el Visor de eventos de esa solicitud. Esto significa:
 - ese filtrado o búsqueda de la Visor de eventos con este identificador de actividad puede ayudar a realizar un seguimiento de todos los eventos relacionados que corresponden a la solicitud de token
 - el mismo ID. de actividad se registra en diferentes equipos, lo que permite solucionar problemas de una solicitud de usuario en varios equipos, como el servidor proxy de Federación (FSP).
 - el ID. de actividad también aparecerá en el explorador del usuario si se produce un error en la solicitud de AD FS, lo que permite al usuario comunicar este identificador al Departamento de soporte técnico o al servicio de ti.

![ActivityId](media/ad-fs-tshoot-logging/activityid2.png)

Para ayudar en el proceso de solución de problemas, AD FS también registra el evento de identificador del llamador cada vez que se produce un error en el proceso de emisión de tokens en un servidor de AD FS. Este evento contiene el tipo de notificaciones y el valor de uno de los siguientes tipos de notificaciones, suponiendo que esta información se pasó al Servicio de federación como parte de una solicitud de token:
- https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountnameh
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upnh
- https://schemas.microsoft.com/ws/2008/06/identity/claims/upn
- http://schemas.xmlsoap.org/claims/UPN
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressh
- https://schemas.microsoft.com/ws/2008/06/identity/claims/emailaddress
- http://schemas.xmlsoap.org/claims/EmailAddress
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
- https://schemas.microsoft.com/ws/2008/06/identity/claims/name
- http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier

El evento de identificador de llamada también registra el ID. de actividad para que pueda usar ese ID. de actividad para filtrar o buscar en los registros de eventos una solicitud determinada.




## <a name="next-steps"></a>Pasos a seguir

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
