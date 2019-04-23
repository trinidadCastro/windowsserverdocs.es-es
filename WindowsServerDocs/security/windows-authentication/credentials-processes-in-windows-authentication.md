---
title: Procesos de las credenciales en la autenticación de Windows
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 52d50a1bb6bfbe9d35146f362a2a5184df0a6504
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839866"
---
# <a name="credentials-processes-in-windows-authentication"></a>Procesos de las credenciales en la autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema de referencia para profesionales de TI describe cómo la autenticación de Windows procesa las credenciales.

Administración de credenciales de Windows es el proceso por el que recibe las credenciales en el servicio o usuario del sistema operativo y asegura que la información de presentación futuras en el destino de autenticación. En el caso de un equipo unido al dominio, el destino de autenticación es el controlador de dominio. Las credenciales de autenticación son documentos digitales que asociación la identidad del usuario a algún tipo de la prueba de autenticidad, como un certificado, una contraseña o un PIN.

De forma predeterminada, se validan las credenciales de Windows con la base de datos de administrador de cuentas de seguridad (SAM) en el equipo local o de Active Directory en un equipo unido al dominio, a través del servicio de Winlogon. Las credenciales se recopilan a través de la entrada del usuario en la interfaz de usuario de inicio de sesión o mediante programación a través de la interfaz de programación de aplicaciones (API) que se presentará en el destino de autenticación.

Información de seguridad local se almacena en el registro bajo **HKEY_LOCAL_MACHINE\SECURITY**. Información almacenada incluye la configuración de directiva, los valores de seguridad predeterminados e información de la cuenta, como las credenciales de inicio de sesión almacenado en caché. También se almacena una copia de la base de datos SAM aquí, aunque es protegido contra escritura.

El siguiente diagrama muestra los componentes que son necesarios y las rutas de acceso que las credenciales llevan por el sistema para autenticar el usuario o proceso para un inicio de sesión correcto.

![Diagrama que muestra los componentes que son necesarios y las rutas de acceso que las credenciales llevan por el sistema para autenticar el usuario o proceso para un inicio de sesión correcto.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

La tabla siguiente describe cada componente que administra las credenciales en el proceso de autenticación en el punto de inicio de sesión.

**Componentes de autenticación para todos los sistemas**

|Componente|Descripción|
|-------|--------|
|Inicio de sesión del usuario|Winlogon.exe es el archivo ejecutable responsable de administrar las interacciones del usuario segura. El servicio Winlogon inicia el proceso de inicio de sesión para los sistemas operativos de Windows, pasando las credenciales recopiladas por acción del usuario en el escritorio seguro (interfaz de usuario de inicio de sesión) para la autoridad de seguridad Local (LSA) a través de Secur32.dll.|
|Inicio de sesión de aplicación|Aplicación o los inicios de sesión de servicio que no requieren inicio de sesión interactivo. La mayoría de los procesos iniciados por el usuario que se ejecutan en modo usuario utilizando Secur32.dll, mientras que los procesos iniciados durante el inicio, como los servicios, se ejecutan en modo kernel usando Ksecdd.sys.<br /><br />Para obtener más información acerca del modo de usuario y modo kernel, vea las aplicaciones y el modo de usuario o servicios y el modo de núcleo en este tema.|
|Secur32.dll|Los proveedores de autenticación múltiples que forman la base del proceso de autenticación.|
|Lsasrv.dll|El servicio servidor LSA, que aplica las directivas de seguridad y actúa como el Administrador de paquetes de seguridad de la LSA. La LSA contiene la función Negotiate, que selecciona el protocolo NTLM o Kerberos después de determinar el protocolo que se va a ser correcto.|
|Proveedores de compatibilidad para seguridad|Un conjunto de proveedores que puede invocar individualmente uno o más protocolos de autenticación. El conjunto predeterminado de los proveedores puede cambiar con cada versión del sistema operativo Windows y se pueden escribir proveedores personalizados.|
|Netlogon.dll|Los servicios que realiza el servicio de Net Logon son los siguientes:<br /><br />-Mantiene desde el canal seguro del equipo (para no confundir con Schannel) a un controlador de dominio.<br />-Pasa las credenciales del usuario a través de un canal seguro para el controlador de dominio y devuelve los identificadores de seguridad (SID) de dominio y derechos de usuario para el usuario.<br />-Publica registros de recursos de servicio en el sistema de nombres de dominio (DNS) y utiliza DNS para resolver nombres en las direcciones de protocolo de Internet (IP) de los controladores de dominio.<br />-Implementa el protocolo de replicación en función de llamada a procedimiento remoto (RPC) para sincronizar los controladores de dominio principal (PDC) y controladores de dominio de reserva (BDC).|
|Samsrv.dll|El Administrador de cuentas de seguridad (SAM), que almacena las cuentas de seguridad local, aplica las directivas almacenadas localmente y es compatible con las API.|
|Registro|El registro contiene una copia de la base de datos SAM, configuración de directiva de seguridad local, los valores de seguridad predeterminados e información de la cuenta que solo es accesible para el sistema.|

Este tema contiene las siguientes secciones:

-   [Entrada de credencial de inicio de sesión de usuario](#BKMK_CrentialInputForUserLogon)

-   [Entrada de credencial de inicio de sesión de servicio y aplicación](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autoridad de seguridad local](#BKMK_LSA)

-   [Validación y las credenciales almacenadas en caché](#BKMK_CachedCredentialsAndValidation)

-   [Almacenamiento de credenciales y la validación](#BKMK_CredentialStorageAndValidation)

-   [Base de datos de administrador de cuentas de seguridad](#BKMK_SAM)

-   [Dominios de confianza y dominios locales](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificados de autenticación de Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Entrada de credencial de inicio de sesión de usuario
En Windows Server 2008 y Windows Vista, la arquitectura de autenticación (GINA) e identificación gráfica se ha reemplazado por un modelo de proveedor de credenciales, que se hizo posible enumerar tipos de inicio de sesión diferente mediante el uso de los iconos de inicio de sesión. A continuación se describen ambos modelos.

**Arquitectura de autenticación e identificación gráfica**

La arquitectura de autenticación (GINA) e identificación gráfica se aplica a los sistemas operativos Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Windows 2000 Professional. En estos sistemas, cada sesión de inicio de sesión interactivo crea una instancia independiente del servicio de Winlogon. La arquitectura GINA se carga en el espacio de proceso usado por Winlogon, recibe y procesa las credenciales y realiza las llamadas a las interfaces de autenticación a través de LSALogonUser.

Las instancias de Winlogon para un inicio de sesión interactivo se ejecutan en la sesión 0. Servicios del sistema de hosts de sesión 0 y otros procesos vitales, incluido el proceso de autoridad de seguridad Local (LSA).

El siguiente diagrama muestra el proceso de credencial de Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Microsoft Windows 2000 Professional.

![Diagrama que muestra el proceso de credenciales de Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Arquitectura de proveedor de credenciales**

La arquitectura de proveedor de credenciales se aplica a las versiones designadas en la **se aplica a** lista al principio de este tema. En estos sistemas, la arquitectura de entrada de credenciales se cambia a un diseño extensible mediante el uso de proveedores de credenciales. Estos proveedores están representados por los iconos de inicio de sesión diferente en el escritorio seguro que permiten a cualquier número de escenarios de inicio de sesión - cuentas diferentes para el mismo usuario y los métodos de autenticación diferentes, como contraseñas, tarjetas inteligentes y biometría.

Con la arquitectura de proveedor de credenciales, Winlogon inicia siempre la interfaz de usuario de inicio de sesión después de recibir un evento de la secuencia de aviso de seguridad. Interfaz de usuario de inicio de sesión, consulta cada proveedor de credenciales para el número de diferentes tipos de credenciales que se configura el proveedor para enumerar. Los proveedores de credenciales tienen la opción de especificar uno de estos iconos como valor predeterminado. Después de que todos los proveedores han enumerado sus iconos, interfaz de usuario de inicio de sesión los muestra al usuario. El usuario interactúa con un icono para proporcionar sus credenciales. Interfaz de usuario de inicio de sesión envía estas credenciales para la autenticación.

Proveedores de credenciales no son mecanismos de cumplimiento. Se usaron para recopilar y serializar las credenciales. Exigir la seguridad de los paquetes de autenticación y la autoridad de seguridad Local.

Los proveedores de credenciales se registran en el equipo y están responsables de lo siguiente:

-   Que describe la información de credenciales necesaria para la autenticación.

-   Controlar la comunicación y la lógica con las entidades de autenticación externo.

-   Las credenciales de empaquetado para interactivo e inicio de sesión de red.

Las credenciales de empaquetado para interactivo e inicio de sesión de red incluye el proceso de serialización. Al serializar las credenciales se pueden mostrar varios iconos de inicio de sesión en la interfaz de usuario de inicio de sesión. Por lo tanto, su organización puede controlar la presentación de inicio de sesión como usuarios, sistemas de destino para el inicio de sesión, inicio de sesión previo acceso a la red y la estación de trabajo Bloquear/desbloquear directivas - mediante el uso de proveedores de credenciales personalizadas. Varios proveedores de credenciales pueden coexistir en el mismo equipo.

Como un proveedor de credenciales estándar o como un proveedor de acceso de inicio de sesión previo, se pueden desarrollar solo proveedores de inicio de sesión (SSO).

Cada versión de Windows contiene el proveedor de credenciales de un valor predeterminado y el inicio de sesión anterior a una etiqueta default-acceso proveedor (PLAP), también conocido como el proveedor de inicio de sesión único. El proveedor de inicio de sesión único permite a los usuarios para realizar una conexión a una red antes de iniciar sesión el equipo local. Cuando se implementa este proveedor, el proveedor no enumera los iconos de la interfaz de usuario de inicio de sesión.

Un proveedor de inicio de sesión único está pensado para usarse en los siguientes escenarios:

-   **Inicio de sesión de autenticación y el equipo de red se controlan mediante proveedores de credenciales diferentes.** Se incluyen las variaciones en este escenario:

    -   Un usuario tiene la opción de conectarse a una red, como la conexión a una red privada virtual (VPN), antes de iniciar sesión en el equipo, pero no es necesario para realizar esta conexión.

    -   Se requiere autenticación de red para recuperar la información utilizada durante la autenticación interactiva en el equipo local.

    -   Varias autenticaciones de red van seguidas de uno de los otros escenarios. Por ejemplo, un usuario se autentica en un proveedor de servicios Internet (ISP), se autentica en una red privada virtual y, a continuación, utiliza sus credenciales de cuenta de usuario para iniciar sesión localmente.

    -   Credenciales almacenadas en caché están deshabilitadas y se requiere una conexión de servicios de acceso remoto a través de VPN antes de inicio de sesión local para autenticar al usuario.

    -   Un usuario de dominio no tiene una cuenta local en un equipo unido al dominio y debe establecer una conexión de servicios de acceso remoto a través de la conexión VPN antes de completar el inicio de sesión interactivo.

-   **Inicio de sesión de autenticación y el equipo de red se controlan mediante el mismo proveedor de credenciales.** En este escenario, el usuario es necesario para conectarse a la red antes de iniciar sesión en el equipo.

**Enumeración de icono de inicio de sesión**

El proveedor de credenciales enumera los iconos de inicio de sesión en los casos siguientes:

-   Para esos sistemas operativos designados en el **se aplica a** lista al principio de este tema.

-   El proveedor de credenciales enumera los iconos de inicio de sesión de estación de trabajo. Normalmente, el proveedor de credenciales serializa las credenciales de autenticación a la entidad de seguridad local. Este proceso muestra iconos específicos para cada usuario y específicos para los sistemas de destino de cada usuario.

-   La arquitectura de inicio de sesión y autenticación permite a un usuario use iconos enumerados por el proveedor de credenciales para desbloquear una estación de trabajo. Normalmente, el usuario ha iniciado sesión actualmente es el icono de forma predeterminada, pero si más de un usuario ha iniciado sesión, se muestran varios iconos.

-   El proveedor de credenciales enumera los iconos en respuesta a una solicitud de usuario para cambiar su contraseña u otra información privada, por ejemplo, un PIN. Normalmente, el usuario ha iniciado sesión actualmente es el icono predeterminado; Sin embargo, si más de un usuario ha iniciado sesión, se muestran varios iconos.

-   El proveedor de credenciales enumera los iconos basados en las credenciales serializadas que se usará para la autenticación en equipos remotos. Credencial de la interfaz de usuario no usa la misma instancia del proveedor como la interfaz de usuario de inicio de sesión, desbloquear la estación de trabajo o cambiar la contraseña. Por lo tanto, no se puede mantener información de estado en el proveedor de entre las instancias de la interfaz de usuario de la credencial. Esta estructura da como resultado un mosaico para cada inicio de sesión del equipo remoto, suponiendo que se han serializado correctamente las credenciales. Este escenario también se usa en el Control de cuentas de usuario (UAC), que puede ayudar a evitar cambios no autorizados a un equipo por preguntar al usuario permiso o una contraseña de administrador antes de permitir que las acciones que podrían afectar el funcionamiento del equipo o que podrían cambiar la configuración que afecta a otros usuarios del equipo.

El siguiente diagrama muestra el proceso de credenciales para los sistemas operativos designado en el **se aplica a** lista al principio de este tema.

![Diagrama que muestra el proceso de credenciales para los sistemas operativos designados en la ** se aplica a ** lista al principio de este tema.](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrada de credencial de inicio de sesión de servicio y aplicación
Autenticación de Windows está diseñada para administrar las credenciales para aplicaciones o servicios que no requieren interacción del usuario. Las aplicaciones en modo de usuario están limitadas en cuanto a qué recursos del sistema que tienen acceso, mientras que los servicios pueden tienen acceso ilimitado a la memoria del sistema y los dispositivos externos.

Servicios del sistema y las aplicaciones de nivel de transporte de acceso a un proveedor de compatibilidad para seguridad (SSP) a través de la interfaz de proveedor de soporte técnico de seguridad (SSPI) en Windows, que proporciona funciones para enumerar los paquetes de seguridad disponibles en un sistema, seleccione un paquete y use ese paquete para obtener una conexión autenticada.

Cuando se autentica una conexión cliente/servidor:

-   La aplicación en el lado cliente de la conexión envía credenciales al servidor mediante la función SSPI `InitializeSecurityContext (General)`.

-   Responde de la aplicación en el servidor de la conexión con la función SSPI `AcceptSecurityContext (General)`.

-   Las funciones SSPI `InitializeSecurityContext (General)` y `AcceptSecurityContext (General)` se repiten hasta que se han intercambiado todos los mensajes de autenticación necesaria para tener éxito o se superará la autenticación.

-   Después de que se ha autenticado la conexión, la LSA en el servidor usa información del cliente para crear el contexto de seguridad, que contiene un token de acceso.

-   El servidor, a continuación, puede llamar a la función SSPI `ImpersonateSecurityContext` para asociar el token de acceso a un subproceso de representación para el servicio.

**Las aplicaciones y el modo de usuario**

Modo de usuario en Windows se compone de dos sistemas capaces de pasar las solicitudes de E/S a los controladores de modo kernel adecuado: el sistema del entorno, que se ejecuta a las aplicaciones escritas para muchos tipos diferentes de los sistemas operativos, y el sistema entero, que funciona funciones específicas del sistema en nombre del sistema del entorno.

El sistema entero administra las funciones operativas de system'specific en nombre del sistema del entorno y consta de un proceso del sistema de seguridad (LSA), un servicio de estación de trabajo y un servicio de servidor. El proceso del sistema de seguridad se ocupa de los tokens de seguridad, se concede o deniega permisos para tener acceso a las cuentas de usuario según los permisos de recursos, controla las solicitudes de inicio de sesión e inicia la autenticación de inicio de sesión y determina qué recursos del sistema en el sistema operativo necesita realizar la auditoría.

Las aplicaciones pueden ejecutar en modo de usuario donde la aplicación puede ejecutarse como cualquier entidad de seguridad, incluidos en el contexto de seguridad del sistema Local (SYSTEM). Las aplicaciones también pueden ejecutar en modo kernel donde la aplicación puede ejecutarse en el contexto de seguridad del sistema Local (SYSTEM).

SSPI está disponible a través del módulo Secur32.dll, que es una API que se utiliza para obtener servicios de seguridad integrados para la autenticación, integridad del mensaje y privacidad de mensajes. Proporciona una capa de abstracción entre los protocolos de nivel de aplicación y los protocolos de seguridad. Dado que las distintas aplicaciones requieren diferentes maneras de identificar o autenticar usuarios y distintas formas de cifrar los datos que viajan a través de una red, SSPI proporciona una manera de obtener acceso a bibliotecas de vínculos dinámicos (DLL) que contienen autenticación diferente y funciones de cifrado. Estos archivos DLL se denominan proveedores de compatibilidad para seguridad (SSP).

Cuentas de servicio administradas y cuentas virtuales se introdujeron en Windows Server 2008 R2 y Windows 7 para proporcionar aplicaciones cruciales, como Microsoft SQL Server e Internet Information Services (IIS), el aislamiento de sus propias cuentas de dominio, mientras que elimina la necesidad de un administrador administre manualmente el nombre principal de servicio (SPN) y las credenciales para estas cuentas. Para obtener más información acerca de estas características y su rol en la autenticación, consulte [Managed Service cuentas documentación para Windows 7 y Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) y [grupo Managed Service Accounts Overview](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Servicios y modo de núcleo**

Aunque la mayoría de las aplicaciones de Windows se ejecuta en el contexto de seguridad del usuario que se inicia, esto no es cierto de servicios. Muchos de los servicios de Windows, como la red y los servicios de impresión, se inician mediante el servicio de controlador cuando el usuario arranca el equipo. Estos servicios podrían ejecutar como servicio Local o sistema Local y podrían seguir ejecutándose después de que el último usuario humano cierra la sesión.

> [!NOTE]
> Normalmente, los servicios se ejecutan en contextos de seguridad conocidos como sistema Local (SYSTEM), servicio de red o servicio Local.  Windows Server 2008 R2 introdujo los servicios que se ejecutan bajo una cuenta de servicio administrada, que son entidades de dominio.

Antes de iniciar un servicio, el controlador del servicio inicia sesión con la cuenta que está designada para el servicio y, a continuación, presenta las credenciales del servicio para la autenticación de la LSA. El servicio de Windows implementa una interfaz de programación que puede usar el Administrador de control de servicio para controlar el servicio. Un servicio de Windows se puede iniciar automáticamente cuando se inicia el sistema o manualmente con un programa de control de servicios. Por ejemplo, cuando un equipo cliente de Windows une a un dominio, el servicio messenger en el equipo se conecta a un controlador de dominio y abre un canal seguro a ella. Para obtener una conexión autenticada, el servicio debe tener credenciales en los que confía la autoridad de seguridad Local (LSA) del equipo remoto. Al comunicarse con otros equipos de la red, LSA usa las credenciales de cuenta de dominio del equipo local, igual que todos los demás servicios que se ejecutan en el contexto de seguridad del sistema Local y servicio de red. Servicios en el equipo local que se ejecute como sistema, por lo que no es necesario que las credenciales se presentará la LSA.

El archivo Ksecdd.sys administra y cifra estas credenciales y usa una llamada a procedimiento local en la LSA. El tipo de archivo es DRV (controlador) y se conoce como el proveedor de soporte técnico de seguridad (SSP) de modo kernel y, en las versiones designadas en la **se aplica a** lista al principio de este tema, es FIPS 140-2 nivel compatible con 1.

Modo kernel tiene acceso completo a los recursos de hardware y del sistema del equipo. El modo de núcleo detiene servicios de modo de usuario y aplicaciones tengan acceso a áreas críticas del sistema operativo que no deberían tener acceso a.

## <a name="BKMK_LSA"></a>Autoridad de seguridad local
La autoridad de seguridad Local (LSA) es un proceso del sistema protegido que autentica y cierran sesión en el equipo local de los usuarios. Además, LSA mantiene información sobre todos los aspectos de seguridad local en un equipo (estos aspectos se conocen colectivamente como la directiva de seguridad local), y proporciona varios servicios de traducción entre nombres e identificadores de seguridad (SID). El proceso del sistema de seguridad Local autoridad de seguridad servidor de servicio (LSASS), realiza un seguimiento de las directivas de seguridad y las cuentas que estén vigentes en un sistema informático.

La LSA valida la identidad del usuario en función de cuál de las siguientes dos entidades que emitió la cuenta de usuario:

-   **Autoridad de seguridad local.** La LSA puede validar la información de usuario mediante la comprobación de la base de datos de administrador de cuentas de seguridad (SAM) ubicado en el mismo equipo. Cualquier servidor miembro o estación de trabajo puede almacenar información acerca de los grupos locales y cuentas de usuario locales. Sin embargo, estas cuentas pueden utilizarse para tener acceso a solo esa estación de trabajo o equipo.

-   **Autoridad de seguridad para el dominio local o para un dominio de confianza.** La LSA pone en contacto con la entidad que emitió la cuenta y la comprobación de las solicitudes que la cuenta es válida y que se originó la solicitud del titular de la cuenta.

El Servicio de subsistema de autoridad de seguridad local (LSASS) almacena las credenciales en memoria en representación de los usuarios con sesiones de Windows activas. Las credenciales almacenadas permiten a los usuarios acceder sin problemas a los recursos de red, como recursos compartidos de archivos, buzones de Exchange Server y sitios de SharePoint, sin volver a escribir sus credenciales para cada servicio remoto.

LSASS puede almacenar credenciales de diferentes formas, por ejemplo:

-   Texto sin formato cifrado de manera reversible

-   Vales de Kerberos (vale de concesión de vales (TGT), vales de servicio)

-   Hash de NT

-   Hash de LAN Manager (LM)

Si el usuario inicia sesión en Windows mediante el uso de una tarjeta inteligente, LSASS no almacena una contraseña de texto simple, pero almacena el valor de hash de NT correspondiente para la cuenta y el PIN de texto simple para la tarjeta inteligente. Si se habilita el atributo de la cuenta para una tarjeta inteligente que sea necesaria para el inicio de sesión interactivo, se generará automáticamente un valor hash de NT para la cuenta, en lugar del hash de contraseña original. El hash de contraseña generado automáticamente al establecer el atributo no cambia.

Si un usuario inicia sesión en un equipo basado en Windows con una contraseña que sea compatible con los valores de hash de LAN Manager (LM), este autenticador está presente en la memoria.

El almacenamiento en memoria de credenciales en texto sin formato no se puede deshabilitar, incluso si los proveedores de credenciales requieren que estén deshabilitados.

Las credenciales almacenadas se asocian directamente con los inicios de sesión del servicio de subsistema de autoridad de seguridad Local (LSASS) que se han iniciado después del último reinicio y no se han cerrado. Por ejemplo, las sesiones LSA con credenciales LSA almacenadas se crean cuando un usuario realiza una de las acciones siguientes:

-   Inicie sesión en una sesión local o una sesión de protocolo de escritorio remoto (RDP) en el equipo

-   Ejecuta una tarea mediante la opción **Ejecutar como**

-   Ejecuta un servicio de Windows activo en el equipo

-   Ejecuta una tarea programada o un trabajo por lotes

-   Ejecuta una tarea en el equipo local mediante una herramienta de administración remota

En algunas circunstancias, los secretos de LSA, que son partes secretas de los datos que son accesibles sólo a los procesos de cuenta del sistema, se almacenan en la unidad de disco duro. Algunos de estos secretos son credenciales que deben persistir después de un reinicio y que se almacenan con formato cifrado en la unidad de disco duro. Las credenciales almacenadas como secretos de LSA son los siguientes:

-   Contraseña de cuenta para la cuenta del equipo los servicios de dominio de Active Directory (AD DS)

-   Contraseñas de cuenta para servicios de Windows configurados en el equipo

-   Contraseñas de cuenta para tareas programadas configuradas

-   Contraseñas de cuenta para grupos de aplicaciones y sitios web de IIS

-   Contraseñas de cuentas de Microsoft

Se introdujo en Windows 8.1, el sistema operativo cliente proporciona protección adicional para LSA de cara a evitar la lectura de memoria y la inserción de código por parte de procesos no protegidos. Esta protección aumenta la seguridad de las credenciales que LSA almacena y administra.

Para obtener más información sobre estas protecciones adicionales, consulte [Configuring Additional LSA Protection](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Validación y las credenciales almacenadas en caché
Mecanismos de validación se basan en la presentación de credenciales en el momento de inicio de sesión. Sin embargo, cuando el equipo se desconecta un controlador de dominio y el usuario está presentando las credenciales de dominio, Windows utiliza el proceso de credenciales almacenadas en caché en el mecanismo de validación.

Cada vez que un usuario inicia sesión en un dominio, Windows almacena en caché las credenciales proporcionadas y los almacena en la sección de seguridad en el registro del sistema operativo.

Con las credenciales almacenadas en caché, el usuario puede iniciar sesión en un miembro del dominio sin estar conectados a un controlador de dominio dentro de ese dominio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Almacenamiento de credenciales y la validación
No siempre es deseable utilizar un conjunto de credenciales para acceder a recursos diferentes. Por ejemplo, un administrador puede usar administrativo en lugar de las credenciales de usuario al tener acceso a un servidor remoto. De forma similar, si un usuario tiene acceso a recursos externos, como una cuenta bancaria, que sólo puede utilizar las credenciales que sean diferentes a sus credenciales de dominio. Las secciones siguientes describen las diferencias en la administración de credenciales entre las versiones actuales de los sistemas operativos de Windows y los sistemas operativos Windows Vista y Windows XP.

### <a name="remote-logon-credential-processes"></a>Procesos de credenciales de inicio de sesión remoto
El protocolo de escritorio remoto (RDP) administra las credenciales del usuario que se conecta a un equipo remoto mediante el cliente de escritorio remoto, que se introdujo en Windows 8. Las credenciales en texto sin formato se envían al host de destino donde el host intenta realizar el proceso de autenticación y, si se realiza correctamente, conecta al usuario a los recursos permitidos. RDP no almacena las credenciales en el cliente, pero las credenciales de dominio del usuario se almacenan en LSASS.

Se introdujo en Windows Server 2012 R2 y Windows 8.1, modo de administración restringida proporciona seguridad adicional para escenarios de inicio de sesión remoto. Este modo de escritorio remoto hace que la aplicación cliente realizar un inicio de sesión red desafío / respuesta con la función unidireccional de NT (NTOWF) o usará un vale de servicio de Kerberos al autenticarse en el host remoto. Una vez autenticado el administrador, el administrador no tiene las credenciales de cuenta correspondiente en LSASS porque no se proporcionan para el host remoto. En su lugar, el administrador tiene las credenciales de cuenta de equipo para la sesión. No se proporcionan credenciales de administrador en el host remoto, por lo que se llevan a cabo acciones como la cuenta de equipo. Los recursos también se limitan a la cuenta de equipo y el administrador no puede tener acceso a recursos con su propia cuenta.

### <a name="automatic-restart-sign-on-credential-process"></a>Proceso de reinicio automático de credenciales de inicio de sesión
Cuando un usuario inicia sesión en un dispositivo Windows 8.1, LSA guarda las credenciales de usuario en la memoria cifrada que son accesibles por LSASS.exe. Cuando Windows Update se inicia un reinicio automático sin la presencia de usuario, estas credenciales se utilizan para configurar el inicio de sesión automático para el usuario.

En el reinicio, el usuario ha iniciado automáticamente a través del mecanismo de inicio de sesión automático y, a continuación, el equipo además está bloqueado para proteger la sesión del usuario. El bloqueo se inicia a través de Winlogon, mientras que la administración de credenciales se realiza mediante la LSA. Al iniciar sesión automáticamente en y el bloqueo de la sesión del usuario en la consola, aplicaciones de pantalla de bloqueo del usuario es reinicien y estén disponibles.

Para obtener más información sobre ARSO, consulte [Winlogon automática reiniciar sesión &#40;ARSO&#41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Los nombres de usuario y contraseñas en Windows Vista y Windows XP
En Windows Server 2008, Windows Server 2003, Windows Vista y Windows XP, **almacena nombres de usuario y contraseñas** en el Panel de Control que simplifica la administración y el uso de varios conjuntos de credenciales de inicio de sesión, incluidos los certificados X.509 usados con las tarjetas inteligentes y credenciales de Windows Live (ahora denominadas cuenta de Microsoft). Las credenciales, parte del perfil de usuario: se almacenan hasta que sea necesario. Esta acción puede aumentar la seguridad en una base por recurso al garantizar que si se ve comprometida una contraseña, no comprometa toda la seguridad.

Después de que un usuario inicia sesión y se intenta obtener acceso a recursos protegidos por contraseña adicionales, como un recurso compartido en un servidor, y si las credenciales de inicio de sesión predeterminada del usuario no son suficientes para tener acceso, **almacena nombres de usuario y contraseñas** se realiza una consulta . Si se guardaron las credenciales alternativas con la información de inicio de sesión correcto en **almacena nombres de usuario y contraseñas**, estas credenciales se utilizan para obtener acceso. En caso contrario, se solicita al usuario que proporcione las credenciales nuevo, que pueden guardarse para su reutilización, más adelante en el inicio de sesión o durante una sesión posterior.

Se aplican las restricciones siguientes:

-   Si **almacena nombres de usuario y contraseñas** contiene credenciales incorrectas o no válidas para un recurso específico, el acceso al recurso denegado y el **almacena nombres de usuario y contraseñas** cuadro de diálogo no parecer.

-   **Los nombres de usuario y contraseñas almacenados** almacena las credenciales sólo de NTLM, el protocolo Kerberos, la cuenta de Microsoft (anteriormente Windows Live ID) y la autenticación de capa de Sockets seguros (SSL). Algunas versiones de Internet Explorer mantienen su propia memoria caché para la autenticación básica.

Estas credenciales se convierten en una parte cifrada de un perfil de usuario local en el directorio Data\Microsoft\Credentials Settings\Nombreusuario\Application \Documents and. Como resultado, estas credenciales pueden moverse con el usuario si la directiva de red del usuario es compatible con perfiles de usuario móviles. Sin embargo, si el usuario tiene copias de **almacena nombres de usuario y contraseñas** en dos equipos diferentes y los cambios de las credenciales que están asociadas con el recurso en uno de estos equipos, el cambio no se propaga a  **Los nombres de usuario y contraseñas almacenados** en el segundo equipo.

### <a name="windows-vault-and-credential-manager"></a>Almacén de Windows y el Administrador de credenciales
El Administrador de credenciales se introdujo en Windows Server 2008 R2 y Windows 7 como una característica de Panel de Control para almacenar y administrar las contraseñas y nombres de usuario. El Administrador de credenciales permite a los usuarios almacenar credenciales relevantes para otros sistemas y sitios Web en el almacén seguro de Windows. Algunas versiones de Internet Explorer utilizan esta característica para autenticación en sitios Web.

El usuario controla la administración de credenciales mediante el Administrador de credenciales en el equipo local. Los usuarios pueden guardar y almacenar credenciales desde exploradores y aplicaciones de Windows admitidos para poder iniciar sesión en estos recursos de forma cómoda cuando lo necesiten. Las credenciales se guardan en carpetas cifradas especiales en el equipo en el perfil del usuario. Las aplicaciones que admiten esta característica (mediante el uso de las API del Administrador de credenciales), como los exploradores web y aplicaciones, pueden presentar las credenciales correctas a otros equipos y sitios Web durante el proceso de inicio de sesión.

Cuando otro equipo solicita autenticación a través de NTLM o el protocolo Kerberos, una aplicación o un sitio Web, aparece un cuadro de diálogo en el que seleccionar el **Actualizar credenciales predeterminadas** o **Guardar contraseña**casilla de verificación. Este cuadro de diálogo que permite al usuario guardar localmente las credenciales se genera mediante una aplicación que admite las API del Administrador de credenciales. Si el usuario selecciona el **Guardar contraseña** realiza un seguimiento de la casilla de verificación, el Administrador de credenciales del usuario nombre de usuario, contraseña e información relacionada para el servicio de autenticación que está en uso.

La próxima vez que se usa el servicio, el Administrador de credenciales suministrará automáticamente la credencial que se almacena en el almacén de Windows. Si no se acepta, se solicitará al usuario la información de acceso correcta. Si se concede acceso con las nuevas credenciales, el Administrador de credenciales sobrescribe la credencial anterior con el nuevo y, a continuación, almacena la nueva credencial en el almacén de Windows.

## <a name="BKMK_SAM"></a>Base de datos de administrador de cuentas de seguridad
El Administrador de cuentas de seguridad (SAM) es una base de datos que almacena los grupos y cuentas de usuario locales. Está presente en cada sistema operativo de Windows; Sin embargo, cuando un equipo está unido a un dominio, Active Directory administra las cuentas de dominio en dominios de Active Directory.

Por ejemplo, los equipos cliente que ejecutan el sistema operativo Windows participan en un dominio de red mediante la comunicación con un controlador de dominio, incluso cuando ningún usuario haya iniciado sesión. Para iniciar las comunicaciones, el equipo debe tener una cuenta activa en el dominio. Antes de aceptar comunicaciones desde el equipo, la LSA en el controlador de dominio autentica la identidad del equipo y, a continuación, construye contexto de seguridad del equipo, al igual que para una entidad de seguridad humana. Este contexto de seguridad define la identidad y las capacidades de un usuario o un servicio en un equipo determinado o un usuario, servicio o equipo de la red. Por ejemplo, el token de acceso dentro del contexto de seguridad define los recursos (por ejemplo, un recurso compartido de archivos o una impresora) que se pueden tener acceso y las acciones (como lectura, escritura o modificación) que se pueden realizar esa entidad de seguridad: un usuario, equipo o servicio en la que recurso.

El contexto de seguridad de un usuario o equipo puede variar de un equipo a otro, por ejemplo, cuando un usuario inicia sesión en un servidor o una estación de trabajo que no sean de la estación de trabajo principal del usuario. También puede variar de una sesión a otra, como cuando un administrador modifica los derechos y permisos del usuario. Además, el contexto de seguridad es normalmente diferente cuando un usuario o equipo funciona de forma independiente, en una red, o como parte de un dominio de Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Dominios de confianza y dominios locales
Cuando existe una confianza entre dos dominios, los mecanismos de autenticación para cada dominio dependen de la validez de las autenticaciones procedentes de otro dominio. Confía en Ayuda para proporcionar acceso controlado a los recursos compartidos en un dominio de recursos (el dominio que confía) mediante la comprobación de que solicitudes de esa autenticación entrantes proceden de una autoridad de confianza (el dominio de confianza). De este modo, las confianzas actúan como puentes que permiten sólo validan viaje de solicitudes de autenticación entre dominios.

Cómo una relación de confianza específico pasa las solicitudes de autenticación depende de cómo esté configurada. Las relaciones de confianza pueden ser unidireccional, proporcionando acceso desde el dominio de confianza a los recursos del dominio que confía o bidireccional, proporcionando acceso de cada dominio a los recursos en el otro dominio. Confianzas también son no transitivas, en cuyo caso existe una relación de confianza solo entre los dominios de dos confianza socios o transitiva, en cuyo caso una relación de confianza automáticamente se extiende a ningún otro dominio que confía en cualquiera de los asociados.

Para obtener información acerca de las relaciones de confianza de bosque y dominio relativos a la autenticación, consulte [la autenticación delegada y relaciones de confianza](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificados de autenticación de Windows
Una infraestructura de clave pública (PKI) es la combinación de software, las tecnologías de cifrado, los procesos y servicios que permiten a una organización proteger sus comunicaciones y las transacciones de negocio. La capacidad de una PKI para proteger las comunicaciones y las transacciones de negocio se basa en el intercambio de certificados digitales, entre los usuarios autenticados y recursos de confianza.

Un certificado digital es un documento electrónico que contiene información acerca de la entidad a la que pertenece, la entidad que emitió, un número de serie único o algunos otro identificación única, emisión y fechas de expiración y una huella digital.

La autenticación es el proceso de determinar si se puede confiar en un host remoto. Para establecer el grado de confianza, el host remoto debe proporcionar un certificado de autenticación aceptable.

Los hosts remotos establecen su confiabilidad mediante la obtención de un certificado de una entidad de certificación (CA). La entidad de certificación a su vez, puede tener la certificación de una entidad superior, que crea una cadena de confianza. Para determinar si un certificado es de confianza, una aplicación debe determinar la identidad de la CA raíz y, a continuación, determinar si se trata de confianza.

De forma similar, el equipo local o un host remoto debe determinar si el certificado presentado por el usuario o la aplicación es auténtico. El certificado presentado por el usuario a través de la LSA y SSPI se evalúa para autenticidad en el equipo local para el inicio de sesión local, en la red o en el dominio a través de los almacenes de certificados en Active Directory.

Para generar un certificado, los datos de autenticación pasa a través de algoritmos hash, por ejemplo, el algoritmo Hash seguro 1 (SHA1), para generar una síntesis del mensaje. La síntesis del mensaje, a continuación, está firmada digitalmente mediante el uso de clave privada del remitente para demostrar que el remitente lo ha generado la síntesis del mensaje.

> [!NOTE]
> SHA1 es el valor predeterminado en Windows 7 y Windows Vista, pero se ha cambiado a SHA2 en Windows 8.

**Autenticación mediante tarjeta inteligente**

Tecnología de tarjeta inteligente es un ejemplo de autenticación basada en certificados. Inicia sesión en una red con una tarjeta inteligente proporciona un medio seguro de autenticación porque usa criptografía de identificación y prueba de posesión al autenticar un usuario a un dominio. Los servicios de certificados de Active Directory (AD CS) proporciona la identificación cifrada a través de la emisión de un certificado de inicio de sesión para cada tarjeta inteligente.

Para obtener información acerca de la autenticación de tarjeta inteligente, consulte el [referencia técnica de tarjeta inteligente de Windows](https://technet.microsoft.com/library/ff404297.aspx).

Tecnología de tarjeta inteligente virtual se introdujo en Windows 8. Almacena el certificado de la tarjeta inteligente en el equipo y, a continuación, se protege mediante el uso de chip de seguridad de módulo de plataforma segura (TPM) de prueba de manipulaciones del dispositivo. De este modo, el equipo realmente se convierte en la tarjeta inteligente que debe recibir el PIN del usuario para autenticarse.

**Autenticación remota e inalámbrica**

Autenticación de redes inalámbricas y remota es otra tecnología que usa certificados para la autenticación. Usan el servicio de autenticación de Internet (IAS) y los servidores de red privada virtual para seguridad de nivel de transporte de protocolo de autenticación Extensible (EAP-TLS), protocolo de autenticación Extensible protegido (PEAP) o seguridad de protocolo Internet (IPsec) realizar la autenticación basada en certificados para muchos tipos de acceso de red, incluidas las conexiones inalámbricas y red privada virtual (VPN).

Para obtener información acerca de la autenticación basada en certificados en las redes, consulte [certificados y la autenticación de acceso de red](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Vea también
[Conceptos de autenticación de Windows](https://technet.microsoft.com/library/d169018.aspx)


