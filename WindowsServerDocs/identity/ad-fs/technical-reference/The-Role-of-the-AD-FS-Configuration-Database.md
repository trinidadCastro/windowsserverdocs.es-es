---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: El papel de la base de datos de configuración de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9ffdd1876e2dfbc044cebb65d7d6ef80880a64b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860168"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>El papel de la base de datos de configuración de AD FS
La base de datos de configuración de AD FS almacena todos los datos de configuración que representan una sola instancia de Servicios de federación de Active Directory (AD FS) \(AD FS\) \(es decir, el servicio de Federación\). La base de datos de configuración de AD FS define el conjunto de parámetros que un servicio de federación necesita para identificar asociados, certificados, almacenes de atributos, notificaciones y diversos datos sobre estas entidades asociadas. Puede almacenar estos datos de configuración en una Microsoft SQL Server&reg; base de datos o en la característica Windows Internal Database \(WID\) que se incluye con Windows Server&reg; 2008, Windows Server 2008 R2 y Windows Server&reg; 2012.  
  
> [!NOTE]  
> El contenido completo de la base de datos de configuración de AD FS se puede almacenar en una instancia de WID o en una instancia de la base de datos de SQL, pero no en ambas. Eso significa que no puedes hacer que unos servidores de federación usen WID y otros usen una base de datos de SQL Server para la misma instancia de la base de datos de configuración de AD FS.  
  
Puedes usar la información de este tema además del contenido de [Consideraciones sobre la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982489.aspx) para conocer las ventajas y desventajas de elegir WID o SQL Server para almacenar la base de datos de configuración de AD FS:  
  
WID usa un almacén de datos relacional y no tiene su propia interfaz de usuario de administración \(\)de IU. En su lugar, los administradores pueden modificar el contenido de la base de datos de configuración de AD FS mediante el ajuste de administración de AD FS\-en los cmdlets de&trade;, Fsconfig. exe o Windows PowerShell.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usar WID para almacenar la base de datos de configuración de AD FS  
Puede crear la base de datos de configuración de AD FS mediante WID como almacén mediante la herramienta de línea de\-comandos de Fsconfig. exe o el Asistente para la configuración del servidor de Federación de AD FS. Cualquiera de estas herramientas permite elegir las siguientes opciones para crear la topología del servidor de federación. Cada una de estas opciones usa WID para almacenar la base de datos de configuración de AD FS:  
  
-   Crear un servidor de Federación\-independiente  
  
-   Crear el primer servidor de federación en una granja de servidores de federación  
  
-   Agregar un servidor de federación a una granja de servidores de federación  
  
Si selecciona la opción de\-independiente, WID se usa para almacenar una única instancia de la base de datos de configuración de AD FS. Esta instancia no se puede compartir entre varios servidores de federación. Está pensada solo para entornos de laboratorio de pruebas. Para obtener más información acerca de la opción de servidor de Federación independiente\-o cómo configurar una, consulte [servidor de Federación independiente con WID](https://technet.microsoft.com/library/gg982486.aspx) o [crear un servidor de Federación](https://technet.microsoft.com/library/ee913579.aspx)independiente.  
  
Si seleccionas la opción del primer servidor de federación en una granja de servidores de federación, WID se configura para escalabilidad, que permite agregar más servidores de federación a la granja más adelante. Para obtener más información sobre cómo implementar una granja de servidores WID o cómo configurar una, consulta [Granja de servidores de federación con WID](https://technet.microsoft.com/library/gg982492.aspx) o [Crear el primer servidor de federación en una granja de servidores de federación](https://technet.microsoft.com/library/dd807070.aspx).  
  
Si seleccionas la opción de agregar un servidor de federación, WID se configura para replicar los cambios de la base de datos de configuración al nuevo servidor de federación a los intervalos establecidos. Para obtener más información sobre cómo agregar un servidor a una granja de servidores WID, consulta [Granja de servidores de federación con WID](https://technet.microsoft.com/library/gg982492.aspx) o [Agregar un servidor de federación a una granja de servidores de federación](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Al implementar una granja de servidores de Federación mediante WID, es posible que algunas características de AD FS no estén disponibles. Para tener acceso a todas las características para configurar la granja de servidores, considera la posibilidad de usar Microsoft SQL Server para almacenar la base de datos de configuración de AD FS. Para obtener más información, consulta [Consideraciones sobre la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Cómo funciona una granja de servidores de federación WID  
En esta sección se describen conceptos importantes que explican cómo la granja de servidores de federación WID replica los datos entre un servidor de federación principal y los servidores de federación secundarios. .  
  
#### <a name="primary-federation-server"></a>Servidor de federación principal  
Un servidor de Federación principal es un equipo que ejecuta Windows Server 2008, Windows Server 2008 R2 o Windows Server&reg; 2012 que se ha configurado en el rol de servidor de Federación con el Asistente para configuración de servidor de Federación de AD FS y que tiene una copia de lectura/escritura de la base de datos de configuración de AD FS. El servidor de Federación principal siempre se crea cuando se usa el Asistente para configuración de servidor de Federación de AD FS y se selecciona la opción para crear un Servicio de federación nuevo y hacer que ese equipo sea el primer servidor de Federación de la granja. Todos los demás servidores de federación de la granja, también conocidos como servidores de federación secundarios, deben sincronizar los cambios que se realicen en el servidor de federación principal con la copia de la base de datos de configuración de AD FS que está almacenada localmente.  
  
#### <a name="secondary-federation-servers"></a>Servidores de federación secundarios  
Los servidores de Federación secundarios almacenan una copia de la AD FS base de datos de configuración del servidor de Federación principal, pero estas copias se leen solo\-. Los servidores de federación secundarios se conectan con los datos del servidor de federación principal de la granja y sincronizan los datos sondeándolo a intervalos regulares para comprobar si los datos han cambiado. Los servidores de Federación secundarios existen para proporcionar tolerancia a errores para el servidor de Federación principal mientras actúan para cargar\-equilibrar las solicitudes de acceso que se realizan en sitios diferentes en todo el entorno de red.  
  
> [!NOTE]  
> Si un servidor de federación principal se bloquea y se desconecta, todos los servidores de federación secundarios continuarán procesando las solicitudes normalmente. Sin embargo, no se pueden realizar cambios el servicio de federación hasta que el servidor de federación principal se vuelva a poner en línea. También puedes designar un servidor de federación secundario como servidor de federación principal mediante Windows PowerShell. Para obtener más información, consulte la [Administración de AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>Sincronización de la base de datos de configuración de AD FS  
Debido al importante rol que se reproduce en la base de datos de configuración de AD FS, está disponible en todos los servidores de Federación de la red para proporcionar tolerancia a errores y cargar capacidades de equilibrio de\-al procesar las solicitudes \(cuando se usan los equilibradores\-carga de red\). Sin embargo, para que los servidores de federación secundarios cumplan esta función, la base de datos de configuración de AD FS que está almacenada en el servidor de federación principal se debe sincronizar.  
  
Cuando se agrega un servidor de federación a la granja, el nuevo equipo que se convierte en un servidor de federación secundario se conecta al servidor de federación principal para replicar la copia de la base de datos de configuración de AD FS. A partir de este punto, el nuevo servidor de federación continúa extrayendo las actualizaciones del servidor de federación principal de forma regular, como se muestra en la ilustración siguiente.  
  
![Roles AD FS](media/adfs2_WID.png)  
  
Todos los servidores de federación secundarios sondean el servidor de federación principal cada cinco minutos para comprobar si hay cambios. Puede ajustar este valor predeterminado de cinco\-minuto o forzar una sincronización inmediata en cualquier momento mediante un cmdlet de Windows PowerShell. Para obtener más información sobre cómo hacerlo, consulte [Administración de AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
El proceso de sincronización de WID también admite transferencias incrementales para una transferencia más eficiente de los cambios intermedios. El proceso de transferencia incremental requiere mucho menos tráfico en la red y las transferencias se completan mucho más rápidamente.  
  
> [!NOTE]  
> Se permite migrar una base de datos de configuración de AD FS desde WID a una instancia de SQL Server. Para obtener más información sobre cómo hacerlo, vea [AD FS: migrar la base de datos de configuración de AD FS a SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) en el sitio wiki de TechNet.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Usar SQL Server para almacenar la base de datos de configuración de AD FS  
Puede crear la base de datos de configuración de AD FS mediante una única instancia de base de datos de SQL Server como almacén mediante la herramienta de línea de\-comandos Fsconfig. exe. Usar una base de datos de SQL Server como base de datos de configuración de AD FS tiene algunas ventajas respecto a WID:  
  
-   Los administradores pueden aprovechar las características de alta disponibilidad de SQL Server  
  
-   Proporciona incrementos adicionales del rendimiento para un volumen de tráfico alto.  
  
-   Proporciona compatibilidad con características de la resolución de artefactos de SAML y la detección de reproducción de tokens de Federación de SAML/WS\-\(se describe a continuación\).  
  
El término "servidor de Federación principal" no se aplica cuando la base de datos de configuración de AD FS se almacena en una instancia de SQL Database, ya que todos los servidores de Federación pueden leer y escribir igualmente en la base de datos de configuración de AD FS que usa la misma instancia de SQL Server en clúster, tal y como se muestra en la siguiente ilustración.  
  
![Roles AD FS](media/adfs2_SQL.png)  
  
Puede usar SQL Server para configurar dos o más servidores para que trabajen juntos como un clúster de servidores con el fin de asegurarse de que AD FS tiene una alta disponibilidad para atender las solicitudes de cliente entrantes. La alta disponibilidad proporciona una arquitectura de escala\-de salida en la que puede aumentar la capacidad del servidor agregando servidores adicionales. Los puntos de concentración de errores se mitigan gracias a la conmutación por error automática del clúster.  
  
Puede lograr alta disponibilidad mediante el equilibrio de\-de carga de red y los servicios de conmutación por error que proporcionan las tecnologías de clúster de SQL. Para obtener más información sobre cómo configurar SQL Server para alta disponibilidad, vea [información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Resolución de artefactos SAML  
Lenguaje de marcado de aserción de seguridad \(SAML\) la resolución de artefactos es un punto de conexión basado en la parte del protocolo SAML 2,0 que describe cómo un usuario de confianza puede recuperar un token directamente de un proveedor de notificaciones. En la primera etapa del proceso de resolución, un cliente de explorador se pone en contacto con un servidor de federación de recursos y le proporciona un artefacto. En la segunda etapa, los servidores de federación de recursos envía el artefacto a la dirección URL del extremo del artefacto SAML que está hospedado en algún lugar de la organización del asociado de cuenta para resolver el mensaje del artefacto. En la etapa final, el servidor de federación de cuenta emite el token al servidor de federación en nombre del cliente de explorador.  
  
> [!NOTE]  
> Si es administrador en una organización de asociado de cuenta, asegúrese de asignar o enlazar un certificado SSL, que se encadena a un certificado raíz de un miembro del programa de certificados raíz de Windows, al sitio web pasivo de Federación en IIS \(<ComputerName>sitios \\\\sitio web predeterminado\\ADFS\\LS\) en todos los servidores de Federación de la cuenta de la granja. Esto es importante para impedir que los servidores de federación de recursos tengan que agregar manualmente el certificado SSL al almacén de certificados Usuarios de confianza de equipos locales o que no sean capaces de resolver el artefacto publicado en tu organización.  
  
### <a name="samlws---federation-token-replay-detection"></a>Detección de reproducción de tokens SAML/WS-Federation  
El término *reproducción de tokens* hace referencia al acto por el que un cliente de explorador en una organización del asociado de cuenta intenta enviar varias veces el mismo token que recibió de un servidor de federación de cuenta para autenticarse en un servidor de federación de recursos.  Esta acción se produce cuando un usuario hace clic en el botón **atrás** del explorador en un esfuerzo por volver a enviar la página de autenticación.  
  
AD FS incluye una característica denominada *detección de reproducción de tokens* que permite detectar y descartar varias solicitudes de token que usan el mismo token. Cuando esta característica está habilitada, la detección de reproducción de tokens protege la integridad de las solicitudes de autenticación tanto en el perfil pasivo de Federación de WS\-como en el perfil de de SAML webs asegurándose de que el mismo token nunca se usa más de una vez. Esta característica debe habilitarse también cuando la seguridad sea un aspecto muy preocupante, por ejemplo, cuando se usan quioscos.  
  
En el ejemplo del quiosco, un usuario puede cerrar sesión en todos los sitios web y, después, un usuario malicioso puede intentar usar el historial de exploración para reenviar la página de autenticación federada que el usuario anterior cargó. Esta característica reduce este problema al almacenar información adicional acerca de cada autenticación correcta realizada por una organización del asociado de cuenta para detectar reproducciones posteriores del token e impedir que se realicen varios intentos de autenticación.  
  

