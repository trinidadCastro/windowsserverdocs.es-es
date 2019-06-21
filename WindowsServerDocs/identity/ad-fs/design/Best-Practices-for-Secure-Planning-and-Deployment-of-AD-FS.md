---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Procedimientos recomendados para planear e implementar AD FS de forma segura
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 95f9fd468df39525a2fe7d18647f399214486bbb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280597"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Procedimientos recomendados para planear e implementar AD FS de forma segura


Este tema proporciona información de procedimientos recomendados para ayudarle a planear y evaluar la seguridad al diseñar la implementación de servicios de federación de Active Directory (AD FS). En este tema es un punto de partida para revisar y valorar las consideraciones que afectan a la seguridad general del uso de AD FS. El objetivo de este tema es ampliar la planificación de seguridad existente y otros procedimientos recomendados de diseño.  
  
## <a name="core-security-best-practices-for-ad-fs"></a>Recomendaciones de seguridad principales para AD FS  
Los siguientes procedimientos recomendados principales son comunes a todas las instalaciones de AD FS donde desea mejorar o ampliar la seguridad del diseño o implementación:  

-   **Protección de AD FS como un sistema de "Nivel 0"** 

    AD FS es, básicamente, un sistema de autenticación.  Por lo tanto, deben tratarse como un sistema de "Nivel 0" como otro sistema de identidad en la red.  [Microsoft Docs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) contiene más información sobre el modelo de nivel administrativo de Active Directory. 


-   **Utilice al Asistente para configuración de seguridad para aplicar prácticas recomendadas de seguridad específicas de AD FS a servidores de federación y equipos de servidores proxy de federación**  
  
    El Asistente para configuración de seguridad (SCW) es una herramienta que viene preinstalada en todos los Windows Server 2008, Windows Server 2008 R2 y equipos de Windows Server 2012. Se puede usar para aplicar recomendaciones de seguridad que pueden ayudar a reducir la superficie expuesta a ataques de un servidor, según los roles de servidor que vaya a instalar.  
  
    Al instalar AD FS, el programa de instalación crea archivos de extensión de rol que se pueden usar con el SCW para crear una directiva de seguridad que se aplicará al rol del servidor de AD FS específico (ya sea un servidor de federación o un servidor proxy de federación) seleccionado durante la instalación.  
  
    Cada archivo de extensión de rol instalado representa el tipo de rol y de subrol para cada equipo con el que está configurado. Los siguientes archivos de extensión de rol se instalan en el directorio C:WindowsADFSScw:  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml (este archivo solo está presente si configuraste el equipo en el rol de servidor proxy de federación).  
  
    Para aplicar las extensiones de rol de AD FS en el SCW, sigue estos pasos en orden:  
  
    1.  Instala AD FS y selecciona el rol del servidor adecuado para el equipo. Para obtener más información, consulte [instalar el servicio de rol Proxy de servicio de federación](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) en la Guía de implementación de AD FS.  
  
    2.  Registra el archivo de extensión de rol correspondiente con la herramienta de línea de comandos scwcmd. Consulta la tabla siguiente para obtener más información sobre el uso de esta herramienta en el rol para el que esté configurado el equipo.  
  
    3.  Compruebe que el comando se ha completado correctamente, examine el archivo SCWRegister_log.xml, que se encuentra en el directorio WindowssecurityMsscwLogs.  
  
    Deberás completar todos estos pasos en todos los servidores de federación o equipos de servidor proxy de federación donde quieras aplicar directivas de seguridad de SCW basadas en AD FS.  
  
    En la tabla siguiente se explica cómo registrar la extensión de rol de SCW adecuada basándose en el rol del servidor de AD FS seleccionado en el equipo donde se instaló AD FS.  
  
    |Rol del servidor de AD FS|Base de datos de configuración de AD FS usada|Escribe el comando siguiente en el símbolo del sistema:|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |Servidor de federación independiente|Windows Internal Database|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |Servidor de federación unido a una granja de servidores|Windows Internal Database|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |Servidor de federación unido a una granja de servidores|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |Servidor proxy de federación|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    Para obtener más información sobre las bases de datos que se pueden usar con AD FS, consulta [Rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Use la detección de reproducción de tokens en situaciones en las que seguridad es un problema importante, por ejemplo, cuando se usa quioscos multimedia.**  
    Detección de reproducción de tokens es una característica de AD FS que garantiza que cualquier intento de responder a una solicitud de token que se realiza en el servicio de federación se detecten y se descarta la solicitud. La detección de respuesta de token está habilitada de manera predeterminada. Funciona tanto con el perfil pasivo de WS-Federation como con el perfil WebSSO de lenguaje de marcado de aserción de seguridad (SAML), ya que garantiza que el mismo token no se use más de una vez.  
  
    Al iniciar el Servicio de federación, este creará una caché de todas las solicitudes de tokens que complete. Con el tiempo, a medida que se agregan solicitudes de token a la caché, también aumentará la capacidad del Servicio de federación para detectar los intentos de responder a una solicitud de token varias veces. Si se deshabilita la detección de respuesta de token y posteriormente se vuelve a habilitar, el Servicio de federación seguirá aceptando tokens durante un período de tiempo que puede haberse usado anteriormente, hasta que la caché de repuesta disponga del tiempo suficiente para recrear su contenido. Para obtener más información, consulta [Rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
-   **Utilice el cifrado de tokens, especialmente si usa la resolución de artefactos SAML auxiliar.**  
  
    Cifrado de tokens es muy recomendable para aumentar la seguridad y protección ante posibles man-in-the-middle (MITM) ataques que podrían intentarse contra la implementación de AD FS. Usar cifrado podría afectar ligeramente al rendimiento; sin embargo, en general no llega a apreciarse y, además, en muchas implementaciones las ventajas de mejorar la seguridad superan cualquier coste en términos de rendimiento del servidor.  
  
    Para habilitar el cifrado de tokens, en primer lugar deberás agregar un certificado de cifrado para las relaciones de confianza para usuarios autenticados. Puedes configurar un certificado de cifrado al crear una relación de confianza para usuario autenticado o bien posteriormente. Para agregar un certificado de cifrado más adelante a una existente de confianza, puede establecer un certificado para su uso en el **cifrado** pestaña dentro de las propiedades de confianza al usar el complemento de AD FS. Para especificar un certificado para una confianza existente mediante los cmdlets de AD FS, use el parámetro EncryptionCertificate de los **Set-ClaimsProviderTrust** o **Set-RelyingPartyTrust** cmdlets. Para establecer un certificado para el servicio de federación que se usará al descifrar tokens, use el **Set-ADFSCertificate** cmdlet y especifique "`Token-Encryption`" para el *CertificateType* parámetro. Para habilitar y deshabilitar el cifrado para una relación de confianza para usuario autenticado específica se puede usar el parámetro *EncryptClaims* o bien el cmdlet **Set-RelyingPartyTrust**.  
  
-   **Usar protección ampliada para la autenticación**  
  
    Para ayudar a proteger sus implementaciones, puede establecer y usar la protección ampliada para la característica de autenticación con AD FS. Esta configuración especifica el nivel de protección ampliada para la autenticación admitida por un servidor de federación.  
  
    Protección ampliada para la autenticación mejora la protección contra ataques de tipo “Man in the middle” (MITM), en que un atacante intercepta las credenciales de cliente y las reenvía a un servidor. Dichos ataques pueden evitarse con un token de enlace de canal (CBT), el cual puede ser requerido, permitido o no requerido por el servidor al establecer comunicaciones con clientes.  
  
    Para habilitar la característica de protección ampliada, usa el parámetro **ExtendedProtectionTokenCheck** en el cmdlet **Set-ADFSProperties**. En la tabla siguiente se describen los valores posibles para esta opción y el nivel de seguridad que proporcionan los valores.  
  
    |Valor del parámetro|Nivel Seguridad|Configuración de protección|  
    |-------------------|------------------|----------------------|  
    |Requerir|El servidor está protegido completamente.|La protección ampliada es obligatoria y siempre se requiere.|  
    |Permitir|El servidor está protegido parcialmente.|La protección ampliada es obligatoria en aquellos sistemas que han sido actualizados para admitirla.|  
    |Ninguno|El servidor es vulnerable.|La protección ampliada no es obligatoria.|  
  
-   **Si usas registro y seguimiento, garantizar la privacidad de la información confidencial.**  
  
    AD FS no, de forma predeterminada, exponer o realizar un seguimiento de información personal identificable (PII) directamente como parte del servicio de federación o las operaciones normales. Cuando se habilitan el registro de eventos y el registro de seguimiento de depuración en AD FS, sin embargo, según la directiva de notificaciones que configurar algunas notificaciones tipos y sus valores asociados contengan PII que puedan registrarse en el evento de AD FS o registros de seguimiento.  
  
    Por lo tanto, el exigir el control de acceso en la configuración de AD FS y sus archivos de registro es muy recomendable. Para que este tipo de información sea visible es necesario deshabilitar el registro, o bien filtrar la PII o información confidencial de los registros antes de compartirlos.  
  
    Las sugerencias siguientes te pueden ayudar a evitar que el contenido de un archivo de registro sea expuesto accidentalmente:  
  
    -   Asegúrese de que el registro de eventos de AD FS y los archivos de registro de seguimiento están protegidos mediante listas de control de acceso (ACL) que limitan el acceso a los administradores de confianza que necesiten obtener acceso a ellos.  
  
    -   No copies o archives los archivos de registro con extensiones de archivo o rutas de acceso a las que se pueda acceder fácilmente mediante una solicitud web. Por ejemplo, la extensión de nombre de archivo .xml no es una opción segura. Consulta la lista de extensiones que pueden proporcionarse en un servidor web en la guía de administración de Internet Information Services (IIS).  
  
    -   Si revisas la ruta de acceso al archivo de registro, asegúrate de especificar una ruta de acceso absoluta para la ubicación del archivo de registro, que debería encontrarse fuera del directorio público de la raíz virtual (vroot) del hospedaje web para evitar que terceros externos puedan acceder mediante un explorador web.  

-   **Protección de bloqueo inteligente de bloqueo temporal de AD FS Extranet y de Extranet de AD FS**  
    
    En el caso de un ataque en el formulario de solicitudes de autenticación con contraseñas invalid(bad) que vienen a través del Proxy de aplicación Web, bloqueo de extranet de AD FS permite proteger a los usuarios de un bloqueo de cuenta de AD FS. Además de proteger a los usuarios de AD FS de bloqueo de cuentas, bloqueo de extranet de AD FS también protege contra los ataques de averiguación de contraseña por fuerza bruta.  
    
    Bloqueo temporal de la Extranet de AD FS en Windows Server 2012 R2 encontrarás [AD FS Extranet suave protección de bloqueo](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md).  

     Para el bloqueo inteligente Extranet para AD FS en Windows Server 2016 vea [AD FS Extranet inteligente protección de bloqueo](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Recomendaciones de seguridad específicas de SQL Server para AD FS  
Las siguientes prácticas recomendadas de seguridad son específicas para el uso de Microsoft SQL Server® o Windows Internal Database (WID) cuando se usan estas tecnologías de base de datos para administrar datos en la implementación y diseño de AD FS.  
  
> [!NOTE]  
> Estas recomendaciones tienen como objetivo ampliar (no sustituir) la guía de seguridad del producto de SQL Server. Para obtener más información acerca de cómo planear una instalación de SQL Server, vea [consideraciones de seguridad para una instalación de SQL](https://go.microsoft.com/fwlink/?LinkID=139831) (https://go.microsoft.com/fwlink/?LinkID=139831).  
  
-   **Siempre puede implementar SQL Server detrás de un firewall en un entorno de red físicamente segura.**  
  
    Las instalaciones de SQL Server nunca deben estar expuestas directamente a internet. Solo los equipos que están dentro de su centro de datos deben ser capaces de conectarse a la instalación de SQL server que es compatible con AD FS. Para obtener más información, consulte [lista de comprobación de mejores prácticas de seguridad](https://go.microsoft.com/fwlink/?LinkID=189229) (https://go.microsoft.com/fwlink/?LinkID=189229).  
  
-   **Ejecute SQL Server en una cuenta de servicio en lugar de usar las cuentas de servicio predeterminada integrada del sistema.**  
  
    De manera predeterminada, SQL Server suele instalarse y configurarse para usar una de las cuentas del sistema predefinidas, como LocalSystem o NetworkService. Para mejorar la seguridad de la instalación de SQL Server para AD FS, siempre que sea posible usar otra cuenta de servicio de acceso al servicio de SQL Server y habilitar la autenticación Kerberos al registrar el nombre principal de seguridad (SPN) de esta cuenta en su Implementación de Active Directory. Esto permite la autenticación mutua entre cliente y servidor. Si no se registra el SPN de una cuenta de servicio separada, SQL Server usará NTLM para autenticación basada en Windows, donde solo se autentica el cliente.  
  
-   **Minimizar el área expuesta de SQL Server.**  
  
    Habilita únicamente los extremos de SQL Server que sean necesarios. De manera predeterminada, SQL Server proporciona un extremo TCP predefinido que no se puede eliminar. AD FS, debe habilitar este extremo TCP para la autenticación Kerberos. Para revisar los extremos TCP actuales y comprobar si se han agregado puertos TCP definidos por el usuario adicionales a una instalación de SQL, usa la instrucción de consulta “SELECT * FROM sys.tcp_endpoints” en una sesión de Transact-SQL (T-SQL). Para obtener más información acerca de la configuración de punto de conexión de SQL Server, vea [How To: Configurar el motor de base de datos para escuchar en varios puertos TCP](https://go.microsoft.com/fwlink/?LinkID=189231) (https://go.microsoft.com/fwlink/?LinkID=189231).  
  
-   **Evite utilizar la autenticación basada en SQL.**  
  
    Para evitar la transferencia de contraseñas como texto no cifrado por la red o almacenar contraseñas en opciones de configuración, usa únicamente la autenticación de Windows en la instalación de SQL Server. La autenticación de SQL Server es un modo de autenticación heredado. No se recomienda almacenar credenciales de inicio de sesión de lenguaje de consulta estructurado (nombres de usuario y contraseñas de SQL) al usar la autenticación de SQL Server. Para obtener más información, consulte [modos de autenticación](https://go.microsoft.com/fwlink/?LinkID=189232) (https://go.microsoft.com/fwlink/?LinkID=189232).  
  
-   **Evalúe cuidadosamente la necesidad de seguridad del canal adicionales en la instalación de SQL.**  
  
    Aunque se aplique la autenticación Kerberos, la interfaz del proveedor de compatibilidad para seguridad (SSPI) de SQL Server no proporciona seguridad de nivel de canal. Pero para instalaciones en las que los servidores se encuentran en una ubicación segura en una red protegida mediante firewall, puede que no sea necesario cifrar las comunicaciones de SQL.  
  
    Aunque el cifrado es una herramienta útil que ayuda a mejorar la seguridad, no es imprescindible para todos los datos o conexiones. Al decidir si implementar o no el cifrado, ten en cuenta cómo accederán los usuarios a los datos. Si los usuarios acceden a los datos desde una red pública, puede que sea necesario cifrar los datos para mejorar la seguridad. Sin embargo, si todo el acceso de datos SQL por AD FS requiere una configuración de intranet segura, cifrado no es posible que sea necesario. Cualquier uso de cifrado también deberá incluir una estrategia de mantenimiento de contraseñas, claves y certificados.  
  
    Si existe la preocupación de que los datos SQL puedan verse o alterarse en la red, usa el protocolo de seguridad de internet (IPsec) o SSL (capa de sockets seguros) para proteger las conexiones SQL. Sin embargo, esto podría tener un impacto negativo en el rendimiento de SQL Server, lo que podría afectar o limitar el rendimiento de AD FS en algunas situaciones. Por ejemplo, el rendimiento de AD FS en la emisión de tokens puede verse afectado cuando las búsquedas de atributos de un almacén de atributos basados en SQL son imprescindibles para la emisión de tokens. La amenaza de alteración de SQL se puede eliminar con una configuración de seguridad perimetral. Por ejemplo, una solución para proteger la instalación de SQL Server es garantizar que permanezca inaccesible para usuarios y equipos de internet y que solo puedan acceder aquellos usuarios o equipos ubicados en el entorno del centro de datos.  
  
    Para obtener más información, consulte [cifrar conexiones a SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) o [cifrado de SQL Server](https://go.microsoft.com/fwlink/?LinkID=189233).  
  
-   **Configurar el acceso diseñada de forma segura mediante el uso de procedimientos almacenados para realizar búsquedas todo basado en SQL mediante datos de AD FS de SQL almacenados.**  
  
    Para mejorar el servicio y el aislamiento de datos puedes crear procedimientos almacenados para todos los comandos de búsqueda en el almacén de atributos. Puedes crear un rol de base de datos y después conceder permisos a dicho rol para ejecutar los procedimientos almacenados. Asignar la identidad de servicio del servicio de Windows de AD FS a este rol de base de datos. El servicio de Windows de AD FS no debe ser capaz de ejecutar otras instrucciones SQL, que no sean los procedimientos almacenados que se usan para la búsqueda de atributos. Bloquear el acceso a la base de datos de SQL Server de este modo reduce el riesgo de un ataque de elevación de privilegios.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
