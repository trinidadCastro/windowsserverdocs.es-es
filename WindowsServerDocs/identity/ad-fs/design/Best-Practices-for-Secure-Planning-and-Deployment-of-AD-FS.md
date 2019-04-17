---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: "Los procedimientos recomendados para el diseño seguro y la implementación de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ed8c36d4bec455879ffd00ad40b72fd5e90484ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Los procedimientos recomendados para el diseño seguro y la implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona información de los procedimientos recomendados que te ayudarán a planear y evaluar la seguridad al diseñar la implementación de servicios de federación de Active Directory (AD FS). Este tema es un punto de partida para revisar y evaluar las consideraciones que afectan a la seguridad general del uso de AD FS. La información de este tema está destinada a complementan y ampliar tu diseño existente de la seguridad y otras recomendaciones de diseño.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Procedimientos recomendados de seguridad principales de AD FS  
Los siguientes procedimientos recomendados de core son comunes a todas las instalaciones de AD FS donde quieres mejorar o ampliar la seguridad de tu diseño o implementación:  
  
-   **Usar al Asistente para configuración de seguridad para aplicar los procedimientos recomendados de seguridad de AD FS específicos a los servidores de federación y equipos de proxy del servidor de federación**  
  
    El Asistente de configuración de seguridad (SCW) es una herramienta que viene preinstalada en todos los Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012 equipos. Puedes usar para aplicar la seguridad de los procedimientos recomendados que pueden ayudar a reducen la superficie de ataque para un servidor, en función de los roles de servidor que se están instalando.  
  
    Al instalar AD FS, el programa de instalación crea rol archivos de extensión que se pueden usar con SCW para crear una directiva de seguridad que se aplicará a la AD FS rol de servidor específico (servidor de federación o proxy del servidor de federación) que elijas durante la instalación.  
  
    Cada archivo de la extensión de rol que está instalado representa el tipo de función y subfunción para el que cada equipo está configurado. Los siguientes archivos de la extensión de rol se instalan en el directorio C:WindowsADFSScw:  
  
    -   Farm.Xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.XML (este archivo está presente solo si has configurado el equipo en la función de proxy del servidor de federación.)  
  
    Para aplicar las extensiones de rol de AD FS de SCW, completa los siguientes pasos en orden:  
  
    1.  Instalar AD FS y elige el rol de servidor adecuado para ese equipo. Para obtener más información, consulta [instalar el servicio de rol de Proxy de servicios de federación](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) en la Guía de implementación de AD FS.  
  
    2.  Registrar el archivo de extensión apropiado rol mediante la herramienta de línea de comandos Scwcmd. Consulta la tabla siguiente para obtener más información sobre cómo usar esta herramienta en la función para que el equipo está configurado.  
  
    3.  Comprueba que el comando se ha completado correctamente examinando el archivo SCWRegister_log.xml, que se encuentra en el directorio WindowssecurityMsscwLogs.  
  
    Debes realizar todos estos pasos en cada servidor de federación o el equipo de proxy de federación server a la que quieres aplicar directivas de seguridad de AD FS basado en SCW.  
  
    La siguiente tabla explica cómo registrar el SCW rol de extensión apropiado, según el rol de servidor de AD FS que has elegido en el equipo donde instalaste AD FS.  
  
    |Rol de servidor de AD FS|Base de datos de configuración de AD FS utiliza|En un símbolo del sistema, escribe el siguiente comando:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Servidor de federación independiente|Base de datos interna de Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Servidor de federación de granja de servidores unidos a un|Base de datos interna de Windows|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Servidor de federación de granja de servidores unidos a un|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Proxy del servidor de federación|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Para obtener más información acerca de las bases de datos que se pueden usar con AD FS, consulta [el rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usa la detección de reproducción token en situaciones en que seguridad es un problema muy importante, por ejemplo, cuando se usan los quioscos multimedia.**  
    Detección de reproducción token es una característica de AD FS que garantiza que se detecta cualquier intento de solicitud de token que se realiza en el servicio de federación de reproducción y se descarta la solicitud. Detección de reproducción token está habilitada de manera predeterminada. Funciona para el perfil de federación de WS pasivo y el perfil de lenguaje de marcado de aserción de seguridad (SAML) WebSSO asegurándose de que el mismo token no se usa nunca más de una vez.  
  
    Cuando se inicia el servicio de federación, comienza crear una memoria caché de las solicitudes de token, satisface. Con el tiempo, como posteriores solicitudes de token se agregan a la memoria caché, aumenta la capacidad para detectar los intentos para reproducir una solicitud de token de varias veces para los servicios de federación. Si deshabilitar la detección de token de reproducción y más adelante decides vuelve a habilitarlo, recuerda que el servicio de federación aún aceptará tokens durante un período de tiempo que pueda haberse usado anteriormente, hasta que la caché de reproducción se ha permitido tiempo suficiente para reconstruir el su contenido. Para obtener más información, consulta [el rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Usar el cifrado de token, especialmente si estás usando la resolución de artefacto SAML auxiliar.**  
  
    Se recomienda el cifrado de tokens para aumentar la seguridad y protección contra ataques de (MITM) man-in-the-middle potenciales que pueden entregarse contra la implementación de AD FS. Mediante el uso de cifrado puede tener un ligero efecto en a lo largo, pero en general, no se deberían apreciar normalmente y en muchas implementaciones las ventajas para una mayor seguridad superen ningún costo en términos de rendimiento del servidor.  
  
    Para habilitar el cifrado de token, el primer conjunto agregar un certificado de cifrado para tus confianza confianzas de terceros. Puedes configurar un certificado de cifrado cuando se crean un confiar confianza de terceros o posterior. Para agregar un certificado de cifrado más tarde a una confianza de terceros de confianza existente, puedes establecer un certificado para su uso en el **cifrado** pestaña dentro de las propiedades de confianza mientras usa el complemento de AD FS. Para especificar un certificado para una confianza existente mediante los cmdlets de AD FS, usa el parámetro EncryptionCertificate de uno de ellos la **conjunto ClaimsProviderTrust** o **conjunto RelyingPartyTrust** cmdlets. Para establecer un certificado para el servicio de federación debe utilizar al descifrar tokens, usa el **conjunto ADFSCertificate** cmdlet y especifica "`Token-Encryption`" para la *CertificateType* parámetro. Habilitar y deshabilitar el cifrado de usuario de confianza específicas de confianza puede realizarse mediante el *EncryptClaims* parámetro de la **conjunto RelyingPartyTrust** cmdlet.  
  
-   **Usar protección extendida para la autenticación**  
  
    Para ayudar a proteger sus implementaciones, puede establecer y usar la protección extendida para la característica de autenticación con AD FS. Esta configuración especifica el nivel de protección extendida compatible con un servidor de federación de autenticación.  
  
    Protección extendida para la autenticación ayuda a proteger contra ataques de man-in-the-middle (MITM), en el que un atacante intercepta las credenciales del cliente y las reenvíe a un servidor. Protección contra estos ataques se realiza a través de un canal de enlace Token (mediante un programa Didáctico) que puede ser necesario, permitido o no requiere el servidor cuando se establece comunicaciones con los clientes.  
  
    Para habilitar la característica de protección extendida, usa el **ExtendedProtectionTokenCheck** parámetro en el **conjunto ADFSProperties** cmdlet. Posibles valores para esta configuración y el nivel de seguridad que proporcionan los valores se describen en la siguiente tabla.  
  
    |Valor del parámetro|Nivel de seguridad|Configuración de protección|  
    |-------------------|------------------|----------------------|  
    |Requerir|Server es totalmente consolidada.|Protección extendida es exigible y siempre es necesario.|  
    |Permitir|Server es reforzado parcialmente.|Se aplica protección extendida donde haya revisado admitirlo sistemas implicados.|  
    |Ninguno|Server es vulnerable.|No se aplica la protección extendida.|  
  
-   **Si estás usando el registro y seguimiento, garantizar la privacidad de cualquier información confidencial.**  
  
    AD FS no es así, de manera predeterminada, exponer o realizar un seguimiento de información personalmente identificable (PII) directamente como parte del funcionamiento normal o servicios de federación. Cuando el registro de seguimiento de depuración y registro de eventos están habilitados en AD FS, sin embargo, según la directiva de notificaciones que configures algunas notificaciones tipos y sus valores asociados pueden contener PII que podría estar registrado en el evento de AD FS o registros de seguimiento.  
  
    Por lo tanto, el cumplimiento de control de acceso en la configuración de AD FS y sus archivos de registro es muy recomendable. Si no desea que este tipo de información que estar visibles, debes deshabilitar iniciando la sesión, o filtrar cualquier PII o datos confidenciales en los registros antes de compartir con otros usuarios.  
  
    Las siguientes sugerencias pueden ayudar a impedir que se expongan accidentalmente el contenido de un archivo de registro:  
  
    -   Asegúrate de que el registro de eventos de AD FS y archivos de registro de seguimiento estén protegidos mediante listas de control de acceso (ACL) que limitan el acceso a solo los administradores de confianza que necesitan acceder a ellos.  
  
    -   No se copia o archivar archivos de registro con extensiones de archivo o rutas de acceso que se pueden servir fácilmente con una solicitud Web. Por ejemplo, la extensión de nombre de archivo .xml no es una opción segura. Puedes consultar la Guía de administración de Internet Information Services (IIS) para ver una lista de extensiones que se pueden servir.  
  
    -   Revisa la ruta de acceso al archivo de registro, asegúrate de especificar una ruta de acceso absoluta para la ubicación del archivo de registro, que debe estar fuera del directorio público de Web host raíz virtual (raíz virtual) para evitar que se accede por una parte externa con un navegador Web.  

-   **AD FS bloqueo Extranet protección**  
    
    En el caso de un ataque en el formulario de solicitudes de autenticación con contraseñas invalid(bad) que vienen a través del Proxy de aplicación Web, bloqueo de AD FS extranet permite proteger a los usuarios de un bloqueo de cuenta de AD FS. Además de proteger a los usuarios de una AD FS de bloqueo de cuentas, bloqueo de AD FS extranet también protege contra los ataques de averiguación de contraseña de fuerza bruta.  Para obtener más información, consulta [AD FS Extranet bloqueo protección](../../ad-fs/operations/Configure-AD-FS-Extranet-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>SQL Server específicos recomendaciones de seguridad de AD FS  
Los siguientes procedimientos recomendados de seguridad son específicos para el uso de Microsoft SQL Server® o Windows Internal Database (WID) cuando se usan estas tecnologías de base de datos para administrar datos de AD FS diseño e implementación.  
  
> [!NOTE]  
> Estas recomendaciones están destinadas a extender, pero no reemplazar, la orientación de seguridad del producto de SQL Server. Para obtener más información sobre cómo planificar una instalación de SQL Server segura, consulta [consideraciones de seguridad para una instalación de SQL segura](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Implementar siempre SQL Server detrás de un firewall en un entorno de red físicamente seguras.**  
  
    Una instalación de SQL Server nunca debe exponer directamente a Internet. Solo los equipos que están dentro de su centro de datos deben ser capaz de alcanzar la instalación de SQL server que es compatible con AD FS. Para obtener más información, consulta [lista de comprobación de las prácticas recomendadas de seguridad](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **SQL Server se ejecuta en una cuenta de servicio en lugar de usar las cuentas de servicio del sistema predeterminada integrada.**  
  
    De manera predeterminada, SQL Server a menudo está instalado y configurado para usar una de las cuentas compatibles integrado del sistema, como las cuentas LocalSystem o NetworkService. Para mejorar la seguridad de la instalación de SQL Server para AD FS, siempre que sea posible usar una cuenta de servicio independiente para acceder a tu servicio de SQL Server y habilitar la autenticación Kerberos al registrar el nombre principal de seguridad (SPN) de esta cuenta en la implementación de Active Directory. Esto permite la autenticación mutua entre el cliente y servidor. Sin registro SPN de una cuenta de servicios independiente, SQL Server usará autenticación NTLM para Windows, donde se autentica solo el cliente.  
  
-   **Minimizar la superficie de SQL Server.**  
  
    Habilitar los extremos de SQL Server que son necesarios. De manera predeterminada, SQL Server proporciona un único extremo TCP predefinido que no se puede quitar. AD FS, deberías permitir que este extremo TCP para la autenticación de Kerberos. Para revisar los extremos TCP actuales para ver si los puertos TCP adicionales definidos por el usuario se agregan a una instalación de SQL, puedes usar la "seleccionar * desde sys.tcp_endpoints" consulta instrucción en una sesión de T-SQL (T-SQL). Para obtener más información sobre la configuración de extremo de SQL Server, consulta [How To: configurar el motor de base de datos para escuchar en los puertos TCP varios](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evita usar la autenticación basada en SQL.**  
  
    Para evitar tener que transferir las contraseñas como texto no cifrado a través de la red o almacenar contraseñas en Opciones de configuración, usa la autenticación de Windows solo con la instalación de SQL Server. Autenticación de SQL Server es un modo de autenticación heredados. Almacenar credenciales de inicio de sesión de lenguaje de consulta estructurado (SQL) (nombres de usuario SQL y las contraseñas) cuando se usa la autenticación de SQL Server no se recomienda. Para obtener más información, consulta [modos de autenticación](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Evalúe cuidadosamente la necesidad de seguridad de canal adicionales en la instalación de SQL.**  
  
    Incluso con la autenticación Kerberos en efecto, la interfaz de proveedor de soporte técnico de seguridad de SQL Server (SSPI) no proporciona seguridad de nivel de canal. Sin embargo, para las instalaciones en el que los servidores de forma segura se encuentran en una red protegida por el firewall, cifrado de las comunicaciones de SQL puede no ser necesario.  
  
    Aunque cifrado es una herramienta valiosa para ayudar a garantizar la seguridad, no deben considerarse para todas las conexiones o datos. Al decidir si se va a implementar el cifrado, considera la posibilidad de cómo los usuarios tendrán acceso a datos. Si los usuarios acceder a datos en una red pública, el cifrado de datos puede que tengas que aumentar la seguridad. Sin embargo, si el acceso a todos los datos SQL por AD FS implica una configuración de seguridad de intranet, cifrado no podría ser necesario. Cualquier uso de cifrado también debes incluir una estrategia de mantenimiento para las contraseñas, claves y certificados.  
  
    Si hay un problema que los datos SQL podrían verse o alterado en su red, usan la seguridad de protocolo de Internet (IPsec) o capa de Sockets seguros (SSL) para ayudar a proteger las conexiones de SQL. Sin embargo, esto podría tener un efecto negativo en el rendimiento de SQL Server, lo que podría afectar o reducir el rendimiento de AD FS en algunas situaciones. Por ejemplo, podría degradar el rendimiento de AD FS de emisión de token cuando las búsquedas de atributo de un almacén de atributo basado en SQL son críticas para la emisión de token. Mejor puede descartar un SQL alteraciones amenaza al disponer de una configuración de seguridad sólida perímetro. Por ejemplo, la mejor solución para proteger la instalación de SQL Server es asegurarse de que sea inaccesible para los usuarios de Internet y equipos y sea accesible solamente por los usuarios o equipos dentro del entorno del centro de datos.  
  
    Para obtener más información, consulta [cifrar las conexiones a SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) o [cifrado de SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurar el acceso diseñada de forma segura mediante el uso de procedimientos para realizar búsquedas basados en SQL todos los datos de AD FS de SQL almacenados.**  
  
    Para proporcionar un mejor servicio y aislamiento de datos, puedes crear procedimientos para todos los comandos de búsqueda de almacén de atributo. Puedes crear un rol de base de datos a la que, a continuación, conceda permiso para ejecutar los procedimientos almacenados. Asignar la identidad del servicio de los servicios de Windows de AD FS a este rol de base de datos. El servicio de Windows de AD FS no podrán ejecutar cualquier otra instrucción de SQL, que no sea el apropiado procedimientos que se usan para la búsqueda de atributo. Bloquear el acceso a la base de datos de SQL Server de este modo reduce el riesgo de un ataque de elevación de privilegios.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
