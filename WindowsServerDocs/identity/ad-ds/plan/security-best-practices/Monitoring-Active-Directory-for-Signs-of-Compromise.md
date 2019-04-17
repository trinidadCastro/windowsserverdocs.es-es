---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: "Supervisión de Active Directory para signos de compromiso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 096f2fa58b9aae53a06bf26c2107eb4cee3aa5d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Supervisión de Active Directory para signos de compromiso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número cinco: vigilancia externa es el precio de seguridad.* - [10 inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
Un sistema de supervisión del registro de eventos de sólido es una parte fundamental de cualquier diseño seguro de Active Directory. Muchos compromisos de seguridad del equipo podrían detectarse desde el principio en el caso si víctimas en vigor el registro de eventos correspondiente supervisión y alertas. Durante cuánto tiempo han admiten esta conclusión los informes independientes. Por ejemplo, el [informe de infracción de 2009 Verizon datos](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) estados:  
  
"La reduce aparente de análisis de supervisión y el registro de eventos sigue siendo un poco de una máquina enigma. La oportunidad de detección es investigadores indican que 66% de víctimas tenía pruebas suficientes disponible dentro de sus registros para detectar el incumplimiento se hubiera más cuidadoso en analizar estos recursos."  
  
Esta falta de supervisión activos registros de eventos sigue siendo un punto débil coherente en los planes de defensa de seguridad de muchas de las empresas. La [informe de fuga de datos de Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) encuentra que aunque 85% de infracciones llevó varias semanas que pasa desapercibido, 84 por ciento de víctimas tenía pruebas de la infracción de sus registros de eventos.  
  
## <a name="windows-audit-policy"></a>Directiva de auditoría de Windows  
Las siguientes son vínculos al blog de soporte técnico de Microsoft oficiales de empresa. El contenido de estos blogs proporciona consejos, instrucciones y recomendaciones acerca de la auditoría que le ayudarán a mejorar la seguridad de la infraestructura de Active Directory y son un recurso valioso al diseñar una directiva de auditoría.  
  
-   [Auditoría de acceso a objetos global es mágico](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -describe un mecanismo de control denominado directiva de configuración de auditoría avanzada que se agregó a Windows 7 y Windows Server 2008 R2 que te permite establecer qué tipos de datos que querías auditar fácilmente y no echar un vistazo scripts y auditpol.exe.  
  
-   [Introducir cambios de auditoría en Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -presenta los cambios de auditoría realizado en Windows Server 2008.  
  
-   [Geniales auditoría trucos en la Vista y 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica interesantes características de auditoría de Windows Vista y Windows Server 2008 puede usarse para la solución de problemas o ver lo que sucede en su entorno.  
  
-   [Un solo lugar para auditar en Windows Server 2008 y Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contiene una compilación de auditoría de características y la información incluida en Windows Server 2008 y Windows Vista.  
  
Los siguientes vínculos proporcionan información sobre las mejoras de auditoría en Windows 8 y Windows Server 2012 de Windows e información acerca de AD DS de auditoría en Windows Server 2008.  
  
-   [Novedades de auditoría de seguridad](https://technet.microsoft.com/library/hh849638.aspx) -proporciona una introducción de nuevas características de Windows 8 y Windows Server 2012 de auditoría de seguridad.  
  
-   [Auditoría de AD DS Step-by-Step guía](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -describe la nueva característica de auditoría de servicios de dominio de Active Directory (AD DS) en Windows Server 2008. También se ofrecen procedimientos para implementar esta nueva característica.  
  
### <a name="windows-audit-categories"></a>Categorías de auditoría de Windows  
Antes de Windows Vista y Windows Server 2008, Windows había solo nueve categorías de directiva de auditoría de registro de eventos:  
  
-   Eventos de inicio de sesión de cuenta  
  
-   Administración de cuentas  
  
-   Acceso al servicio de directorio  
  
-   Eventos de inicio de sesión  
  
-   Acceso a objetos  
  
-   Cambio de directiva  
  
-   Uso de privilegios  
  
-   Seguimiento de procesos  
  
-   Eventos del sistema  
  
Estos nueve categorías de auditoría tradicional constan de una directiva de auditoría. Se puede habilitar cada categoría de directiva de auditoría de aciertos, errores o aciertos y errores eventos. Sus descripciones se incluyen en la siguiente sección.  
  
#### <a name="audit-policy-category-descriptions"></a>Descripciones de categoría de directiva de auditoría  
Las categorías de directiva de auditoría habilita a los siguientes tipos de mensaje de registro de eventos.  
  
##### <a name="audit-account-logon-events"></a>Auditar eventos de inicio de sesión de cuenta  
Notifica cada instancia de una entidad de seguridad (por ejemplo, usuario, PC o cuenta de servicio) que se está iniciando sesión o cierre de sesión de un equipo en el que otro equipo se usa para validar la cuenta. Eventos de inicio de sesión de cuenta se generan cuando una cuenta de entidad de seguridad de dominio se autentica en un controlador de dominio. Autenticación de un usuario local en un equipo local, genera un evento de inicio de sesión que se registre en el registro de seguridad local. Se registra ningún evento de cierre de sesión de cuenta.  
  
Esta categoría genera una gran cantidad de "ruido" debido a que Windows es tener constantemente iniciar la sesión de cuentas y fuera de los equipos locales y remotos durante el curso normal del negocio. Sin embargo, cualquier plan de seguridad debe incluir la aciertos y errores de esta categoría de auditoría.  
  
##### <a name="audit-account-management"></a>Administración de cuentas de auditoría  
Esta configuración de auditoría determina si se debe realizar un seguimiento de administración de usuarios y grupos. Por ejemplo, los usuarios y grupos deben realizar un seguimiento cuando se crea, cambia o elimina; un usuario o cuenta de equipo, un grupo de seguridad o un grupo de distribución Cuando se cambia el nombre, deshabilitada o habilitada; una cuenta de usuario o equipo o bien, cuando se cambia una contraseña de usuario o equipo. Puede generar un evento para los usuarios o grupos que se agregan o quitan de otros grupos.  
  
##### <a name="audit-directory-service-access"></a>Auditar el acceso del servicio de directorio  

Esta configuración de directiva determina si para auditar el acceso de entidad de seguridad a un objeto de Active Directory que tiene su propia lista de control de acceso especificada del sistema (SACL). En general, esta categoría solo debe estar habilitada en controladores de dominio. Cuando se habilita, esta configuración genera una gran cantidad de "ruido".  
  
##### <a name="audit-logon-events"></a>Auditar eventos de inicio de sesión  
Eventos de inicio de sesión se generan cuando una entidad de seguridad local se autentica en un equipo local. Registra los eventos de inicio de sesión inicios de sesión de dominio que se producen en el equipo local. No se generan eventos de cierre de sesión de cuenta. Cuando está habilitada, los eventos de inicio de sesión genera una gran cantidad de "ruido", pero debe estar habilitados de manera predeterminada en cualquier plan de auditoría de seguridad.  
  
##### <a name="audit-object-access"></a>Auditar el acceso a objetos  
Acceso a objetos puede generar eventos cuando se accede a objetos definidos posteriormente está habilitada la auditoría (por ejemplo, abierto, lectura, nombre cambiado, eliminados o cerrado). Después de la categoría de auditoría principal está habilitada, el administrador debe definir individualmente qué objetos habrás habilita la auditoría. Muchos de los objetos de sistema de Windows incluyen auditoría habilitado, para habilitar esta categoría se iniciará normalmente generar eventos antes de que el administrador ha definido cualquiera.  
  
Esta categoría es muy "ruidosa" y genera eventos de cinco a diez para cada acceso a objetos. Puede resultar difícil para los administradores de nuevos para auditar objetos obtener información útil. Solo debe habilitarse cuando sea necesario.  
  
##### <a name="auditing-policy-change"></a>Cambio de directiva de auditoría  
Esta configuración de directiva determina si se debe auditar cada caso de un cambio en las directivas de asignación de derechos de usuario, las directivas de Firewall de Windows, las directivas de confianza o cambios en la directiva de auditoría. Esta categoría debe estar habilitada en todos los equipos. Genera ruido muy poco.  
  
##### <a name="audit-privilege-use"></a>Auditar uso de privilegios  

Hay decenas de derechos de usuario y permisos en Windows (por ejemplo, inicio de sesión como trabajo por lotes y actuar como parte del sistema operativo). Esta configuración de directiva determina si se debe auditar cada instancia de una entidad de seguridad por ejercer un derecho de usuario o el privilegio. Habilitación de esta categoría se traduce una gran cantidad de "ruido", pero puede ser útil para controlar las cuentas de seguridad principal con privilegios elevados.  
  
##### <a name="audit-process-tracking"></a>Auditar el seguimiento de procesos  
Esta configuración de directiva determina si se debe auditar la información de seguimiento para eventos como la activación de programas, salir del proceso, duplicación de identificadores y acceso indirecto a objetos de proceso detallado. Es útil para realizar el seguimiento de los usuarios malintencionados y los programas que usan.  
  
Habilitar el seguimiento de proceso de auditoría genera un gran número de eventos, por lo está establecida en **sin auditoría**. Sin embargo, esta configuración puede proporcionar una gran ventaja durante una respuesta a incidentes desde el registro detallado de los procesos iniciados y el tiempo que se inician. Controladores de dominio y otros servidores único rol infraestructura, esta categoría puede activarse con seguridad de todo el tiempo. Servidores única función no se generan proceso mucho tráfico de seguimiento durante el curso normal de sus funciones. Como tal, puede habilitarse para capturar los eventos no autorizados si se producen.  
  
##### <a name="system-events-audit"></a>Eventos de auditoría del sistema  

Eventos del sistema es casi una categoría de capturar todos los genérica, registrar varios eventos que afectan el equipo, su seguridad del sistema o el registro de seguridad. Incluye eventos para apagados del equipo y se reinicia, errores de energía, cambios de tiempo del sistema, código de inicialización de paquete de autenticación, clearings de registro de auditoría, problemas de representación y un host de otros eventos generales. En general, al habilitar esta categoría de auditoría genera una gran cantidad de "ruido", pero genera suficiente eventos muy útiles que es difícil nunca recomendamos no se habilitan.  
  
#### <a name="advanced-audit-policies"></a>Directivas de auditoría avanzada  
A partir de Windows Vista y Windows Server 2008, Microsoft ha mejorado la forma en que las selecciones de categoría de registro de eventos pueden realizarse creando subcategorías dentro de cada categoría de auditoría principal. Subcategorías permiten la auditoría para que sea mucho más pormenorizado de lo contrario, podría mediante el uso de las categorías principales. Con subcategorías, puedes habilitar solo algunas partes de una categoría principal determinada y omitir generar eventos para el que puedes no que use. Se puede habilitar cada subcategoría de la directiva de auditoría de aciertos, errores o aciertos y errores eventos.  
  
Para obtener una lista de todas las subcategorías de auditoría disponibles, revisa el contenedor de directiva de auditoría avanzada en un objeto de directiva de grupo o escribe lo siguiente en un símbolo del sistema en cualquier equipo que ejecute Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8, Windows 7 o Windows Vista:  
  
**Auditpol//list /subcategory: \ ***  
  
Para obtener una lista de las subcategorías de auditoría configuradas actualmente en un equipo que ejecute Windows Server 2012, Windows Server 2008 R2 o Windows 2008, escribe lo siguiente:  
  
**Auditpol//get /category: \ ***  
  
Captura de pantalla siguiente muestra un ejemplo de auditpol.exe la descripción de la directiva de auditoría actual.  
  
![Supervisión de anuncios](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Directiva de grupo no informa siempre con precisión el estado de todas las directivas de auditoría habilitados, mientras que cambia auditpol.exe. Consulta [obtener la directiva de auditoría eficaz en Windows 7 y 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obtener más detalles.  
  
Cada categoría principal tiene varias subcategorías. A continuación te mostramos una lista de categorías y subcategorías con una descripción de sus funciones.  
  
### <a name="auditing-subcategories-descriptions"></a>Descripciones de subcategorías de auditoría  
Subcategorías de directiva de auditoría permiten a los siguientes tipos de mensaje de registro de eventos:  
  
#### <a name="account-logon"></a>Inicio de sesión de cuenta  
  
##### <a name="credential-validation"></a>Validación de credenciales  
Esta subcategoría informa de los resultados de pruebas de validación de credenciales enviadas para una solicitud de inicio de sesión de cuenta de usuario. Estos eventos se producen en el equipo que está autorizado para las credenciales. Para las cuentas de dominio, el controlador de dominio está autorizado, mientras que para las cuentas locales, el equipo local está autorizado.  
  
En entornos de dominio, la mayoría de los eventos de inicio de sesión de cuenta se registra en el registro de seguridad de los controladores de dominio que están autorizados para las cuentas de dominio. Sin embargo, estos eventos pueden producirse en otros equipos de la organización cuando las cuentas locales se usan para iniciar sesión.  
  
##### <a name="kerberos-service-ticket-operations"></a>Operaciones de vales de servicio Kerberos  
Esta subcategoría notifica eventos generados por los procesos de solicitud de vale Kerberos en el controlador de dominio que está autorizado para la cuenta de dominio.  
  
##### <a name="kerberos-authentication-service"></a>Servicio de autenticación Kerberos  
Esta subcategoría notifica los eventos generados por el servicio de autenticación de Kerberos. Estos eventos se producen en el equipo que está autorizado para las credenciales.  
  
##### <a name="other-account-logon-events"></a>Otros eventos de inicio de sesión de cuenta  
Esta subcategoría notifica los eventos que se producen en respuesta a las credenciales enviado para una solicitud de inicio de sesión de cuenta de usuario que no relacionados con la validación de credenciales ni vales Kerberos. Estos eventos se producen en el equipo que está autorizado para las credenciales. Para las cuentas de dominio, el controlador de dominio está autorizado, mientras que para las cuentas locales, el equipo local está autorizado.  
  
En entornos de dominio, la mayoría de los eventos de inicio de sesión de cuenta se registra en el registro de seguridad de los controladores de dominio que están autorizados para las cuentas de dominio. Sin embargo, estos eventos pueden producirse en otros equipos de la organización cuando las cuentas locales se usan para iniciar sesión. Algunos ejemplos pueden incluir lo siguiente:  
  
-   Desconexiones de sesión de servicios de escritorio remotos  
  
-   Nuevas sesiones de servicios de escritorio remoto  
  
-   Bloqueo y desbloqueo de una estación de trabajo  
  
-   Invocar un protector de pantalla  
  
-   Descartar un protector de pantalla  
  
-   Detección de un Kerberos reproducir ataque, en el que se recibe una solicitud de Kerberos con la misma información dos veces  
  
-   Acceso a una red inalámbrica concedido a una cuenta de usuario o equipo  
  
-   Acceso a una red 802.1X cableada x concedido a una cuenta de usuario o equipo de red  
  
#### <a name="account-management"></a>Administración de cuentas  
  
##### <a name="user-account-management"></a>Administración de cuentas de usuario  
Esta subcategoría notifica cada evento de administración de cuentas de usuario, como cuando se crea, cambia o elimina; una cuenta de usuario una cuenta de usuario se cambió de nombre, deshabilitada o habilitada; o bien, establecer o cambiar una contraseña. Si se habilita esta configuración de directiva de auditoría, los administradores pueden eventos de seguimiento para detectar malintencionada, accidental y autorizada creación de cuentas de usuario.  
  
##### <a name="computer-account-management"></a>Administración de cuentas de equipo  
Esta subcategoría notifica cada evento de administración de cuentas de equipo, como cuando una cuenta de equipo es crea, cambia, elimina, cambia el nombre, deshabilitada o habilitada.  
  
##### <a name="security-group-management"></a>Administración del grupo de seguridad  
Esta subcategoría notifica cada evento de administración del grupo de seguridad, como cuando se crea, cambia o elimina un grupo de seguridad o cuando se agrega o quita un grupo de seguridad de un miembro. Si se habilita esta configuración de directiva de auditoría, los administradores pueden eventos de seguimiento para detectar malintencionada, accidental y autorizada creación de cuentas de grupo de seguridad.  
  
##### <a name="distribution-group-management"></a>Administración del grupo de distribución  
Esta subcategoría notifica cada evento de administración del grupo de distribución, como cuando se crea, cambia o elimina un grupo de distribución o cuando se agrega un miembro o se quita de un grupo de distribución. Si se habilita esta configuración de directiva de auditoría, los administradores pueden eventos de seguimiento para detectar malintencionada, accidental y autorizada creación de cuentas de grupo.  
  
##### <a name="application-group-management"></a>Administración del grupo de aplicaciones  
Esta subcategoría notifica cada evento de administración del grupo de la aplicación en un equipo, como cuando se crea, cambia o elimina un grupo de aplicación o cuando se agrega un miembro o se quita de un grupo de aplicaciones. Si se habilita esta configuración de directiva de auditoría, los administradores pueden eventos de seguimiento para detectar malintencionada, accidental y autorizada creación de cuentas de grupo de la aplicación.  
  
##### <a name="other-account-management-events"></a>Otros eventos de administración de cuentas  
Esta subcategoría notifica otros eventos de administración de cuentas.  
  
#### <a name="detailed-process-tracking"></a>Seguimiento de procesos detallada  
  
##### <a name="process-creation"></a>Creación de procesos  
Esta subcategoría notifica la creación de un proceso y el nombre del usuario o programa que la creó.  
  
##### <a name="process-termination"></a>Finalización de procesos  
Esta subcategoría se notifica cuando un proceso finaliza.  
  
##### <a name="dpapi-activity"></a>Actividad DPAPI  
Esta subcategoría notifica cifrar o descifrar llama a la interfaz de programación de aplicaciones (DPAPI) de protección frente a datos. DPAPI se utiliza para proteger información confidencial, como contraseña almacenada y la información clave.  
  
##### <a name="rpc-events"></a>Eventos de RPC  
Este procedimiento remoto de informes de subcategoría llamar a los eventos de conexión (RPC).  
  
#### <a name="directory-service-access"></a>Acceso al servicio de directorio  
  
##### <a name="directory-service-access"></a>Acceso al servicio de directorio  
Esta subcategoría se notifica cuando se accede a un objeto de AD DS. Solo los objetos con SACL configurados generan eventos de auditoría que generado y solamente cuando se obtiene acceso a una forma que coincida con las entradas de la SACL. Estos eventos son similares a los eventos de acceso del servicio de directorio en versiones anteriores de Windows Server. Esta subcategoría se aplica solo a los controladores de dominio.  
  
##### <a name="directory-service-changes"></a>Cambios de servicio de directorio  
Esta subcategoría notifica cambios en objetos de AD DS. Los tipos de cambios que se notifican son crear, modificar, mover y recuperar las operaciones realizadas en un objeto. Auditoría servicios de directorio cambio, si procede, indica los valores antiguos y nuevos de las propiedades modificadas de los objetos que se han cambiado. Solo los objetos con SACL provocar eventos de auditoría que generado y solamente cuando se obtiene acceso a una forma que coincida con sus entradas de la SACL. Algunos objetos y propiedades no hacen que se generen debido a la configuración de la clase de objeto en el esquema de eventos de auditoría. Esta subcategoría se aplica solo a los controladores de dominio.  
  
##### <a name="directory-service-replication"></a>Replicación de servicio de directorio  
Esta subcategoría se notifica cuando replicación entre dos controladores de dominio empieza y termina.  
  
##### <a name="detailed-directory-service-replication"></a>Replicación de servicio de directorio detallada  
Esta subcategoría notifica información detallada acerca de los datos replicados entre controladores de dominio. Estos eventos pueden ser muy altos por volumen.  
  
#### <a name="logonlogoff"></a>Inicio/cierre de sesión  
  
##### <a name="logon"></a>Inicio de sesión  
Esta subcategoría se notifica cuando un usuario intenta iniciar sesión en el sistema. Estos eventos se producen en el equipo al que se accede. Para los inicios de sesión interactivo, la generación de estos eventos se produce en el equipo que se ha iniciado sesión. Si un inicio de sesión de red tiene lugar para obtener acceso a un recurso compartido, estos eventos se generan en el equipo que hospeda el recurso al que se accede. Si esta opción se configura como **sin auditoría**, es difícil o imposible determinar que el usuario tiene acceso o ha intentado acceder a los equipos de la organización.  
  
##### <a name="network-policy-server"></a>Servidor de directivas de red  
Esta subcategoría notifica los eventos generados por las solicitudes de acceso de usuario de RADIUS (IAS) y la protección de acceso de red (NAP). Estos requisitos pueden ser **concesión**, **denegar**, **descartar**, **cuarentena**, **bloqueo**, y **desbloqueo**. Esta configuración de auditoría dará como resultado un medio o alto volumen de registros en servidores NPS y IAS.  
  
##### <a name="ipsec-main-mode"></a>Modo principal de IPsec  
Esta subcategoría informa de los resultados del protocolo de intercambio de claves (IKE) y protocolo de Internet autenticado (AuthIP) durante las negociaciones de modo principal.  
  
##### <a name="ipsec-extended-mode"></a>Modo extendido de IPsec  
Esta subcategoría informa de los resultados de AuthIP durante las negociaciones de modo extendido.  
  
##### <a name="other-logonlogoff-events"></a>Otros eventos de inicio/cierre de sesión  
Esta subcategoría notifica otro inicio de sesión y los eventos relacionados con el cierre de sesión, como los servicios de escritorio remoto sesión se desconecta y vuelve a conectar, mediante RunAs ejecutar procesos con una cuenta diferente y bloquear y desbloquear una estación de trabajo.  
  
##### <a name="logoff"></a>Cierre de sesión  
Esta subcategoría se notifica cuando un usuario cierra el sistema. Estos eventos se producen en el equipo al que se accede. Para los inicios de sesión interactivo, la generación de estos eventos se produce en el equipo que se ha iniciado sesión. Si un inicio de sesión de red tiene lugar para obtener acceso a un recurso compartido, estos eventos se generan en el equipo que hospeda el recurso al que se accede. Si esta opción se configura como **sin auditoría**, es difícil o imposible determinar que el usuario tiene acceso o ha intentado acceder a los equipos de la organización.  
  
##### <a name="account-lockout"></a>Bloqueo de cuenta  
Esta subcategoría se notifica cuando se bloquea una cuenta de usuario como resultado de demasiados intentos de inicio de sesión erróneos.  
  
##### <a name="ipsec-quick-mode"></a>Modo rápido de IPsec  
Esta subcategoría informa de los resultados del protocolo de IKE y AuthIP durante las negociaciones de modo rápido.  
  
##### <a name="special-logon"></a>Inicio de sesión especial  
Esta subcategoría se notifica cuando se usa un inicio de sesión especial. Un inicio de sesión especial es un inicio de sesión que tenga el equivalente privilegios de administrador y puede usarse para elevar un proceso a un nivel superior.  
  
#### <a name="policy-change"></a>Cambio de directiva  
  
##### <a name="audit-policy-change"></a>Cambio de directiva de auditoría  
Esta subcategoría notifica cambios en la directiva de auditoría, incluidos los cambios de la SACL.  
  
##### <a name="authentication-policy-change"></a>Cambio de directiva de autenticación  
Esta subcategoría notifica cambios en la directiva de autenticación.  
  
##### <a name="authorization-policy-change"></a>Cambio de directiva de autorización  
Esta subcategoría notifica cambios en la directiva de autorización, incluidos los cambios de permisos (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Cambio de directiva de nivel de reglas MPSSVC  
Esta subcategoría notifica cambios en las reglas de directiva de la (MPSSVC.exe) servicio de protección de Microsoft. Usa este servicio Firewall de Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Cambio de directiva de plataforma de filtrado  
Esta subcategoría notifica la adición o eliminación de objetos desde WFP, incluidos los filtros de inicio. Estos eventos pueden ser muy altos por volumen.  
  
##### <a name="other-policy-change-events"></a>Otros eventos de cambio de directiva  
Esta subcategoría informa de otros tipos de cambios de directiva de seguridad como la configuración del módulo de plataforma segura (TPM) o proveedores de servicios criptográficos.  
  
#### <a name="privilege-use"></a>Uso de privilegios  
  
##### <a name="sensitive-privilege-use"></a>Uso de privilegios confidenciales  
Esta subcategoría se notifica cuando una cuenta de usuario o servicio usa un privilegio sensible. Un privilegio sensible incluye los siguientes derechos de usuario: actuar como parte del sistema operativo, copia archivos y directorios, crear un objeto símbolo (token), depurar programas, habilitar cuentas de equipo y el usuario para que sea de confianza para delegación, generar auditorías de seguridad, suplantar a un cliente tras la autenticación, cargar y descargar controladores de dispositivo, administrar la auditoría y el registro de seguridad, modificar los valores del entorno de firmware, reemplazar un token de nivel de proceso, restaurar archivos y directorios y tomar posesión de archivos u otros objetos. Esta subcategoría de auditoría, se creará un alto volumen de eventos.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso de privilegios no confidenciales  
Esta subcategoría se notifica cuando una cuenta de usuario o servicio usa un privilegios no confidenciales. Un privilegio no confidenciales incluye los siguientes derechos de usuario: tener acceso al administrador de credenciales como un llamador de confianza, acceso a este equipo desde la red, agregar estaciones de trabajo al dominio, ajustar las cuotas de memoria para un proceso, permitir el registro local, permitir el inicio de sesión en a través de servicios de escritorio remoto, omitir la comprobación, cambiar la hora del sistema, crea un archivo de paginación, crear objetos globales, crear objetos compartidos permanentes, crear vínculos simbólicos, denegar el acceso a este equipo desde la red, Denegar inicio de sesión como trabajo por lotes, Denegar inicio de sesión como servicio, denegar el inicio de sesión localmente, Denegar inicio de sesión a través de servicios de escritorio remoto, Forzar cierre desde un sistema remoto, aumentar un conjunto de trabajo del proceso, aumentar la prioridad de programación, bloquear páginas en la memoria, iniciar sesión como trabajo por lotes, iniciar sesión como servicio, modificar la etiqueta de un objeto, realizar tareas de mantenimiento de volumen, el perfil de proceso único, analizar el rendimiento del sistema, quitar equipo de la estación de acoplamiento, apagar el sistema y sincronizar datos del servicio de directorio. Esta subcategoría de auditoría, se creará un gran volumen de eventos.  
  
##### <a name="other-privilege-use-events"></a>Otros eventos de uso de privilegios  
Esta configuración de directiva de seguridad no se utiliza actualmente.  
  
#### <a name="object-access"></a>Acceso a objetos  
  
##### <a name="file-system"></a>Sistema de archivos  
Esta subcategoría se notifica cuando se accede a objetos de sistema de archivos. Solo objetos de sistema de archivos con la SACL provocar eventos de auditoría que generado y solamente cuando se obtiene acceso a estos de manera que coincida con sus entradas de la SACL. Por sí mismo, esta configuración de directiva no hará que todos los eventos de auditoría. Determina si se debe auditar el evento de un usuario que accede a un objeto de sistema de archivos que tiene una lista de control de acceso especificada del sistema (SACL), eficazmente habilitar la auditoría para llevará a cabo.  
  
Si se configura la configuración Auditar el acceso a objetos en **éxito**, se genera una entrada de auditoría cada vez que un usuario accede con éxito a un objeto con una SACL especificada. Si esta configuración de directiva se configura en **error**, se genera una entrada de auditoría cada vez que se produce un error de un usuario en un intento de acceso a un objeto con una SACL especificada.  
  
##### <a name="registry"></a>Registro  
Esta subcategoría se notifica cuando se accede a objetos del registro. Solo los objetos del registro con la SACL provocar eventos de auditoría que generado y solamente cuando se obtiene acceso a estos de manera que coincida con sus entradas de la SACL. Por sí mismo, esta configuración de directiva no hará que todos los eventos de auditoría.  
  
##### <a name="kernel-object"></a>Objeto de kernel  
Esta subcategoría se notifica cuando se accede a los objetos de kernel como exclusiones mutuas y procesos. Solo los objetos de kernel con SACL provocar eventos de auditoría que generado y solamente cuando se obtiene acceso a estos de manera que coincida con sus entradas de la SACL. Por lo general objetos de kernel solo reciben SACL si están habilitadas las opciones de auditoría AuditBaseObjects o AuditBaseDirectories.  
  
##### <a name="sam"></a>SAM  
Esta subcategoría se notifica cuando se accede a objetos de base de datos de autenticación de administrador de cuentas de seguridad (SAM) local.  
  
##### <a name="certification-services"></a>Servicios de certificación  
Esta subcategoría se notifica cuando se realizan operaciones de servicios de certificación.  
  
##### <a name="application-generated"></a>Aplicación generada  
Esta subcategoría se notifica cuando las aplicaciones intentan generar eventos de auditoría mediante la auditoría de interfaces de programación de aplicaciones (API) de Windows.  
  
##### <a name="handle-manipulation"></a>Manipulación de identificadores  
Esta subcategoría se notifica cuando se abre o cierra un identificador de objeto. Solo los objetos con SACL causar estos eventos generados, y solo si la operación de identificador deseada coincide con las entradas de la SACL. Solo se generan eventos de manipulación de controlador para los tipos de objeto donde se habilita la subcategoría de acceso a objeto correspondiente (por ejemplo, sistema de archivos o del registro).  
  
##### <a name="file-share"></a>Recurso compartido de archivos  
Esta subcategoría se notifica cuando se accede a un recurso compartido de archivos. Por sí mismo, esta configuración de directiva no hará que todos los eventos de auditoría. Determina si se debe auditar el evento de un usuario que accede a un objeto de recurso compartido de archivos que tiene una lista de control de acceso especificada del sistema (SACL), eficazmente habilitar la auditoría para llevará a cabo.  
  
##### <a name="filtering-platform-packet-drop"></a>Colocación de paquetes de la plataforma de filtrado  
Esta subcategoría se notifica cuando se descartan paquetes por plataforma de filtrado de Windows (WFP). Estos eventos pueden ser muy altos por volumen.  
  
##### <a name="filtering-platform-connection"></a>Conexión de plataforma de filtrado  
Esta subcategoría se notifica cuando se permite o bloquea WFP conexiones. Estos eventos pueden ser altos por volumen.  
  
##### <a name="other-object-access-events"></a>Otros eventos de acceso del objeto  
Esta subcategoría notifica otros eventos relacionados con el acceso de objetos como trabajos del programador de tareas y los objetos COM +.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Cambio de estado de seguridad  
Esta subcategoría notifica cambios en el estado de seguridad del sistema, como cuando se inicia y detiene el subsistema de seguridad.  
  
##### <a name="security-system-extension"></a>Extensión del sistema de seguridad  
Esta subcategoría notifica la carga del código de extensión como paquetes de autenticación mediante el subsistema de seguridad.  
  
##### <a name="system-integrity"></a>Integridad del sistema  
Esta subcategoría informes sobre las infracciones de la integridad del subsistema de seguridad.  
  
Controlador IPsec  
  
Esta subcategoría informes sobre las actividades del controlador de protocolo de Internet (IPsec) de seguridad.  
  
##### <a name="other-system-events"></a>Otros eventos del sistema  
Esta subcategoría informes sobre otros eventos del sistema.  
  
Para obtener más información sobre las descripciones de la subcategoría, consulte la [herramienta de Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Cada organización debe revisar las categorías cubiertas anteriores y subcategorías y habilitar las que mejor se ajusten a su entorno. Siempre se deberán probar los cambios en la directiva de auditoría antes de la implementación en un entorno de producción.  
  
### <a name="configuring-windows-audit-policy"></a>Configurar directiva de auditoría de Windows  
Directiva de auditoría de Windows se puede establecer con ediciones de registro, API, auditpol.exe o las directivas de grupo. Los métodos recomendados para la configuración de directiva de auditoría para la mayoría de las empresas son directivas de grupo o auditpol.exe. La configuración de directiva de auditoría del sistema requiere permisos de cuenta de administrador o los permisos adecuados delegados.  
  
> [!NOTE]  
> La **Administrar registro de seguridad y auditoría** deben tener privilegios a entidades de seguridad (los administradores tienen de manera predeterminada) para permitir la modificación de opciones de recursos individuales como archivos, objetos de Active Directory y claves del registro de auditoría de acceso a objetos.  
  
#### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Directiva de auditoría de Windows de configuración mediante la directiva de grupo  
Para configurar la directiva de auditoría en directivas de grupo, configurar las categorías de auditoría adecuado ubicadas debajo **equipo\Configuración de Windows\Configuración de seguridad\Directivas Locales\directiva** (consulta la siguiente captura de pantalla para ver un ejemplo en el Editor de directivas de grupo Local de (gpedit.msc)). Cada categoría de directiva de auditoría puede habilitarse para **éxito**, **error**, o **éxito** y eventos de error.  
  
![Supervisión de anuncios](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Directiva de auditoría avanzada puede establecerse mediante Active Directory o las directivas de grupo local. Para establecer la directiva de auditoría avanzada, configurar las subcategorías apropiadas ubicadas debajo **equipo\Configuración de Windows\Configuración de seguridad\configuración directiva** (consulta la siguiente captura de pantalla para ver un ejemplo en el Editor de directivas de grupo Local de (gpedit.msc)). Cada subcategoría de la directiva de auditoría puede habilitarse para **éxito**, **error**, o **éxito** y **error** eventos.  
  
![Supervisión de anuncios](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
#### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Directiva de auditoría de Windows de configuración con Auditpol.exe  
Auditpol.exe (para la configuración de directiva de auditoría de Windows) se habilitó en Windows Server 2008 y Windows Vista. Inicialmente, solo auditpol.exe puede usarse para establecer la directiva de auditoría avanzada, pero la directiva de grupo puede usarse en Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008, Windows 8 y Windows 7.  
  
Auditpol.exe es una utilidad de línea de comandos. La sintaxis es la siguiente manera:  
  
**Auditpol//set / < categoría | Subcategoría >:<audit category> / < éxito | error: > / < habilitar | deshabilitar >**  
  
Ejemplos de sintaxis Auditpol.exe:  
  
**Auditpol//set /subcategory: "administración de cuentas de usuario" /success: habilitar /failure: habilitar**  
  
**Auditpol//set /subcategory: "inicio de sesión" /success: habilitar /failure: habilitar**  
  
**Auditpol//set /subcategory: "Modo principal de IPSEC" /failure: habilitar**  
  
> [!NOTE]  
> Auditpol.exe establece Directiva de auditoría avanzada localmente. Si la directiva local en conflicto con la directiva de grupo local o de Active Directory, configuración de directiva de grupo se impondrá normalmente sobre configuración auditpol.exe. Cuando existen múltiples grupo o los conflictos de directiva local, se impondrá solo una directiva (que es, reemplazar). No se combinará las directivas de auditoría.  
  
#### <a name="scripting-auditpol"></a>Secuencias de comandos Auditpol  
Microsoft proporciona un [script de muestra](https://support.microsoft.com/kb/921469) para los administradores que deseen para establecer la directiva de auditoría avanzada mediante un script en lugar de escribir manualmente en cada comando auditpol.exe.  
  
**Ten en cuenta** directiva de grupo no siempre con precisión informa el estado de todas las directivas de auditoría habilitados, mientras que cambia auditpol.exe. Consulta [obtener la directiva de auditoría eficaz en Windows 7 y Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obtener más detalles.  
  
#### <a name="other-auditpol-commands"></a>Otros comandos Auditpol  
Auditpol.exe puede usarse para guardar y restaurar una directiva de auditoría local y para ver otros comandos relacionados auditoría. Estos son los demás **auditpol** comandos.  
  
**Auditpol/borrar** - se usa para borrar y restablecer las directivas de auditoría local  
  
**Auditpol /backup /file:<filename>** - se usa para la copia de seguridad de una directiva de auditoría local en un archivo binario  
  
**Auditpol /restore /file:<filename>** - se usa para importar un archivo de directiva de auditoría guardada previamente para una directiva de auditoría local  
  
**Auditpol / < obtener o establecer > /option:<CrashOnAuditFail> / < habilitar o deshabilitar >** -si se habilita esta configuración de directiva de auditoría, hace que el sistema detener inmediatamente (con STOP: C0000244 {Error de auditoría} mensaje) si no se puede registrar una auditoría de seguridad por cualquier motivo. Por lo general, no se registran cuando el registro de auditoría de seguridad está completo y el método de retención especificado para el registro de seguridad es un evento **no sobrescribir eventos** o **sobrescribir eventos por días**. Por lo general solo está habilitada en entornos que necesitan seguridad superior que se está iniciando el registro de seguridad. Si habilita esta opción, los administradores deben ver el tamaño del registro de seguridad estrechamente y girar registros según sea necesario. También se puede establecer con la directiva de grupo mediante la modificación de la opción de seguridad **auditoría: apagar el sistema de inmediato si no se pueden registrar auditorías de seguridad** (predeterminado = deshabilitado).  
  
**Auditpol / < obtener o establecer > /option:<AuditBaseObjects> / < habilitar o deshabilitar >** -esta configuración de directiva de auditoría determina si se debe auditar el acceso de objetos globales del sistema. Si se habilita esta directiva, los objetos del sistema, como exclusiones mutuas, eventos, semáforos y dispositivos de DOS crearse con una lista de control de acceso predeterminada sistema (SACL). La mayoría de los administradores considera la posibilidad de auditar objetos globales del sistema para tener "ruido" y solo te permitirá si se sospecha piratería malintencionadas. Solo los objetos con nombre se proporcionan una SACL. Si también se habilita la auditoría objeto acceso auditoría directiva (o subcategoría de auditoría del objeto de Kernel), se audita el acceso a estos objetos del sistema. Al configurar esta configuración de seguridad, los cambios no surtirán efecto hasta que reinicie Windows. Esta directiva también se pueden establecer con la directiva de grupo mediante la modificación de la opción de seguridad Auditar el acceso de objetos globales del sistema (predeterminado = deshabilitado).  
  
**Auditpol / < obtener o establecer > /option:<AuditBaseDirectories> / < habilitar o deshabilitar >** -esta configuración de directiva de auditoría especifica que se se reciben SACL cuando se crean objetos de kernel con nombre (como exclusiones mutuas y semáforos). AuditBaseDirectories afecta a los objetos de contenedor mientras AuditBaseObjects afecta a los objetos que no pueden contener otros objetos.  
  
**Auditpol / < obtener o establecer > /option:<FullPrivilegeAuditing> / < habilitar o deshabilitar >** -  
  
Esta configuración de directiva de auditoría especifica si el cliente genera un evento cuando uno o varios de estos privilegios se asignan a un token de seguridad de usuario: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege y TcbPrivilege. Si esta opción no está habilitada (predeterminado = Disabled), no se registran los privilegios BackupPrivilege y RestorePrivilege. Al habilitar esta opción puede hacer que el registro de seguridad muy llamativas (a veces cientos de eventos una segunda) durante una operación de copia de seguridad. Esta directiva también se pueden establecer con la directiva de grupo mediante la modificación de la opción de seguridad **auditoría: auditar el uso del privilegio de copia de seguridad y restauración**.  
  
> [!NOTE]  
> Parte de la información provista aquí se tomó de Microsoft [tipo de la opción de auditoría](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) y la herramienta Microsoft SCM.  
  
### <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Cumplimiento de auditoría avanzada o auditoría tradicional  
En Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 y Windows Vista, los administradores pueden elegir para habilitar las nueve categorías tradicionales o para usar las subcategorías. Es una selección binaria que debe realizarse en cada sistema de Windows. Las principales categorías que se puede habilitar o no puede ser la subcategoriesit.  
  
Para evitar que la directiva de categoría tradicional heredado sobrescribir subcategorías de directiva de auditoría, debes habilitar la **configuración de subcategorías de directiva de auditoría de fuerza (Windows Vista o posterior) para invalidar la configuración de categoría de directiva de auditoría** configuración de directiva se encuentra en **equipo\Configuración de Windows\Configuración de seguridad\Directivas Locales\opciones de seguridad**.  
  
Te recomendamos que las subcategorías estar habilitadas y configuradas en lugar de las principales nueve categorías. Esto requiere que una configuración de directiva de grupo habilitarse (para permitir subcategorías para reemplazar las categorías de auditoría) junto con la configuración de las subcategorías diferentes que admiten las directivas de auditoría.  
  
Subcategorías de auditoría pueden configurarse mediante el uso de varios métodos, como la directiva de grupo y el programa de línea de comandos auditpol.exe.  
  
Para obtener más información acerca de la auditoría de Windows, consulte los siguientes artículos:  
  
-   [Auditoría en Windows 7 y Windows Server 2008 R2 de seguridad avanzada](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
-   [Auditoría y cumplimiento en Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
-   [Cómo usar la directiva de grupo para configurar opciones de configuración para equipos basados en Windows Vista y Windows Server 2008 en un dominio de Windows Server 2008, en un dominio de Windows Server 2003 o en un dominio de Windows 2000 de auditoría de seguridad detallada](https://support.microsoft.com/kb/921469)  
  
-   [Avanzada de directiva de auditoría de seguridad Step-by-Step guía](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
  


