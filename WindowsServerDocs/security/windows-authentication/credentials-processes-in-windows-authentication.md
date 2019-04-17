---
title: "Procesos de credenciales de autenticación de Windows"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-processes-in-windows-authentication"></a>Procesos de credenciales de autenticación de Windows

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de referencia para profesionales de TI describe la forma en que la autenticación de Windows procesa las credenciales.

Administración de las credenciales de Windows es el proceso por el que el sistema operativo recibe las credenciales del usuario o servicio y protege esta información para futuras presentación en el destino de autenticación. En el caso de un equipo unido al dominio, el destino de autenticación es el controlador de dominio. Las credenciales usadas para la autenticación son documentos digitales que asociar la identidad del usuario para alguna forma de comprobación de autenticidad, como un certificado, una contraseña o un PIN.

De manera predeterminada, las credenciales de Windows se validan con la base de datos del Administrador de cuentas de seguridad (SAM) en el equipo local o de Active Directory en un equipo unido al dominio, a través del servicio de Winlogon. Las credenciales se recopilan a través de la entrada del usuario en la interfaz de usuario de inicio de sesión o mediante programación a través de la interfaz de programación de aplicaciones (API) que se presente en el destino de autenticación.

Información de seguridad local se almacena en el registro bajo **HKEY_LOCAL_MACHINE\SECURITY**. Información almacenada incluye la configuración de directiva, los valores predeterminados de seguridad y la información de cuenta, como las credenciales de inicio de sesión almacenada en caché. También se almacena una copia de la base de datos de SAM aquí, aunque es protegido contra escritura.

El siguiente diagrama muestra los componentes que son necesarios y las rutas de acceso que credenciales tardar a través del sistema para autenticar el usuario o el proceso de inicio de sesión correcto.

![Diagrama que muestra las rutas de acceso y de los componentes que se necesitan credenciales realizar a través del sistema para autenticar el usuario o el proceso de inicio de sesión correcto.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

La siguiente tabla describe cada componente que administra credenciales en el proceso de autenticación en el punto de inicio de sesión.

**Componentes de autenticación para todos los sistemas**

|Componente|Descripción|
|-------|--------|
|Inicio de sesión de usuario|Winlogon.exe es el archivo ejecutable que se encarga de administrar las interacciones del usuario segura. El servicio de Winlogon inicia el proceso de inicio de sesión para los sistemas operativos Windows pasando las credenciales recopiladas por la acción del usuario en el escritorio seguro (interfaz de usuario de inicio de sesión) a la autoridad de seguridad Local (LSA) a través de Secur32.dll.|
|Inicio de sesión de aplicación|Aplicación o los inicios de sesión de servicio que no requieren el inicio de sesión interactivo. La mayoría de los procesos iniciados por el usuario que se ejecutan en modo de usuario mediante Secur32.dll, mientras que los procesos iniciados en el inicio, como los servicios, que se ejecutan en modo kernel mediante Ksecdd.sys.<br /><br />Para obtener más información acerca del modo de usuario y modo kernel, consulta aplicaciones y el modo de usuario o servicios y modo Kernel en este tema.|
|Secur32.dll|Los varios proveedores de autenticación que forman la base del proceso de autenticación.|
|Lsasrv.dll|El servicio de servidor de LSA, que actúa como el Administrador de paquetes de seguridad para la LSA y aplica directivas de seguridad. La LSA contiene la función de Negotiate, que selecciona el protocolo Kerberos o NTLM después de determinar qué protocolo es se realice correctamente.|
|Proveedores de compatibilidad para seguridad|Un conjunto de proveedores que puede invocar individualmente uno o varios protocolos de autenticación. Puede cambiar el conjunto predeterminado de proveedores con cada versión del sistema operativo Windows, y pueden escribirse proveedores personalizados.|
|Netlogon.dll|Los servicios que realiza el servicio de Net Logon son los siguientes:<br /><br />-Mantiene el canal seguro del equipo (para no deben confundirse con Schannel) un controlador de dominio.<br />-Pasa las credenciales del usuario a través de un canal seguro al controlador de dominio y devuelve los identificadores de seguridad (SID) del dominio y derechos de usuario para que el usuario.<br />-Publica registros de recursos de servicio en el sistema de nombres de dominio (DNS) y utiliza DNS para resolver los nombres para las direcciones de protocolo de Internet (IP) de los controladores de dominio.<br />-Implementa el protocolo de replicación en función de llamada a procedimiento remoto (RPC) para sincronizar los controladores de dominio principal (PDC) y controladores de dominio de reserva (BDC).|
|Samsrv.dll|El Administrador de cuentas de seguridad (SAM), que almacena las cuentas de seguridad local, aplica directivas almacenadas localmente y admite las API.|
|Registro|El registro contiene una copia de la base de datos de SAM, configuración de directiva de seguridad local, valores de seguridad de forma predeterminada e información de cuenta que solo es accesible para el sistema.|

Este tema contiene las siguientes secciones:

-   [Entrada de credenciales de inicio de sesión de usuario](#BKMK_CrentialInputForUserLogon)

-   [Entrada de credenciales para el inicio de sesión de servicio y aplicaciones](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autoridad de seguridad local](#BKMK_LSA)

-   [Validación y las credenciales almacenadas en caché](#BKMK_CachedCredentialsAndValidation)

-   [Almacenamiento de credenciales y validación](#BKMK_CredentialStorageAndValidation)

-   [Base de datos de administrador de cuentas de seguridad](#BKMK_SAM)

-   [Dominios locales y dominios de confianza](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificados de autenticación de Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Entrada de credenciales de inicio de sesión de usuario
En Windows Server 2008 y Windows Vista, la arquitectura de gráficos de identificación y autenticación (GINA) se reemplazó por un modelo de proveedor de credenciales, que permite a enumerar tipos diferentes de inicio de sesión mediante el uso de iconos de inicio de sesión. A continuación se describen los dos modelos.

**Arquitectura de identificación y autenticación gráficas**

La arquitectura de gráficos de identificación y autenticación (GINA) se aplica a los sistemas operativos Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Windows 2000 Professional. En estos sistemas, cada sesión de inicio de sesión interactivo crea una instancia diferente del servicio de Winlogon. La arquitectura de GINA se carga en el espacio de proceso usado por Winlogon, recibe y procesa las credenciales y hace que las llamadas a las interfaces de autenticación a través de LSALogonUser.

Las instancias de Winlogon para un inicio de sesión interactivo ejecutar en sesión 0. Servicios del sistema de hosts de Session 0 y otros procesos críticos, incluido el proceso de autoridad de seguridad Local (LSA).

El siguiente diagrama muestra el proceso de credenciales de Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Microsoft Windows 2000 Professional.

![Diagrama que muestra el proceso de credenciales para Windows Server 2003, Microsoft Windows 2000 Server, Windows XP y Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Arquitectura de proveedor de credenciales**

La arquitectura de proveedor de credenciales se aplica a las versiones indicadas en el **se aplica a** lista al principio de este tema. En estos sistemas, la arquitectura de entrada de credenciales se cambia a un diseño extensible mediante el uso de proveedores de credenciales. Estos proveedores se representan mediante los iconos de inicio de sesión diferente en el escritorio seguro que permiten cualquier número de escenarios de inicio de sesión: cuentas diferentes para el mismo usuario y los métodos de autenticación diferentes, como contraseñas, tarjetas inteligentes y biométrica.

Con la arquitectura de proveedor de credenciales, Winlogon siempre comienza la interfaz de usuario de inicio de sesión después de recibir un evento de la secuencia de aviso de seguridad. Interfaz de usuario de inicio de sesión consulta cada proveedor de credenciales para el número de diferentes tipos de credenciales que el proveedor está configurado para enumerar. Proveedores de credenciales tienen la opción de especificar uno de estos iconos como el valor predeterminado. Después de que todos los proveedores han enumerado sus iconos, interfaz de usuario de inicio de sesión mostrarlos al usuario. El usuario interactúa con un icono para proporcionar sus credenciales. Interfaz de usuario de inicio de sesión envía estas credenciales para la autenticación.

Proveedores de credenciales no están mecanismos de aplicación. Se utilizan para recopilar y serialización de credenciales. Los paquetes de la autoridad de seguridad Local y autenticación reforzar la seguridad.

Proveedores de credenciales se registran en el equipo y son responsables de las siguientes acciones:

-   Describir la información de credenciales necesaria para la autenticación.

-   Control de comunicación y lógica de entidades de autenticación externo.

-   Credenciales de empaquetado para interactivos y de inicio de sesión de red.

Credenciales de empaquetado para interactivos y de inicio de sesión de red incluye el proceso de serialización. Mediante la serialización credenciales varios iconos de inicio de sesión pueden mostrarse en la interfaz de usuario de inicio de sesión. Por lo tanto, la organización puede controlar la pantalla de inicio de sesión, como los usuarios, los sistemas de destino para el inicio de sesión, preinicio acceso a la red y la estación de trabajo bloquear y desbloquear directivas - mediante el uso de proveedores de credenciales personalizada. Varios proveedores de credenciales pueden coexistir en el mismo equipo.

Solo proveedores de inicio de sesión único (SSO) se pueden desarrollar como un proveedor de credenciales estándar o como un proveedor de acceso de inicio de sesión anterior.

Cada versión de Windows contiene el proveedor de credenciales de un valor predeterminado y uno predeterminado previo-inicio de sesión de acceso de proveedor (PLAP), también conocida como el proveedor SSO. El proveedor de inicio de sesión único permite a los usuarios para realizar una conexión a una red antes de iniciar sesión en el equipo local. Cuando se implementa este proveedor, el proveedor no enumerar iconos en la interfaz de usuario de inicio de sesión.

Un proveedor SSO está pensado para usarse en los siguientes escenarios:

-   **Inicio de sesión de autenticación y el equipo de red se controlan mediante proveedores de credenciales diferentes.** Variaciones de este escenario se incluyen:

    -   Un usuario tiene la opción de la conexión a una red, como la conexión a una red privada virtual (VPN), antes de iniciar sesión en el equipo, pero no es necesario para realizar esta conexión.

    -   Se requiere autenticación de red para recuperar la información que se usa durante la autenticación interactiva en el equipo local.

    -   Varias autenticaciones de red se tratan en uno de los otros escenarios. Por ejemplo, un usuario se autentica en un proveedor de servicios de Internet (ISP), autentica a una VPN y, a continuación, usa sus credenciales de cuenta de usuario para iniciar sesión localmente.

    -   Se deshabilitan credenciales almacenadas en caché, y se requiere una conexión de servicios de acceso remoto a través de VPN antes de inicio de sesión local para autenticar al usuario.

    -   Un usuario de dominio no tiene una cuenta local configurado en un equipo unido al dominio y debe establecer una conexión de servicios de acceso remoto a través de la conexión VPN antes de finalizar el inicio de sesión interactivo.

-   **Inicio de sesión de autenticación y el equipo de red se controlan mediante el mismo proveedor de credenciales.** En este escenario, el usuario es necesario para conectarse a la red antes de iniciar sesión en el equipo.

**Enumeración de icono de inicio de sesión**

El proveedor de credenciales enumera los iconos de inicio de sesión en los casos siguientes:

-   Para estos sistemas operativos que se indican en la **se aplica a** lista al principio de este tema.

-   El proveedor de credenciales enumera los iconos de inicio de sesión de la estación de trabajo. El proveedor de credenciales, normalmente serializa credenciales de autenticación a la autoridad de seguridad local. Este proceso aparecen los iconos específicos para cada usuario y específicos para los sistemas de destino de cada usuario.

-   La arquitectura de inicio de sesión y autenticación permite al usuario usar iconos enumerados por el proveedor de credenciales para desbloquear una estación de trabajo. Por lo general, el usuario ha iniciado sesión es el icono predeterminado, pero si más de un usuario inicia sesión, aparecen muchos iconos.

-   El proveedor de credenciales enumera los iconos en respuesta a una solicitud de usuario para cambiar su contraseña u otros datos privados, como un PIN. Por lo general, el usuario ha iniciado sesión es el icono predeterminado; Sin embargo, si más de un usuario inicia sesión, aparecen muchos iconos.

-   El proveedor de credenciales enumera los iconos de acuerdo con las credenciales que se usará para la autenticación en los equipos remotos serializadas. Credenciales de interfaz de usuario no utiliza la misma instancia del proveedor como la interfaz de usuario de inicio de sesión, desbloquear la estación de trabajo o para cambiar la contraseña. Por lo tanto, no se puede mantener información de estado en el proveedor entre instancias de la interfaz de usuario de credenciales. Esta estructura como resultado un icono para cada inicio de sesión del equipo remoto, suponiendo que las credenciales se han serializado correctamente. Este escenario también se usa en el Control de cuentas de usuario (UAC), que puede ayudar a evitar cambios no autorizados en un equipo que pedir al usuario permiso o una contraseña de administrador antes de permitir que las acciones que podría afectar posiblemente las operaciones del equipo o podría cambiar algunas configuraciones que afectan a otros usuarios del equipo.

El siguiente diagrama muestra el proceso de credenciales para los sistemas operativos que se indican en la **se aplica a** lista al principio de este tema.

![Diagrama que muestra el proceso de credenciales para los sistemas operativos que se indican en la ** se aplica a ** lista al principio de este tema](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrada de credenciales para el inicio de sesión de servicio y aplicaciones
Autenticación de Windows está diseñada para administrar credenciales para aplicaciones o servicios que no requieren interacción del usuario. Las aplicaciones en modo de usuario son limitadas en cuanto a los recursos del sistema tienen acceso a, mientras que los servicios pueden tienen acceso ilimitado a la memoria del sistema y los dispositivos externos.

Servicios del sistema y aplicaciones de transporte obtener acceso a un proveedor de soporte técnico de seguridad (SSP) a través de la interfaz de proveedor de soporte técnico de seguridad (SSPI) en Windows, que proporciona funciones para enumerar los paquetes de seguridad disponibles en un sistema, seleccionar un paquete y el uso de ese paquete para obtener una conexión autenticada.

Cuando se autentica una conexión de cliente/servidor:

-   La aplicación en el lado del cliente de la conexión envía las credenciales al servidor mediante la función SSPI `InitializeSecurityContext (General)`.

-   La aplicación en el servidor de la conexión responde con la función SSPI `AcceptSecurityContext (General)`.

-   Las funciones SSPI `InitializeSecurityContext (General)` y `AcceptSecurityContext (General)` se repite hasta que se han intercambiado todos los mensajes de autenticación necesarias para tener éxito o un error de autenticación.

-   Después de que se ha autenticado la conexión, la LSA en el servidor usa la información del cliente para crear el contexto de seguridad, que contiene un token de acceso.

-   El servidor, a continuación, llamar a la función SSPI `ImpersonateSecurityContext` adjuntar el token de acceso a un subproceso de representación para el servicio.

**Aplicaciones y el modo de usuario**

Modo de usuario en Windows se compone de dos sistemas capaces de pasar las solicitudes de E/S a los controladores de modo kernel adecuada: el sistema del entorno, que se ejecuta en aplicaciones escritas para diferentes tipos de sistemas operativos, y el sistema integral, que funciona funciones específicas del sistema en nombre del sistema del entorno.

El sistema integral administra funciones system'specific operativas en nombre del sistema del entorno y consta de un proceso del sistema de seguridad (LSA), un servicio de la estación de trabajo y un servicio de servidor. El proceso del sistema de seguridad se ocupa de tokens de seguridad, concede o deniega permisos para acceder a las cuentas de usuario en función de los permisos de recursos, controla las solicitudes de inicio de sesión e inicia la autenticación de inicio de sesión y determina qué recursos del sistema necesita el sistema operativo para auditar.

Las aplicaciones pueden ejecutar en modo de usuario donde la aplicación se puede ejecutar como cualquier principal, incluidos en el contexto de seguridad de sistema Local (sistema). También pueden ejecutar aplicaciones en modo kernel que puede ejecutar la aplicación en el contexto de seguridad de sistema Local (sistema).

SSPI está disponible a través del módulo Secur32.dll, que es una API usada para obtener servicios de seguridad integrada para la autenticación, la integridad de los mensajes y la privacidad de los mensajes. Proporciona una capa de abstracción entre los protocolos de nivel de la aplicación y los protocolos de seguridad. Debido a las distintas aplicaciones requieren modos de identificar o autenticar usuarios y modos de cifrado de datos se transmite a través de una red, SSPI proporciona una forma de obtener acceso a bibliotecas de vínculos dinámicos (DLL) que contienen funciones criptográficas y autenticación diferentes. Estos archivos DLL se denominan proveedores de compatibilidad para seguridad (SSP).

Administra el servicio de cuentas y virtual se introdujeron en Windows Server 2008 R2 y Windows 7 para proporcionar aplicaciones esenciales, como Microsoft SQL Server y de Internet Information Services (IIS), el aislamiento de sus propias cuentas de dominio, mientras que elimina la necesidad de un administrador manualmente administrar el nombre de entidad de seguridad de servicio (SPN) y credenciales para estas cuentas. Para obtener más información sobre estas características y su función en la autenticación, consulta [administrado servicio de cuentas de documentación para Windows 7 y Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) y [Introducción a las cuentas servicio grupo administrado](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Modo kernel y servicios**

Aunque la mayoría de las aplicaciones Windows se ejecuta en el contexto de seguridad del usuario que inicia, esto no es cierto de servicios. Muchos de los servicios de Windows, como la red y servicios de impresión, se inician mediante el controlador de servicio cuando el usuario inicie el equipo. Estos servicios podrían ejecutarse como sistema Local y servicio Local y pueden seguir ejecutándose después cierra la sesión del último usuario humano.

> [!NOTE]
> Normalmente, los servicios se ejecutan en contextos de seguridad conocidos como sistema Local (sistema), el servicio de red o el servicio Local.  Windows Server 2008 R2 introdujo los servicios que se ejecutan con una cuenta de servicio administrada, que son las entidades de dominio.

Antes de iniciar un servicio, el controlador del servicio inicia sesión con la cuenta que está designada para el servicio y, a continuación, presenta las credenciales del servicio de autenticación de LSA. El servicio de Windows implementa una interfaz de programación que puedes usar el Administrador de controlador de servicio para controlar el servicio. Un servicio de Windows se puede iniciar automáticamente cuando se inicie el sistema o manualmente con un programa de control de servicios. Por ejemplo, cuando un equipo de cliente de Windows se une a un dominio, el servicio de messenger en el equipo se conecta a un controlador de dominio y abre un canal seguro a ella. Para obtener una conexión autenticada, el servicio debe tener las credenciales que confía la autoridad de seguridad Local (LSA) del equipo remoto. Cuando se comunica con otros equipos de la red, la LSA usa las credenciales de cuenta de dominio del equipo local, igual que todos los demás servicios que se ejecutan en el contexto de seguridad del sistema Local y servicio de red. Servicios en el equipo local que se ejecuten como sistema para que las credenciales no es necesario aparecer en la LSA.

El archivo Ksecdd.sys administra y cifra estas credenciales y usa una llamada a procedimiento local en la LSA. El tipo de archivo es la unidad (controlador) y se conoce como el proveedor de soporte técnico de seguridad (SSP) de modo kernel y, en las versiones indicadas en el **se aplica a** lista al principio de este tema, es FIPS 140-2 nivel compatible con 1.

El modo de kernel tiene acceso completo a los recursos de hardware y del sistema del equipo. El modo kernel deja de servicios de modo de usuario y accedan a áreas críticas del sistema operativo que no deben tener acceso a las aplicaciones.

## <a name="BKMK_LSA"></a>Autoridad de seguridad local
La autoridad de seguridad Local (LSA) es un proceso del sistema protegido que autentica y registra los usuarios en el equipo local. Además, la LSA mantiene información acerca de todos los aspectos de seguridad local en un equipo (estos aspectos se conocen como la directiva de seguridad local), y proporciona varios servicios de traducción entre los nombres y los identificadores de seguridad (SID). El proceso de sistema de seguridad, seguridad autoridad servidor Servicio Local (LSASS), realiza un seguimiento de las directivas de seguridad y las cuentas que están en vigor en un equipo.

La LSA valida la identidad de un usuario en función de cuál de las siguientes dos entidades emitidas la cuenta del usuario:

-   **Autoridad de seguridad local.** La LSA puede validar la información del usuario mediante la comprobación de la base de datos del Administrador de cuentas de seguridad (SAM) ubicado en el mismo equipo. Cualquier servidor miembro o estación de trabajo puede almacenar información acerca de los grupos locales y cuentas de usuario locales. Sin embargo, estas cuentas pueden usarse para acceder a solo ese equipo o estación de trabajo.

-   **Autoridad de seguridad para el dominio local o para un dominio de confianza.** La LSA pone en contacto con la entidad que emitió la cuenta y la comprobación de las solicitudes que la cuenta es válida y que la solicitud proviene del titular de la cuenta.

El servicio de subsistema del entidad de seguridad (LSASS) Local almacena las credenciales en la memoria en el nombre de los usuarios que tengan sesiones activas de Windows. Las credenciales almacenadas permiten a los usuarios acceder sin problemas a los recursos de red, como recursos compartidos de archivos, los buzones de Exchange Server y los sitios de SharePoint, sin necesidad de volver a escribir sus credenciales para cada servicio remoto.

LSASS puede almacenar las credenciales en varios formatos, incluidos:

-   Cifrado reversible texto no cifrado

-   Vales Kerberos (vales de concesión de vales (TGT), vales de servicio)

-   Hash de NT

-   Hash de LAN Manager (LM)

Si el usuario inicia sesión en Windows mediante el uso de una tarjeta inteligente, LSASS no almacena una contraseña de texto no cifrado, pero almacena el valor de hash de NT correspondiente para la cuenta y el texto no cifrado PIN de la tarjeta inteligente. Si se habilita el atributo de cuenta para una tarjeta inteligente que se necesita para el inicio de sesión interactivo, se genera automáticamente un valor de hash de NT aleatorio de la cuenta en lugar de hash de contraseña original. No cambiar el valor de hash de contraseña que se genera automáticamente cuando se establece el atributo.

Si un usuario inicia sesión en un equipo basado en Windows con una contraseña que sea compatible con los valores de hash de LAN Manager (LM), este autenticador está presente en la memoria.

No se puede deshabilitar el almacenamiento de credenciales de texto no cifrado en la memoria incluso si se deshabilitan los proveedores de credenciales que los necesitan.

Las credenciales almacenadas están asociadas directamente con las sesiones de inicio de sesión de servicio de subsistema de autoridad de seguridad Local (LSASS) que se ha iniciado después de la última se reinicia y se han cerrado. Por ejemplo, las sesiones LSA con credenciales LSA almacenadas se crean cuando un usuario hace cualquiera de las siguientes acciones:

-   Inicia sesión en una sesión local o una sesión de protocolo de escritorio remoto (RDP) en el equipo

-   Se ejecuta una tarea mediante el **RunAs** opción

-   Ejecuta un servicio de Windows activo en el equipo

-   Se ejecuta un trabajo programado de tarea o por lotes

-   Ejecuta una tarea en el equipo local mediante una herramienta de administración remota

En algunos casos, se almacenan los secretos de LSA, que son secretas fragmentos de datos que se puede acceder únicamente a los procesos de la cuenta de sistema, en la unidad de disco duro. Algunos de estos secretos son las credenciales que deben mantenerse después del reinicio, y se almacenan de forma cifrada en la unidad de disco duro. Las credenciales almacenadas como secretos de LSA podrían ser:

-   Contraseña de cuenta de la cuenta del equipo los servicios de dominio de Active Directory (AD DS)

-   Contraseñas de cuentas para los servicios de Windows que están configurados en el equipo

-   Contraseñas de cuentas para las tareas programadas configuradas

-   Contraseñas de cuenta para la aplicación IIS y sitios Web

-   Contraseñas de cuentas de Microsoft

Introducidas en Windows 8.1, el sistema operativo cliente ofrece protección adicional para la LSA inyección de código no protegidas procesos y evitar que la memoria de lectura. Esta protección aumenta la seguridad de las credenciales que la LSA almacena y administra.

Para obtener más información sobre estas protecciones adicionales, consulta [configurar la protección adicional de LSA](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Validación y las credenciales almacenadas en caché
Mecanismos de validación dependen de la presentación de credenciales en el momento de inicio de sesión. Sin embargo, cuando el equipo esté desconectado de un controlador de dominio y el usuario es presentar las credenciales de dominio, Windows usa el proceso de credenciales almacenadas en caché en el mecanismo de validación.

Cada vez que un usuario inicia sesión en un dominio, Windows almacena en caché las credenciales proporcionadas y los almacena en el subárbol de seguridad en el registro del sistema operativo.

Con las credenciales almacenadas en caché, el usuario puede iniciar sesión a un miembro de dominio sin estar conectado a un controlador de dominio en ese dominio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Almacenamiento de credenciales y validación
No siempre es conveniente usar un conjunto de credenciales para acceder a recursos diferentes. Por ejemplo, un administrador convendría usar administrativas en lugar de las credenciales de usuario al obtener acceso a un servidor remoto. Del mismo modo, si un usuario accede a recursos externos, como una cuenta bancaria, autorizó sólo puede utilizar las credenciales que son diferentes de sus credenciales de dominio. Las siguientes secciones describen las diferencias en la administración de credenciales entre las versiones actuales de los sistemas operativos Windows y los sistemas operativos Windows Vista y Windows XP.

### <a name="remote-logon-credential-processes"></a>Procesos de credenciales de inicio de sesión remotos
El protocolo de escritorio remoto (RDP) administra las credenciales del usuario que se conecta a un equipo remoto mediante el cliente de escritorio remoto, que se introdujo en Windows 8. Las credenciales en texto sin formato se envían al host de destino donde el host intenta realizar el proceso de autenticación y, si se realiza correctamente, conecta al usuario a los recursos de aplicaciones permitidos. RDP no guarda las credenciales en el cliente, pero las credenciales de dominio del usuario se almacenan en el LSASS.

Introducidas en Windows Server 2012 R2 y Windows 8.1, el modo de administrador restringido proporciona seguridad adicional para escenarios de inicio de sesión remotos. Este modo de escritorio remoto hace que la aplicación cliente realizar un inicio de sesión red desafío y respuesta con la función unidireccional de NT (NTOWF) o utilizar un vale de servicio Kerberos para autenticar con el host remoto. Después de autentica el administrador, el administrador no tiene las credenciales de cuenta respectivos en LSASS porque no se proporcionan para el host remoto. En su lugar, el administrador tiene las credenciales de cuenta de equipo para la sesión. Credenciales de administrador no se suministran con el host remoto, por lo que se llevan a cabo acciones como la cuenta de equipo. Recursos también están limitados a la cuenta de equipo y el administrador no puede acceder a recursos con su propia cuenta.

### <a name="automatic-restart-sign-on-credential-process"></a>Proceso de reinicio automático de credenciales de inicio de sesión
Cuando un usuario inicia sesión en un dispositivo de Windows 8.1, LSA guarda las credenciales del usuario en la memoria cifrado que sólo tienen acceso a LSASS.exe. Cuando Windows Update se inicia un reinicio automático sin la presencia del usuario, estas credenciales se usan para configurar el inicio de sesión automático para el usuario.

En el reinicio, el usuario inicie automáticamente la sesión mediante el mecanismo de inicio de sesión automático y, a continuación, el equipo está bloqueado además para proteger la sesión del usuario. El bloqueo se inicia a través de Winlogon, mientras que la administración de credenciales se realiza mediante la LSA. Al iniciar sesión automáticamente y el bloqueo de la sesión del usuario en la consola, las aplicaciones de pantalla de bloqueo del usuario está reiniciado y disponible.

Para obtener más información sobre ARSO, consulta [Winlogon automática reiniciar sesión & #40; ARSO & #41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Los nombres de usuario y contraseñas en Windows Vista y Windows XP
En Windows Server 2008, Windows Server 2003, Windows Vista y Windows XP, **los nombres de usuario y contraseñas** en el Panel de Control, simplifica la administración y el uso de varios conjuntos de credenciales de inicio de sesión, incluidos los certificados X.509 utilizados con tarjetas inteligentes y las credenciales de Windows Live (ahora denominadas cuenta de Microsoft). Se almacenan las credenciales - parte del perfil de usuario - hasta que sea necesario. Esta acción puede aumentar la seguridad de forma según el recurso asegurándose de que si se compromete una contraseña, no comprometa toda la seguridad.

Después de que un usuario inicia sesión e intenta obtener acceso a recursos protegidos por contraseña adicionales, como un recurso compartido en un servidor, y si las credenciales del usuario predeterminado de inicio de sesión no son suficientes para obtener acceso, **los nombres de usuario y contraseñas** se consulta. Si se han guardado credenciales alternativas con la información de inicio de sesión correcto en **los nombres de usuario y contraseñas**, se usan estas credenciales para obtener acceso. De lo contrario, se pide al usuario para proporcionar a las nuevas credenciales, que, a continuación, se pueden guardar para su reutilización, más adelante en el inicio de sesión o durante una sesión posterior.

Se aplican las siguientes restricciones:

-   Si **los nombres de usuario y contraseñas** contiene válidas o es incorrectas de credenciales para un recurso específico, acceso al recurso denegado y el **los nombres de usuario y contraseñas** no aparece el cuadro de diálogo.

-   **Nombres de usuario y contraseñas almacenados** almacena las credenciales solo para NTLM, el protocolo Kerberos, cuenta de Microsoft (anteriormente Windows Live ID) y autenticación de capa de Sockets seguros (SSL). Algunas versiones de Internet Explorer Mantén su propia memoria caché para la autenticación básica.

Estas credenciales se convierten en una parte de un perfil de usuario local en el directorio \Documents y Settings\nombreDeUsuario\Datos Data\Microsoft\Credentials cifrada. Como resultado, estas credenciales se pueden muevan con el usuario si la directiva del usuario red admite perfiles móviles de usuario. Sin embargo, si el usuario tiene copias de **los nombres de usuario y contraseñas** en dos equipos diferentes y cambios de las credenciales asociadas con el recurso en uno de estos equipos, el cambio no se propaga a **los nombres de usuario y contraseñas** en el segundo equipo.

### <a name="windows-vault-and-credential-manager"></a>Credenciales de Windows y el Administrador de credenciales
El Administrador de credenciales se introdujo en Windows Server 2008 R2 y Windows 7 como una característica de Panel de Control para almacenar y administrar las contraseñas y nombres de usuario. El Administrador de credenciales permite a los usuarios almacenar las credenciales correspondientes a los sistemas y sitios Web en el almacén seguro de Windows. Algunas versiones de Internet Explorer usan esta característica para la autenticación a los sitios Web.

Administración de credenciales mediante el Administrador de credenciales está controlada por el usuario en el equipo local. Los usuarios guardar y almacenar credenciales de exploradores y aplicaciones de Windows para que resulte cómodo cuando los necesitan iniciar sesión en estos recursos. Las credenciales se guardan en carpetas cifradas especiales en el equipo en el perfil del usuario. Las aplicaciones que admiten esta característica (mediante el uso de las API del Administrador de credenciales), como exploradores web y aplicaciones, pueden presentar las credenciales correctas a otros equipos y sitios Web durante el proceso de inicio de sesión.

Cuando un sitio Web, una aplicación o en otro equipo solicita la autenticación mediante el protocolo Kerberos o NTLM, aparece un cuadro de diálogo en el que seleccionar el **actualización de credenciales predeterminadas** o **Guardar contraseña** casilla de verificación. Este cuadro de diálogo que permite al usuario guardar las credenciales localmente generado por una aplicación que admite las API del Administrador de credenciales. Si el usuario selecciona el **Guardar contraseña** casilla de verificación, realiza un seguimiento de administrador de credenciales el nombre de usuario, contraseña, e información relacionada con el servicio de autenticación que está en uso.

La próxima vez que se usa el servicio, el Administrador de credenciales proporciona automáticamente la credencial que se almacena en el almacén de Windows. Si no se acepta, se pide al usuario la información de acceso correcta. Si se concede acceso con las nuevas credenciales, el Administrador de credenciales sobrescribe la credencial anterior con la nueva y, a continuación, almacena la nueva credencial en el almacén de Windows.

## <a name="BKMK_SAM"></a>Base de datos de administrador de cuentas de seguridad
El Administrador de cuentas de seguridad (SAM) es una base de datos que almacena los grupos y cuentas de usuario locales. Está presente en todos los sistemas operativos Windows; Sin embargo, cuando un equipo está unido a un dominio, Active Directory administra las cuentas de dominio de dominios de Active Directory.

Por ejemplo, equipos cliente que ejecutan un sistema operativo Windows participan en un dominio de red mediante la comunicación con un controlador de dominio, incluso cuando no está conectado ningún usuario humano. Para iniciar las comunicaciones, el equipo debe tener una cuenta activa en el dominio. Antes de aceptar comunicaciones desde el equipo, la LSA en el controlador de dominio autentica la identidad del equipo y a continuación, crea el contexto de seguridad del equipo como lo hace con una entidad de seguridad humana. Este contexto de seguridad define la identidad y las funcionalidades de un usuario o un servicio en un equipo en particular o un usuario, servicio o equipo en una red. Por ejemplo, el token de acceso dentro del contexto de seguridad define los recursos (como un recurso compartido de archivos o una impresora) que pueden tener acceso y las acciones (por ejemplo, lectura, escritura o modificar) que se pueden realizar a ese principal: un usuario, el equipo o el servicio en la que recurso.

El contexto de seguridad de un usuario o equipo puede variar de un equipo a otro, por ejemplo, cuando un usuario inicia sesión en un servidor o una estación de trabajo que no sean de la estación de trabajo principal del usuario. También puede variar de una sesión a otra, por ejemplo, cuando un administrador modifica los derechos y permisos del usuario. Además, el contexto de seguridad es normalmente diferente cuando un usuario o equipo funciona de forma independiente, en una red, o como parte de un dominio de Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Dominios locales y dominios de confianza
Cuando existe una relación de confianza entre dos dominios, los mecanismos de autenticación para cada dominio dependen de la validez de las autenticaciones procedente de otro dominio. Confía ayuda a proporcionar acceso controlado a recursos compartidos en un dominio de recursos (el dominio que confía), comprobando que la autenticación entrante solicita proceden de una entidad de confianza (el dominio de confianza). De este modo, confianzas actúan como puentes que solo permiten validan viajes de solicitudes de autenticación entre dominios.

Cómo una relación de confianza específicas pasa las solicitudes de autenticación depende de cómo esté configurado. Relaciones de confianza pueden ser unidireccional, proporcionando acceso desde el dominio de confianza a los recursos en el dominio de confianza o bidireccional, proporcionando acceso de cada dominio a los recursos en el otro dominio. Relaciones de confianza también son intransitivas, en cuyo caso existe una confianza solo entre los dominios de socio de confianza de dos o transitiva, en cuyo caso extiende automáticamente una relación de confianza a ningún otro dominio que confía en cualquiera de los socios.

Para obtener información sobre las relaciones de confianza de bosque y dominio en relación con la autenticación, vea [delegado autenticación y relaciones de confianza](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificados de autenticación de Windows
Una infraestructura de clave pública (PKI) es la combinación de software, las tecnologías de cifrado, procesos y servicios que permiten una organización proteger sus comunicaciones y las transacciones comerciales. La capacidad de una PKI para proteger las comunicaciones y las transacciones comerciales se basa en el intercambio de certificados digitales entre usuarios autenticados y recursos de confianza.

Un certificado digital es un documento electrónico que contiene información sobre la entidad a la que pertenece, la entidad que emitió, un número de serie único o algunos otro identificación única, emisión y las fechas de caducidad y una huella digital.

Autenticación es el proceso de determinación de si se puede confiar en un host remoto. Para establecer el grado de confianza, el host remoto debe proporcionar un certificado de autenticación aceptable.

Los hosts remotos establecen el grado de confianza mediante la obtención de un certificado de una entidad de certificación (CA). La entidad de certificación a su vez, puedes tener la certificación de una autoridad superior, que crea una cadena de confianza. Para determinar si un certificado es de confianza, una aplicación debe determinar la identidad de la CA raíz y, a continuación, determina si es de confianza.

Del mismo modo, el equipo local o host remoto debe determinar si el certificado presentado por el usuario o la aplicación es auténtico. El certificado proporcionado por el usuario a través de la LSA y SSPI se evalúa autenticidad en el equipo local para el inicio de sesión local, en la red o en el dominio a través de los almacenes de certificados de Active Directory.

Para generar un certificado, los datos de autenticación pasa a través de algoritmos de hash, por ejemplo, el algoritmo Hash seguro 1 (SHA1), para generar un resumen de mensaje. La síntesis del mensaje, a continuación, se firma digitalmente usando la clave privada del remitente para demostrar que se creó la síntesis del mensaje por el remitente.

> [!NOTE]
> SHA1 es el predeterminado de Windows 7 y Windows Vista, pero se ha cambiado a SHA2 en Windows 8.

**Autenticación con tarjeta inteligente**

Tecnología de tarjetas inteligentes es un ejemplo de autenticación basada en certificados. Inicio de sesión una red con una tarjeta inteligente proporciona un formulario seguro de autenticación porque usa identificación basada en criptografía y prueba de posesión cuando el usuario se autentica en un dominio. Los servicios de certificados de Active Directory (AD CS) proporciona la identificación cifrada a través de la emisión de un certificado de inicio de sesión para cada tarjeta inteligente.

Para obtener información sobre la autenticación con tarjeta inteligente, consulta el [referencia técnica de tarjeta inteligente de Windows](https://technet.microsoft.com/library/ff404297.aspx).

Tecnología de tarjeta inteligente virtual se introdujo en Windows 8. Almacena el certificado de la tarjeta inteligente en el equipo y, a continuación, se protege mediante el chip de seguridad de módulo de plataforma segura (TPM) del dispositivo a prueba de manipulaciones. De este modo, el equipo realmente se convierte en la tarjeta inteligente que deben recibir el PIN del usuario para su autenticación.

**Autenticación remota e inalámbrica**

Autenticación de red inalámbrica y remoto es otra tecnología que usa certificados para la autenticación. El servicio de autenticación de Internet (IAS) y los servidores de red privada virtual usan seguridad de nivel de transporte de protocolo de autenticación Extensible (EAP-TLS), el protocolo de autenticación Extensible protegido (PEAP) o seguridad de protocolo de Internet (IPsec) para realizar la autenticación basada en certificados para muchos tipos de acceso a la red, como una red privada virtual (VPN) y conexiones inalámbricas.

Para obtener información acerca de la autenticación basada en certificados en las redes, consulte [certificados y autenticación de acceso de red](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Consulta también
[Conceptos de autenticación de Windows](https://technet.microsoft.com/library/d169018.aspx)


