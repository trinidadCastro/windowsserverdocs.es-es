---
description: 'Más información sobre: procedimientos recomendados para la planeación e implementación seguros de AD FS'
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: Procedimientos recomendados para planear e implementar AD FS de forma segura
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e4de8bf5564277a41ee5719aba9bb54041c2dff0
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041303"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>Procedimientos recomendados para planear e implementar AD FS de forma segura


En este tema se proporciona información sobre prácticas recomendadas para ayudarle a planear y evaluar la seguridad al diseñar la implementación de Servicios de federación de Active Directory (AD FS) (AD FS). Este tema es un punto de partida para revisar y evaluar las consideraciones que afectan a la seguridad global del uso de AD FS. El objetivo de este tema es ampliar la planificación de seguridad existente y otros procedimientos recomendados de diseño.

## <a name="core-security-best-practices-for-ad-fs"></a>Recomendaciones de seguridad principales para AD FS
Los siguientes procedimientos recomendados principales son comunes para todas las instalaciones de AD FS donde desea mejorar o ampliar la seguridad del diseño o la implementación:

-   **Proteger AD FS como sistema de "nivel 0"**

    AD FS es, fundamentalmente, un sistema de autenticación.  Por lo tanto, debe tratarse como un sistema de "nivel 0" como otro sistema de identidad de la red.  [Microsoft docs](../../securing-privileged-access/securing-privileged-access-reference-material.md) dispone de más información sobre el modelo de nivel administrativo Active Directory.


-   **Usa el Asistente para configuración de seguridad para aplicar recomendaciones de seguridad específicas de AD FS a servidores de federación y equipos de servidor proxy de federación**

    El Asistente para configuración de seguridad (SCW) es una herramienta que viene preinstalada en todos los equipos con Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012. Se puede usar para aplicar recomendaciones de seguridad que pueden ayudar a reducir la superficie expuesta a ataques de un servidor, según los roles de servidor que vaya a instalar.

    Al instalar AD FS, el programa de instalación crea archivos de extensión de rol que se pueden usar con el SCW para crear una directiva de seguridad que se aplicará al rol del servidor de AD FS específico (ya sea un servidor de federación o un servidor proxy de federación) seleccionado durante la instalación.

    Cada archivo de extensión de rol instalado representa el tipo de rol y de subrol para cada equipo con el que está configurado. Los siguientes archivos de extensión de rol se instalan en el directorio C:WindowsADFSScw:

    -   Farm.xml

    -   SQLFarm.xml

    -   StandAlone.xml

    -   Proxy.xml (este archivo solo está presente si configuraste el equipo en el rol de servidor proxy de federación).

    Para aplicar las extensiones de rol de AD FS en el SCW, sigue estos pasos en orden:

    1.  Instala AD FS y selecciona el rol del servidor adecuado para el equipo. Para obtener más información, consulte [instalación del servicio de rol proxy de servicio de Federación](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md) en la guía de implementación de AD FS.

    2.  Registra el archivo de extensión de rol correspondiente con la herramienta de línea de comandos scwcmd. Consulta la tabla siguiente para obtener más información sobre el uso de esta herramienta en el rol para el que esté configurado el equipo.

    3.  Compruebe que el comando se ha completado correctamente examinando el archivo de SCWRegister_log.xml, que se encuentra en el directorio WindowssecurityMsscwLogs

    Deberás completar todos estos pasos en todos los servidores de federación o equipos de servidor proxy de federación donde quieras aplicar directivas de seguridad de SCW basadas en AD FS.

    En la tabla siguiente se explica cómo registrar la extensión de rol de SCW adecuada basándose en el rol del servidor de AD FS seleccionado en el equipo donde se instaló AD FS.

    |Rol del servidor de AD FS|Base de datos de configuración de AD FS usada|Escribe el comando siguiente en el símbolo del sistema:|
    |---------------------|-------------------------------------|---------------------------------------------------|
    |Servidor de federación independiente|Windows Internal Database|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|
    |Servidor de federación unido a una granja de servidores|Windows Internal Database|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|
    |Servidor de federación unido a una granja de servidores|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|
    |Servidor proxy de federación|N/D|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|

    Para obtener más información sobre las bases de datos que se pueden usar con AD FS, consulta [Rol de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).

-   **Use la detección de respuesta de token en situaciones en las que la seguridad es un problema importante (por ejemplo, cuando se usan quioscos multimedia).**
    La detección de reproducción de tokens es una característica de AD FS que garantiza que se detecten todos los intentos de reproducir una solicitud de token que se realice en el Servicio de federación y que se descarte la solicitud. La detección de respuesta de token está habilitada de manera predeterminada. Funciona tanto con el perfil pasivo de WS-Federation como con el perfil WebSSO de lenguaje de marcado de aserción de seguridad (SAML), ya que garantiza que el mismo token no se use más de una vez.

    Al iniciar el Servicio de federación, este creará una caché de todas las solicitudes de tokens que complete. Con el tiempo, a medida que se agregan solicitudes de token a la caché, también aumentará la capacidad del Servicio de federación para detectar los intentos de responder a una solicitud de token varias veces. Si se deshabilita la detección de respuesta de token y posteriormente se vuelve a habilitar, el Servicio de federación seguirá aceptando tokens durante un período de tiempo que puede haberse usado anteriormente, hasta que la caché de repuesta disponga del tiempo suficiente para recrear su contenido. Para más información, consulte [Rol de base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).

-   **Se recomienda usar el cifrado de tokens, especialmente si usas la resolución de artefactos de SAML.**

    Se recomienda encarecidamente el cifrado de tokens para aumentar la seguridad y la protección frente a posibles ataques de tipo "Man in the Middle" (MITM) que podrían intentarse contra la implementación de AD FS. Usar cifrado podría afectar ligeramente al rendimiento; sin embargo, en general no llega a apreciarse y, además, en muchas implementaciones las ventajas de mejorar la seguridad superan cualquier coste en términos de rendimiento del servidor.

    Para habilitar el cifrado de tokens, en primer lugar deberás agregar un certificado de cifrado para las relaciones de confianza para usuarios autenticados. Puedes configurar un certificado de cifrado al crear una relación de confianza para usuario autenticado o bien posteriormente. Para agregar un certificado de cifrado más adelante a una relación de usuario de confianza existente, puede establecer un certificado para usarlo en la pestaña **cifrado** dentro de propiedades de confianza mientras usa el complemento AD FS. Para especificar un certificado para una confianza existente mediante los cmdlets de AD FS, use el parámetro EncryptionCertificate de los cmdlets **set-ClaimsProviderTrust** o **set-RelyingPartyTrust** . Para establecer un certificado para el Servicio de federación que se usará al descifrar tokens, use el cmdlet **set-ADFSCertificate** y especifique " `Token-Encryption` " para el parámetro *CertificateType* . Para habilitar y deshabilitar el cifrado para una relación de confianza para usuario autenticado específica se puede usar el parámetro *EncryptClaims* o bien el cmdlet **Set-RelyingPartyTrust**.

-   **Usar protección ampliada para la autenticación**

    Para ayudar a proteger las implementaciones, puede establecer y usar la característica protección ampliada para la autenticación con AD FS. Esta configuración especifica el nivel de protección ampliada para la autenticación admitida por un servidor de Federación.

    Protección ampliada para la autenticación mejora la protección contra ataques de tipo “Man in the middle” (MITM), en que un atacante intercepta las credenciales de cliente y las reenvía a un servidor. Dichos ataques pueden evitarse con un token de enlace de canal (CBT), el cual puede ser requerido, permitido o no requerido por el servidor al establecer comunicaciones con clientes.

    Para habilitar la característica de protección ampliada, usa el parámetro **ExtendedProtectionTokenCheck** en el cmdlet **Set-ADFSProperties**. En la tabla siguiente se describen los valores posibles para esta opción y el nivel de seguridad que proporcionan los valores.

    |Valor del parámetro|Nivel de seguridad|Configuración de protección|
    |-------------------|------------------|----------------------|
    |Requerir|El servidor está protegido completamente.|La protección ampliada es obligatoria y siempre se requiere.|
    |Allow|El servidor está protegido parcialmente.|La protección ampliada es obligatoria en aquellos sistemas que han sido actualizados para admitirla.|
    |None|El servidor es vulnerable.|La protección ampliada no es obligatoria.|

-   **Si usas registro y seguimiento, garantiza la privacidad de la información confidencial.**

    De forma predeterminada, AD FS no expone ni realiza un seguimiento de información de identificación personal (PII) directamente como parte de las operaciones de Servicio de federación o normal. Sin embargo, cuando el registro de eventos y el registro de seguimiento de depuración están habilitados en AD FS, en función de la Directiva de notificaciones que configure algunos tipos de notificaciones y sus valores asociados podrían contener PII que podría registrarse en el evento de AD FS o en los registros de seguimiento.

    Por lo tanto, es muy recomendable exigir el control de acceso en la configuración de AD FS y sus archivos de registro. Para que este tipo de información sea visible es necesario deshabilitar el registro, o bien filtrar la PII o información confidencial de los registros antes de compartirlos.

    Las sugerencias siguientes te pueden ayudar a evitar que el contenido de un archivo de registro sea expuesto accidentalmente:

    -   Asegúrese de que el registro de eventos de AD FS y los archivos de registro de seguimiento están protegidos por listas de control de acceso (ACL) que limitan el acceso a los administradores de confianza que requieren acceso a ellos.

    -   No copies o archives los archivos de registro con extensiones de archivo o rutas de acceso a las que se pueda acceder fácilmente mediante una solicitud web. Por ejemplo, la extensión de nombre de archivo .xml no es una opción segura. Puede comprobar la guía de administración de Internet Information Services (IIS) para ver una lista de extensiones que se pueden servir.

    -   Si revisas la ruta de acceso al archivo de registro, asegúrate de especificar una ruta de acceso absoluta para la ubicación del archivo de registro, que debería encontrarse fuera del directorio público de la raíz virtual (vroot) del hospedaje web para evitar que terceros externos puedan acceder mediante un explorador web.

-   **AD FS el bloqueo flexible de extranet y la protección de bloqueo inteligente de AD FS extranet**

    En caso de que se produzca un ataque en forma de solicitudes de autenticación con contraseñas no válidas (incorrectas) que atraviesan el proxy de aplicación Web, AD FS el bloqueo de la extranet le permite proteger a los usuarios de un bloqueo de cuenta de AD FS. Además de proteger a los usuarios de un bloqueo de cuenta de AD FS, AD FS el bloqueo de extranet también protege contra los ataques por fuerza bruta de adivinación de contraseñas.

    Para el bloqueo temporal de extranet para AD FS en Windows Server 2012 R2, consulte [AD FS protección contra el bloqueo flexible de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md).

     Para el bloqueo inteligente de extranet para AD FS en Windows Server 2016, consulte [AD FS la protección de bloqueo inteligente de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md).

## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>Recomendaciones de seguridad específicas de SQL Server para AD FS
Las siguientes prácticas recomendadas de seguridad son específicas para el uso de Microsoft SQL Server &reg; o Windows Internal Database (WID) cuando se usan estas tecnologías de base de datos para administrar datos en AD FS diseño e implementación.

> [!NOTE]
> Estas recomendaciones tienen como objetivo ampliar (no sustituir) la guía de seguridad del producto de SQL Server. Para obtener más información acerca de cómo planear una instalación de SQL Server segura, vea [consideraciones de seguridad para una instalación de SQL Server](https://go.microsoft.com/fwlink/?LinkID=139831) ( https://go.microsoft.com/fwlink/?LinkID=139831) .

-   **SQL Server siempre debe implementarse en un entorno de red con seguridad física y protegido por un firewall.**

    Las instalaciones de SQL Server nunca deben estar expuestas directamente a internet. Solo los equipos que están dentro de su centro de recursos deben poder tener acceso a la instalación de SQL Server que admita AD FS. Para obtener más información, consulte [lista de comprobación de prácticas recomendadas de seguridad](https://go.microsoft.com/fwlink/?LinkID=189229) ( https://go.microsoft.com/fwlink/?LinkID=189229) .

-   **Ejecuta SQL Server con una cuenta de servicio, en lugar de usar las cuentas de servicio del sistema predefinidas.**

    De manera predeterminada, SQL Server suele instalarse y configurarse para usar una de las cuentas del sistema predefinidas, como LocalSystem o NetworkService. Para mejorar la seguridad de la instalación de SQL Server para AD FS, siempre que sea posible, use una cuenta de servicio independiente para tener acceso al servicio de SQL Server y habilite la autenticación Kerberos registrando el nombre de la entidad de seguridad (SPN) de esta cuenta en la implementación de Active Directory. Esto permite la autenticación mutua entre cliente y servidor. Si no se registra el SPN de una cuenta de servicio separada, SQL Server usará NTLM para autenticación basada en Windows, donde solo se autentica el cliente.

-   **Intenta minimizar el área expuesta de SQL Server.**

    Habilita únicamente los extremos de SQL Server que sean necesarios. De manera predeterminada, SQL Server proporciona un extremo TCP predefinido que no se puede eliminar. Por AD FS, debe habilitar este extremo TCP para la autenticación Kerberos. Para revisar los extremos TCP actuales y comprobar si se han agregado puertos TCP definidos por el usuario adicionales a una instalación de SQL, usa la instrucción de consulta “SELECT * FROM sys.tcp_endpoints” en una sesión de Transact-SQL (T-SQL). Para obtener más información acerca de la configuración del punto de conexión de SQL Server, consulte [How to: Configure the motor de base de datos para escuchar en varios puertos TCP](https://go.microsoft.com/fwlink/?LinkID=189231) ( https://go.microsoft.com/fwlink/?LinkID=189231) .

-   **No uses autenticación basada en SQL.**

    Para evitar la transferencia de contraseñas como texto no cifrado por la red o almacenar contraseñas en opciones de configuración, usa únicamente la autenticación de Windows en la instalación de SQL Server. La autenticación de SQL Server es un modo de autenticación heredado. No se recomienda almacenar credenciales de inicio de sesión de lenguaje de consulta estructurado (nombres de usuario y contraseñas de SQL) al usar la autenticación de SQL Server. Para obtener más información, vea [modos de autenticación](https://go.microsoft.com/fwlink/?LinkID=189232) ( https://go.microsoft.com/fwlink/?LinkID=189232) .

-   **Piensa si es necesario mejorar la seguridad del canal en la instalación de SQL.**

    Aunque se aplique la autenticación Kerberos, la interfaz del proveedor de compatibilidad para seguridad (SSPI) de SQL Server no proporciona seguridad de nivel de canal. Pero para instalaciones en las que los servidores se encuentran en una ubicación segura en una red protegida mediante firewall, puede que no sea necesario cifrar las comunicaciones de SQL.

    Aunque el cifrado es una valiosa herramienta para ayudar a garantizar la seguridad, no está indicado para todos los datos o conexiones. Cuando decida si debe implementar el cifrado, debe tener en cuenta el modo en que los usuarios obtendrán acceso a los datos. Si los usuarios tienen acceso a los datos a través de una red pública, podría ser necesario el cifrado de datos para aumentar la seguridad. Sin embargo, si todo el acceso a los datos de SQL por AD FS implica una configuración de intranet segura, puede que el cifrado no sea necesario. Cualquier uso del cifrado también debería incluir una estrategia de mantenimiento para las contraseñas, las claves y los certificados.

    Si existe la preocupación de que los datos SQL puedan verse o alterarse en la red, usa el protocolo de seguridad de internet (IPsec) o SSL (capa de sockets seguros) para proteger las conexiones SQL. Sin embargo, esto podría tener un efecto negativo en el rendimiento de SQL Server, lo que podría afectar o limitar AD FS rendimiento en algunas situaciones. Por ejemplo, AD FS rendimiento en la emisión de tokens podría degradarse cuando las búsquedas de atributos desde un almacén de atributos basado en SQL son críticas para la emisión de tokens. La amenaza de alteración de SQL se puede eliminar con una configuración de seguridad perimetral. Por ejemplo, una solución para proteger la instalación de SQL Server es garantizar que permanezca inaccesible para usuarios y equipos de internet y que solo puedan acceder aquellos usuarios o equipos ubicados en el entorno del centro de datos.

    Para obtener más información, vea [cifrar conexiones a SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234) o [SQL Server cifrado](https://go.microsoft.com/fwlink/?LinkID=189233).

-   **Configure el acceso diseñado de forma segura mediante procedimientos almacenados para realizar todas las búsquedas basadas en SQL AD FS de datos almacenados en SQL.**

    Para mejorar el servicio y el aislamiento de datos puedes crear procedimientos almacenados para todos los comandos de búsqueda en el almacén de atributos. Puedes crear un rol de base de datos y después conceder permisos a dicho rol para ejecutar los procedimientos almacenados. Asigne la identidad de servicio del AD FS servicio de Windows a este rol de base de datos. El AD FS servicio de Windows no debe ser capaz de ejecutar ninguna otra instrucción SQL, excepto los procedimientos almacenados adecuados que se usan para la búsqueda de atributos. Bloquear el acceso a la base de datos de SQL Server de este modo reduce el riesgo de un ataque de elevación de privilegios.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
