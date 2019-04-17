---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: "La función de la base de datos de configuración de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>La función de la base de datos de configuración de AD FS
La base de datos de configuración de AD FS almacena todos los datos de configuración que representa una sola instancia de los servicios de federación de Active Directory \(AD FS\) \(that is, the Federation Service\). La base de datos de configuración de AD FS define el conjunto de parámetros que requiere un servicio de federación para identificar asociados, certificados, almacenes de atributo, notificaciones y varios datos acerca de estas entidades asociadas. Puedes almacenar estos datos de configuración en una base de datos Microsoft SQL Server® o la característica de Windows Internal Database \(WID\) que se incluye con Windows Server® 2008, Windows Server 2008 R2 y Windows Server® 2012.  
  
> [!NOTE]  
> Puede ser todo el contenido de la base de datos de configuración de AD FS almacenados en una instancia de WID o en una instancia de la base de datos SQL, pero no ambos. Esto significa que no puede tener algunos servidores federación con WID y otros usuarios con una base de datos de SQL Server para la misma instancia de la base de datos de configuración de AD FS.  
  
Puedes usar la siguiente información en este tema junto con el contenido que se proporcionan en [consideraciones de la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982489.aspx) para obtener información sobre las ventajas y desventajas de elegir WID o SQL Server para almacenar la base de datos de configuración de AD FS:  
  
Usos WID unos datos relacionales de la tienda y no tiene su propio usuario de administración de la interfaz \(UI\). En su lugar, los administradores pueden modificar el contenido de la base de datos de configuración de AD FS mediante la administración de AD FS en snap\, Fsconfig.exe o Windows PowerShell™ cmdlets.  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>Usando WID para almacenar la base de datos de configuración de AD FS  
Puedes crear la base de datos de configuración de AD FS con WID como la tienda mediante la herramienta de línea de Web\ Fsconfig.exe o el Asistente para configuración del servidor de federación de AD FS. Al usar cualquiera de estas herramientas, puedes elegir cualquiera de las siguientes opciones para crear la topología de servidor de federación. Usa cada una de estas opciones WID para almacenar la base de datos de configuración de AD FS:  
  
-   Crear un servidor de federación stand\ solo  
  
-   Crear el primer servidor de federación en una granja de servidores de federación  
  
-   Agregar un servidor de federación a una granja de servidores de federación  
  
Si seleccionas la opción stand\ solo, WID se usa para almacenar una sola instancia de la base de datos de configuración de AD FS. Esta instancia no puede compartirse entre varios servidores de federación. Está diseñada para entornos de laboratorio de prueba solamente. Para obtener más información acerca de la opción de servidor de federación stand\ solo o cómo configurarla, consulta [independiente federación servidor usando WID](https://technet.microsoft.com/library/gg982486.aspx) o [crear un servidor de federación independiente](https://technet.microsoft.com/library/ee913579.aspx).  
  
Si seleccionas el primer servidor de federación en una opción de granja de servidor de federación, WID está configurado para escalabilidad que permitirá a los servidores de federación adicionales que se agregue a la granja de servidores en un momento posterior. Para obtener más información acerca de cómo implementar una granja de servidores WID o quieres saber cómo configurar uno, consulta [federación servidor granja usando WID](https://technet.microsoft.com/library/gg982492.aspx) o [crea el primer servidor de federación en una granja de servidores de federación](https://technet.microsoft.com/library/dd807070.aspx)  
  
Si seleccionas la opción de servidor de federación de agregar, WID está configurado para replicar los cambios de la base de datos de configuración al servidor de federación de nuevo a intervalos fijos. Para obtener más información sobre cómo agregar un servidor de federación a una granja de servidores WID, consulta [federación servidor granja usando WID](https://technet.microsoft.com/library/gg982492.aspx) o [agregar un servidor de federación a una granja de servidores de federación](https://technet.microsoft.com/library/ee913575.aspx).  
  
> [!NOTE]  
> Al implementar una granja de servidores de federación con WID, algunas características de AD FS no estén disponibles. Para tener acceso a la característica completa cuando se configura la granja de servidores, considera usar Microsoft SQL Server para almacenar la base de datos de configuración de AD FS en su lugar. Para obtener más información, consulta [consideraciones de la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx).  
  
### <a name="how-a-wid-federation-server-farm-works"></a>Cómo funciona una granja de servidores WID federación  
Esta sección describen los conceptos importantes que describen cómo la granja de servidores WID federación replica los datos entre un servidor de federación principal y los servidores de federación secundario. .  
  
#### <a name="primary-federation-server"></a>Servidor de federación principal  
Un servidor de federación principal es un equipo que ejecute Windows Server 2008, Windows Server 2008 R2 o Windows Server® 2012 que se ha configurado en el rol de servidor de federación con el Asistente para configuración del servidor de federación de AD FS y que tiene una copia de lectura y escritura de la base de datos de configuración de AD FS. El servidor de federación principal siempre se crea al usar al Asistente para configuración del servidor de federación de AD FS y selecciona la opción de crear un nuevo servicio de federación y especificar el primer servidor de federación de que el equipo de la batería. Todos los otros servidores de federación de servidores, también conocidos como los servidores de federación secundario, deben sincronizar los cambios realizados en el servidor de federación principal para una copia de la base de datos de configuración de AD FS que se almacena localmente.  
  
#### <a name="secondary-federation-servers"></a>Servidores de federación secundario  
Los servidores de federación secundario almacenan una copia de la base de datos de configuración de AD FS desde el servidor de federación principal, pero estas copias son solo read\. Los servidores de federación secundario conectan y sincronizarán los datos con el servidor de federación principal de la batería mediante el sondeo a intervalos regulares para comprobar si los datos han cambiado. Los servidores de federación secundario existen para proporcionar tolerancia a errores del servidor de federación principal actuando a load\ equilibrar las solicitudes de acceso que se realizan en otros sitios en todo el entorno de red.  
  
> [!NOTE]  
> Si un servidor de federación principal se bloquea y está sin conexión, todos los servidores de federación secundario seguirán procesar las solicitudes de forma habitual. Sin embargo, no hay nuevos cambios pueden realizarse para los servicios de federación de hasta que el servidor de federación principal se coloca en línea. También puede designar un servidor de federación secundaria para convertirse en el servidor de federación principal mediante Windows PowerShell. Para obtener más información, consulta el [AD FS administración con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
#### <a name="how-the-ad-fs-configuration-database-is-synchronized"></a>¿Cómo se sincroniza la base de datos de configuración de AD FS  
Debido a la función importante que se reproduce la base de datos de configuración de AD FS, estará disponible en todos los servidores de federación de la red para proporcionar tolerancia a errores y equilibrio load\ capacidades de procesamiento de solicitudes de \ (cuando equilibradores load\ de red son used\). Sin embargo, para que servidores de federación secundario funcionar de esta manera, la base de datos de configuración de AD FS que se almacena en el servidor de federación principal debe estar sincronizada.  
  
Cuando agregas un servidor de federación a la granja de servidores, el nuevo equipo que se convertirán en un servidor de federación secundario se conecta al servidor de federación principal para replicar la copia de la base de datos de configuración de AD FS. Desde este punto, el servidor de federación nuevo continúa extraer las actualizaciones desde el servidor de federación principal de forma regular, como se muestra en la siguiente ilustración.  
  
![Roles de AD FS](media/adfs2_WID.png)  
  
Cada servidor de federación secundario sondea el servidor de federación principal cada cinco minutos para cambios. Puedes ajustar este valor de hora five\ predeterminado o forzar una sincronización inmediata en cualquier momento mediante el uso de un cmdlet de Windows PowerShell. Para obtener más información sobre cómo hacerlo, consulta [AD FS administración con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
El proceso de sincronización WID también admite a transferencias incrementales de transferencia más eficaz de cambios intermedios. El proceso de transferencia incremental requiere bastante menos tráfico en una red, y las transferencias se completan con mayor rapidez.  
  
> [!NOTE]  
> Se admite la migración de una base de configuración AD FS WID a una instancia de SQL Server. Para obtener más información sobre cómo hacerlo, consulta [AD FS: migrar la base de datos de configuración de AD FS a SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) en el sitio de la Wiki de TechNet.  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>Mediante SQL Server que almacena la base de datos de configuración de AD FS  
Puedes crear la base de datos de configuración de AD FS con una sola instancia de base de datos de SQL Server como la tienda mediante la herramienta de línea de Web\ Fsconfig.exe. Con una base de datos de SQL Server como la base de datos de configuración de AD FS ofrece las siguientes ventajas sobre WID:  
  
-   Los administradores pueden aprovechar las características de alta disponibilidad de SQL Server  
  
-   Proporciona un aumento del rendimiento adicionales para alto volumen de tráfico.  
  
-   Proporciona compatibilidad con la característica de resolución de artefacto SAML y SAML, WS\-federación reproducción token detección \(described below\).  
  
El término "servidor de federación principal" no se aplica cuando la base de datos de configuración de AD FS se almacena en una instancia de base de datos SQL porque todos los servidores de federación igualmente pueden leer y escribir en la base de datos de configuración de AD FS que está usando la misma instancia de SQL Server agrupada, como se muestra en la siguiente ilustración.  
  
![Roles de AD FS](media/adfs2_SQL.png)  
  
Puedes usar SQL Server para configurar los servidores de dos o más para funcionar juntas como un clúster de servidores para garantizar que AD FS es altamente disponibles para las solicitudes entrantes de cliente de servicio. Alta disponibilidad proporciona una arquitectura de scale\ de salida en el que puede aumentar la capacidad del servidor al agregar servidores adicionales. Puntos de error únicos son mitigados por la conmutación por error automática del clúster.  
  
Puedes lograr alta disponibilidad mediante el equilibrio load\ de la red y servicios de conmutación por error que proporcionan tecnologías de clúster de SQL. Para obtener más información sobre cómo configurar SQL Server para una alta disponibilidad, consulta [alta disponibilidad Solutions Overview](https://go.microsoft.com/fwlink/?LinkId=179853).  
  
### <a name="saml-artifact-resolution"></a>Resolución de artefacto SAML  
Resolución de artefacto de lenguaje de marcado de aserción \(SAML\) de seguridad es un extremo según por parte del protocolo de SAML 2.0 que se describe cómo un usuario de confianza puede recuperar un token directamente desde un proveedor de notificaciones. En la primera etapa del proceso de resolución, un cliente de explorador pone en contacto con un servidor de federación de recursos y proporciona un artefacto. En la segunda fase, servidores de federación de recursos envían el artefacto a una URL de extremo de artefacto SAML que está hospedada en algún lugar en una organización de partner de la cuenta para resolver el mensaje artefacto. En la fase final, el servidor de federación de cuenta emite el token al servidor de federación en nombre del cliente de explorador.  
  
> [!NOTE]  
> Si eres un administrador de una organización de partner de cuenta, asegúrate de que asignar o enlazar un certificado SSL, que se encadene a un certificado raíz de un miembro del programa de certificados raíz de Windows, en el sitio Web de federación pasivo en IIS \ (<ComputerName>\\Sites\\Default Web Site\\adfs\\ls\) en todos los servidores de federación de cuenta de la batería. Esto es importante para evitar que los servidores de federación de recursos de tener que agregar manualmente el certificado SSL en el almacén de certificados de personas de confianza de los equipos locales o de que no se puede resolver el artefacto que se publica en la organización.  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS - token federación la detección de reproducción  
El término *reproducción token* hace referencia a la acción que intenta enviar el mismo token recibido desde un servidor de federación de cuentas de varias veces para autenticar a un servidor de federación de recursos un explorador del cliente en una organización de partner de la cuenta.  Esta acción se produce cuando un usuario hace clic en el **Atrás** botón de su explorador en un esfuerzo para volver a enviar la página de autenticación.  
  
AD FS ofrece una característica denominada *token detección de reproducción* por qué varias solicitudes de tokens usando el mismo token se puede detectar y, a continuación, se descarta. Cuando esta característica está habilitada, la detección de reproducción token protege la integridad de las solicitudes de autenticación en el perfil de federación de WS\ pasivo y el perfil de SAML WebSSO asegurándose de que el mismo token no se usa nunca más de una vez. Esta característica debe estar habilitada en situaciones donde la seguridad es una preocupación muy alto, como cuando se usan los quioscos multimedia.  
  
En el ejemplo de pantalla completa, un usuario puede iniciar sesión en todos los sitios Web y más adelante un usuario malintencionado puede intentar usar el historial del explorador para volver a enviar la página de autenticación federados que se cargó por el usuario anterior. Esta función mitiga este problema al almacenar la información adicional sobre cada autenticación correcta realizada por una organización de partner de cuenta para detectar reproducciones posteriores del token y evitar que varios intentos de autenticación tengan éxito.  
  

