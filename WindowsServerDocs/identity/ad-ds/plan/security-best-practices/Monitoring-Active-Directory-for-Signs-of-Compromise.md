---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Supervisión de Active Directory en busca de indicios de riesgo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 40d0d06f8d6d25c2c1dbf4662d3296a996d22055
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882946"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory en busca de indicios de riesgo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número cinco: Vigilancia infinita es el precio de seguridad.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un registro de eventos sólido sistema de supervisión es una parte fundamental de cualquier diseño de Active Directory seguro. Compromisos de seguridad de muchos equipos podrían detectarse pronto en el evento si las víctimas activado el registro de eventos apropiado, supervisión y alertas. Informes independientes han admitido larga esta conclusión. Por ejemplo, el [informe de infracción de datos de Verizon 2009](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) estados:  
  
"Sigue siendo cierto modo una enigma la reduce aparente de análisis de supervisión y registro de eventos. La oportunidad de detección está allí; investigadores indican que 66% de las víctimas tenía suficientes pruebas disponibles dentro de sus registros para descubrir la brecha se hubiera más diligentes para analizar dichos recursos."  
  
Esta falta de supervisión de los registros de eventos activos sigue siendo una debilidad coherente en los planes de defensa de seguridad de muchas de las empresas. El [informe de vulneración de datos de Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) encontrado que, aunque el 85 por ciento de las infracciones de tardaron varias semanas para que pueda percibirlo, 84 por ciento de las víctimas tenía la evidencia de la infracción en sus registros de eventos.  
  
## <a name="windows-audit-policy"></a>Directiva de auditoría de Windows

Los siguientes son vínculos al blog de soporte técnico de Microsoft enterprise oficial. El contenido de estos blogs proporciona consejos, instrucciones y recomendaciones acerca de la auditoría que le ayudarán a mejorar la seguridad de su infraestructura de Active Directory y son un valioso recurso al diseñar una directiva de auditoría.  
  
* [Auditoría de acceso a objetos global es la magia](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -describe un mecanismo de control denominado directiva de configuración de auditoría avanzada que se agregó a Windows 7 y Windows Server 2008 R2 que permite establecer qué tipos de datos que desea auditar fácilmente y no jugar con secuencias de comandos y auditpol.exe.  
* [Introducción a la auditoría de cambios en Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -presenta la auditorías cambios realizados en Windows Server 2008.  
* [Niveles de acceso esporádico trucos de auditoría en la Vista y 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica las interesantes características de auditoría de Windows Vista y Windows Server 2008 que puede usarse para solucionar problemas o ver lo que sucede en su entorno.  
* [Tienda integral para la auditoría en Windows Server 2008 y Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene una compilación de la auditoría de las características y la información incluida en Windows Server 2008 y Windows Vista.  
  
Los vínculos siguientes proporcionan información acerca de las mejoras de auditoría en Windows 8 y Windows Server 2012 de Windows e información acerca de AD DS y auditoría en Windows Server 2008.  
  
* [Novedades de la auditoría de seguridad](https://technet.microsoft.com/library/hh849638.aspx) -proporciona una introducción de nuevas características en Windows 8 y Windows Server 2012 de auditoría de seguridad.  
* [Guía paso a paso para la auditoría de AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -describe la nueva característica de auditoría de los servicios de dominio de Active Directory (AD DS) en Windows Server 2008. También proporciona procedimientos para implementar esta nueva característica.  
  
### <a name="windows-audit-categories"></a>Categorías de auditoría de Windows

Antes de Windows Vista y Windows Server 2008, Windows tenían solo nueve categorías de directiva de auditoría de registro de eventos:  
  
* Eventos de inicio de sesión de cuenta  
* Administración de cuentas  
* Acceso al servicio de directorio  
* Eventos de inicio de sesión  
* Acceso a objetos  
* Cambio de directiva  
* Uso de privilegios  
* Seguimiento de procesos  
* Eventos del sistema  
  
Estas nueve categorías de auditoría tradicional conforman una directiva de auditoría. Cada categoría de directiva de auditoría se puede habilitar con éxito, error, o correctos y erróneos eventos. En la sección siguiente, se incluyen sus descripciones.  
  
#### <a name="audit-policy-category-descriptions"></a>Descripciones de categorías de directiva de auditoría  
Las categorías de directiva de auditoría permiten a los siguientes tipos de mensaje de registro de eventos.  
  
##### <a name="audit-account-logon-events"></a>Auditar sucesos de inicio de sesión de cuenta  
Informa de cada instancia de una entidad de seguridad (por ejemplo, usuario, equipo o cuenta de servicio) que se está iniciando sesión o cierre de sesión de un equipo en el que se usa otro equipo para validar la cuenta. Eventos de inicio de sesión de cuenta se generan cuando una cuenta de entidad de seguridad de dominio se autentica en un controlador de dominio. Autenticación de un usuario local en un equipo local genera un evento de inicio de sesión que se registra en el registro de seguridad local. Debe registrarse ningún evento de cierre de sesión de cuenta.  
  
Esta categoría genera una gran cantidad de "ruido" porque Windows constantemente tiene cuentas de inicio de sesión en y fuera de los equipos locales y remotos durante el transcurso normal del negocio. No obstante, cualquier plan de seguridad debe incluir los aciertos y errores de esta categoría de auditoría.  
  
##### <a name="audit-account-management"></a>Auditar la administración de cuenta  
Esta configuración de auditoría determina si se debe realizar un seguimiento de la administración de usuarios y grupos. Por ejemplo, usuarios y grupos deben realizarse un seguimiento cuando se crea, cambiado o eliminado; un usuario o cuenta de equipo, un grupo de seguridad o un grupo de distribución Cuando se cambia el nombre, deshabilitada o habilitada; una cuenta de usuario o equipo o bien, cuando se cambia una contraseña de usuario o equipo. Un evento puede generarse para usuarios o grupos que se agregan o quitan de otros grupos.  
  
##### <a name="audit-directory-service-access"></a>Auditar el acceso del servicio de directorio  

Esta configuración de directiva determina si auditar el acceso entidad de seguridad a un objeto de Active Directory que tiene su propia lista de control de acceso especificado del sistema (SACL). En general, esta categoría solo debe habilitarse en los controladores de dominio. Cuando se habilita, esta configuración genera una gran cantidad de "ruido".  
  
##### <a name="audit-logon-events"></a>Auditar eventos de inicio de sesión  
Eventos de inicio de sesión se generan cuando una entidad de seguridad local se autentica en un equipo local. Los eventos Logon registra los inicios de sesión de dominio que se producen en el equipo local. No se generan eventos de cierre de sesión de cuenta. Cuando se habilita, los eventos de inicio de sesión genera una gran cantidad de "ruido", pero debe habilitarse de forma predeterminada en cualquier plan de auditoría de seguridad.  
  
##### <a name="audit-object-access"></a>Auditar el acceso a objetos  
Acceso a objetos puede generar eventos cuando se tiene acceso a objetos definidos posteriormente con la auditoría habilitada (por ejemplo, Opened, lectura, se cambió el nombre, eliminado o cerrada). Después de la categoría de auditoría principal está habilitada, el administrador debe definir individualmente los objetos tendrán habilitada la auditoría. Muchos de los objetos de sistema de Windows incluyen está habilitada la auditoría, por lo que la habilitación de esta categoría se comenzará normalmente generar eventos antes de que el administrador ha definido alguno.  
  
Esta categoría es muy "ruidosa" y generará eventos de cinco a diez para cada acceso a objetos. Puede ser difícil para los administradores de nuevos a la auditoría de objeto obtener información útil. Solo debería habilitarse cuando sea necesario.  
  
##### <a name="auditing-policy-change"></a>Cambio de directiva de auditoría  
Esta configuración de directiva determina si se debe auditar cada caso de un cambio en las directivas de asignación de derechos de usuario, las directivas de Firewall de Windows, las directivas de confianza o los cambios realizados en la directiva de auditoría. Esta categoría debe habilitarse en todos los equipos. Genera mucho ruido.  
  
##### <a name="audit-privilege-use"></a>Auditar el uso de privilegios  

Existen docenas de derechos de usuario y permisos en Windows (por ejemplo, inicio de sesión como trabajo por lotes y actuar como parte del sistema operativo). Esta configuración de directiva determina si se debe auditar cada instancia de una entidad de seguridad ejecutando un derecho de usuario o el privilegio. Habilitación de esta categoría se traduce en una gran cantidad de "ruido", pero puede ser útil para controlar las cuentas de entidades de seguridad con privilegios elevados.  
  
##### <a name="audit-process-tracking"></a>Auditar el seguimiento de procesos  
Esta configuración de directiva determina si se debe auditar la información de seguimiento de eventos, como la activación de programas, salida de procesos, duplicación de identificadores y acceso indirecto a objetos de proceso detallado. Es útil para realizar el seguimiento de los usuarios malintencionados y los programas que usan.  
  
Habilitar el seguimiento de proceso de auditoría genera un gran número de eventos, por lo que normalmente está establecido en **sin auditoría**. Sin embargo, esta configuración puede proporcionar una gran ventaja durante la respuesta a incidentes desde el registro detallado de los procesos que se inician y el momento en que se iniciaron. Para los controladores de dominio y otros servidores de infraestructura solo rol, esta categoría puede activarse con seguridad en todo momento. Servidores con el único rol no generan proceso mucho tráfico de seguimiento durante el curso normal de sus funciones. Por lo tanto, puede habilitarse para capturar los eventos no autorizados si se producen.  
  
##### <a name="system-events-audit"></a>Auditoría de eventos del sistema  

Eventos del sistema es casi una categoría de comodín genérica, registrar varios eventos que afectan el equipo, la seguridad del sistema o el registro de seguridad. Incluye eventos apagados del equipo y reinicios, eléctrico, cambios de hora del sistema, inicializaciones del paquete de autenticación, clearings de registro de auditoría, problemas de suplantación y un sinfín de otros eventos generales. En general, habilitar esta categoría de auditoría genera una gran cantidad de "ruido", pero genera suficientes eventos muy útiles que resulta difícil nunca se recomienda no volver a habilitarla.  
  
#### <a name="advanced-audit-policies"></a>Directivas de auditoría avanzada

A partir de Windows Vista y Windows Server 2008, Microsoft mejoró la forma en que se pueden realizar selecciones de categoría de registro de eventos mediante la creación de subcategorías de cada categoría de auditoría principal. Las subcategorías permiten que la auditoría sea mucho más granular en caso contrario, podría mediante el uso de las categorías principales. Mediante el uso de las subcategorías, puede habilitar sólo algunas partes de una determinada categoría principal y omitir generar eventos para el que no tiene se utilizará. Cada subcategoría de directiva de auditoría se puede habilitar para eventos correctos, erróneos, o correctos y erróneos.  
  
Para obtener una lista de todas las subcategorías de auditorías disponibles, revise el contenedor de directivas de auditoría avanzada en un objeto de directiva de grupo, o escriba lo siguiente en un símbolo del sistema en cualquier equipo que ejecute Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8, Windows 7 o Windows Vista:  
  
`auditpol /list /subcategory:\*`
  
Para obtener una lista de subcategorías de auditorías configuradas actualmente en un equipo que ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows 2008, escriba lo siguiente:  
  
`auditpol /get /category:\*`
  
Captura de pantalla siguiente muestra un ejemplo de auditpol.exe enumerar la directiva de auditoría actual.  
  
![supervisión de AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Directiva de grupo no informe siempre con precisión el estado de todas las directivas de auditorías habilitadas, mientras que hace de auditpol.exe. Consulte [obtener la directiva de auditoría efectiva en Windows 7 y 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obtener más detalles.  
  
Cada categoría principal tiene varias subcategorías. A continuación es una lista de categorías, sus subcategorías y una descripción de sus funciones.  
  
### <a name="auditing-subcategories-descriptions"></a>Descripciones de las subcategorías de auditoría  
Las subcategorías de directiva de auditoría habiliten a los siguientes tipos de mensaje de registro de eventos:  
  
#### <a name="account-logon"></a>Inicio de sesión de cuenta  
  
##### <a name="credential-validation"></a>Validación de credenciales  
Esta subcategoría notifica los resultados de pruebas de validación en las credenciales que se envía para una solicitud de inicio de sesión de cuenta de usuario. Estos eventos se producen en el equipo autorizado para las credenciales. Para las cuentas de dominio, el controlador de dominio es autoritativo, mientras que para las cuentas locales, el equipo local está autorizado.  
  
En entornos de dominio, la mayoría de los eventos de inicio de sesión de cuenta se registra en el registro de seguridad de los controladores de dominio que sean autoritativos para las cuentas de dominio. Sin embargo, estos eventos pueden producirse en otros equipos de la organización cuando se usan las cuentas locales para iniciar sesión.  
  
##### <a name="kerberos-service-ticket-operations"></a>Auditar operaciones de vales de servicio Kerberos  
Esta subcategoría notifica los eventos generados por los procesos de solicitud de vale de Kerberos en el controlador de dominio que sea autoritativo para la cuenta de dominio.  
  
##### <a name="kerberos-authentication-service"></a>Servicio de autenticación Kerberos  
Esta subcategoría notifica los eventos generados por el servicio de autenticación Kerberos. Estos eventos se producen en el equipo autorizado para las credenciales.  
  
##### <a name="other-account-logon-events"></a>Otros eventos de inicio de sesión de cuentas  
Esta subcategoría notifica los eventos que se producen en respuesta a las credenciales que se envía una solicitud de inicio de sesión de cuenta de usuario para la que no hacen referencia a la validación de credenciales o vales de Kerberos. Estos eventos se producen en el equipo autorizado para las credenciales. Para las cuentas de dominio, el controlador de dominio es autoritativo, mientras que para las cuentas locales, el equipo local está autorizado.  
  
En entornos de dominio, la mayoría de los eventos de inicio de sesión de cuenta se registra en el registro de seguridad de los controladores de dominio que sean autoritativos para las cuentas de dominio. Sin embargo, estos eventos pueden producirse en otros equipos de la organización cuando se usan las cuentas locales para iniciar sesión. Algunos ejemplos pueden incluir lo siguiente:  
  
* Desconexiones de sesión de servicios de escritorio remotas  
* Nuevas sesiones de servicios de escritorio remoto  
* Bloqueo y desbloqueo de una estación de trabajo  
* Invocación de un protector de pantalla  
* Descarte de un protector de pantalla  
* Detección de un Kerberos ataque, en el que se recibe una solicitud de Kerberos con la misma información dos veces por repetición  
* Acceso a una red inalámbrica por parte de una cuenta de usuario o de equipo  
* Acceso a una red 802.1x cableada por parte de una cuenta de usuario o de equipo  
  
#### <a name="account-management"></a>Administración de cuentas  
  
##### <a name="user-account-management"></a>Administración de cuentas de usuario  
Informa de esta subcategoría cada evento de administración de cuentas de usuario, por ejemplo, cuando se crea una cuenta de usuario, cambiada o eliminada; una cuenta de usuario se cambia el nombre, deshabilitada o habilitada; o bien, configurar o cambiar una contraseña. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden seguimiento de eventos para detectar la creación accidental, sean maliciosas y autorizada de las cuentas de usuario.  
  
##### <a name="computer-account-management"></a>Administración de cuentas de equipo  
Esta subcategoría informa de cada evento de administración de cuentas de equipo, por ejemplo, cuando una cuenta de equipo se creado, puede cambiar, eliminar, cambia el nombre, deshabilitada o habilitada.  
  
##### <a name="security-group-management"></a>Administración de grupos de seguridad  
Esta subcategoría informa de cada evento de administración de grupos de seguridad, como cuando se crea, cambia o elimina un grupo de seguridad o cuando un miembro se agrega o quita de un grupo de seguridad. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden seguimiento de eventos para detectar la creación accidental, sean maliciosas y autorizada de las cuentas de grupo de seguridad.  
  
##### <a name="distribution-group-management"></a>Administración de grupos de distribución  
Esta subcategoría informa de cada evento de administración de grupos de distribución, por ejemplo, cuando se crea, cambia o elimina un grupo de distribución o cuando un miembro se agrega o quita de un grupo de distribución. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden seguimiento de eventos para detectar la creación accidental, sean maliciosas y autorizada de las cuentas de grupo.  
  
##### <a name="application-group-management"></a>Administración de grupos de aplicaciones  
Esta subcategoría informa de cada evento de administración del grupo de aplicaciones en un equipo, por ejemplo, cuando se crea, cambia o elimina un grupo de aplicaciones o cuando un miembro se agrega o quita de un grupo de aplicaciones. Si esta configuración de directiva de auditoría está habilitada, los administradores pueden seguimiento de eventos para detectar la creación accidental, sean maliciosas y autorizada de cuentas de grupo de aplicaciones.  
  
##### <a name="other-account-management-events"></a>Otros eventos de administración de cuentas  
Informa de esta subcategoría otros eventos de administración de cuentas.  
  
#### <a name="detailed-process-tracking"></a>Seguimiento de procesos detallada  
  
##### <a name="process-creation"></a>Creación de procesos  
Informa de esta subcategoría de la creación de un proceso y el nombre del usuario o programa que lo creó.  
  
##### <a name="process-termination"></a>Terminación del proceso  
Esta subcategoría notifica cuando finaliza un proceso.  
  
##### <a name="dpapi-activity"></a>Actividad DPAPI  
Informa de esta subcategoría cifrar o descifrar llama a la interfaz de programación de aplicaciones (DPAPI) de protección datos. Se utiliza DPAPI para proteger la información de clave y la información secreta como contraseña almacenada.  
  
##### <a name="rpc-events"></a>Eventos RPC  
Este procedimiento remoto de los informes de subcategory, llame a los eventos de conexión (RPC).  
  
#### <a name="directory-service-access"></a>Acceso al servicio de directorio  
  
##### <a name="directory-service-access"></a>Acceso al servicio de directorio  
Esta subcategoría se emite cuando se tiene acceso a un objeto de AD DS. Solo los objetos con configurado SACL provocar eventos de auditoría sea generado y solo cuando se accede a estos de forma que coincida con las entradas de la SACL. Estos eventos son similares a los eventos de acceso del servicio de directorio en versiones anteriores de Windows Server. Esta subcategoría se aplica solo a los controladores de dominio.  
  
##### <a name="directory-service-changes"></a>Cambios de servicio de directorio  
Esta subcategoría informa de los cambios a los objetos de AD DS. Los tipos de cambios que se notifican son crear, modificar, mover y recuperar las operaciones que se realizan en un objeto. Directory service auditoría de cambio, en su caso, indica los valores antiguos y nuevos de las propiedades modificadas de los objetos que se han cambiado. Solo los objetos con las SACL provocar eventos de auditoría sea generado y solo cuando se accede a estos de forma que coincida con sus entradas de la SACL. Algunos objetos y propiedades no hacen que se generen eventos de auditoría debido a la configuración de la clase de objeto en el esquema. Esta subcategoría se aplica solo a los controladores de dominio.  
  
##### <a name="directory-service-replication"></a>Replicación del servicio de directorio  
Esta subcategoría notifica si la replicación entre dos controladores de dominio comienza y finaliza.  
  
##### <a name="detailed-directory-service-replication"></a>Replicación del servicio de directorio detallada  
Esta subcategoría notifica información detallada acerca de la información que se replica entre controladores de dominio. Estos eventos pueden ser muy altos volumen.  
  
#### <a name="logonlogoff"></a>Inicio/cierre de sesión  
  
##### <a name="logon"></a>Inicio de sesión  
Esta subcategoría informa cuando un usuario intenta iniciar sesión en el sistema. Estos eventos se producen en el equipo que se tiene acceso. Para los inicios de sesión interactivo, la generación de estos eventos se produce en el equipo que se ha iniciado sesión. Si un inicio de sesión tiene lugar para tener acceso a un recurso compartido, estos eventos se generan en el equipo que hospeda el recurso que se tiene acceso. Si esta opción se configura para **ninguna auditoría**, resulta difícil o imposible determinar qué usuario tiene acceso a o se intentó obtener acceso a los equipos de la organización.  
  
##### <a name="network-policy-server"></a>Servidor de directivas de redes  
Esta subcategoría notifica los eventos generados por las solicitudes de acceso de usuario de RADIUS (IAS) y protección de acceso a redes (NAP). Estas solicitudes pueden ser **Grant**, **Deny**, **descartar**, **cuarentena**, **bloqueo**y **Desbloquear**. Auditoría de esta configuración dará como resultado un medio o alto volumen de registros en servidores NPS e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modo principal de IPsec  
Esta subcategoría notifica los resultados del protocolo de intercambio de claves de Internet (IKE) y protocolo de Internet autenticado (AuthIP) durante las negociaciones de modo principal.  
  
##### <a name="ipsec-extended-mode"></a>Modo extendido de IPsec  
Esta subcategoría notifica los resultados de AuthIP durante las negociaciones de modo extendido.  
  
##### <a name="other-logonlogoff-events"></a>Otros eventos de inicio/cierre de sesión  
Informa de esta subcategoría otro inicio de sesión y los eventos relacionados con el cierre de sesión, por ejemplo, servicios de escritorio remoto sesión se desconecta y se vuelve a conectar, con RunAs para ejecutar procesos en una cuenta diferente y bloquear y desbloquear una estación de trabajo.  
  
##### <a name="logoff"></a>La opción de cierre de sesión  
Esta subcategoría informa cuando un usuario cierra el sistema. Estos eventos se producen en el equipo que se tiene acceso. Para los inicios de sesión interactivo, la generación de estos eventos se produce en el equipo que se ha iniciado sesión. Si un inicio de sesión tiene lugar para tener acceso a un recurso compartido, estos eventos se generan en el equipo que hospeda el recurso que se tiene acceso. Si esta opción se configura para **ninguna auditoría**, resulta difícil o imposible determinar qué usuario tiene acceso a o se intentó obtener acceso a los equipos de la organización.  
  
##### <a name="account-lockout"></a>Bloqueo de cuenta  
Esta subcategoría notifica si una cuenta de usuario se bloquea como resultado de demasiados intentos de inicio de sesión.  
  
##### <a name="ipsec-quick-mode"></a>Modo rápido de IPsec  
Esta subcategoría notifica los resultados de protocolo IKE y AuthIP durante las negociaciones de modo rápido.  
  
##### <a name="special-logon"></a>Inicio de sesión especial  
Esta subcategoría informa cuando se usa un inicio de sesión especial. Un inicio de sesión especial es un inicio de sesión que tenga privilegios de administrador equivalentes y se puede usar para elevar un proceso a un nivel superior.  
  
#### <a name="policy-change"></a>Cambio de directiva  
  
##### <a name="audit-policy-change"></a>Cambio de directiva de auditoría  
Esta subcategoría informa de los cambios en la directiva de auditoría, incluidos los cambios de la SACL.  
  
##### <a name="authentication-policy-change"></a>Cambio de directiva de autenticación  
Esta subcategoría informa de los cambios en la directiva de autenticación.  
  
##### <a name="authorization-policy-change"></a>Cambio de directiva de autorización  
Esta subcategoría informa de los cambios en la directiva de autorización, incluidos los cambios de permisos (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Cambio de directiva de nivel de reglas MPSSVC  
Esta subcategoría informa de los cambios en las reglas de directiva utilizados por el servicio de protección de Microsoft (MPSSVC.exe). Este servicio está usando Firewall de Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Cambio de directiva de la plataforma de filtrado  
Informa de esta subcategoría de la adición y eliminación de objetos de WFP, incluidos los filtros de inicio. Estos eventos pueden ser muy altos volumen.  
  
##### <a name="other-policy-change-events"></a>Otros eventos de cambio de directiva  
Informa de esta subcategoría otros tipos de cambios de directiva de seguridad, como la configuración del módulo de plataforma segura (TPM) o proveedores de servicios criptográficos.  
  
#### <a name="privilege-use"></a>Uso de privilegios  
  
##### <a name="sensitive-privilege-use"></a>Uso de privilegios confidenciales  
Informes de esta subcategoría, cuando una cuenta de usuario o el servicio utiliza un privilegio confidencial. Un privilegio confidencial incluye los siguientes derechos de usuario: actuar como parte del sistema operativo, realizar una copia de seguridad de archivos y directorios, cree un objeto de token, depurar programas, habilitar cuentas de usuario y equipo sea de confianza para delegación, generar auditorías de seguridad suplantar a un cliente tras la autenticación, cargar y descargar controladores de dispositivo, Administrar registro de auditoría y seguridad, modifique los valores de entorno firmware, reemplazar un token de nivel de proceso, restaurar archivos y directorios y tomar posesión de archivos u otros objetos. Auditoría de esta subcategoría, creará un gran volumen de eventos.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso de privilegios no confidenciales  
Informes de esta subcategoría, cuando una cuenta de usuario o el servicio utiliza un privilegio no confidenciales. Privilegio no confidenciales incluye los siguientes derechos de usuario: tener acceso al administrador de credenciales como un llamador de confianza, tener acceso a este equipo desde la red, agregar estaciones de trabajo al dominio, ajustar las cuotas de memoria para un proceso, permitir el inicio de sesión localmente, permitir inicio de sesión a través de remoto Escritorio de servicios, recorrido de omisión de comprobación, cambiar la hora del sistema, cree un archivo de paginación, crear objetos globales, crear objetos compartidos permanentes, cree vínculos simbólicos, denegar el acceso a este equipo desde la red, Denegar inicio de sesión como trabajo por lotes, Denegar inicio de sesión como servicio, Denegar inicio de sesión localmente, Denegar inicio de sesión a través de servicios de escritorio remoto, Forzar cierre desde un sistema remoto, aumentar el espacio de trabajo de un proceso, aumentar prioridad de programación, bloquear páginas en memoria, inicie sesión como trabajo por lotes, inicie sesión como un servicio, modifique la etiqueta de un objeto, lleve a cabo volumen tareas de mantenimiento, analizar un solo proceso, generar perfiles de rendimiento del sistema, quitar el equipo de estación de acoplamiento, apague el sistema y sincronización los datos del servicio de directorio. Auditoría de esta subcategoría, creará un volumen muy alto de eventos.  
  
##### <a name="other-privilege-use-events"></a>Otros eventos de uso de privilegios  
Esta configuración de directiva de seguridad no se utiliza actualmente.  
  
#### <a name="object-access"></a>Acceso a objetos  
  
##### <a name="file-system"></a>Sistema de archivos  
Esta subcategoría informa cuando se accede a los objetos del sistema de archivos. Solo objetos de sistema de archivos con las SACL provocar eventos de auditoría sea generado y solo cuando se tiene acceso a una forma que sus entradas de la SACL coincidentes. Por sí solo, esta configuración de directiva no hará que la auditoría de todos los eventos. Determina si se debe auditar los eventos de un usuario que tiene acceso a un objeto de sistema de archivo que tiene una lista de control de acceso especificado del sistema (SACL), eficazmente al habilitar la auditoría tener lugar.  
  
Si el valor Auditar el acceso a objetos está configurado para **éxito**, se genera una entrada de auditoría cada vez que un usuario obtiene acceso a un objeto con una SACL especificada. Si esta configuración de directiva está configurada para **error**, se genera una entrada de auditoría cada vez que se produce un error de un usuario en un intento de acceder a un objeto con una SACL especificada.  
  
##### <a name="registry"></a>Registro  
Esta subcategoría se emite cuando se tiene acceso a objetos del registro. Solo los objetos del registro con las SACL provocar eventos de auditoría sea generado y solo cuando se tiene acceso a una forma que sus entradas de la SACL coincidentes. Por sí solo, esta configuración de directiva no hará que la auditoría de todos los eventos.  
  
##### <a name="kernel-object"></a>Objeto de kernel  
Esta subcategoría informa cuando se accede a los objetos de kernel, como procesos y las exclusiones mutuas. Solo los objetos de kernel con las SACL provocar eventos de auditoría sea generado y solo cuando se tiene acceso a una forma que sus entradas de la SACL coincidentes. Normalmente, los objetos de kernel solo tienen las SACL si están habilitadas las opciones de auditoría AuditBaseObjects o AuditBaseDirectories.  
  
##### <a name="sam"></a>SAM  
Esta subcategoría se emite cuando se tiene acceso a objetos de base de datos de autenticación de administrador de cuentas de seguridad (SAM) local.  
  
##### <a name="certification-services"></a>Servicios de certificación  
Esta subcategoría informa cuando se realizan operaciones de servicios de certificación.  
  
##### <a name="application-generated"></a>Aplicación generada  
Esta subcategoría se emite cuando las aplicaciones intenten generar eventos de auditoría mediante el uso de la auditoría de interfaces de programación de aplicaciones (API) de Windows.  
  
##### <a name="handle-manipulation"></a>Manipulación de identificadores  
Esta subcategoría se emite cuando se abre o cierra un identificador de objeto. Sólo los objetos con las SACL que estos eventos sea generado y solo si la operación intentada identificador coincide con las entradas de la SACL. Controlar eventos de manipulación solo se generan para los tipos de objetos donde está habilitada la subcategoría de acceso de objeto correspondiente (por ejemplo, sistema de archivos o del registro).  
  
##### <a name="file-share"></a>Recurso compartido de archivos  
Esta subcategoría se emite cuando se tiene acceso a un recurso compartido de archivos. Por sí solo, esta configuración de directiva no hará que la auditoría de todos los eventos. Determina si se debe auditar los eventos de un usuario que tiene acceso a un objeto de recurso compartido de archivo que tiene una lista de control de acceso especificado del sistema (SACL), eficazmente al habilitar la auditoría tener lugar.  
  
##### <a name="filtering-platform-packet-drop"></a>Colocación de paquetes de la plataforma de filtrado  
Esta subcategoría notifica si los paquetes se descartan por Windows Filtering Platform (WFP). Estos eventos pueden ser muy altos volumen.  
  
##### <a name="filtering-platform-connection"></a>Conexión de la plataforma de filtrado  
Esta subcategoría notifica si las conexiones están permitidas o bloqueadas por WFP. Estos eventos pueden ser de gran volumen.  
  
##### <a name="other-object-access-events"></a>Otros eventos de acceso de objeto  
Informa de esta subcategoría otros eventos relacionados con el acceso de objeto, como los trabajos de programador de tareas y objetos COM +.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Cambio de estado de seguridad  
Esta subcategoría informa de los cambios en el estado de seguridad del sistema, como cuando se inicia y detiene el subsistema de seguridad.  
  
##### <a name="security-system-extension"></a>Extensión de seguridad del sistema  
Esta subcategoría indica la carga de código de extensión como paquetes de autenticación mediante el subsistema de seguridad.  
  
##### <a name="system-integrity"></a>Integridad del sistema  
Esta subcategoría informa sobre las infracciones de integridad del subsistema de seguridad.  
  
Controlador IPsec  
  
Esta subcategoría informa sobre las actividades del controlador de protocolo de Internet (IPsec) de seguridad.  
  
##### <a name="other-system-events"></a>Otros eventos del sistema  
Informa de esta subcategoría en otros eventos del sistema.  
  
Para obtener más información acerca de las descripciones de subcategory, consulte el [herramienta Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Cada organización debe revisar las categorías cubiertas anteriores y subcategorías y habilitar los que mejor se adapten a su entorno. Siempre se deben probar los cambios realizados en la directiva de auditoría antes de la implementación en un entorno de producción.  
  
## <a name="configuring-windows-audit-policy"></a>Configurar directiva de auditoría de Windows

Directiva de auditoría de Windows se puede establecer con modificaciones del registro, auditpol.exe, API o las directivas de grupo. Los métodos recomendados para configurar la directiva de auditoría para la mayoría de las empresas son la directiva de grupo o auditpol.exe. La configuración de directiva de auditoría del sistema requiere permisos de cuenta de administrador o los permisos delegados adecuados.  
  
> [!NOTE]  
> El **Administrar registro de auditoría y seguridad** privilegios deben proporcionarse a entidades de seguridad (los administradores tienen de forma predeterminada) para permitir la modificación de las opciones de los recursos individuales, como los archivos, Active la auditoría de acceso a objetos Los objetos de directorio y las claves del registro.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Configurar la directiva de auditoría de Windows mediante la directiva de grupo

Para establecer una directiva de auditoría mediante directivas de grupo, configure las categorías de auditoría correspondiente ubicadas en **equipo\Configuración de Windows\Configuración de seguridad\Directivas Locales\directiva** (consulte la siguiente captura de pantalla de un ejemplo desde el Editor de directivas de grupo de Local (gpedit.msc)). Cada categoría de directiva de auditoría se puede habilitar para **éxito**, **error**, o **éxito** y eventos de error.  
  
![supervisión de AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Directiva de auditoría avanzada puede establecerse mediante el uso de directivas de grupo local o de Active Directory. Para establecer una directiva de auditoría avanzada, configure las subcategorías correspondientes ubicadas en **configuración del equipo\Configuración de Windows\Configuración de Seguridad\directiva de auditoría** (consulte la siguiente captura de pantalla para obtener un ejemplo de la variable Local Editor directiva de grupo (gpedit.msc)). Cada subcategoría de directiva de auditoría se puede habilitar para **éxito**, **error**, o **éxito** y **error** eventos.  
  
![supervisión de AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Establecimiento de la directiva de auditoría de Windows mediante Auditpol.exe

Auditpol.exe (para establecer la directiva de auditoría de Windows) se introdujo en Windows Server 2008 y Windows Vista. Inicialmente, solo auditpol.exe podría usarse para establecer la directiva de auditoría avanzada, pero se puede usar la directiva de grupo en Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8 y Windows 7.  
  
Auditpol.exe es una utilidad de línea de comandos. La sintaxis es como sigue:  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Ejemplos de sintaxis de auditpol.exe:  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol.exe directiva de auditoría avanzada se establece localmente. Si la directiva local entra en conflicto con la directiva de grupo local o de Active Directory, configuración de directiva de grupo normalmente prevalece sobre las opciones de auditpol.exe. Cuando existen varios grupo o los conflictos de directivas locales, solo con una directiva prevalecerá (que es, reemplace). No se combinarán las directivas de auditoría.  
  
#### <a name="scripting-auditpol"></a>Secuencias de comandos Auditpol

Microsoft proporciona un [script de ejemplo](https://support.microsoft.com/kb/921469) para administradores que desean para establecer la directiva de auditoría avanzada mediante una secuencia de comandos en lugar de escribir manualmente en cada comando de auditpol.exe.  
  
**Tenga en cuenta** directiva de grupo no siempre informar con exactitud el estado de todas las directivas de auditorías habilitadas, mientras que hace de auditpol.exe. Consulte [obtener la directiva de auditoría efectiva en Windows 7 y Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obtener más detalles.  
  
#### <a name="other-auditpol-commands"></a>Otros comandos Auditpol

Auditpol.exe puede usarse para guardar y restaurar una directiva de auditoría local y para ver otros comandos de auditoría relacionados. Estos son los otros **auditpol** comandos.  
  
`auditpol /clear` : Se utiliza para borrar y restablecer las directivas de auditoría local  
  
`auditpol /backup /file:<filename>` : Se utiliza para realizar una copia de seguridad de una directiva de auditoría local actual en un archivo binario  
  
`auditpol /restore /file:<filename>` : Se utiliza para importar un archivo de directiva de auditoría guardado previamente a una directiva de auditoría local  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` -Si está habilitada esta configuración de directiva de auditoría, hace que el sistema detener inmediatamente (con pausa: Mensaje de C0000244 {Error de auditoría}) si la auditoría de seguridad no se pueden registrar por cualquier motivo. Normalmente, no se registra cuando el registro de auditoría de seguridad está lleno y el método de retención especificado para el registro de seguridad es un evento **no sobrescribir eventos** o **sobrescribir eventos por días**. Normalmente, sólo se habilita los entornos que necesitan mayor garantía que está registrando el registro de seguridad. Si está habilitada, los administradores deben ver el tamaño del registro de seguridad estrechamente y alternar los registros según sea necesario. También se puede establecer con directiva de grupo mediante la modificación de la opción de seguridad **auditoría: Apagar el sistema inmediatamente si no se puede registrar las auditorías de seguridad** (predeterminado = deshabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` -Esta configuración de directiva de auditoría determina si se debe auditar el acceso de objetos globales del sistema. Si esta directiva está habilitada, hace que los objetos del sistema, como exclusiones mutuas, semáforos, eventos y dispositivos de DOS a crearse con una lista de control de acceso predeterminada del sistema (SACL). Considere la posibilidad de mayoría de los administradores auditar objetos del sistema global para que sea demasiado "ruido" y solo permitirá si sospecha que la piratería malintencionado. Solo los objetos con nombre tienen una SACL. Si también está habilitada la auditoría objeto acceso auditoría directiva (o subcategoría de auditoría del objeto de Kernel), se audita el acceso a estos objetos del sistema. Al configurar esta configuración de seguridad, los cambios no surtirán efecto hasta que reinicie Windows. Esta directiva también se puede establecer con la directiva de grupo mediante la modificación de la opción de seguridad Auditar el acceso de objetos globales del sistema (predeterminado = deshabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` -Esta configuración de directiva de auditoría especifica que se ofrecerá las SACL cuando se crean los objetos de kernel con nombre (por ejemplo, exclusiones mutuas y semáforos). AuditBaseDirectories afecta a los objetos del contenedor mientras AuditBaseObjects afecta a los objetos que no pueden contener otros objetos.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` -Esta configuración de directiva de auditoría especifica si el cliente genera un evento cuando uno o varios de estos privilegios se asignan a un token de seguridad de usuario: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege y TcbPrivilege. Si esta opción no está habilitada (valor predeterminado = deshabilitado), no se registran los privilegios BackupPrivilege y RestorePrivilege. Al habilitar esta opción puede hacer que el registro de seguridad muy ruidoso (a veces, cientos de eventos de un segundo) durante una operación de copia de seguridad. Esta directiva también se puede establecer con la directiva de grupo mediante la modificación de la opción de seguridad **auditoría: Auditar el uso de privilegios de copia de seguridad y restauración**.  
  
> [!NOTE]  
> Alguna información proporcionada aquí se ha tomado de la Microsoft [tipo de opción de auditoría](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) y la herramienta Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Exigir la auditoría tradicional o auditoría avanzada

En Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 y Windows Vista, los administradores pueden elegir para habilitar las nueve categorías tradicionales o para usar las subcategorías. Es una opción binaria que se debe realizar en cada sistema de Windows. Las principales categorías se pueden habilitar o no puede ser el subcategoriesit.  
  
Para evitar que la directiva heredada categoría tradicional sobrescribir las subcategorías de directiva de auditoría, debe habilitar la **forzar la configuración de subcategoría de auditoría de directiva (Windows Vista o posterior) para invalidar la configuración de categoría de directiva de auditoría** configuración de directiva se encuentra en **equipo\Configuración de Windows\Configuración de seguridad\Directivas Locales\opciones de seguridad**.  
  
Se recomienda que las subcategorías se habilitado y configuradas en lugar de las nueve categorías principales. Esto requiere que una configuración de directiva de grupo esté habilitada (para permitir que las subcategorías invalidar las categorías de auditoría), junto con la configuración de las subcategorías diferentes que admiten las directivas de auditoría.  
  
Subcategorías de auditoría pueden configurarse mediante varios métodos, incluida la directiva de grupo y el programa de línea de comandos, auditpol.exe.  
  
## <a name="next-steps"></a>Pasos siguientes
  
* [Auditoría en Windows 7 y Windows Server 2008 R2 de seguridad avanzada](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Auditoría y cumplimiento en Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Cómo usar Directiva de grupo para configurar la seguridad detallada de configuración de auditoría para equipos basados en Windows Vista y Windows Server 2008 en un dominio de Windows Server 2008, en un dominio de Windows Server 2003 o en un dominio de Windows 2000](https://support.microsoft.com/kb/921469)  
  
* [Guía paso a paso de directiva de auditoría de seguridad avanzada](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
