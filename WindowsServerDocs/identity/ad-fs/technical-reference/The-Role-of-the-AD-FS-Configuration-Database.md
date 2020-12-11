---
description: 'Más información sobre: el rol de la base de datos de configuración de AD FS'
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: Papel de la base de datos de configuración de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 19b11885e28c23f789031e4b0c619cd660bd22e3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045283"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>Papel de la base de datos de configuración de AD FS

La base de datos de configuración de AD FS almacena todos los datos de configuración que representan una sola instancia de Servicios de federación de Active Directory (AD FS) \( AD FS \) \( es decir, el servicio de Federación \) . La base de datos de configuración de AD FS define el conjunto de parámetros que un servicio de federación necesita para identificar asociados, certificados, almacenes de atributos, notificaciones y diversos datos sobre estas entidades asociadas. Puede almacenar estos datos de configuración en una base de datos Microsoft SQL Server &reg; o en la característica WID de Windows Internal Database \( \) que se incluye con Windows Server &reg; 2008, Windows Server 2008 R2 y Windows Server &reg; 2012.

> [!NOTE]
> El contenido completo de la base de datos de configuración de AD FS se puede almacenar en una instancia de WID o en una instancia de la base de datos de SQL, pero no en ambas. Eso significa que no puedes hacer que unos servidores de federación usen WID y otros usen una base de datos de SQL Server para la misma instancia de la base de datos de configuración de AD FS.

Puedes usar la información de este tema además del contenido de [Consideraciones sobre la topología de implementación de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11)) para conocer las ventajas y desventajas de elegir WID o SQL Server para almacenar la base de datos de configuración de AD FS:

WID usa un almacén de datos relacional y no tiene su propia interfaz de usuario de la interfaz de usuario de administración \( \) . En su lugar, los administradores pueden modificar el contenido de la base de datos de configuración de AD FS mediante el complemento \- de administración de AD FS en, Fsconfig.exe o los cmdlets de Windows PowerShell &trade; .

## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usar WID para almacenar la base de datos de configuración de AD FS
Puede crear la base de datos de configuración de AD FS mediante WID como almacén mediante la herramienta de línea de comandos Fsconfig.exe \- o el Asistente para la configuración del servidor de Federación de AD FS. Cualquiera de estas herramientas permite elegir las siguientes opciones para crear la topología del servidor de federación. Cada una de estas opciones usa WID para almacenar la base de datos de configuración de AD FS:

-   Crear un \- servidor de Federación independiente

-   Crear el primer servidor de federación en una granja de servidores de federación

-   Agregar un servidor de federación a una granja de servidores de federación

Si selecciona la opción independiente \- , WID se usa para almacenar una única instancia de la base de datos de configuración de AD FS. Esta instancia no se puede compartir entre varios servidores de federación. Está pensada solo para entornos de laboratorio de pruebas. Para obtener más información acerca de la \- opción de servidor de Federación independiente o cómo configurar una, consulte [servidor de Federación independiente con WID](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982486(v=ws.11)) o [crear un servidor de Federación de Stand-Alone](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913579(v=ws.11)).

Si seleccionas la opción del primer servidor de federación en una granja de servidores de federación, WID se configura para escalabilidad, que permite agregar más servidores de federación a la granja más adelante. Para obtener más información sobre cómo implementar una granja de servidores WID o cómo configurar una, consulta [Granja de servidores de federación con WID](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982492(v=ws.11)) o [Crear el primer servidor de federación en una granja de servidores de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807070(v=ws.11)).

Si seleccionas la opción de agregar un servidor de federación, WID se configura para replicar los cambios de la base de datos de configuración al nuevo servidor de federación a los intervalos establecidos. Para obtener más información sobre cómo agregar un servidor a una granja de servidores WID, consulta [Granja de servidores de federación con WID](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982492(v=ws.11)) o [Agregar un servidor de federación a una granja de servidores de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913575(v=ws.11)).

> [!NOTE]
> Al implementar una granja de servidores de Federación mediante WID, es posible que algunas características de AD FS no estén disponibles. Para tener acceso a todas las características para configurar la granja de servidores, considera la posibilidad de usar Microsoft SQL Server para almacenar la base de datos de configuración de AD FS. Para obtener más información, consulta [Consideraciones sobre la topología de implementación de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/gg982489(v=ws.11)).

### <a name="how-a-wid-federation-server-farm-works"></a>Cómo funciona una granja de servidores de federación WID
En esta sección se describen conceptos importantes que explican cómo la granja de servidores de federación WID replica los datos entre un servidor de federación principal y los servidores de federación secundarios. .

#### <a name="primary-federation-server"></a>Servidor de federación principal
Un servidor de Federación principal es un equipo que ejecuta Windows Server 2008, Windows Server 2008 R2 o Windows Server &reg; 2012 que se ha configurado en el rol de servidor de Federación con el Asistente para configuración de servidor de Federación de AD FS y que tiene una copia de lectura/escritura de la base de datos de configuración de AD FS. El servidor de Federación principal siempre se crea cuando se usa el Asistente para configuración de servidor de Federación de AD FS y se selecciona la opción para crear un Servicio de federación nuevo y hacer que ese equipo sea el primer servidor de Federación de la granja. Todos los demás servidores de federación de la granja, también conocidos como servidores de federación secundarios, deben sincronizar los cambios que se realicen en el servidor de federación principal con la copia de la base de datos de configuración de AD FS que está almacenada localmente.

#### <a name="secondary-federation-servers"></a>Servidores de federación secundarios
Los servidores de Federación secundarios almacenan una copia de la AD FS base de datos de configuración del servidor de Federación principal, pero estas copias son de \- solo lectura. Los servidores de federación secundarios se conectan con los datos del servidor de federación principal de la granja y sincronizan los datos sondeándolo a intervalos regulares para comprobar si los datos han cambiado. Los servidores de Federación secundarios existen para proporcionar tolerancia a errores para el servidor de Federación principal mientras se actúa para equilibrar la carga de \- las solicitudes de acceso que se realizan en sitios diferentes en todo el entorno de red.

> [!NOTE]
> Si un servidor de federación principal se bloquea y se desconecta, todos los servidores de federación secundarios continuarán procesando las solicitudes normalmente. Sin embargo, no se pueden realizar cambios el servicio de federación hasta que el servidor de federación principal se vuelva a poner en línea. También puedes designar un servidor de federación secundario como servidor de federación principal mediante Windows PowerShell. Para más información, vea [Administración de AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).

#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>Sincronización de la base de datos de configuración de AD FS
Debido al importante rol que se reproduce en la base de datos de configuración de AD FS, está disponible en todos los servidores de Federación de la red para proporcionar capacidades de tolerancia a errores y equilibrio de carga \- al procesar las solicitudes \( cuando \- se usan equilibradores de carga de red \) . Sin embargo, para que los servidores de federación secundarios cumplan esta función, la base de datos de configuración de AD FS que está almacenada en el servidor de federación principal se debe sincronizar.

Cuando se agrega un servidor de federación a la granja, el nuevo equipo que se convierte en un servidor de federación secundario se conecta al servidor de federación principal para replicar la copia de la base de datos de configuración de AD FS. A partir de este punto, el nuevo servidor de federación continúa extrayendo las actualizaciones del servidor de federación principal de forma regular, como se muestra en la ilustración siguiente.

![Roles AD FS](media/adfs2_WID.png)

Todos los servidores de federación secundarios sondean el servidor de federación principal cada cinco minutos para comprobar si hay cambios. Puede ajustar este \- valor predeterminado de cinco minutos o forzar una sincronización inmediata en cualquier momento mediante un cmdlet de Windows PowerShell. Para más información acerca de cómo hacer esto, vea [Administración de AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).

El proceso de sincronización de WID también admite transferencias incrementales para una transferencia más eficiente de los cambios intermedios. El proceso de transferencia incremental requiere mucho menos tráfico en la red y las transferencias se completan mucho más rápidamente.

> [!NOTE]
> Se permite migrar una base de datos de configuración de AD FS desde WID a una instancia de SQL Server. Para obtener más información sobre cómo hacerlo, vea [AD FS: migrar la base de datos de configuración de AD FS a SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) en el sitio wiki de TechNet.

## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Usar SQL Server para almacenar la base de datos de configuración de AD FS
Puede crear la base de datos de configuración de AD FS mediante una única instancia de base de datos de SQL Server como almacén mediante la herramienta de línea de comandos Fsconfig.exe \- . Usar una base de datos de SQL Server como base de datos de configuración de AD FS tiene algunas ventajas respecto a WID:

-   Los administradores pueden aprovechar las características de alta disponibilidad de SQL Server

-   Proporciona incrementos adicionales del rendimiento para un volumen de tráfico alto.

-   Proporciona compatibilidad con características de la resolución de artefactos de SAML y la detección de reproducción de tokens de Federación de SAML/WS que \- \( se describe a continuación \) .

El término "servidor de Federación principal" no se aplica cuando la base de datos de configuración de AD FS se almacena en una instancia de SQL Database, ya que todos los servidores de Federación pueden leer y escribir igualmente en la base de datos de configuración de AD FS que usa la misma instancia de SQL Server en clúster, tal y como se muestra en la siguiente ilustración.

![Roles AD FS](media/adfs2_SQL.png)

Puede usar SQL Server para configurar dos o más servidores para que trabajen juntos como un clúster de servidores con el fin de asegurarse de que AD FS tiene una alta disponibilidad para atender las solicitudes de cliente entrantes. La alta disponibilidad proporciona una \- arquitectura de escalado horizontal en la que puede aumentar la capacidad del servidor agregando servidores adicionales. Los puntos de concentración de errores se mitigan gracias a la conmutación por error automática del clúster.

Puede lograr alta disponibilidad mediante el equilibrio de carga de red \- y los servicios de conmutación por error que proporcionan las tecnologías de clúster de SQL. Para obtener más información sobre cómo configurar SQL Server para alta disponibilidad, vea [información general sobre soluciones de alta disponibilidad](https://go.microsoft.com/fwlink/?LinkId=179853).

### <a name="saml-artifact-resolution"></a>Resolución de artefactos SAML
Lenguaje de marcado de aserción de seguridad \( \) la resolución de artefactos de SAML es un punto de conexión basado en la parte del protocolo SAML 2,0 que describe cómo un usuario de confianza puede recuperar un token directamente de un proveedor de notificaciones. En la primera etapa del proceso de resolución, un cliente de explorador se pone en contacto con un servidor de federación de recursos y le proporciona un artefacto. En la segunda etapa, los servidores de federación de recursos envía el artefacto a la dirección URL del extremo del artefacto SAML que está hospedado en algún lugar de la organización del asociado de cuenta para resolver el mensaje del artefacto. En la etapa final, el servidor de federación de cuenta emite el token al servidor de federación en nombre del cliente de explorador.

> [!NOTE]
> Si es administrador en una organización de asociado de cuenta, asegúrese de asignar o enlazar un certificado SSL, que se encadena a un certificado raíz de un miembro del programa de certificados raíz de Windows, al sitio web pasivo de Federación en \( <ComputerName> \\ sitios Web \\ \\ AD FS \\ LS \) en todos los servidores de Federación de la cuenta de la granja. Esto es importante para impedir que los servidores de federación de recursos tengan que agregar manualmente el certificado SSL al almacén de certificados Usuarios de confianza de equipos locales o que no sean capaces de resolver el artefacto publicado en tu organización.

### <a name="samlws---federation-token-replay-detection"></a>Detección de reproducción de tokens SAML/WS-Federation
El término *reproducción de tokens* hace referencia al acto por el que un cliente de explorador en una organización del asociado de cuenta intenta enviar varias veces el mismo token que recibió de un servidor de federación de cuenta para autenticarse en un servidor de federación de recursos.  Esto se produce cuando un usuario hace clic en el botón **Atrás** de su explorador en un intento de reenviar la página de autenticación.

AD FS incluye una característica denominada *detección de reproducción de tokens* que permite detectar y descartar varias solicitudes de token que usan el mismo token. Cuando esta característica está habilitada, la detección de la reproducción de tokens protege la integridad de las solicitudes de autenticación en el \- perfil pasivo de WS Federation y en el perfil de webs de SAML asegurándose de que el mismo token nunca se usa más de una vez. Esta característica debe habilitarse también cuando la seguridad sea un aspecto muy preocupante, por ejemplo, cuando se usan quioscos.

En el ejemplo del quiosco, un usuario puede cerrar sesión en todos los sitios web y, después, un usuario malicioso puede intentar usar el historial de exploración para reenviar la página de autenticación federada que el usuario anterior cargó. Esta característica reduce esta preocupación porque almacena información adicional acerca de cada autenticación correcta realizada por una organización del asociado de cuenta con el fin de detectar posteriores reproducciones del token y evitar que los múltiples intentos de autenticación se realicen correctamente.
