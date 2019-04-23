---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Recomendaciones de la directiva de auditoría
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 343a9a7aedf22e9c021249f00fb628f871a2ce1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835756"
---
# <a name="audit-policy-recommendations"></a>Recomendaciones de la directiva de auditoría

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Esta sección trata sobre la configuración de directiva de auditoría de Windows de forma predeterminada, baseline recomienda configuraciones de directiva de auditoría y las recomendaciones más exigentes de Microsoft, para productos de estación de trabajo y el servidor.  

Las recomendaciones de línea de base SCM que se muestra a continuación, junto con la configuración recomendada para ayudar a detectar riesgos, sólo pretenden ser una guía de línea base inicial a los administradores. Cada organización debe realizar sus propias decisiones con respecto a las amenazas que se enfrentan, sus las tolerancias al riesgo aceptable y qué categorías de directiva de auditoría o las subcategorías debe habilitar. Para obtener más información acerca de las amenazas, consulte el [Guía de amenazas y contramedidas](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Los administradores sin una directiva de auditoría meditado en su lugar se recomienda comenzar con la configuración recomendada en este caso y a continuación, modificar y probar, antes de implementar en su entorno de producción.  

Las recomendaciones son para equipos de clase empresarial, que Microsoft se define como los equipos que tienen requisitos de seguridad Media y requieren un alto nivel de funcionalidad operativa. Las entidades que necesitan una mayor seguridad que considere más agresivos los requisitos de directivas de auditoría.  

> [!NOTE]  
> El valor predeterminado es Microsoft Windows y recomendaciones de línea base se han realizado desde la [herramienta Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Se recomienda la siguiente configuración de directiva de auditoría de línea de base para los equipos de seguridad normal que no se sabe que esté siendo atacado correcta, active determinados adversarios o malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Recomienda las directivas de auditoría del sistema operativo  
Esta sección contiene las tablas que enumeran las recomendaciones de configuración de auditoría que se aplican a los sistemas operativos siguientes:  

-   Windows Server 2016 

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Estas tablas contienen la configuración predeterminada de Windows, las recomendaciones de línea base y las recomendaciones más fuertes para estos sistemas operativos.  

**Leyenda de las tablas de directiva de auditoría**  

|||  
|-|-|  
|**Notación**|**Recomendación**|  
|SÍ|En general habilitar escenarios|  
|NO|Hacer **no** en general habilitar escenarios|  
|IF|Habilitar si es necesario para un escenario concreto, o si un rol o característica para que la auditoría es el deseado está instalado en el equipo|  
|DC|Habilitar en los controladores de dominio|  
|[En blanco]|Ninguna recomendación|  

**Windows 10, Windows 8 y las recomendaciones de configuración de Windows 7 auditoría**  

**La directiva de auditoría**  

|Categoría de directiva de auditoría o subcategoría|Valor predeterminado de Windows<br /><br />Error de operación correcta|Recomendación de línea base<br /><br />Error de operación correcta|Recomendación más fuerte<br /><br />Error de operación correcta|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Inicio de sesión de cuenta**||||  
|Auditar validación de credenciales|No    No|Sí No|Sí, sí|  
|Auditar servicio de autenticación Kerberos|||Sí, sí|  
|Auditar operaciones de vales de servicio Kerberos|||Sí, sí|  
|Auditar otros eventos de inicio de sesión de cuentas|||Sí, sí|  
|**Administración de cuentas**||||  
|Auditar administración de grupos de aplicaciones||||  
|Auditar administración de cuentas de equipo||Sí No|Sí, sí|  
|Auditar administración de grupos de distribución||||  
|Auditar otros eventos de administración de cuentas||Sí No|Sí, sí|  
|Auditar administración de grupos de seguridad||Sí No|Sí, sí|  
|Auditar administración de cuentas de usuario|Sí No|Sí No|Sí, sí|  
|**Seguimiento detallado**||||  
|Auditar actividad DPAPI|||Sí, sí|  
|Auditar creación de procesos||Sí No|Sí, sí|  
|Auditar finalización de procesos||||  
|Auditar eventos de RPC||||  
|**Acceso DS**||||  
|Auditar replicación de servicio de directorio detallada||||  
|Auditar el acceso del servicio de directorio||||  
|Auditar cambios de servicio de directorio||||  
|Auditar replicación de servicio de directorio||||  
|**Inicio de sesión y cierre de sesión**||||  
|Auditar bloqueo de cuentas|Sí No||Sí No|  
|Auditar reclamaciones de usuario o dispositivo||||  
|Auditar modo extendido de IPsec||||  
|Auditar modo principal de IPsec|||IF     IF|  
|Auditar modo rápido de IPsec||||  
|Auditar cierre de sesión|Sí No|Sí No|Sí No|  
|Inicio de sesión de auditoría <sup>1</sup>|Sí, sí|Sí, sí|Sí, sí|  
|Auditar Servidor de directivas de redes|Sí, sí|||  
|Auditar otros eventos de inicio y cierre de sesión||||  
|Auditar inicio de sesión especial|Sí No|Sí No|Sí, sí|  
|**Acceso a objetos**||||  
|Auditar aplicación generada||||  
|Auditar servicios de certificación||||  
|Auditar recurso compartido de archivos detallado||||  
|Auditar recurso compartido de archivos||||  
|Auditar sistema de archivos||||  
|Auditar conexión de Plataforma de filtrado||||  
|Auditar colocación de paquetes de la Plataforma de filtrado||||  
|Auditar manipulación de identificadores||||  
|Auditar objeto de kernel||||  
|Auditar otros eventos de acceso a objetos||||  
|Auditar Registro||||  
|Auditar almacenamiento extraíble||||  
|Auditar SAM||||  
|Auditar almacenamiento provisional de directiva de acceso central||||  
|**Cambio de directiva**||||  
|Auditar cambio de directiva de auditoría|Sí No|Sí, sí|Sí, sí|  
|Auditar cambio de directiva de autenticación|Sí No|Sí No|Sí, sí|  
|Auditar cambio de directiva de autorización||||  
|Auditar cambio de directiva de la Plataforma de filtrado||||  
|Auditar cambio de directiva del nivel de reglas de MPSSVC|||Sí  |  
|Auditar otros eventos de cambio de directiva||||  
|**Uso de privilegios**||||  
|Auditar uso de privilegios no confidenciales||||  
|Auditar otros eventos de uso de privilegios||||  
|Auditar uso de privilegios confidenciales||||  
|**Sistema**||||  
|Auditar controlador IPsec||Sí, sí|Sí, sí|  
|Auditar otros eventos del sistema|Sí, sí|||  
|Auditar cambio de estado de seguridad|Sí No|Sí, sí|Sí, sí|  
|Auditar extensión del sistema de seguridad||Sí, sí|Sí, sí|  
|Auditar integridad del sistema|Sí, sí|Sí, sí|Sí, sí|  
|**Acceso a objetos global de auditoría**||||  
|Auditar controlador IPsec||||  
|Auditar otros eventos del sistema||||  
|Auditar cambio de estado de seguridad||||  
|Auditar extensión del sistema de seguridad||||  
|Auditar integridad del sistema||||  

<sup>1</sup> a partir de Windows 10 versión 1809, auditar inicio de sesión de forma predeterminada está habilitada para correctos y erróneos. En versiones anteriores de Windows, éxito solo está habilitada de forma predeterminada.

**Recomendaciones de configuración de auditoría de Windows Server 2008, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2016**  

|Categoría de directiva de auditoría o subcategoría|Valor predeterminado de Windows<br /><br />Error de operación correcta|Recomendación de línea base<br /><br />Error de operación correcta|Recomendación más fuerte<br /><br />Error de operación correcta|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Inicio de sesión de cuenta**||||  
|Auditar validación de credenciales|No    No|Sí, sí|Sí, sí|  
|Auditar servicio de autenticación Kerberos|||Sí, sí|  
|Auditar operaciones de vales de servicio Kerberos|||Sí, sí|  
|Auditar otros eventos de inicio de sesión de cuentas|||Sí, sí|  
|**Administración de cuentas**||||  
|Auditar administración de grupos de aplicaciones||||  
|Auditar administración de cuentas de equipo||Sí DC|Sí, sí|  
|Auditar administración de grupos de distribución||||  
|Auditar otros eventos de administración de cuentas||Sí, sí|Sí, sí|  
|Auditar administración de grupos de seguridad||Sí, sí|Sí, sí|  
|Auditar administración de cuentas de usuario|Sí No|Sí, sí|Sí, sí|  
|**Seguimiento detallado**||||  
|Auditar actividad DPAPI|||Sí, sí|  
|Auditar creación de procesos||Sí No|Sí, sí|  
|Auditar finalización de procesos||||  
|Auditar eventos de RPC||||  
|**Acceso DS**||||  
|Auditar replicación de servicio de directorio detallada||||  
|Auditar el acceso del servicio de directorio||DC    DC|DC    DC|  
|Auditar cambios de servicio de directorio||DC    DC|DC    DC|  
|Auditar replicación de servicio de directorio||||  
|**Inicio de sesión y cierre de sesión**||||  
|Auditar bloqueo de cuentas|Sí No||Sí No|  
|Auditar reclamaciones de usuario o dispositivo||||  
|Auditar modo extendido de IPsec||||  
|Auditar modo principal de IPsec|||IF     IF|  
|Auditar modo rápido de IPsec||||  
|Auditar cierre de sesión|Sí No|Sí No|Sí No|  
|Auditar inicio de sesión|Sí, sí|Sí, sí|Sí, sí|  
|Auditar Servidor de directivas de redes|Sí, sí|||  
|Auditar otros eventos de inicio y cierre de sesión|||Sí, sí|  
|Auditar inicio de sesión especial|Sí No|Sí No|Sí, sí|  
|**Acceso a objetos**||||  
|Auditar aplicación generada||||  
|Auditar servicios de certificación||||  
|Auditar recurso compartido de archivos detallado||||  
|Auditar recurso compartido de archivos||||  
|Auditar sistema de archivos||||  
|Auditar conexión de Plataforma de filtrado||||  
|Auditar colocación de paquetes de la Plataforma de filtrado||||  
|Auditar manipulación de identificadores||||  
|Auditar objeto de kernel||||  
|Auditar otros eventos de acceso a objetos||||  
|Auditar Registro||||  
|Auditar almacenamiento extraíble||||  
|Auditar SAM||||  
|Auditar almacenamiento provisional de directiva de acceso central||||  
|**Cambio de directiva**||||  
|Auditar cambio de directiva de auditoría|Sí No|Sí, sí|Sí, sí|  
|Auditar cambio de directiva de autenticación|Sí No|Sí No|Sí, sí|  
|Auditar cambio de directiva de autorización||||  
|Auditar cambio de directiva de la Plataforma de filtrado||||  
|Auditar cambio de directiva del nivel de reglas de MPSSVC|||Sí  |  
|Auditar otros eventos de cambio de directiva||||  
|**Uso de privilegios**||||  
|Auditar uso de privilegios no confidenciales||||  
|Auditar otros eventos de uso de privilegios||||  
|Auditar uso de privilegios confidenciales||||  
|**Sistema**||||  
|Auditar controlador IPsec||Sí, sí|Sí, sí|  
|Auditar otros eventos del sistema|Sí, sí|||  
|Auditar cambio de estado de seguridad|Sí No|Sí, sí|Sí, sí|  
|Auditar extensión del sistema de seguridad||Sí, sí|Sí, sí|  
|Auditar integridad del sistema|Sí, sí|Sí, sí|Sí, sí|  
|**Acceso a objetos global de auditoría**||||  
|Auditar controlador IPsec||||  
|Auditar otros eventos del sistema||||  
|Auditar cambio de estado de seguridad||||  
|Auditar extensión del sistema de seguridad||||  
|Auditar integridad del sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Establecer directiva de auditoría en servidores y estaciones de trabajo  
Todos los planes de administración de registro de eventos deben supervisar los servidores y estaciones de trabajo. Un error común es supervisar sólo servidores o controladores de dominio. Hacking malintencionado inicialmente a menudo se produce en estaciones de trabajo, las estaciones de trabajo de supervisión no está omitiendo el origen mejor y más antiguo de información.  

Los administradores deben revisar cuidadosamente y se probar cualquier directiva de auditoría antes de la implementación en su entorno de producción.  

## <a name="events-to-monitor"></a>Eventos para supervisar  
Un identificador de evento perfecto para generar una alerta de seguridad debe contener los siguientes atributos:  

-   Alta probabilidad de aparición indica actividades no autorizadas  

-   Deben disponer de un número reducido de falsos positivos.  

-   Repetición debe tener como resultado una respuesta de investigación o forense.  

Dos tipos de eventos se deben supervisar y recibir alertas:  

1.  Los eventos en las que incluso una sola aparición indica actividades no autorizadas  

2.  Una acumulación de eventos por encima de un punto de referencia esperado y aceptado.  

Un ejemplo del primer evento es:  

Si se prohíben Admins. del dominio (DAs) inicien sesión en equipos que no son controladores de dominio, una aparición única de un miembro DA inicio de sesión en una estación de trabajo del usuario final debe generar una alerta e investigarse. Este tipo de alerta es fácil generar mediante el evento de auditoría de inicio de sesión especial 4964 (grupos especiales se han asignado a un nuevo inicio de sesión). Otros ejemplos de las alertas de instancia única:  

-   Si nunca debe conectar el servidor al servidor B, la alerta cuando se conectan entre sí.  

-   Envía una alerta si una cuenta de usuario final normal inesperadamente se agrega a un grupo de seguridad confidenciales.  

-   Si los empleados en una ubicación de fábrica nunca funcionan durante la noche, alerta cuando un usuario inicia sesión a medianoche.  

-   Una alerta si se instala un servicio no autorizado en un controlador de dominio.  

-   Investigue si un usuario final normal intenta iniciar sesión directamente en un servidor de SQL Server para el que no tengan ninguna razón para ello, desactive.  

-   Si no tiene ningún miembro en el grupo DA y alguien agrega a sí mismos no existe, compruebe inmediatamente.  

Es un ejemplo del segundo evento:  

Un número de inicios de sesión incorrectos aberrante podría indicar un ataque de averiguación de contraseñas. Para que una empresa proporcionar una alerta para un número inusualmente elevado de inicios de sesión incorrectos, primero deben comprender los niveles normales de inicios de sesión incorrectos dentro de su entorno antes de un evento de seguridad malintencionado.  

Para obtener una lista completa de eventos que se deben incluir al supervisar en busca de indicios de riesgo, consulte [Apéndice L: Eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Objetos de Active Directory y los atributos para el Monitor  
Los siguientes son las cuentas, grupos y atributos que se deben supervisar para ayudar a detectar intentos de poner en peligro la instalación de Active Directory Domain Services.  

-   Sistemas de deshabilitación o eliminación de software antivirus y antimalware (automáticamente reiniciar la protección cuando se deshabilita manualmente)  

-   Cuentas de administrador para los cambios no autorizados  

-   Actividades que se realizan mediante el uso de las cuentas con privilegios (automáticamente la cuenta de eliminación cuando se completan o se asigna tiempo de actividades sospechosas ha expirado)  

-   Con privilegios y cuentas de VIP en AD DS. Supervise los cambios realizados, especialmente los cambios en atributos en la pestaña cuenta (por ejemplo, cn, nombre, sAMAccountName, userPrincipalName o userAccountControl). Además de supervisar las cuentas, restringir quién puede modificar las cuentas con como pequeños de un conjunto de usuarios administrativos como sea posible.  

Consulte [Apéndice L: Eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obtener una lista de eventos recomendados para supervisar, sus clasificaciones de gravedad y un resumen de mensaje de evento.  

-   Servidores de grupo mediante la clasificación de sus cargas de trabajo, lo que permite identificar rápidamente los servidores que deben ser el mejor supervisado y configurado de forma más rigurosa  

-   Cambios en las propiedades y la pertenencia de los siguientes grupos de AD DS: Los administradores de Enterprise (EA), Admins. del dominio (DA), los administradores (BA) y administradores de esquema (SA)  

-   Deshabilitado las cuentas con privilegios (por ejemplo, cuentas de administrador integradas en Active Directory y en los sistemas de miembro) para habilitar las cuentas  

-   Cuentas de administración para iniciar todas las escrituras en la cuenta  

-   Asistente de configuración de seguridad integrados para configurar el servicio, el registro, auditoría y configuración del firewall para reducir la superficie expuesta a ataques del servidor. Use este asistente si implementa servidores de salto como parte de su estrategia de host administrativo.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Información adicional para la supervisión de servicios de dominio de Active Directory  
Revise los siguientes vínculos para obtener más información sobre la supervisión de AD DS:  
  
-   [Auditoría de acceso a objetos global es la magia](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -proporciona información sobre cómo configurar y usar la directiva de configuración de auditoría avanzada que se agregó a Windows 7 y Windows Server 2008 R2.  

-   [Introducción a la auditoría de cambios en Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -presenta la auditorías cambios realizados en Windows 2008.  

-   [Niveles de acceso esporádico trucos de auditoría en la Vista y 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica las características más interesantes de la auditoría en Windows Vista y Windows Server 2008 que puede usarse para solucionar problemas o ver lo que sucede en su entorno.  

-   [Tienda integral para la auditoría en Windows Server 2008 y Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene una compilación de la auditoría de las características y la información incluida en Windows Server 2008 y Windows Vista.  

-   [Guía paso a paso para la auditoría de AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -describe la nueva característica de auditoría de los servicios de dominio de Active Directory (AD DS) en Windows Server 2008. También proporciona procedimientos para implementar esta nueva característica.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Lista general de los elementos más importantes de seguridad Event ID recomendación  
Todas las recomendaciones de Id. de evento están acompañadas por una importancia crítica de clasificación como sigue:  

**Alta:** Id. de evento con una puntuación de importancia alta debe siempre e inmediatamente una alerta e investigar.  

**Medio:** Un identificador de evento con una clasificación de gravedad Media podría indicar una actividad malintencionada, pero debe ir acompañada de algún otro comportamiento anormal (por ejemplo, un número inusual que se producen en un período de tiempo determinado, repeticiones inesperadas o apariciones en un equipo que Normalmente no se espera que se registra el evento.). Un evento de importancia del medio, es posible que también recopilan como una métrica de r y en comparación con el tiempo.  

**Baja:** Y no debe infunde atención o se generan alertas, a menos que se correlacionan con los eventos de importancia Media o alta Id. de evento con los eventos de importancia baja.  

Estas recomendaciones están diseñadas para proporcionar a una guía de la línea de base para el administrador. Todas las recomendaciones se deberían revisar completamente antes de la implementación en un entorno de producción.  

Consulte [Apéndice L: Eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obtener una lista de los eventos recomendados al monitor, sus clasificaciones de gravedad y un resumen de mensaje de evento.  
