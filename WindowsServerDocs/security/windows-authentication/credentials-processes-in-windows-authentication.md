---
title: Procesos de las credenciales en la autenticación de Windows
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 3d2948c632697e6278b716784f68c7a0085eab07
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640081"
---
# <a name="credentials-processes-in-windows-authentication"></a>Procesos de las credenciales en la autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de referencia para profesionales de TI se describe cómo la autenticación de Windows procesa las credenciales.

La administración de credenciales de Windows es el proceso por el que el sistema operativo recibe las credenciales del servicio o usuario, y protege esa información para su futura presentación en el destino de autenticación. En el caso de un equipo unido a un dominio, el destino de autenticación es el controlador de dominio. Las credenciales usadas en la autenticación son documentos digitales que asocian la identidad del usuario a algún tipo de prueba de autenticidad, como un certificado, una contraseña o un PIN.

De forma predeterminada, las credenciales de Windows se validan en la base de datos del administrador de cuentas de seguridad (SAM) en el equipo local o en Active Directory en un equipo unido a un dominio, a través del servicio Winlogon. Las credenciales se recopilan a través de los datos proporcionados por el usuario en la interfaz de usuario de inicio de sesión o mediante programación a través de la interfaz de programación de aplicaciones (API) para presentarse al destino de autenticación.

La información de seguridad local se almacena en el registro en **HKEY_LOCAL_MACHINE \Security**. La información almacenada incluye la configuración de Directiva, los valores de seguridad predeterminados y la información de la cuenta, como las credenciales de inicio de sesión en caché. Aquí también se almacena una copia de la base de datos de SAM, aunque está protegida contra escritura.

En el diagrama siguiente se muestran los componentes necesarios y las rutas de acceso que las credenciales toman a través del sistema para autenticar el usuario o el proceso para un inicio de sesión correcto.

![Diagrama que muestra los componentes necesarios y las rutas de acceso que las credenciales toman a través del sistema para autenticar el usuario o el proceso para un inicio de sesión correcto.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

En la tabla siguiente se describen los componentes que administran las credenciales en el proceso de autenticación en el momento del inicio de sesión.

**Componentes de autenticación para todos los sistemas**

|Componente|Descripción|
|-------|--------|
|Inicio de sesión del usuario|Winlogon.exe es el archivo ejecutable responsable de la administración de interacciones de usuario seguras. El servicio Winlogon inicia el proceso de inicio de sesión para los sistemas operativos Windows pasando las credenciales recopiladas por la acción del usuario en el escritorio seguro (IU de inicio de sesión) a la autoridad de seguridad local (LSA) a través de Secur32.dll.|
|Inicio de sesión de aplicación|Inicios de sesión de aplicaciones o servicios que no requieren inicio de sesión interactivo. La mayoría de los procesos iniciados por el usuario se ejecutan en modo de usuario mediante el uso de Secur32.dll mientras que los procesos iniciados en el inicio, como los servicios, se ejecutan en modo kernel mediante Ksecdd.sys.<p>Para obtener más información sobre el modo de usuario y el modo kernel, consulte aplicaciones y modo de usuario o servicios y modo kernel en este tema.|
|Secur32.dll|Los proveedores de autenticación múltiple que forman la base del proceso de autenticación.|
|Lsasrv.dll|El servicio LSA Server, que aplica las directivas de seguridad y actúa como el administrador de paquetes de seguridad de la LSA. La LSA contiene la función Negotiate, que selecciona el protocolo NTLM o Kerberos después de determinar qué protocolo debe ser correcto.|
|Proveedores de compatibilidad para seguridad|Un conjunto de proveedores que pueden invocar individualmente uno o más protocolos de autenticación. El conjunto predeterminado de proveedores puede cambiar con cada versión del sistema operativo Windows y se pueden escribir proveedores personalizados.|
|Netlogon.dll|Los servicios que realiza el servicio Inicio de sesión de red son los siguientes:<p>-Mantiene el canal seguro del equipo (no debe confundirse con Schannel) con un controlador de dominio.<br />: Pasa las credenciales del usuario a través de un canal seguro al controlador de dominio y devuelve los identificadores de seguridad (SID) del dominio y los derechos de usuario para el usuario.<br />-Publica registros de recursos de servicio en el sistema de nombres de dominio (DNS) y usa DNS para resolver nombres en las direcciones de protocolo de Internet (IP) de los controladores de dominio.<br />-Implementa el protocolo de replicación basado en llamada a procedimiento remoto (RPC) para sincronizar los controladores de dominio principal (PDC) y los controladores de dominio de reserva (BDC).|
|Samsrv.dll|El administrador de cuentas de seguridad (SAM), que almacena las cuentas de seguridad locales, aplica las directivas almacenadas localmente y admite las API.|
|Registro|El registro contiene una copia de la base de datos SAM, la configuración de la Directiva de seguridad local, los valores de seguridad predeterminados y la información de la cuenta a la que solo puede tener acceso el sistema.|

Este tema contiene las siguientes secciones:

-   [Entrada de credenciales para el inicio de sesión de usuario](#BKMK_CrentialInputForUserLogon)

-   [Entrada de credenciales para el inicio de sesión de aplicación y servicio](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autoridad de seguridad local](#BKMK_LSA)

-   [Credenciales y validación almacenadas en caché](#BKMK_CachedCredentialsAndValidation)

-   [Almacenamiento y validación de credenciales](#BKMK_CredentialStorageAndValidation)

-   [Base de datos Administrador de cuentas de seguridad](#BKMK_SAM)

-   [Dominios locales y dominios de confianza](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificados en la autenticación de Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="credential-input-for-user-logon"></a><a name="BKMK_CrentialInputForUserLogon"></a>Entrada de credenciales para el inicio de sesión de usuario
En Windows Server 2008 y Windows Vista, la arquitectura de identificación y autenticación gráfica (GINA) se ha reemplazado por un modelo de proveedor de credenciales, que hizo posible enumerar distintos tipos de inicio de sesión mediante el uso de iconos de inicio de sesión. Ambos modelos se describen a continuación.

**Arquitectura de identificación y autenticación gráfica**

La arquitectura de identificación y autenticación gráfica (GINA) se aplica a los sistemas operativos Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Windows 2000 Professional. En estos sistemas, cada sesión de inicio de sesión interactiva crea una instancia independiente del servicio Winlogon. La arquitectura de GINA se carga en el espacio de proceso que usa Winlogon, recibe y procesa las credenciales y realiza las llamadas a las interfaces de autenticación a través de LSALogonUser.

Las instancias de Winlogon para un inicio de sesión interactivo se ejecutan en la sesión 0. La sesión 0 hospeda los servicios del sistema y otros procesos críticos, incluido el proceso de la autoridad de seguridad local (LSA).

En el diagrama siguiente se muestra el proceso de credenciales para Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Microsoft Windows 2000 Professional.

![Diagrama que muestra el proceso de credenciales para Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Arquitectura del proveedor de credenciales**

La arquitectura del proveedor de credenciales se aplica a las versiones designadas en la lista **se aplica a que se** encuentra al principio de este tema. En estos sistemas, la arquitectura de entrada de credenciales ha cambiado a un diseño extensible mediante el uso de proveedores de credenciales. Estos proveedores se representan mediante los distintos iconos de inicio de sesión en el escritorio seguro que permiten cualquier número de escenarios de inicio de sesión: cuentas diferentes para el mismo usuario y métodos de autenticación diferentes, como la contraseña, la tarjeta inteligente y la biometría.

Con la arquitectura del proveedor de credenciales, Winlogon siempre inicia la interfaz de usuario de inicio de sesión después de recibir un evento de secuencia de atención segura. La interfaz de usuario de inicio de sesión consulta cada proveedor de credenciales para el número de tipos de credenciales diferentes que el proveedor está configurado para enumerar. Los proveedores de credenciales tienen la opción de especificar uno de estos mosaicos como valor predeterminado. Una vez que todos los proveedores han enumerado sus iconos, la interfaz de usuario de inicio de sesión los muestra al usuario. El usuario interactúa con un icono para proporcionar sus credenciales. La interfaz de usuario de inicio de sesión envía estas credenciales para la autenticación.

Los proveedores de credenciales no son mecanismos de cumplimiento. Se usan para recopilar y serializar las credenciales. La autoridad de seguridad local y los paquetes de autenticación imponen seguridad.

Los proveedores de credenciales se registran en el equipo y son responsables de lo siguiente:

-   Que describe la información de credenciales necesaria para la autenticación.

-   Control de la comunicación y la lógica con entidades de autenticación externas.

-   Empaquetando credenciales para el inicio de sesión interactivo y de red.

El empaquetado de credenciales para el inicio de sesión interactivo y de red incluye el proceso de serialización. Al serializar las credenciales, se pueden mostrar varios iconos de inicio de sesión en la interfaz de usuario de inicio de sesión. Por lo tanto, su organización puede controlar la presentación de inicio de sesión, como usuarios, sistemas de destino para el inicio de sesión, acceso previo al inicio de sesión a las directivas de bloqueo/desbloqueo de la red y estación de trabajo mediante el uso de proveedores de credenciales personalizados. Varios proveedores de credenciales pueden coexistir en el mismo equipo.

Los proveedores de inicio de sesión único (SSO) se pueden desarrollar como un proveedor de credenciales estándar o como un proveedor de acceso previo al inicio de sesión.

Cada versión de Windows contiene un proveedor de credenciales predeterminado y un proveedor de acceso previo al inicio de sesión predeterminado (PLAP), también conocido como proveedor de SSO. El proveedor de SSO permite que los usuarios realicen una conexión a una red antes de iniciar sesión en el equipo local. Cuando se implementa este proveedor, el proveedor no enumera los iconos en la interfaz de usuario de inicio de sesión.

Un proveedor de SSO está diseñado para usarse en los escenarios siguientes:

-   **Los distintos proveedores de credenciales controlan la autenticación de red y el inicio de sesión del equipo.** Las variaciones en este escenario incluyen:

    -   Un usuario tiene la opción de conectarse a una red, como conectarse a una red privada virtual (VPN), antes de iniciar sesión en el equipo, pero no es necesario para realizar esta conexión.

    -   La autenticación de red es necesaria para recuperar la información utilizada durante la autenticación interactiva en el equipo local.

    -   Varias autenticaciones de red van seguidas de uno de los otros escenarios. Por ejemplo, un usuario se autentica en un proveedor de servicios Internet (ISP), se autentica en una VPN y, a continuación, usa sus credenciales de cuenta de usuario para iniciar sesión localmente.

    -   Las credenciales almacenadas en caché están deshabilitadas y se requiere una conexión de servicios de acceso remoto a través de VPN antes del inicio de sesión local para autenticar al usuario.

    -   Un usuario de dominio no tiene una cuenta local configurada en un equipo unido a un dominio y debe establecer una conexión de servicios de acceso remoto a través de la conexión VPN antes de completar el inicio de sesión interactivo.

-   **El mismo proveedor de credenciales administra la autenticación de red y el inicio de sesión del equipo.** En este escenario, es necesario que el usuario se conecte a la red antes de iniciar sesión en el equipo.

**Enumeración de iconos Logon**

El proveedor de credenciales enumera los iconos de inicio de sesión en las instancias siguientes:

-   Para los sistemas operativos designados en la lista **se aplica a que se** encuentra al principio de este tema.

-   El proveedor de credenciales enumera los iconos del inicio de sesión de la estación de trabajo. Normalmente, el proveedor de credenciales serializa las credenciales para la autenticación en la autoridad de seguridad local. Este proceso muestra iconos específicos para cada usuario y específico para los sistemas de destino de cada usuario.

-   La arquitectura de inicio de sesión y autenticación permite al usuario usar iconos enumerados por el proveedor de credenciales para desbloquear una estación de trabajo. Normalmente, el usuario que ha iniciado sesión actualmente es el icono predeterminado, pero si más de un usuario ha iniciado sesión, se muestran numerosos iconos.

-   El proveedor de credenciales enumera los mosaicos en respuesta a una solicitud de usuario para cambiar su contraseña u otra información privada, como un PIN. Normalmente, el usuario que ha iniciado sesión actualmente es el icono predeterminado; sin embargo, si hay más de un usuario que ha iniciado sesión, se muestran numerosos iconos.

-   El proveedor de credenciales enumera los iconos según las credenciales serializadas que se van a usar para la autenticación en equipos remotos. La interfaz de usuario de credenciales no utiliza la misma instancia del proveedor que la interfaz de usuario de inicio de sesión, desbloquear la estación de trabajo o cambiar la contraseña. Por lo tanto, no se puede mantener la información de estado en el proveedor entre instancias de la interfaz de usuario de credenciales. Esta estructura da como resultado un icono para cada inicio de sesión de equipo remoto, suponiendo que las credenciales se han serializado correctamente. Este escenario también se usa en el control de cuentas de usuario (UAC), que puede ayudar a evitar cambios no autorizados en un equipo solicitando al usuario permiso o una contraseña de administrador antes de permitir acciones que podrían afectar al funcionamiento del equipo o que podrían cambiar la configuración que afecta a otros usuarios del equipo.

En el siguiente diagrama se muestra el proceso de credenciales para los sistemas operativos designados en la lista **se aplica a que se** encuentra al principio de este tema.

![Diagrama que muestra el proceso de credenciales para los sistemas operativos designados en la lista * * se aplica a * * al principio de este tema.](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="credential-input-for-application-and-service-logon"></a><a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrada de credenciales para el inicio de sesión de aplicación y servicio
La autenticación de Windows está diseñada para administrar las credenciales de las aplicaciones o servicios que no requieren la interacción del usuario. Las aplicaciones en modo usuario están limitadas en cuanto a los recursos del sistema a los que tienen acceso, mientras que los servicios pueden tener acceso sin restricciones a la memoria del sistema y a los dispositivos externos.

Los servicios del sistema y las aplicaciones de nivel de transporte tienen acceso a un proveedor de compatibilidad para seguridad (SSP) a través de la interfaz del proveedor de compatibilidad para seguridad (SSPI) de Windows, que proporciona funciones para enumerar los paquetes de seguridad disponibles en un sistema, seleccionar un paquete y usar ese paquete para obtener una conexión autenticada.

Cuando se autentica una conexión cliente/servidor:

-   La aplicación en el lado cliente de la conexión envía las credenciales al servidor mediante la función SSPI `InitializeSecurityContext (General)` .

-   La aplicación en el lado servidor de la conexión responde con la función SSPI `AcceptSecurityContext (General)` .

-   Las funciones SSPI `InitializeSecurityContext (General)` y `AcceptSecurityContext (General)` se repiten hasta que todos los mensajes de autenticación necesarios se han intercambiado para que se realice correctamente o no se supere la autenticación.

-   Una vez que se ha autenticado la conexión, la LSA del servidor usa la información del cliente para compilar el contexto de seguridad, que contiene un token de acceso.

-   Después, el servidor puede llamar a la función SSPI `ImpersonateSecurityContext` para adjuntar el token de acceso a un subproceso de suplantación para el servicio.

**Aplicaciones y modo de usuario**

El modo de usuario de Windows se compone de dos sistemas capaces de pasar las solicitudes de e/s a los controladores adecuados en modo kernel: el sistema de entorno, que ejecuta las aplicaciones escritas para muchos tipos diferentes de sistemas operativos, y el sistema entero, que opera en función de las funciones específicas del sistema en nombre del sistema de entorno.

El sistema entero administra funciones operativas de system'specific en nombre del sistema de entorno y consta de un proceso del sistema de seguridad (LSA), un servicio de estación de trabajo y un servicio de servidor. El proceso del sistema de seguridad se encarga de los tokens de seguridad, concede o deniega permisos para obtener acceso a las cuentas de usuario en función de los permisos de recursos, controla las solicitudes de inicio de sesión e inicia la autenticación de inicio de sesión y determina los recursos del sistema que el sistema operativo debe auditar.

Las aplicaciones se pueden ejecutar en modo de usuario, donde la aplicación se puede ejecutar como cualquier entidad de seguridad, incluso en el contexto de seguridad del sistema local (sistema). Las aplicaciones también se pueden ejecutar en modo kernel, donde la aplicación se puede ejecutar en el contexto de seguridad del sistema local (sistema).

SSPI está disponible a través del módulo de Secur32.dll, que es una API que se usa para obtener servicios de seguridad integrados para la autenticación, la integridad de los mensajes y la privacidad de los mensajes. Proporciona una capa de abstracción entre los protocolos de nivel de aplicación y los protocolos de seguridad. Dado que las aplicaciones diferentes requieren diferentes maneras de identificar o autenticar a los usuarios y diferentes maneras de cifrar los datos mientras viajan por la red, SSPI proporciona una manera de tener acceso a las bibliotecas de vínculos dinámicos (dll) que contienen diferentes funciones de autenticación y de cifrado. Estos archivos DLL se denominan proveedores de compatibilidad para seguridad (SSP).

Las cuentas de servicio administradas y las cuentas virtuales se introdujeron en Windows Server 2008 R2 y Windows 7 para proporcionar aplicaciones cruciales, como Microsoft SQL Server y Internet Information Services (IIS), con el aislamiento de sus propias cuentas de dominio, a la vez que eliminan la necesidad de que un administrador administre manualmente el nombre de entidad de seguridad de servicio (SPN) y las credenciales de estas cuentas. Para obtener más información sobre estas características y su rol en la autenticación, vea la [documentación sobre las cuentas de servicio administradas para Windows 7 y Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641731(v=ws.10)) y la [información general sobre las cuentas de servicio administradas de grupo](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Servicios y modo kernel**

Aunque la mayoría de las aplicaciones de Windows se ejecutan en el contexto de seguridad del usuario que las inicia, esto no se aplica a los servicios. Muchos servicios de Windows, como los servicios de impresión y red, los inicia el controlador de servicio cuando el usuario inicia el equipo. Estos servicios pueden ejecutarse como servicio local o sistema local y pueden continuar ejecutándose después de que el último usuario humano cierre la sesión.

> [!NOTE]
> Normalmente, los servicios se ejecutan en contextos de seguridad conocidos como sistema local (sistema), servicio de red o servicio local.  Windows Server 2008 R2 presentó servicios que se ejecutan en una cuenta de servicio administrada, que son entidades de seguridad de dominio.

Antes de iniciar un servicio, el controlador de servicio inicia sesión con la cuenta designada para el servicio y, a continuación, presenta las credenciales del servicio para la autenticación de la LSA. El servicio de Windows implementa una interfaz de programación que el administrador de controladores de servicio puede utilizar para controlar el servicio. Un servicio de Windows puede iniciarse automáticamente cuando se inicia el sistema o manualmente con un programa de control de servicios. Por ejemplo, cuando un equipo cliente de Windows se une a un dominio, el servicio mensajero del equipo se conecta a un controlador de dominio y abre un canal seguro para él. Para obtener una conexión autenticada, el servicio debe tener credenciales de confianza para la autoridad de seguridad local (LSA) del equipo remoto. Al comunicarse con otros equipos de la red, LSA usa las credenciales para la cuenta de dominio del equipo local, al igual que todos los demás servicios que se ejecutan en el contexto de seguridad del sistema local y del servicio de red. Los servicios del equipo local se ejecutan como sistema, por lo que no es necesario presentar las credenciales a la LSA.

El archivo Ksecdd.sys administra y cifra estas credenciales y usa una llamada a procedimiento local en la LSA. El tipo de archivo es DRV (controlador) y se conoce como proveedor de compatibilidad para seguridad (SSP) de modo kernel y, en las versiones designadas en la lista **se aplica a** al principio de este tema, es compatible con el nivel 1 de FIPS 140-2.

El modo kernel tiene acceso total a los recursos de hardware y del sistema del equipo. El modo kernel impide que los servicios de modo de usuario y las aplicaciones tengan acceso a las áreas críticas del sistema operativo a las que no deberían tener acceso.

## <a name="local-security-authority"></a><a name="BKMK_LSA"></a>Autoridad de seguridad local
La autoridad de seguridad local (LSA) es un proceso del sistema protegido que autentica los usuarios y los registra en el equipo local. Además, LSA mantiene información sobre todos los aspectos de la seguridad local en un equipo (estos aspectos se conocen colectivamente como la Directiva de seguridad local) y proporcionan varios servicios para la traducción entre los nombres y los identificadores de seguridad (SID). El proceso del sistema de seguridad, el servicio del servidor de autoridad de seguridad local (LSASS), realiza un seguimiento de las directivas de seguridad y las cuentas que están en vigor en un sistema informático.

LSA valida la identidad de un usuario en función de las dos entidades siguientes emitidas por la cuenta del usuario:

-   **Autoridad de seguridad local.** La LSA puede validar la información de usuario comprobando la base de datos del administrador de cuentas de seguridad (SAM) ubicada en el mismo equipo. Cualquier estación de trabajo o servidor miembro puede almacenar cuentas de usuario locales e información acerca de los grupos locales. Sin embargo, estas cuentas solo se pueden usar para tener acceso a esa estación de trabajo o equipo.

-   **Autoridad de seguridad para el dominio local o para un dominio de confianza.** La LSA se pone en contacto con la entidad que emitió la cuenta y solicita la comprobación de que la cuenta es válida y que la solicitud se originó en el titular de la cuenta.

El Servicio de subsistema de autoridad de seguridad local (LSASS) almacena las credenciales en memoria en representación de los usuarios con sesiones de Windows activas. Las credenciales almacenadas permiten a los usuarios tener acceso sin problemas a los recursos de red, como recursos compartidos de archivos, buzones de Exchange Server y sitios de SharePoint, sin tener que volver a escribir sus credenciales para cada servicio remoto.

LSASS puede almacenar credenciales de diferentes formas, por ejemplo:

-   Texto sin formato cifrado de manera reversible

-   Vales de Kerberos (vales de concesión de vales (TGT), vales de servicio)

-   Hash de NT

-   Hash de LAN Manager (LM)

Si el usuario inicia sesión en Windows mediante una tarjeta inteligente, LSASS no almacena una contraseña en texto sin formato, sino que almacena el valor hash de NT correspondiente para la cuenta y el PIN de texto sin formato de la tarjeta inteligente. Si se habilita el atributo de la cuenta para una tarjeta inteligente que sea necesaria para el inicio de sesión interactivo, se generará automáticamente un valor hash de NT para la cuenta, en lugar del hash de contraseña original. El hash de contraseña generado automáticamente al establecer el atributo no cambia.

Si un usuario inicia sesión en un equipo basado en Windows con una contraseña compatible con hashes de LAN Manager (LM), este autenticador se encuentra en la memoria.

El almacenamiento en memoria de credenciales en texto sin formato no se puede deshabilitar, incluso si los proveedores de credenciales requieren que estén deshabilitados.

Las credenciales almacenadas se asocian directamente con las sesiones de inicio de sesión de Servicio de subsistema de autoridad de seguridad local (LSASS) (LSASS) que se han iniciado después del último reinicio y que no se han cerrado. Por ejemplo, las sesiones LSA con credenciales LSA almacenadas se crean cuando un usuario realiza una de las acciones siguientes:

-   Inicia sesión en una sesión local o Protocolo de escritorio remoto (RDP) en el equipo.

-   Ejecuta una tarea mediante la opción **Ejecutar como**

-   Ejecuta un servicio de Windows activo en el equipo

-   Ejecuta una tarea programada o un trabajo por lotes

-   Ejecuta una tarea en el equipo local mediante una herramienta de administración remota

En algunas circunstancias, los secretos de LSA, que son partes secretas de datos a los que solo pueden acceder los procesos de la cuenta del sistema, se almacenan en la unidad de disco duro. Algunos de estos secretos son credenciales que deben persistir después de un reinicio y que se almacenan con formato cifrado en la unidad de disco duro. Las credenciales almacenadas como secretos de LSA son los siguientes:

-   Contraseña de la cuenta de la cuenta de Active Directory Domain Services (AD DS) del equipo

-   Contraseñas de cuenta para servicios de Windows configurados en el equipo

-   Contraseñas de cuenta para tareas programadas configuradas

-   Contraseñas de cuenta para grupos de aplicaciones y sitios web de IIS

-   Contraseñas de las cuentas de Microsoft

Introducido en Windows 8.1, el sistema operativo cliente proporciona protección adicional para LSA para evitar la lectura de memoria y la inserción de código por parte de procesos no protegidos. Esta protección aumenta la seguridad de las credenciales que LSA almacena y administra.

Para obtener más información acerca de estas protecciones adicionales, consulte Configuración de la [protección LSA adicional](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="cached-credentials-and-validation"></a><a name="BKMK_CachedCredentialsAndValidation"></a>Credenciales y validación almacenadas en caché
Los mecanismos de validación dependen de la presentación de credenciales en el momento del inicio de sesión. Sin embargo, cuando el equipo se desconecta de un controlador de dominio y el usuario presenta las credenciales de dominio, Windows usa el proceso de credenciales almacenadas en caché en el mecanismo de validación.

Cada vez que un usuario inicia sesión en un dominio, Windows almacena en caché las credenciales proporcionadas y las almacena en el subárbol de seguridad en el registro del sistema operativo.

Con las credenciales almacenadas en caché, el usuario puede iniciar sesión en un miembro del dominio sin estar conectado a un controlador de dominio dentro de ese dominio.


## <a name="credential-storage-and-validation"></a><a name="BKMK_CredentialStorageAndValidation"></a>Almacenamiento y validación de credenciales
No siempre es conveniente usar un conjunto de credenciales para el acceso a diferentes recursos. Por ejemplo, un administrador podría querer usar las credenciales administrativas en lugar de las de usuario al obtener acceso a un servidor remoto. Del mismo modo, si un usuario tiene acceso a recursos externos, como una cuenta bancaria, solo puede usar credenciales que sean diferentes de las credenciales de su dominio. En las secciones siguientes se describen las diferencias en la administración de credenciales entre las versiones actuales de los sistemas operativos Windows y los sistemas operativos Windows Vista y Windows XP.

### <a name="remote-logon-credential-processes"></a>Procesos de credenciales de inicio de sesión remoto
El Protocolo de escritorio remoto (RDP) administra las credenciales del usuario que se conecta a un equipo remoto mediante el cliente de Escritorio remoto, que se presentó en Windows 8. Las credenciales en formato de texto no cifrado se envían al host de destino en el que el host intenta realizar el proceso de autenticación y, si se realiza correctamente, conecta el usuario a los recursos permitidos. RDP no almacena las credenciales en el cliente, pero las credenciales de dominio del usuario se almacenan en el LSASS.

Introducido en Windows Server 2012 R2 y Windows 8.1, el modo de administración restringida proporciona seguridad adicional a los escenarios de inicio de sesión remoto. Este modo de Escritorio remoto hace que la aplicación cliente realice un desafío-respuesta de inicio de sesión de red con la función unidireccional de NT (NTOWF) o use un vale de servicio de Kerberos al autenticarse en el host remoto. Una vez autenticado el administrador, el administrador no tiene las credenciales de cuenta respectivas en LSASS porque no se proporcionaron al host remoto. En su lugar, el administrador tiene las credenciales de la cuenta de equipo para la sesión. Las credenciales de administrador no se proporcionan al host remoto, por lo que las acciones se realizan como la cuenta de equipo. Los recursos también se limitan a la cuenta de equipo y el administrador no puede acceder a los recursos con su propia cuenta.

### <a name="automatic-restart-sign-on-credential-process"></a>Proceso de credenciales de inicio de sesión de reinicio automático
Cuando un usuario inicia sesión en un dispositivo Windows 8.1, LSA guarda las credenciales de usuario en la memoria cifrada a las que solo tienen acceso LSASS.exe. Cuando Windows Update inicia un reinicio automático sin presencia de usuario, estas credenciales se usan para configurar el inicio de sesión automático para el usuario.

Al reiniciar, el usuario inicia sesión automáticamente mediante el mecanismo de inicio de sesión automático y, a continuación, el equipo se bloquea además para proteger la sesión del usuario. El bloqueo se inicia a través de Winlogon, mientras que la administración de credenciales se realiza mediante LSA. Al iniciar sesión automáticamente y bloquear la sesión del usuario en la consola, las aplicaciones de la pantalla de bloqueo del usuario se reinician y están disponibles.

Para obtener más información acerca de ARSO, consulte [Inicio de sesión con reinicio automático de Winlogon &#40;ARSO&#41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Nombres de usuario y contraseñas almacenados en Windows Vista y Windows XP
En Windows Server 2008, Windows Server 2003, Windows Vista y Windows XP, **los nombres de usuario y contraseñas almacenados** en el panel de control simplifican la administración y el uso de varios conjuntos de credenciales de inicio de sesión, incluidos los certificados X. 509 usados con tarjetas inteligentes y las credenciales de Windows Live (ahora denominados cuenta de Microsoft). Las credenciales: parte del perfil del usuario, se almacenan hasta que sea necesario. Esta acción puede aumentar la seguridad en función de cada recurso asegurándose de que si una contraseña está en peligro, no ponga en peligro toda la seguridad.

Una vez que un usuario inicia sesión e intenta tener acceso a recursos adicionales protegidos mediante contraseña, como un recurso compartido en un servidor, y si las credenciales de inicio de sesión predeterminadas del usuario no son suficientes para obtener acceso, se consultan **los nombres de usuario y contraseñas almacenados** . Si se han guardado credenciales alternativas con la información de inicio de sesión correcta en **nombres de usuario y contraseñas almacenados**, estas credenciales se usan para obtener acceso. De lo contrario, se solicita al usuario que proporcione nuevas credenciales, que se pueden guardar para su reutilización, ya sea después en la sesión de inicio de sesión o durante una sesión posterior.

Se aplican las restricciones que se indican a continuación:

-   Si los **nombres de usuario y contraseñas almacenados** contienen credenciales no válidas o incorrectas para un recurso específico, se deniega el acceso al recurso y no aparece el cuadro de diálogo **nombres de usuario y contraseñas almacenados** .

-   **Los nombres de usuario y contraseñas almacenados** almacenan las credenciales solo para NTLM, el protocolo Kerberos, cuenta de Microsoft (anteriormente Windows Live ID) y la autenticación de capa de sockets seguros (SSL). Algunas versiones de Internet Explorer mantienen su propia memoria caché para la autenticación básica.

Estas credenciales se convierten en una parte cifrada del perfil local de un usuario en el directorio \Documents and Settings\Username\Application Data\Microsoft\Credentials Como resultado, estas credenciales pueden moverse con el usuario si la Directiva de red del usuario es compatible con los perfiles de usuario móviles. Sin embargo, si el usuario tiene copias de **nombres de usuario y contraseñas almacenados** en dos equipos diferentes y cambia las credenciales asociadas al recurso en uno de estos equipos, el cambio no se propaga a **los nombres de usuario y contraseñas almacenados** en el segundo equipo.

### <a name="windows-vault-and-credential-manager"></a>Windows Vault y el administrador de credenciales
El administrador de credenciales se presentó en Windows Server 2008 R2 y Windows 7 como una característica del panel de control para almacenar y administrar nombres de usuario y contraseñas. El administrador de credenciales permite que los usuarios almacenen las credenciales relevantes para otros sistemas y sitios web en el almacén seguro de Windows. Algunas versiones de Internet Explorer usan esta característica para la autenticación en sitios Web.

El usuario controla la administración de credenciales mediante el Administrador de credenciales en el equipo local. Los usuarios pueden guardar y almacenar credenciales desde exploradores y aplicaciones de Windows admitidos para poder iniciar sesión en estos recursos de forma cómoda cuando lo necesiten. Las credenciales se guardan en carpetas cifradas especiales en el equipo bajo el perfil del usuario. Las aplicaciones que admiten esta característica (mediante el uso de las API del administrador de credenciales), como las aplicaciones y los exploradores Web, pueden presentar las credenciales correctas a otros equipos y sitios web durante el proceso de inicio de sesión.

Cuando un sitio web, una aplicación u otro equipo solicita la autenticación a través de NTLM o del protocolo Kerberos, aparece un cuadro de diálogo en el que se activa la casilla **Actualizar credenciales predeterminadas** o **guardar contraseña** . Este cuadro de diálogo que permite al usuario guardar las credenciales localmente se genera mediante una aplicación que admite las API del administrador de credenciales. Si el usuario activa la casilla **guardar contraseña** , el administrador de credenciales realiza un seguimiento del nombre de usuario, la contraseña y la información relacionada del usuario para el servicio de autenticación que está en uso.

La próxima vez que se use el servicio, el administrador de credenciales proporcionará automáticamente las credenciales que se almacenan en el almacén de Windows. Si no se acepta, se solicitará al usuario la información de acceso correcta. Si se concede el acceso con las nuevas credenciales, el administrador de credenciales sobrescribe la credencial anterior con la nueva y, a continuación, almacena la nueva credencial en el almacén de Windows.

## <a name="security-accounts-manager-database"></a><a name="BKMK_SAM"></a>Base de datos Administrador de cuentas de seguridad
El administrador de cuentas de seguridad (SAM) es una base de datos que almacena grupos y cuentas de usuario locales. Está presente en todos los sistemas operativos de Windows; sin embargo, cuando un equipo se une a un dominio, Active Directory administra cuentas de dominio en Active Directory dominios.

Por ejemplo, los equipos cliente que ejecutan un sistema operativo Windows participan en un dominio de red mediante la comunicación con un controlador de dominio, incluso cuando ningún usuario humano inicia sesión. Para iniciar las comunicaciones, el equipo debe tener una cuenta activa en el dominio. Antes de aceptar las comunicaciones del equipo, la LSA del controlador de dominio autentica la identidad del equipo y, a continuación, construye el contexto de seguridad del equipo tal y como lo hace para una entidad de seguridad humana. Este contexto de seguridad define la identidad y las capacidades de un usuario o servicio en un equipo determinado o un usuario, un servicio o un equipo de una red. Por ejemplo, el token de acceso contenido dentro del contexto de seguridad define los recursos (por ejemplo, un recurso compartido de archivos o una impresora) a los que se puede tener acceso y las acciones (como lectura, escritura o modificación) que puede realizar esa entidad de seguridad (un usuario, un equipo o un servicio en ese recurso).

El contexto de seguridad de un usuario o equipo puede variar de un equipo a otro, por ejemplo, cuando un usuario inicia sesión en un servidor o en una estación de trabajo que no sea la propia estación de trabajo principal del usuario. También puede variar de una sesión a otra, por ejemplo, cuando un administrador modifica los derechos y permisos del usuario. Además, el contexto de seguridad suele ser diferente cuando un usuario o un equipo está trabajando de forma independiente, en una red o como parte de un dominio de Active Directory.

## <a name="local-domains-and-trusted-domains"></a><a name="BKMK_LocalDomainsAndTrustedDomains"></a>Dominios locales y dominios de confianza
Cuando existe una confianza entre dos dominios, los mecanismos de autenticación para cada dominio dependen de la validez de las autenticaciones procedentes del otro dominio. Las confianzas ayudan a proporcionar acceso controlado a los recursos compartidos en un dominio de recursos (el dominio que confía) comprobando que las solicitudes de autenticación entrantes proceden de una autoridad de confianza (el dominio de confianza). De esta manera, las confianzas actúan como puentes que permiten que solo las solicitudes de autenticación validadas viajen entre dominios.

La forma en que una confianza específica pasa solicitudes de autenticación depende de cómo esté configurada. Las relaciones de confianza pueden ser unidireccionales, ya que proporcionan acceso desde el dominio de confianza a los recursos del dominio que confía, o bien de forma bidireccional, al proporcionar acceso desde cada dominio a los recursos del otro dominio. Las confianzas también son no transitivas, en cuyo caso solo existe una confianza entre los dos dominios de asociados de confianza, o transitiva, en cuyo caso una confianza se extiende automáticamente a cualquier otro dominio en el que uno de los asociados confíe.

Para obtener información sobre las relaciones de dominio y de confianza de bosque con respecto a la autenticación, vea [autenticación delegada y relaciones de confianza](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169022(v=ws.10)).

## <a name="certificates-in-windows-authentication"></a><a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificados en la autenticación de Windows
Una infraestructura de clave pública (PKI) es la combinación de software, tecnologías de cifrado, procesos y servicios que permiten a una organización proteger sus comunicaciones y transacciones empresariales. La capacidad de una PKI para proteger las comunicaciones y las transacciones empresariales se basa en el intercambio de certificados digitales entre usuarios autenticados y recursos de confianza.

Un certificado digital es un documento electrónico que contiene información sobre la entidad a la que pertenece, la entidad de la que fue emitido, un número de serie único o cualquier otra identificación única, fechas de emisión y de expiración, y una huella digital.

La autenticación es el proceso de determinar si se puede confiar en un host remoto. Para establecer su confiabilidad, el host remoto debe proporcionar un certificado de autenticación aceptable.

Los hosts remotos establecen su confiabilidad mediante la obtención de un certificado de una entidad de certificación (CA). La CA puede, a su vez, tener la certificación de una autoridad superior, lo que crea una cadena de confianza. Para determinar si un certificado es de confianza, una aplicación debe determinar la identidad de la CA raíz y, a continuación, determinar si es de confianza.

Del mismo modo, el host remoto o el equipo local deben determinar si el certificado presentado por el usuario o la aplicación es auténtico. El certificado presentado por el usuario a través de LSA y SSPI se evalúa para su autenticidad en el equipo local para el inicio de sesión local, en la red o en el dominio a través de los almacenes de certificados de Active Directory.

Para generar un certificado, los datos de autenticación pasan a través de algoritmos hash, como Algoritmo hash seguro 1 (SHA1), para generar una síntesis del mensaje. La síntesis del mensaje se firma digitalmente mediante la clave privada del remitente para demostrar que el remitente produjo la síntesis del mensaje.

> [!NOTE]
> SHA1 es el valor predeterminado en Windows 7 y Windows Vista, pero se cambió a SHA2 en Windows 8.

**Autenticación con tarjeta inteligente**

La tecnología de tarjeta inteligente es un ejemplo de autenticación basada en certificados. El inicio de sesión en una red con una tarjeta inteligente proporciona una forma segura de autenticación, ya que usa la identificación basada en criptografía y la prueba de posesión al autenticar a un usuario en un dominio. Active Directory servicios de Certificate Server (AD CS) proporciona la identificación basada en cifrado a través de la emisión de un certificado de inicio de sesión para cada tarjeta inteligente.

Para obtener información acerca de la autenticación mediante tarjeta inteligente, consulte la [referencia técnica de la tarjeta inteligente de Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff404297(v=ws.10)).

La tecnología de tarjeta inteligente virtual se presentó en Windows 8. Almacena el certificado de la tarjeta inteligente en el equipo y, a continuación, lo protege mediante el chip de seguridad del Módulo de plataforma segura de la prueba de alteración del dispositivo (TPM). De esta manera, el equipo se convierte realmente en la tarjeta inteligente, que debe recibir el PIN del usuario para autenticarse.

**Autenticación remota e inalámbrica**

La autenticación de red remota e inalámbrica es otra tecnología que usa certificados para la autenticación. El servicio de autenticación de Internet (IAS) y los servidores de red privada virtual usan el protocolo de autenticación extensible (EAP-TLS), el protocolo de autenticación extensible protegido (PEAP) o el protocolo de seguridad de Internet (IPsec) para realizar la autenticación basada en certificados para muchos tipos de acceso de red, incluidas las conexiones de red privada virtual (VPN) y inalámbricas.

Para obtener información acerca de la autenticación basada en certificados en redes, consulte [autenticación de acceso a la red y certificados](/previous-versions/windows/it-pro/windows-server-2003/cc759575(v=ws.10)).

## <a name="see-also"></a><a name="BKMK_SeeAlso"></a>Vea también
[Conceptos de autenticación de Windows](./windows-authentication-concepts.md)