---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Supervisión de Active Directory en busca de indicios de riesgo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e51b7ea151db1ca5d53a8cacef3b042e345175de
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949633"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory en busca de indicios de riesgo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número 5: la vigilancia del infinitas es el precio de la seguridad.* - [10 leyes inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un sistema de supervisión de registro de eventos sólido es una parte fundamental de cualquier diseño de Active Directory seguro. Muchos peligros de seguridad del equipo podrían detectarse en el primer momento en el caso de que las víctimas hayan actuado en la supervisión y las alertas adecuadas del registro de eventos. Los informes independientes tienen una larga compatibilidad con esta conclusión. Por ejemplo, el [Informe de infracciones de datos 2009 de Verizon](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) indica:  
  
"La aparente inefectividad de la supervisión de eventos y el análisis de registros sigue siendo un poco de una enigma. Existe la oportunidad de detección. los investigadores han observado que el 66 por ciento de las víctimas tenían suficientes pruebas disponibles dentro de sus registros para detectar la infracción en el análisis de estos recursos.  
  
Esta falta de supervisión de los registros de eventos activos sigue siendo una debilidad coherente en los planes de defensa de seguridad de muchas empresas. El [Informe de infracciones de datos 2012 de Verizon](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) encontró que, aunque el 85 por ciento de las infracciones tardó varias semanas en observarse, el 84 por ciento de las víctimas tenían evidencia de la infracción en sus registros de eventos.  
  
## <a name="windows-audit-policy"></a>Directiva de auditoría de Windows

A continuación se incluyen vínculos al blog de soporte técnico de Microsoft Official Enterprise. El contenido de estos blogs proporciona consejos, instrucciones y recomendaciones sobre la auditoría que le ayudarán a mejorar la seguridad de la infraestructura de Active Directory y son un recurso valioso al diseñar una directiva de auditoría.  
  
* La [Auditoría de acceso a objetos global es mágica](https://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) : describe un mecanismo de control denominado configuración de directiva de auditoría avanzada que se agregó a Windows 7 y windows Server 2008 R2 que le permite establecer los tipos de datos que desea auditar fácilmente y no los scripts y Auditpol. exe.  
* [Introducción a los cambios de auditoría en windows 2008](https://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) : presenta los cambios de auditoría realizados en windows Server 2008.  
* [Trucos de auditoría fantásticos en vista y 2008](https://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) : se explican las características de auditoría interesantes de Windows Vista y windows Server 2008 que se pueden usar para solucionar problemas o ver lo que sucede en su entorno.  
* [Tienda única para la auditoría en Windows server 2008 y Windows Vista](https://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) : contiene una compilación de características de auditoría e información contenida en windows Server 2008 y Windows Vista.  
  
Los vínculos siguientes proporcionan información acerca de las mejoras en la auditoría de Windows en Windows 8 y Windows Server 2012, e información sobre la auditoría de AD DS en Windows Server 2008.  
  
* [Novedades de la auditoría de seguridad](https://technet.microsoft.com/library/hh849638.aspx) : proporciona información general sobre las nuevas características de auditoría de seguridad de Windows 8 y windows Server 2012.  
* [AD DS guía paso a paso de auditoría](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) : describe la nueva característica de auditoría de Active Directory Domain Services (AD DS) en Windows Server 2008. También se proporcionan procedimientos para implementar esta nueva característica.  
  
### <a name="windows-audit-categories"></a>Categorías de auditoría de Windows

Antes de Windows Vista y Windows Server 2008, Windows solo tenía nueve categorías de directiva de auditoría de registro de eventos:  
  
* Eventos de inicio de sesión de cuenta  
* Administración de cuentas  
* Acceso al servicio de directorio  
* Eventos de inicio de sesión  
* Acceso a objetos  
* Cambio de directiva  
* Uso de privilegios  
* Seguimiento de procesos  
* Eventos del sistema  
  
Estas nueve categorías de auditoría tradicionales constituyen una directiva de auditoría. Cada categoría de directiva de auditoría se puede habilitar para eventos correctos, erróneos, o correctos y erróneos. Las descripciones se incluyen en la sección siguiente.  
  
#### <a name="audit-policy-category-descriptions"></a>Descripciones de categoría de directiva de auditoría  
Las categorías de directiva de auditoría habilitan los siguientes tipos de mensaje de registro de eventos.  
  
##### <a name="audit-account-logon-events"></a>Auditar eventos de inicio de sesión de cuenta  
Informa de cada instancia de una entidad de seguridad (por ejemplo, un usuario, un equipo o una cuenta de servicio) que inicia o cierra sesión en un equipo en el que se usa otro equipo para validar la cuenta. Los eventos de inicio de sesión de cuenta se generan cuando se autentica una cuenta de entidad de seguridad de dominio en un controlador de dominio. La autenticación de un usuario local en un equipo local genera un evento de inicio de sesión que se registra en el registro de seguridad local. No se registra ningún evento de cierre de sesión de cuenta.  
  
Esta categoría genera una gran cantidad de "ruido", ya que Windows está constantemente con cuentas que inician sesión en los equipos locales y remotos durante el curso normal de la empresa. Aun así, cualquier plan de seguridad debe incluir el éxito y el fracaso de esta categoría de auditoría.  
  
##### <a name="audit-account-management"></a>Auditar la administración de cuentas  
Esta configuración de auditoría determina si se va a realizar un seguimiento de la administración de usuarios y grupos. Por ejemplo, se debe realizar un seguimiento de los usuarios y grupos cuando se crea, cambia o elimina una cuenta de usuario o equipo, un grupo de seguridad o un grupo de distribución. Cuando se cambia el nombre de una cuenta de usuario o equipo, está deshabilitada o habilitada; o cuando se cambia la contraseña de un usuario o equipo. Se puede generar un evento para los usuarios o grupos que se agregan o se quitan de otros grupos.  
  
##### <a name="audit-directory-service-access"></a>Auditar el acceso del servicio de directorio  

Esta configuración de directiva determina si se va a auditar el acceso de la entidad de seguridad a un Active Directory objeto que tiene su propia lista de control de acceso de sistema (SACL) especificada. En general, esta categoría solo debe estar habilitada en los controladores de dominio. Cuando está habilitada, esta configuración genera mucha "ruido".  
  
##### <a name="audit-logon-events"></a>Auditar eventos de inicio de sesión  
Los eventos de inicio de sesión se generan cuando se autentica una entidad de seguridad local en un equipo local. Los eventos de inicio de sesión registran los inicios de sesión de dominio que se producen en el equipo local. No se generan eventos de cierre de sesión de cuenta. Cuando está habilitado, los eventos de inicio de sesión generan una gran cantidad de "ruido", pero deben estar habilitados de forma predeterminada en cualquier plan de auditoría de seguridad.  
  
##### <a name="audit-object-access"></a>Auditar el acceso a objetos  
El acceso a objetos puede generar eventos cuando se tiene acceso a objetos definidos posteriormente con la auditoría habilitada (por ejemplo, abrir, leer, cambiar de nombre, eliminar o cerrar). Una vez habilitada la categoría de auditoría principal, el administrador debe definir individualmente qué objetos tendrán habilitada la auditoría. Muchos objetos del sistema de Windows incorporan auditorías habilitadas, por lo que habilitar esta categoría normalmente comenzará a generar eventos antes de que el administrador haya definido cualquiera.  
  
Esta categoría es muy "ruidosa" y generará entre cinco y diez eventos para cada acceso a objetos. Puede ser difícil para los administradores de nuevas auditorías de objetos obtener información útil. Solo se debe habilitar cuando sea necesario.  
  
##### <a name="auditing-policy-change"></a>Cambio de directiva de auditoría  
Esta configuración de directiva determina si se deben auditar todas las incidencias de cambios en las directivas de asignación de derechos de usuario, las directivas de Firewall de Windows, las directivas de confianza o los cambios en la Directiva de auditoría. Esta categoría debe estar habilitada en todos los equipos. Genera muy poco ruido.  
  
##### <a name="audit-privilege-use"></a>Auditar el uso de privilegios  

Hay docenas de derechos de usuario y permisos en Windows (por ejemplo, iniciar sesión como un trabajo por lotes y actuar como parte del sistema operativo). Esta configuración de directiva determina si se va a auditar cada instancia de una entidad de seguridad mediante un derecho de usuario o un privilegio. Habilitar esta categoría da como resultado una gran cantidad de "ruido", pero puede ser útil para realizar el seguimiento de las cuentas principales de seguridad con privilegios elevados.  
  
##### <a name="audit-process-tracking"></a>Auditar el seguimiento de procesos  
Esta configuración de directiva determina si se debe auditar información detallada de seguimiento de procesos para eventos como la activación de programas, la salida de procesos, la duplicación de identificadores y el acceso a objetos indirectos. Resulta útil para realizar el seguimiento de usuarios malintencionados y los programas que usan.  
  
Al habilitar el seguimiento de procesos de auditoría, se genera un gran número de eventos, por lo que normalmente se establece en **sin auditoría**. Sin embargo, esta configuración puede proporcionar una gran ventaja durante una respuesta a un incidente desde el registro detallado de los procesos iniciados y el momento en que se iniciaron. En el caso de los controladores de dominio y otros servidores de infraestructura de rol único, esta categoría se puede activar de forma segura en todo momento. Los servidores de rol único no generan mucho tráfico de seguimiento de procesos durante el curso normal de sus tareas. Como tal, se pueden habilitar para capturar eventos no autorizados si se producen.  
  
##### <a name="system-events-audit"></a>Auditoría de eventos del sistema  

Eventos del sistema es casi una categoría de detección de todos los genéricos, registrando varios eventos que afectan al equipo, a la seguridad del sistema o al registro de seguridad. Incluye eventos para apagados y reinicios del equipo, errores de energía, cambios de hora del sistema, inicializaciones de paquetes de autenticación, borrado de registros de auditoría, problemas de suplantación y un host de otros eventos generales. En general, la habilitación de esta categoría de auditoría genera una gran cantidad de "ruido", pero genera suficientes eventos muy útiles que es difícil de no habilitar.  
  
#### <a name="advanced-audit-policies"></a>Directivas de auditoría avanzadas

A partir de Windows Vista y Windows Server 2008, Microsoft mejoró la forma en que se pueden realizar las selecciones de categorías de registro de eventos mediante la creación de subcategorías en cada categoría de auditoría principal. Las subcategorías permiten que la auditoría sea mucho más granular de lo que, de lo contrario, podría usar las categorías principales. Mediante el uso de subcategorías, solo puede habilitar partes de una determinada categoría principal y omitir la generación de eventos para los que no tiene ningún uso. Cada subcategoría de directiva de auditoría se puede habilitar para eventos correctos, erróneos, o correctos y erróneos.  
  
Para enumerar todas las subcategorías de auditoría disponibles, revise el contenedor de directivas de auditoría avanzada en un objeto de directiva de grupo o escriba lo siguiente en un símbolo del sistema en cualquier equipo que ejecute Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8, Windows 7 o Windows Vista:  
  
`auditpol /list /subcategory:*`
  
Para obtener una lista de las subcategorías de auditoría configuradas actualmente en un equipo que ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows 2008, escriba lo siguiente:  
  
`auditpol /get /category:*`
  
En la captura de pantalla siguiente se muestra un ejemplo de Auditpol. exe que enumera la Directiva de auditoría actual.  
  
![supervisar AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Directiva de grupo no notifica siempre con precisión el estado de todas las directivas de auditoría habilitadas, mientras que Auditpol. exe sí lo hace. Consulte [obtención de la Directiva de auditoría efectiva en Windows 7 y 2008 R2](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obtener más detalles.  
  
Cada categoría principal tiene varias subcategorías. A continuación se muestra una lista de categorías, sus subcategorías y una descripción de sus funciones.  
  
### <a name="auditing-subcategories-descriptions"></a>Descripciones de subcategorías de auditoría  
Las subcategorías de directiva de auditoría habilitan los siguientes tipos de mensaje de registro de eventos:  
  
#### <a name="account-logon"></a>Inicio de sesión de cuenta  
  
##### <a name="credential-validation"></a>Validación de credenciales  
Esta subcategoría informa de los resultados de las pruebas de validación de las credenciales enviadas para una solicitud de inicio de sesión de cuenta de usuario. Estos eventos se producen en el equipo autorizado para las credenciales. En el caso de las cuentas de dominio, el controlador de dominio es autoritativo, mientras que para las cuentas locales, el equipo local es autoritativo.  
  
En entornos de dominio, la mayoría de los eventos de inicio de sesión de cuenta se registran en el registro de seguridad de los controladores de dominio que son autoritativos para las cuentas de dominio. Sin embargo, estos eventos pueden producirse en otros equipos de la organización cuando se utilizan cuentas locales para iniciar sesión.  
  
##### <a name="kerberos-service-ticket-operations"></a>Auditar operaciones de vales de servicio Kerberos  
Esta subcategoría informa de los eventos generados por los procesos de solicitud de vales de Kerberos en el controlador de dominio que es autoritativo para la cuenta de dominio.  
  
##### <a name="kerberos-authentication-service"></a>Servicio de autenticación Kerberos  
Esta subcategoría informa de los eventos generados por el servicio de autenticación Kerberos. Estos eventos se producen en el equipo autorizado para las credenciales.  
  
##### <a name="other-account-logon-events"></a>Otros eventos de inicio de sesión de cuentas  
Esta subcategoría informa de los eventos que se producen en respuesta a las credenciales enviadas para una solicitud de inicio de sesión de cuenta de usuario que no están relacionadas con la validación de credenciales o vales de Kerberos. Estos eventos se producen en el equipo autorizado para las credenciales. En el caso de las cuentas de dominio, el controlador de dominio es autoritativo, mientras que para las cuentas locales, el equipo local es autoritativo.  
  
En entornos de dominio, la mayoría de los eventos de inicio de sesión de cuenta se registran en el registro de seguridad de los controladores de dominio que son autoritativos para las cuentas de dominio. Sin embargo, estos eventos pueden producirse en otros equipos de la organización cuando se utilizan cuentas locales para iniciar sesión. Algunos ejemplos pueden incluir lo siguiente:  
  
* Desconexiones de Servicios de Escritorio remoto sesión  
* Nuevas sesiones de Servicios de Escritorio remoto  
* Bloqueo y desbloqueo de una estación de trabajo  
* Invocación de un protector de pantalla  
* Descarte de un protector de pantalla  
* Detección de un ataque de reproducción de Kerberos, en el que se recibe dos veces una solicitud Kerberos con información idéntica  
* Acceso a una red inalámbrica por parte de una cuenta de usuario o de equipo  
* Acceso a una red 802.1x cableada por parte de una cuenta de usuario o de equipo  
  
#### <a name="account-management"></a>Administración de cuentas  
  
##### <a name="user-account-management"></a>User Account Management (Administración de cuentas de usuario)  
Esta subcategoría informa de cada evento de la administración de cuentas de usuario, como cuando se crea, cambia o elimina una cuenta de usuario. se cambia el nombre de una cuenta de usuario, se deshabilita o se habilita; o bien, se establece o se cambia una contraseña. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden realizar un seguimiento de los eventos para detectar la creación de cuentas de usuario malintencionadas, accidentales y autorizadas.  
  
##### <a name="computer-account-management"></a>Administración de cuentas de equipo  
Esta subcategoría informa de cada evento de administración de cuentas de equipo, por ejemplo, cuando se crea, cambia, elimina, cambia el nombre, se deshabilita o se habilita una cuenta de equipo.  
  
##### <a name="security-group-management"></a>Administración de grupos de seguridad  
Esta subcategoría informa de cada evento de administración de grupos de seguridad, como cuando se crea, se modifica o se elimina un grupo de seguridad o cuando se agrega o se quita un miembro de un grupo de seguridad. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden realizar un seguimiento de los eventos para detectar la creación malintencionada, accidental y autorizada de cuentas de grupo de seguridad.  
  
##### <a name="distribution-group-management"></a>Administración de grupos de distribución  
Esta subcategoría informa de cada evento de la administración de grupos de distribución, como cuando se crea, cambia o elimina un grupo de distribución, o cuando se agrega o se quita un miembro de un grupo de distribución. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden realizar un seguimiento de los eventos para detectar la creación de cuentas de grupo malintencionadas, accidentales y autorizadas.  
  
##### <a name="application-group-management"></a>Administración de grupos de aplicaciones  
Esta subcategoría informa de cada evento de la administración de grupos de aplicaciones en un equipo, por ejemplo, cuando se crea, cambia o elimina un grupo de aplicaciones o cuando se agrega o se quita un miembro de un grupo de aplicaciones. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden realizar un seguimiento de los eventos para detectar la creación malintencionada, accidental y autorizada de cuentas de grupo de aplicaciones.  
  
##### <a name="other-account-management-events"></a>Otros eventos de administración de cuentas  
Esta subcategoría notifica otros eventos de administración de la cuenta.  
  
#### <a name="detailed-process-tracking"></a>Seguimiento detallado de procesos  
  
##### <a name="process-creation"></a>Creación de un proceso  
Esta subcategoría informa de la creación de un proceso y el nombre del usuario o programa que lo creó.  
  
##### <a name="process-termination"></a>Finalización del proceso  
Esta subcategoría informa cuando finaliza un proceso.  
  
##### <a name="dpapi-activity"></a>Actividad DPAPI  
Esta subcategoría informa sobre cómo cifrar o descifrar las llamadas en la interfaz de programación de aplicaciones de protección de datos (DPAPI). DPAPI se usa para proteger la información secreta, como la contraseña almacenada y la información de clave.  
  
##### <a name="rpc-events"></a>Eventos RPC  
Esta subcategoría notifica los eventos de conexión de llamada a procedimiento remoto (RPC).  
  
#### <a name="directory-service-access"></a>Acceso al servicio de directorio  
  
##### <a name="directory-service-access"></a>Acceso al servicio de directorio  
Esta subcategoría informa cuando se tiene acceso a un objeto de AD DS. Solo los objetos con SACL configuradas provocan que se generen eventos de auditoría y solo cuando se tiene acceso a ellos de forma que coincida con las entradas SACL. Estos eventos son similares a los eventos de acceso del servicio de directorio en versiones anteriores de Windows Server. Esta subcategoría solo se aplica a los controladores de dominio.  
  
##### <a name="directory-service-changes"></a>Cambios en el servicio de directorio  
Esta subcategoría informa de los cambios en los objetos de AD DS. Los tipos de cambios que se indican son las operaciones de creación, modificación, movimiento y eliminación que se realizan en un objeto. La auditoría de cambios del servicio de directorio, si procede, indica los valores antiguos y nuevos de las propiedades cambiadas de los objetos que se cambiaron. Solo los objetos con SACL provocan que se generen eventos de auditoría y solo cuando se tiene acceso a ellos de forma que coincida con sus entradas SACL. Algunos objetos y propiedades no hacen que se generen eventos de auditoría debido a la configuración de la clase de objeto en el esquema. Esta subcategoría solo se aplica a los controladores de dominio.  
  
##### <a name="directory-service-replication"></a>Replicación de servicio de directorio  
Esta subcategoría informa cuando se inicia y finaliza la replicación entre dos controladores de dominio.  
  
##### <a name="detailed-directory-service-replication"></a>Replicación de servicio de directorio detallada  
Esta subcategoría informa de información detallada acerca de la información replicada entre controladores de dominio. Estos eventos pueden ser muy altos en el volumen.  
  
#### <a name="logonlogoff"></a>Inicio/cierre de sesión  
  
##### <a name="logon"></a>Inicio de sesión  
Esta subcategoría informa cuando un usuario intenta iniciar sesión en el sistema. Estos eventos se producen en el equipo al que se accede. En el caso de los inicios de sesión interactivos, la generación de estos eventos se produce en el equipo en el que se ha iniciado sesión. Si se produce un inicio de sesión de red para tener acceso a un recurso compartido, estos eventos se generan en el equipo que hospeda el recurso al que se accede. Si esta opción se configura como **sin auditoría**, es difícil o imposible determinar qué usuario ha tenido acceso o ha intentado tener acceso a los equipos de la organización.  
  
##### <a name="network-policy-server"></a>Servidor de directivas de redes  
Esta subcategoría informa de los eventos generados por las solicitudes de acceso de usuario de RADIUS (IAS) y protección de acceso a redes (NAP). Estas solicitudes se pueden **conceder**, **denegar**, **descartar**, **poner en cuarentena**, **bloquear**y **desbloquear**. La auditoría de esta configuración dará como resultado un volumen medio o alto de registros en los servidores NPS e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modo principal de IPsec  
Esta subcategoría informa de los resultados del protocolo Intercambio de claves por red (IKE) y protocolo de Internet autenticado (AuthIP) durante las negociaciones de modo principal.  
  
##### <a name="ipsec-extended-mode"></a>Modo extendido de IPsec  
Esta subcategoría informa de los resultados de AuthIP durante las negociaciones de modo extendido.  
  
##### <a name="other-logonlogoff-events"></a>Otros eventos Logon/Logoff  
Esta subcategoría informa de otros eventos relacionados con el inicio de sesión y el cierre de sesión, como Servicios de Escritorio remoto sesión se desconecta y se vuelve a conectar, mediante runas para ejecutar procesos en una cuenta diferente y bloquear y desbloquear una estación de trabajo.  
  
##### <a name="logoff"></a>La opción de cierre de sesión  
Esta subcategoría informa cuando un usuario cierra la sesión del sistema. Estos eventos se producen en el equipo al que se accede. En el caso de los inicios de sesión interactivos, la generación de estos eventos se produce en el equipo en el que se ha iniciado sesión. Si se produce un inicio de sesión de red para tener acceso a un recurso compartido, estos eventos se generan en el equipo que hospeda el recurso al que se accede. Si esta opción se configura como **sin auditoría**, es difícil o imposible determinar qué usuario ha tenido acceso o ha intentado tener acceso a los equipos de la organización.  
  
##### <a name="account-lockout"></a>Bloqueo de cuenta  
Esta subcategoría informa cuando se bloquea la cuenta de un usuario como resultado de demasiados intentos de inicio de sesión incorrectos.  
  
##### <a name="ipsec-quick-mode"></a>Modo rápido de IPsec  
Esta subcategoría informa de los resultados del protocolo IKE y AuthIP durante las negociaciones de modo rápido.  
  
##### <a name="special-logon"></a>Inicio de sesión especial  
Esta subcategoría informa cuando se usa un inicio de sesión especial. Un inicio de sesión especial es un inicio de sesión que tiene privilegios equivalentes de administrador y se puede usar para elevar un proceso a un nivel superior.  
  
#### <a name="policy-change"></a>Cambio de directiva  
  
##### <a name="audit-policy-change"></a>Auditar cambio de Directiva  
Esta subcategoría informa de los cambios en la Directiva de auditoría, incluidos los cambios de SACL.  
  
##### <a name="authentication-policy-change"></a>Cambio de directiva de autenticación  
Esta subcategoría informa de los cambios en la Directiva de autenticación.  
  
##### <a name="authorization-policy-change"></a>Cambio de directiva de autorización  
Esta subcategoría informa de los cambios en la Directiva de autorización, incluidos los cambios de permisos (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Cambio de directiva de nivel de regla de MPSSVC  
Esta subcategoría informa de los cambios en las reglas de directiva usadas por el servicio de protección de Microsoft (MPSSVC. exe). El Firewall de Windows usa este servicio.  
  
##### <a name="filtering-platform-policy-change"></a>Filtrar cambio de directiva de plataforma  
Esta subcategoría informa de la adición y eliminación de objetos de WFP, incluidos los filtros de inicio. Estos eventos pueden ser muy altos en el volumen.  
  
##### <a name="other-policy-change-events"></a>Otros eventos de cambio de Directiva  
Esta subcategoría notifica otros tipos de cambios en la Directiva de seguridad, como la configuración del Módulo de plataforma segura (TPM) o los proveedores de servicios criptográficos.  
  
#### <a name="privilege-use"></a>Uso de privilegios  
  
##### <a name="sensitive-privilege-use"></a>Uso de privilegios confidenciales  
Esta subcategoría informa cuando una cuenta de usuario o servicio utiliza un privilegio confidencial. Un privilegio confidencial incluye los siguientes derechos de usuario: actuar como parte del sistema operativo, realizar una copia de seguridad de archivos y directorios, crear un objeto de token, depurar programas, habilitar el equipo y las cuentas de usuario para que sean de confianza para la delegación, generar auditorías de seguridad, suplantar a un cliente tras la autenticación, cargar y descargar controladores de dispositivo, administrar registro de auditoría y de seguridad, modificar valores de entorno de firmware, reemplazar un token de nivel de proceso, restaurar archivos y directorios y tomar posesión de archivos u otros objetos. La auditoría de esta subcategoría creará un gran volumen de eventos.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso de privilegios no confidenciales  
Esta subcategoría informa cuando una cuenta de usuario o servicio utiliza un privilegio no confidencial. Un privilegio no confidencial incluye los siguientes derechos de usuario: acceder al administrador de credenciales como llamador de confianza, acceder a este equipo desde la red, agregar estaciones de trabajo al dominio, ajustar cuotas de memoria para un proceso, permitir el inicio de sesión local, permitir el inicio de sesión a través de un equipo remoto Servicios de escritorio, Omitir comprobación de recorrido, cambiar la hora del sistema, crear un archivo de paginación, crear objetos globales, crear objetos compartidos permanentes, crear vínculos simbólicos, denegar el acceso a este equipo desde la red, denegar el inicio de sesión como un trabajo por lotes, denegar el inicio de sesión como servicio, denegar el inicio de sesión local, denegar el inicio de sesión a través de Servicios de Escritorio remoto, forzar el cierre desde un sistema remoto, aumentar un espacio de trabajo del proceso, aumentar la prioridad de programación, bloquear páginas en la memoria, iniciar sesión como un trabajo por lotes, iniciar sesión como servicio, modificar una etiqueta de objeto, realizar un volumen tareas de mantenimiento, generar perfiles de un solo proceso, generar perfiles en el rendimiento del sistema, quitar el equipo de la estación de acoplamiento, apagar el sistema y sincronizar los datos del servicio de directorio. La auditoría de esta subcategoría creará un gran volumen de eventos.  
  
##### <a name="other-privilege-use-events"></a>Otros eventos de uso de privilegios  
Esta configuración de directiva de seguridad no se usa actualmente.  
  
#### <a name="object-access"></a>Acceso a objetos  
  
##### <a name="file-system"></a>Sistema de archivos  
Esta subcategoría informa de Cuándo se tiene acceso a los objetos del sistema de archivos. Solo los objetos del sistema de archivos con SACL provocan que se generen eventos de auditoría y solo cuando se tiene acceso a ellos de forma que coincidan con sus entradas SACL. Por sí solo, esta configuración de Directiva no provocará la auditoría de ningún evento. Determina si se debe auditar el evento de un usuario que tiene acceso a un objeto del sistema de archivos que tiene una lista de control de acceso de sistema (SACL) especificada, lo que permite que se realice la auditoría.  
  
Si el valor auditar el acceso a objetos se configura como **correcto**, se genera una entrada de auditoría cada vez que un usuario obtiene acceso correctamente a un objeto con una SACL especificada. Si esta configuración de Directiva se configura como **error**, se genera una entrada de auditoría cada vez que un usuario produce un error al intentar obtener acceso a un objeto con una SACL especificada.  
  
##### <a name="registry"></a>Registro  
Esta subcategoría informa cuando se tiene acceso a los objetos del registro. Solo los objetos del registro con SACL provocan que se generen eventos de auditoría y solo cuando se tiene acceso a ellos de forma que coincidan con sus entradas SACL. Por sí solo, esta configuración de Directiva no provocará la auditoría de ningún evento.  
  
##### <a name="kernel-object"></a>Objeto de kernel  
Esta subcategoría informa cuando se tiene acceso a objetos de kernel como procesos y exclusiones mutuas. Solo los objetos de kernel con SACL provocan que se generen eventos de auditoría y solo cuando se tiene acceso a ellos de forma que coincidan con sus entradas SACL. Normalmente, los objetos de kernel solo reciben SACL si están habilitadas las opciones de auditoría AuditBaseObjects o AuditBaseDirectories.  
  
##### <a name="sam"></a>SAM  
Esta subcategoría informa cuando se tiene acceso a los objetos de base de datos de autenticación del administrador de cuentas de seguridad (SAM) local.  
  
##### <a name="certification-services"></a>Servicios de certificación  
Esta subcategoría informa del momento en que se realizan las operaciones de servicios de certificación.  
  
##### <a name="application-generated"></a>Aplicación generada  
Esta subcategoría informa cuando las aplicaciones intentan generar eventos de auditoría mediante las interfaces de programación de aplicaciones (API) de auditoría de Windows.  
  
##### <a name="handle-manipulation"></a>Manipulación de identificadores  
Esta subcategoría informa cuando un identificador de un objeto se abre o se cierra. Solo los objetos con SACL hacen que se generen estos eventos y solo si la operación de identificador intentada coincide con las entradas SACL. Los eventos de manipulación de identificadores solo se generan para los tipos de objeto en los que está habilitada la subcategoría de acceso a objetos correspondiente (por ejemplo, sistema de archivos o registro).  
  
##### <a name="file-share"></a>Recurso compartido de archivos  
Esta subcategoría informa cuando se tiene acceso a un recurso compartido de archivos. Por sí solo, esta configuración de Directiva no provocará la auditoría de ningún evento. Determina si se debe auditar el evento de un usuario que tiene acceso a un objeto de recurso compartido de archivos que tiene una lista de control de acceso de sistema (SACL) especificada, lo que permite que se produzca la auditoría.  
  
##### <a name="filtering-platform-packet-drop"></a>Filtrar eliminación de paquetes de plataforma  
Esta subcategoría informa cuando la plataforma de filtrado de Windows (WFP) quita paquetes. Estos eventos pueden ser muy altos en el volumen.  
  
##### <a name="filtering-platform-connection"></a>Filtrar conexión de plataforma  
Esta subcategoría informa cuando WFP permite o bloquea las conexiones. Estos eventos pueden ser grandes en el volumen.  
  
##### <a name="other-object-access-events"></a>Otros eventos de acceso a objetos  
Esta subcategoría informa sobre otros eventos relacionados con el acceso a objetos, como Programador de tareas trabajos y objetos COM+.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Cambio de estado de seguridad  
Esta subcategoría informa de los cambios en el estado de seguridad del sistema, como cuando se inicia y se detiene el subsistema de seguridad.  
  
##### <a name="security-system-extension"></a>Extensión del sistema de seguridad  
Esta subcategoría informa de la carga del código de extensión, como los paquetes de autenticación, por parte del subsistema de seguridad.  
  
##### <a name="system-integrity"></a>Integridad del sistema  
Esta subcategoría informa de las infracciones de la integridad del subsistema de seguridad.  
  
Controlador IPsec  
  
Esta subcategoría informa sobre las actividades del controlador del Protocolo de seguridad de Internet (IPsec).  
  
##### <a name="other-system-events"></a>Otros eventos del sistema  
Esta subcategoría informa sobre otros eventos del sistema.  
  
Para obtener más información acerca de las descripciones de subcategoría, consulte la [herramienta Administrador de cumplimiento de seguridad de Microsoft](https://technet.microsoft.com/library/cc677002.aspx).  
  
Cada organización debe revisar las categorías y subcategorías anteriores y habilitar las que mejor se adaptan a su entorno. Los cambios en la Directiva de auditoría siempre deben probarse antes de la implementación en un entorno de producción.  
  
## <a name="configuring-windows-audit-policy"></a>Configuración de la Directiva de auditoría de Windows

La Directiva de auditoría de Windows se puede establecer mediante directivas de grupo, Auditpol. exe, API o ediciones del registro. Los métodos recomendados para configurar la Directiva de auditoría para la mayoría de las empresas son directiva de grupo o Auditpol. exe. La configuración de la Directiva de auditoría del sistema requiere permisos de cuenta de nivel de administrador o los permisos delegados adecuados.  
  
> [!NOTE]  
> Se debe conceder el privilegio **Administrar registro de seguridad y auditoría** a las entidades de seguridad (los administradores lo tienen de forma predeterminada) para permitir la modificación de las opciones de auditoría de acceso a objetos de recursos individuales, como archivos, objetos de Active Directory y claves del registro.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Configuración de la Directiva de auditoría de Windows mediante directiva de grupo

Para establecer la Directiva de auditoría mediante directivas de grupo, configure las categorías de auditoría adecuadas que se encuentran en configuración de equipo \ configuración de seguridad\Directivas **Locales\directiva Directiva** (consulte la siguiente captura de pantalla para obtener un ejemplo del editor de directivas de grupo local (gpedit. msc)). Cada categoría de directiva de auditoría se puede habilitar para eventos **correctos**, **erróneos**, o **correctos** y erróneos.  
  
![supervisar AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
La Directiva de auditoría avanzada se puede establecer mediante el Active Directory o las directivas de grupo local. Para establecer la Directiva de auditoría avanzada, configure las subcategorías adecuadas que se encuentran en configuración de equipo \ \ \ \ \ **Directiva de auditoría de seguridad\directiva** (consulte la siguiente captura de pantalla para obtener un ejemplo del editor de directivas de grupo local (gpedit. msc)). Cada subcategoría de directiva de auditoría se puede habilitar para eventos **correctos**, **erróneos**, o **correctos** y **erróneos** .  
  
![supervisar AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Configuración de la Directiva de auditoría de Windows mediante Auditpol. exe

AuditPol. exe (para establecer la Directiva de auditoría de Windows) se incorporó en Windows Server 2008 y Windows Vista. Inicialmente, solo se podía usar Auditpol. exe para establecer la Directiva de auditoría avanzada, pero directiva de grupo se puede usar en Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8 y Windows 7.  
  
AuditPol. exe es una utilidad de línea de comandos. La sintaxis es la siguiente:  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Ejemplos de sintaxis de Auditpol. exe:  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> AuditPol. exe establece localmente la Directiva de auditoría avanzada. Si la directiva local entra en conflicto con Active Directory o directiva de grupo local, la configuración de directiva de grupo normalmente prevalece sobre la configuración de Auditpol. exe. Cuando existen varios conflictos de directiva de grupo o local, solo se prevalecerá una directiva (es decir, reemplazar). Las directivas de auditoría no se combinarán.  
  
#### <a name="scripting-auditpol"></a>Scripting Auditpol

Microsoft proporciona un [script de ejemplo](https://support.microsoft.com/kb/921469) para los administradores que desean establecer una directiva de auditoría avanzada mediante el uso de un script en lugar de escribir manualmente en cada comando Auditpol. exe.  
  
**Nota:** Directiva de grupo no notifica siempre con precisión el estado de todas las directivas de auditoría habilitadas, mientras que Auditpol. exe sí lo hace. Consulte [obtención de la Directiva de auditoría efectiva en Windows 7 y windows 2008 R2](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obtener más detalles.  
  
#### <a name="other-auditpol-commands"></a>Otros comandos Auditpol

AuditPol. exe se puede usar para guardar y restaurar una directiva de auditoría local y para ver otros comandos relacionados con la auditoría. Estos son los otros comandos **Auditpol** .  
  
`auditpol /clear`: se usa para borrar y restablecer directivas de auditoría local  
  
`auditpol /backup /file:<filename>`: se usa para hacer una copia de seguridad de una directiva de auditoría local actual en un archivo binario  
  
`auditpol /restore /file:<filename>`: se usa para importar un archivo de directiva de auditoría guardado previamente a una directiva de auditoría local  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>`: Si esta configuración de directiva de auditoría está habilitada, hace que el sistema se detenga inmediatamente (con STOP: C0000244 {Audit FAIL} Message) si no se puede registrar una auditoría de seguridad por alguna razón. Normalmente, un evento no se registra cuando el registro de auditoría de seguridad está lleno y el método de retención especificado para el registro de seguridad **no sobrescribe eventos** o **sobrescribe eventos por días**. Normalmente solo lo habilitan los entornos que necesitan una mayor garantía de que el registro de seguridad está registrando. Si está habilitada, los administradores deben observar atentamente el tamaño del registro de seguridad y girar los registros según sea necesario. También se puede establecer con directiva de grupo modificando la opción de seguridad **Auditoría: apagar el sistema inmediatamente si no se pueden registrar las auditorías de seguridad** (valor predeterminado = deshabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>`: esta configuración de directiva de auditoría determina si se debe auditar el acceso de los objetos globales del sistema. Si esta directiva está habilitada, hace que los objetos del sistema, como exclusiones mutuas, eventos, semáforos y dispositivos DOS, se creen con una lista de control de acceso de sistema (SACL) predeterminada. La mayoría de los administradores consideran que las auditorías de objetos globales del sistema son demasiado "ruidosos" y solo lo habilitan si se sospecha de ataques malintencionados. Solo se proporciona una SACL a los objetos con nombre. Si la Directiva auditar el acceso a objetos (o la subcategoría de auditoría de objetos de kernel) también está habilitada, se audita el acceso a estos objetos del sistema. Al configurar esta configuración de seguridad, los cambios no surtirán efecto hasta que reinicie Windows. Esta Directiva también se puede establecer con directiva de grupo modificando la opción de seguridad auditar el acceso de objetos globales del sistema (valor predeterminado = deshabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>`: esta configuración de directiva de auditoría especifica que los objetos de kernel con nombre (como exclusiones mutuas y semáforos) se les asignarán SACL cuando se creen. AuditBaseDirectories afecta a los objetos de contenedor, mientras que AuditBaseObjects afecta a objetos que no pueden contener otros objetos.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>`: esta configuración de directiva de auditoría especifica si el cliente genera un evento cuando uno o varios de estos privilegios se asignan a un token de seguridad de usuario: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege y TcbPrivilege. Si esta opción no está habilitada (valor predeterminado = deshabilitado), los privilegios BackupPrivilege y RestorePrivilege no se registran. La habilitación de esta opción puede hacer que el registro de seguridad sea extremadamente ruidoso (a veces cientos de eventos por segundo) durante una operación de copia de seguridad. Esta Directiva también se puede establecer con directiva de grupo modificando la opción de seguridad **Auditoría: auditar el uso de los privilegios de copia de seguridad y restauración**.  
  
> [!NOTE]  
> La información que se proporciona aquí se tomó del [tipo de opción de auditoría](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) de Microsoft y de la herramienta SCM de Microsoft.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Aplicación de auditorías y auditorías avanzadas tradicionales

En Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 y Windows Vista, los administradores pueden optar por habilitar las nueve categorías tradicionales o usar las subcategorías. Se trata de una elección binaria que debe realizarse en cada sistema de Windows. Puede habilitar las categorías principales o el subcategoriesit no puede ser ambos.  
  
Para evitar que la Directiva de categoría tradicional heredada sobrescriba las subcategorías de la Directiva de auditoría, debe habilitar la configuración de la **Subcategoría forzar Directiva de auditoría (Windows Vista o posterior) para invalidar** la configuración de la categoría configuración de categorías de directiva de auditoría ubicada en configuración del equipo \ configuración de seguridad\Directivas **locales\Opciones**de directivas.  
  
Se recomienda que las subcategorías estén habilitadas y configuradas en lugar de las nueve categorías principales. Esto requiere que se habilite una configuración de directiva de grupo (para permitir que las subcategorías invaliden las categorías de auditoría) junto con la configuración de las diferentes subcategorías que admiten las directivas de auditoría.  
  
Las subcategorías de auditoría se pueden configurar mediante varios métodos, como directiva de grupo y el programa de línea de comandos, Auditpol. exe.  
  
## <a name="next-steps"></a>Pasos siguientes
  
* [Auditoría de seguridad avanzada en Windows 7 y Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Auditoría y cumplimiento en Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Cómo usar directiva de grupo para configurar las opciones detalladas de auditoría de seguridad para equipos basados en Windows Vista y Windows Server 2008 en un dominio de Windows Server 2008, en un dominio de Windows Server 2003 o en un dominio de Windows 2000](https://support.microsoft.com/kb/921469)  
  
* [Guía paso a paso de la Directiva de auditoría de seguridad avanzada](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
