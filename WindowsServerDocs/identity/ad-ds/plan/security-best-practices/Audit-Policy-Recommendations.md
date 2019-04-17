---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: "Recomendaciones de la directiva de auditoría"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d03d38834f89f8cda80b7af147e2bd3e31f4f990
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="audit-policy-recommendations"></a>Recomendaciones de la directiva de auditoría

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta sección describe la configuración de directiva de auditoría predeterminada de Windows, línea base recomienda opciones de configuración de directiva de auditoría y las recomendaciones más exigentes de Microsoft, para los productos de servidor y la estación de trabajo.  

Las recomendaciones de línea base SCM que se muestra aquí, junto con la configuración recomendada para ayudar a detectar compromiso, están destinadas solo a ser una guía de referencia inicial para los administradores. Cada organización debe decidir su propio las amenazas que se enfrentan su tolerancia riesgo aceptable y las categorías de directiva de auditoría o las subcategorías te deberían permitir. Para obtener más información acerca de las amenazas, consulte la [Guía de amenazas y contramedidas](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Se recomienda empezar con la configuración recomendada en este caso, los administradores sin una directiva de auditoría detallista en su lugar y, a continuación, modificar y probar antes de implementar en su entorno de producción.  

Las recomendaciones son para los equipos de clase empresarial, Microsoft se define como equipos que tienen requisitos de seguridad promedio y requieren un alto grado de funcionamiento. Entidades necesidad de mayor seguridad que se consideren más agresivos requisitos de directivas de auditoría.  

> [!NOTE]  
> Valores predeterminados de Microsoft Windows y las recomendaciones de línea base se tomaron desde el [herramienta de Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

Se recomienda la siguiente configuración de directiva de auditoría de línea base para los equipos de seguridad normales que no se sabe que son atacado activo, correcta con determinados adversarios o malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Recomienda directivas de auditoría de sistema operativo  
Esta sección contiene tablas que enumeran las recomendaciones de configuración de auditoría que se aplican a los siguientes sistemas operativos:  

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 8  

-   Windows 7  

Estas tablas contienen la configuración predeterminada de Windows, las recomendaciones de línea base y las recomendaciones más sólidas para estos sistemas operativos.  

**Leyenda de tablas de directiva de auditoría**  

|||  
|-|-|  
|**Notación**|**Recomendación**|  
|Sí|En general habilitan escenarios|  
|No|Hacer **no** en general habilitan escenarios|  
|IF|Habilitar si es necesario para un escenario específico, o si un rol o característica que auditoría se desea está instalado en el equipo|  
|CONTROLADOR DE DOMINIO|Habilitar en controladores de dominio|  
|[En blanco]|Sin recomendación|  

**Windows 8 y recomendaciones de configuración de Windows 7 auditoría**  

**Directiva de auditoría**  

|Categoría de directiva de auditoría o subcategoría|Valor predeterminado de Windows<br /><br />Error de aciertos|Recomendación de línea base<br /><br />Error de aciertos|Recomendación más sólida<br /><br />Error de aciertos|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Inicio de sesión de cuenta**||||  
|Auditar validación de credenciales|No No|Sí No|Sí, sí|  
|Auditar servicio de autenticación Kerberos|||Sí, sí|  
|Auditar operaciones de vales de servicio Kerberos|||Sí, sí|  
|Auditar otros eventos de inicio de sesión de cuenta|||Sí, sí|  
|**Administración de cuentas**||||  
|Auditar aplicación Administración de grupos||||  
|Auditar administración de cuentas de equipo||Sí No|Sí, sí|  
|Administración de grupos de distribución de auditoría||||  
|Auditar otros eventos de administración de cuentas||Sí No|Sí, sí|  
|Administración del grupo de seguridad de auditoría||Sí No|Sí, sí|  
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
|Auditar bloqueo de cuenta|Sí No||Sí No|  
|Auditar reclamaciones de usuario o dispositivo||||  
|Auditar modo extendido de IPsec||||  
|Auditar modo principal de IPsec|||IF IF|  
|Auditar modo rápido de IPsec||||  
|Auditar cierre de sesión|Sí No|Sí No|Sí No|  
|Auditar inicio de sesión|Sí No|Sí No|Sí, sí|  
|Auditar servidor de directivas de red|Sí, sí|||  
|Auditar otros eventos de inicio/cierre de sesión||||  
|Auditar inicio de sesión especial|Sí No|Sí No|Sí, sí|  
|**Acceso a objetos**||||  
|Auditar aplicación generada||||  
|Auditar servicios de certificación||||  
|Auditar recurso compartido de archivos detallado||||  
|Auditar recurso compartido de archivos||||  
|Auditar sistema de archivos||||  
|Auditar conexión de plataforma de filtrado||||  
|Colocación de paquetes de plataforma de filtrado de auditoría||||  
|Auditar manipulación de identificadores||||  
|Auditar objeto de Kernel||||  
|Auditar otros eventos de acceso del objeto||||  
|Registro de auditoría||||  
|Auditar almacenamiento extraíble||||  
|Auditar SAM||||  
|Auditar almacenamiento provisional de directiva de acceso Central||||  
|**Cambio de directiva**||||  
|Auditar cambio de directiva de auditoría|Sí No|Sí, sí|Sí, sí|  
|Auditar cambio de directiva de autenticación|Sí No|Sí No|Sí, sí|  
|Auditar cambio de directiva de autorización||||  
|Auditar cambio de directiva de plataforma de filtrado||||  
|Auditar cambio de directiva de nivel de reglas MPSSVC|||Sí  |  
|Auditar otros eventos de cambio de directiva||||  
|**Uso de privilegios**||||  
|Auditar el uso de privilegios no confidenciales||||  
|Auditar otros eventos de uso de privilegios||||  
|Auditar el uso de privilegios confidenciales||||  
|**Sistema**||||  
|Auditar controlador IPsec||Sí, sí|Sí, sí|  
|Auditar otros eventos del sistema|Sí, sí|||  
|Cambio de estado de seguridad de auditoría|Sí No|Sí, sí|Sí, sí|  
|Auditar extensión del sistema de seguridad||Sí, sí|Sí, sí|  
|Auditar integridad del sistema|Sí, sí|Sí, sí|Sí, sí|  
|**Acceso a objetos global de auditoría**||||  
|Auditar controlador IPsec||||  
|Auditar otros eventos del sistema||||  
|Cambio de estado de seguridad de auditoría||||  
|Auditar extensión del sistema de seguridad||||  
|Auditar integridad del sistema||||  

**Recomendaciones de configuración de auditoría de Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012**  

|Categoría de directiva de auditoría o subcategoría|Valor predeterminado de Windows<br /><br />Error de aciertos|Recomendación de línea base<br /><br />Error de aciertos|Recomendación más sólida<br /><br />Error de aciertos|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Inicio de sesión de cuenta**||||  
|Auditar validación de credenciales|No No|Sí, sí|Sí, sí|  
|Auditar servicio de autenticación Kerberos|||Sí, sí|  
|Auditar operaciones de vales de servicio Kerberos|||Sí, sí|  
|Auditar otros eventos de inicio de sesión de cuenta|||Sí, sí|  
|**Administración de cuentas**||||  
|Auditar aplicación Administración de grupos||||  
|Auditar administración de cuentas de equipo||Sí DC|Sí, sí|  
|Administración de grupos de distribución de auditoría||||  
|Auditar otros eventos de administración de cuentas||Sí, sí|Sí, sí|  
|Administración del grupo de seguridad de auditoría||Sí, sí|Sí, sí|  
|Auditar administración de cuentas de usuario|Sí No|Sí, sí|Sí, sí|  
|**Seguimiento detallado**||||  
|Auditar actividad DPAPI|||Sí, sí|  
|Auditar creación de procesos||Sí No|Sí, sí|  
|Auditar finalización de procesos||||  
|Auditar eventos de RPC||||  
|**Acceso DS**||||  
|Auditar replicación de servicio de directorio detallada||||  
|Auditar el acceso del servicio de directorio||EL CONTROLADOR DE DOMINIO DE DC|EL CONTROLADOR DE DOMINIO DE DC|  
|Auditar cambios de servicio de directorio||EL CONTROLADOR DE DOMINIO DE DC|EL CONTROLADOR DE DOMINIO DE DC|  
|Auditar replicación de servicio de directorio||||  
|**Inicio de sesión y cierre de sesión**||||  
|Auditar bloqueo de cuenta|Sí No||Sí No|  
|Auditar reclamaciones de usuario o dispositivo||||  
|Auditar modo extendido de IPsec||||  
|Auditar modo principal de IPsec|||IF IF|  
|Auditar modo rápido de IPsec||||  
|Auditar cierre de sesión|Sí No|Sí No|Sí No|  
|Auditar inicio de sesión|Sí No|Sí, sí|Sí, sí|  
|Auditar servidor de directivas de red|Sí, sí|||  
|Auditar otros eventos de inicio/cierre de sesión|||Sí, sí|  
|Auditar inicio de sesión especial|Sí No|Sí No|Sí, sí|  
|**Acceso a objetos**||||  
|Auditar aplicación generada||||  
|Auditar servicios de certificación||||  
|Auditar recurso compartido de archivos detallado||||  
|Auditar recurso compartido de archivos||||  
|Auditar sistema de archivos||||  
|Auditar conexión de plataforma de filtrado||||  
|Colocación de paquetes de plataforma de filtrado de auditoría||||  
|Auditar manipulación de identificadores||||  
|Auditar objeto de Kernel||||  
|Auditar otros eventos de acceso del objeto||||  
|Registro de auditoría||||  
|Auditar almacenamiento extraíble||||  
|Auditar SAM||||  
|Auditar almacenamiento provisional de directiva de acceso Central||||  
|**Cambio de directiva**||||  
|Auditar cambio de directiva de auditoría|Sí No|Sí, sí|Sí, sí|  
|Auditar cambio de directiva de autenticación|Sí No|Sí No|Sí, sí|  
|Auditar cambio de directiva de autorización||||  
|Auditar cambio de directiva de plataforma de filtrado||||  
|Auditar cambio de directiva de nivel de reglas MPSSVC|||Sí  |  
|Auditar otros eventos de cambio de directiva||||  
|**Uso de privilegios**||||  
|Auditar el uso de privilegios no confidenciales||||  
|Auditar otros eventos de uso de privilegios||||  
|Auditar el uso de privilegios confidenciales||||  
|**Sistema**||||  
|Auditar controlador IPsec||Sí, sí|Sí, sí|  
|Auditar otros eventos del sistema|Sí, sí|||  
|Cambio de estado de seguridad de auditoría|Sí No|Sí, sí|Sí, sí|  
|Auditar extensión del sistema de seguridad||Sí, sí|Sí, sí|  
|Auditar integridad del sistema|Sí, sí|Sí, sí|Sí, sí|  
|**Acceso a objetos global de auditoría**||||  
|Auditar controlador IPsec||||  
|Auditar otros eventos del sistema||||  
|Cambio de estado de seguridad de auditoría||||  
|Auditar extensión del sistema de seguridad||||  
|Auditar integridad del sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Establece la directiva de auditoría en los servidores y estaciones de trabajo  
Todos los planes de administración de registro de eventos deben supervisar los servidores y estaciones de trabajo. Un error frecuente es supervisar solo los servidores o controladores de dominio. Como piratería malintencionado a menudo inicialmente se produce en estaciones de trabajo, no supervisa las estaciones de trabajo está omitiendo la mejor y más antigua fuente de información.  

Los administradores deben revisar cuidadosamente y se prueba cualquier directiva de auditoría antes de la implementación en su entorno de producción.  

## <a name="events-to-monitor"></a>Eventos de Monitor  
Un identificador de evento perfecto para generar una alerta de seguridad debe contener los siguientes atributos:  

-   Alta probabilidad aparición indica actividades no autorizadas  

-   Número reducido de falsos positivos  

-   Aparición debe dar como resultado una respuesta de investigación y análisis  

Dos tipos de eventos deben supervisar y alertas:  

1.  Dichos eventos en el que incluso una sola aparición indica actividades no autorizadas  

2.  Acumulación de eventos por encima de una línea base esperada y aceptado  

Es un ejemplo del primer evento:  

Si iniciar sesión en equipos que no son los controladores de dominio prohibidos administradores de dominio (DAs), una sola aparición de un miembro DA iniciar sesión en una estación de trabajo del usuario final debe generar una alerta e investigarse. Este tipo de alerta es fácil generar mediante el evento de auditoría de inicio de sesión especial 4964 (se han asignado grupos especiales a un nuevo inicio de sesión). Otros ejemplos de alertas de instancia única:  

-   Si el servidor A nunca debe conectarse al servidor B, alerta cuando se conectan entre sí.  

-   Una alerta si una cuenta de usuario final normal inesperadamente se agrega a un grupo de seguridad con información confidencial.  

-   Si los empleados de la ubicación de fábrica A nunca funcionan por la noche, alerta cuando un usuario inicia sesión en la medianoche.  

-   Una alerta si un servicio no autorizado está instalado en un controlador de dominio.  

-   Investiga si un usuario final normal intenta iniciar sesión directamente en un servidor SQL Server para los que no tienen ningún motivo claro para hacerlo.  

-   Si no tiene miembros del grupo DA y alguien agrega a sí mismos allí, comprobarla inmediatamente.  

Es un ejemplo del segundo evento:  

Un número de inicios de sesión fallidos aberrante podría indicar una ataque de contraseña. Para que una empresa proporcionar una alerta para un elevado número de inicios de sesión fallidos, deben comprender los niveles normales de inicios de sesión fallidos dentro de su entorno antes de un evento de seguridad malintencionado.  

Para obtener una lista completa de los eventos que se debe incluir al supervisar signos de compromiso, consulte [Apéndice L: eventos al Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Objetos de Active Directory y atributos a Monitor  
Éstas son las cuentas, grupos y atributos que se deben supervisar para detectar los intentos de comprometer la instalación de los servicios de dominio de Active Directory.  

-   Sistemas para deshabilitar o eliminación de software antivirus y antimalware (automáticamente reinicio protección cuando está desactivado manualmente)  

-   Cuentas de administrador para cambios no autorizados  

-   Actividades que se realizan mediante cuentas con privilegios (automáticamente ha caducado quitar cuenta cuando se complete o asignadas tiempo actividades sospechosas)  

-   Con privilegios y cuentas VIP en AD DS. Supervisar los cambios, especialmente los cambios de atributos en la pestaña de la cuenta (por ejemplo, cn, nombre, sAMAccountName, userPrincipalName o userAccountControl). Además de supervisar las cuentas, restringir quién puede modificar las cuentas que se como pequeño un conjunto de usuarios administrativos como sea posible.  

Consulte [Apéndice L: eventos al Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obtener una lista de eventos recomendadas para el monitor, las clasificaciones de la gravedad y un resumen de mensaje de evento.  

-   Grupo servidores por la clasificación de sus cargas de trabajo, que permite identificar rápidamente los servidores que deben ser la mejor supervisada y más estrictas configurado  

-   Cambia a las propiedades y la pertenencia de los siguientes grupos de AD DS: los administradores de empresa (EA), los administradores de dominio (DA), los administradores (BA) y administradores de esquema (SA)  

-   Cuentas con privilegios deshabilitadas (por ejemplo, cuentas de administrador integradas en Active Directory y en los sistemas de miembro) para habilitar las cuentas  

-   Cuentas de administración para registrar todas las escrituras en la cuenta  

-   Asistente de configuración de seguridad integrada para configurar el servicio, el registro, auditoría y configuración del firewall para reducir la superficie de ataque del servidor. Usar a este asistente si implementas los servidores de accesos directos como parte de tu estrategia de host administrativas.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Información adicional para supervisar los servicios de dominio de Active Directory  
Revisa los siguientes vínculos para obtener más información acerca de la supervisión de AD DS:  
  
-   [Auditoría de acceso a objetos global es mágico](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -proporciona información sobre cómo configurar y usar la directiva de configuración de auditoría avanzada que se agregó a Windows 7 y Windows Server 2008 R2.  

-   [Introducir cambios de auditoría en Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -presenta los cambios de auditoría realizado en Windows 2008.  

-   [Geniales auditoría trucos en la Vista y 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica interesantes nuevas características de auditoría en Windows Vista y Windows Server 2008 puede usarse para la solución de problemas o ver lo que sucede en su entorno.  

-   [Un solo lugar para auditar en Windows Server 2008 y Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene una compilación de auditoría de características y la información incluida en Windows Server 2008 y Windows Vista.  

-   [Guía paso a paso de auditoría de AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -describe la nueva característica de auditoría de servicios de dominio de Active Directory (AD DS) en Windows Server 2008. También se ofrecen procedimientos para implementar esta nueva característica.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Lista general de Criticalities de recomendación de identificador de evento de seguridad  
Todas las recomendaciones de identificador de evento se complementa con una importancia clasificación como sigue:  

**Alta:** identificadores de evento con una clasificación de gravedad alta deben siempre e inmediatamente una alerta e investigar.  

**Mediano:** un identificador de evento con una clasificación de gravedad Media podría indicar actividades malintencionadas, pero se debe estar acompañado de otras anomalías (por ejemplo, un número inusual que se producen en un período de tiempo determinado, repeticiones inesperadas o repeticiones en un equipo que normalmente no se espera que registrar el evento.). Un evento de gravedad del medio también puede recopilarse como una métrica r y en comparación con el tiempo.  

**Baja:** y el identificador de evento con los eventos de poca importancia no deben ganen atención o provocar alertas, a menos que se ordenan con los eventos de gravedad Media o alta.  

Estas recomendaciones están destinadas a proporcionar a una guía de línea base para el administrador. Todas las recomendaciones deben ser revisadas completamente antes de la implementación en un entorno de producción.  

Consulte [Apéndice L: eventos al Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obtener una lista de los eventos recomendadas para el monitor, las clasificaciones de la gravedad y un resumen de mensaje de evento.  
