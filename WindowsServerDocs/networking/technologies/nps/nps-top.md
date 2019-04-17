---
title: Servidor de directivas de redes (NPS)
description: Este tema proporciona una descripción general de servidor de directivas de red en Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f48cbdaec82e9e60f734a4a8949082187b13166
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-nps"></a>Servidor de directivas de redes (NPS)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información general de servidor de directivas de red en Windows Server 2016.

>[!NOTE]
>Además de este tema, la siguiente documentación NPS está disponible.
> - [Procedimientos recomendados de servidor de directivas de red](nps-best-practices.md)
> - [Tareas iniciales con el servidor de directivas de red](nps-getstart-top.md)
> - [Planear el servidor de directivas de red](nps-plan-top.md)
> - [Implementar el servidor de directivas de red](nps-deploy.md)
> - [Administrar el servidor de directivas de red](nps-manage-top.md)
> - [Cmdlets de servidor (NPS) de directiva de red](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2016 y Windows 10
> - [Cmdlets de directiva de servidor (NPS) en Windows PowerShell de red](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 R2 y Windows 8.1
> - [Cmdlets NPS en Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 y Windows 8

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización de autorización y autenticación de solicitud de conexión.

También puedes configurar NPS como un proxy de servicio de autenticación remota telefónica de usuario (RADIUS) para enviar solicitudes de conexión a un NPS remoto u otro servidor RADIUS para que pueda cargar equilibrar las solicitudes de conexión y reenviarlos al dominio correcto para la autenticación y autorización.

NPS permite configurar y administrar cuentas con las siguientes características, autorización y autenticación de acceso de red de forma centralizada:

- **Servidor RADIUS**. NPS realiza centralizada la autenticación, autorización y contabilidad para redes inalámbricas, autenticación conmutador, acceso remoto telefónico y conexiones de red privada virtual (VPN). Cuando usas NPS como un servidor de radio, se configuran los servidores de acceso de red, como puntos de acceso inalámbrico y servidores VPN, como clientes RADIUS en NPS. También configurar las directivas de red que usa NPS para autorizar las solicitudes de conexión y puede configurar cuentas RADIUS para que NPS registra información de cuentas para registrar los archivos en el disco duro local o en una base de datos de Microsoft SQL Server. Para obtener más información, consulta [RADIUS server](#bkmk_server).
- **Proxy RADIUS**. Cuando se usa NPS como un proxy RADIUS, configurar las directivas de la solicitud de conexión que indican el servidor NPS qué solicitudes de conexión para reenviar a otros servidores RADIUS y a qué servidores RADIUS que quieras reenviar las solicitudes de conexión. También puedes configurar NPS para reenviar datos de cuentas para iniciar sesión por uno o más equipos en un grupo de servidores RADIUS remotos. Para configurar NPS como un servidor proxy RADIUS, consulta los siguientes temas. Para obtener más información, consulta [proxy RADIUS](#bkmk_proxy).
    - [Configurar las directivas de solicitud de conexión](nps-crp-configure.md)
- **Cuentas RADIUS**. Puedes configurar NPS para registrar eventos en un archivo de registro local o a una instancia local o remota de Microsoft SQL Server. Para obtener más información, consulta [registro NPS](#bkmk_logging).

>[!IMPORTANT]
>Red protección de acceso a \(NAP\), \(HRA\) entidad de registro de salud y protocolo de autorización de credenciales de Host \(HCAP\) quedaron en desuso en Windows Server 2012 R2 y no están disponibles en Windows Server 2016. Si tienes una implementación de NAP con sistemas operativos anteriores a Windows Server 2016, la implementación de NAP no puede migrar a Windows Server 2016.

Puedes configurar NPS con cualquier combinación de estas características. Por ejemplo, puede configurar un servidor NPS como un servidor de radio para las conexiones VPN y también como un proxy de radio para reenviar algunas solicitudes de conexión a los miembros de un grupo de servidores RADIUS remoto para autenticación y autorización en otro dominio.

## <a name="windows-server-editions-and-nps"></a>Las ediciones de servidor de Windows y NPS

NPS proporciona funcionalidad diferente según la edición de Windows Server que instales.

### <a name="windows-server-2016-datacenter-edition"></a>Windows Server 2016 Datacenter Edition

Con NPS en Windows Server 2016 Datacenter, puedes configurar un número ilimitado de clientes RADIUS y grupos de servidores RADIUS remotos. Además, puedes configurar a los clientes RADIUS mediante la especificación de un intervalo de direcciones IP.

### <a name="windows-server-2016-standard-edition"></a>Windows Server 2016 Standard Edition

Con NPS en Windows Server 2016 estándar, puedes configurar un máximo de 50 clientes RADIUS y un máximo de 2 grupos de servidores RADIUS remotos. Puedes definir a un cliente RADIUS mediante un nombre de dominio completo o una dirección IP, pero no se puede definir grupos de clientes de RADIUS mediante la especificación de un intervalo de direcciones IP. Si el nombre de dominio completo de un cliente RADIUS se resuelve en varias direcciones IP, el servidor NPS usa la primera dirección IP devuelta en la consulta del sistema de nombres de dominio (DNS).

Las secciones siguientes proporcionan información más detallada sobre NPS como un proxy y servidor RADIUS.

## <a name="radius-server-and-proxy"></a>Proxy y servidor RADIUS

Puedes usar NPS como un servidor RADIUS, un proxy RADIUS o ambos.

### <a name="bkmk_server"></a>Servidor RADIUS

NPS es la implementación de Microsoft del estándar RADIUS especificada por el \(IETF\) grupo de trabajo de ingeniería de Internet en RFC 2865 y 2866. Como un servidor RADIUS, NPS realiza Contabilidad, autorización y autenticación de conexión centralizada para muchos tipos de acceso a la red, incluidos inalámbrica, autenticar modificador, telefónico y acceso remoto de red privada virtual \(VPN\) y conexiones a enrutador.

>[!NOTE]
>Para obtener información sobre cómo implementar NPS como un servidor RADIUS, vea [implementar el servidor de directivas de red](nps-deploy.md).

NPS permite el uso de un conjunto heterogénea de conexión inalámbrica, cambiar, acceso remoto o equipos de VPN. Puedes usar NPS con el servicio de acceso remoto, que está disponible en Windows Server 2016.

NPS usa un dominio de Active Directory Domain Services \(AD DS\) o el Administrador de cuentas de seguridad (SAM) usuario cuentas base de datos local para autenticar las credenciales de usuario para la conexión de intentos. Cuando un servidor que ejecuta NPS es miembro de un dominio de AD DS, NPS usa el servicio de directorio como su base de datos de la cuenta de usuario y forma parte de una solución de inicio de sesión único. Se usa el mismo conjunto de credenciales para el control de acceso de red \ (autenticar y autorizar el acceso a un red\) y para iniciar sesión en un dominio de AD DS.

>[!NOTE]
>NPS usa las propiedades de marcación rápida de las directivas de red y la cuenta de usuario para autorizar la conexión.

Internet servicio proveedores \(ISPs\) y las organizaciones que mantienen el acceso de red tienen el mayor desafío de administrar todos los tipos de acceso a la red desde un único punto de administración, independientemente del tipo de equipo de acceso de red usado. El estándar RADIUS admite esta funcionalidad en entornos homogéneos y heterogéneos. RADIO es un protocolo de cliente / servidor que permite el equipo de acceso de red (usado como clientes RADIUS) para enviar solicitudes de autenticación y administración de cuentas en un servidor RADIUS.

Un servidor RADIUS tiene acceso a información de cuenta de usuario y puede comprobar las credenciales de autenticación de acceso de red. Si se autentican las credenciales de usuario y se autoriza el intento de conexión, el servidor RADIUS autoriza el acceso de usuario según las condiciones especificadas y, a continuación, registra la conexión de acceso de red en un registro de cuentas. El uso de RADIUS permite la autenticación de usuario de acceso de red, autorización y datos de cuentas que pueden recopilarse y mantenerse en una ubicación central, en lugar de en cada servidor de acceso.

#### <a name="using-nps-as-a-radius-server"></a>Usar NPS como un servidor de radio

Puedes usar NPS como un servidor de radio cuando:

- Estás usando un dominio de AD DS o la base de datos de cuentas de usuario de SAM local como la base de datos de la cuenta de usuario para los clientes de acceso. 
- Utiliza el acceso remoto en varios servidores de acceso telefónico, servidores VPN, o enrutadores marcado a petición y desea centralizar la configuración de directivas de red y conexión de registro y cuentas. 
- Subcontrata la telefónico, VPN o acceso inalámbrico a un proveedor de servicios. Los servidores de acceso usan RADIUS para autenticar y autorizar conexiones que realizan los miembros de la organización.
- Quieres centralizar la autenticación, autorización y contabilidad para un conjunto heterogéneo de servidores de acceso.

La siguiente ilustración muestra NPS como un servidor de radio para una variedad de clientes de acceso. 

![NPS como un servidor de radio](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>Proxy RADIUS

Como un proxy RADIUS, NPS reenvía la autenticación de mensajes y cuentas NPS y otros servidores RADIUS. Puedes usar NPS como un proxy de radio para proporcionar el enrutamiento de radio mensajes entre clientes RADIUS \ (también denominado servidores\ de acceso de red) y los servidores RADIUS realizan Contabilidad, autorización y autenticación de usuario para el intento de conexión. 

Cuando se usa como un proxy RADIUS, NPS es una conmutación central o el punto de distribución a través de qué flujo de mensajes de acceso y administración de cuentas de RADIUS. NPS registra información en un registro de cuentas acerca de los mensajes que se reenvía.

#### <a name="using-nps-as-a-radius-proxy"></a>Usar NPS como un proxy de radio

Puedes usar NPS como un proxy RADIUS cuando:

- Eres un proveedor de servicios que ofrece servicios de acceso de red inalámbrica, VPN o telefónico externo a varios clientes. Sus NAS envían solicitudes de conexión proxy NPS RADIUS. En función de la parte del nombre de usuario en la solicitud de conexión territorio, el proxy NPS RADIUS reenvía la solicitud de conexión a un servidor RADIUS mantenido por el cliente y puede autenticar y autorizar el intento de conexión.
- Tendrás que proporcionar autenticación y autorización de cuentas de usuario que no son miembros del dominio en el que el servidor NPS es un miembro o en otro dominio que tenga una confianza bidireccional con el dominio en el que el servidor NPS es un miembro. Esto incluye cuentas de dominios de confianza, los dominios de confianza unidireccionales y otros bosques. En lugar de configurar los servidores de acceso para enviar solicitudes de su conexión a un servidor NPS RADIUS, puede configurar para enviar solicitudes de su conexión a un proxy NPS RADIUS. El proxy NPS RADIUS usa la parte del nombre de dominio del nombre de usuario y reenvía la solicitud a un servidor NPS en el dominio o bosque correcto. Los intentos de conexión para el usuario se pueden autenticar cuentas en un dominio o bosque para NAS en otro dominio o bosque.
- Quieres realizar la autenticación y autorización mediante una base de datos que no es una base de datos de cuenta de Windows. En este caso, las solicitudes de conexión que coinciden con un nombre de dominio Kerberos especificado se envían a un servidor de radio, que tiene acceso a una base de datos diferentes cuentas de usuario y datos de autorización. Algunos ejemplos de otras bases de datos de usuario son bases de datos de servicios de directorio Novell (NDS) y lenguaje de consulta estructurado (SQL).
- Quieres procesar un gran número de solicitudes de conexión. En este caso, en lugar de configurar los clientes RADIUS para intentar equilibrar sus solicitudes de cuentas y la conexión entre varios servidores RADIUS, puede configurar para enviar sus solicitudes de cuentas y la conexión a un proxy NPS RADIUS. El proxy NPS RADIUS equilibra dinámicamente la carga de conexión contabilidad solicitudes y entre varios servidores RADIUS y aumenta el procesamiento de un gran número de clientes de RADIUS y autenticaciones por segundo.
- Desea proporcionar autenticación y autorización para proveedores de servicios externos RADIUS y minimizar la configuración de firewall de intranet. Es un servidor de seguridad de intranet entre la red perimetral (la red entre la intranet e Internet) y la intranet. Al colocar un servidor NPS en la red perimetral, el firewall entre la red perimetral y la intranet debe permitir el tráfico entre el servidor NPS y varios controladores de dominio. Reemplazando el servidor NPS por un proxy NPS, el firewall debe permitir solo tráfico RADIUS entre el proxy NPS y uno o varios servidores NPS dentro de la intranet.

La siguiente ilustración muestra NPS como proxy RADIUS entre clientes RADIUS y servidores RADIUS.

![NPS como un Proxy de radio](../../media/Nps-Proxy/Nps-Proxy.jpg)


Con NPS, las organizaciones también pueden subcontratar infraestructura de acceso remoto a un proveedor de servicios mientras conserva el control de cuentas, autorización y autenticación de usuario.

Se pueden crear configuraciones de NPS para los siguientes escenarios:

- Acceso inalámbrico
- Acceso remoto de organización telefónico o red privada virtual (VPN)
- Telefónico externo o acceso inalámbrico
- Acceso a Internet
- Autenticados acceso a recursos extranet para socios comerciales

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Servidor RADIUS y ejemplos de configuración de proxy de radio

Los ejemplos de configuración siguientes muestran cómo se puede configurar NPS como un servidor de radio y un proxy RADIUS.

**NPS como un servidor RADIUS**. En este ejemplo, NPS está configurado como un servidor RADIUS, la directiva de solicitud de conexión predeterminada es la única directiva configurada y todas las solicitudes de conexión se procesan por el servidor NPS local. El servidor NPS puede autenticar y autorizar usuarios cuyas cuentas se están en el dominio del servidor NPS y en dominios de confianza.

**NPS como un proxy RADIUS**. En este ejemplo, el servidor NPS está configurado como un proxy RADIUS que reenvíe solicitudes de conexión a grupos de servidores RADIUS remotos en dos dominios de confianza. Se elimina la directiva de solicitud de conexión predeterminada y se crean dos nuevas directivas de solicitud de conexión para reenviar solicitudes a cada uno de los dos dominios de confianza. En este ejemplo, NPS no procesa ninguna solicitud de conexión en el servidor local. 

**NPS RADIUS server y proxy RADIUS**. Además de la directiva predeterminada de solicitud de conexión, que indica que las solicitudes de conexión se procesan localmente, se crea una nueva directiva de solicitud de conexión que reenvía solicitudes de conexión a NPS u otro servidor RADIUS en un dominio de confianza. Esta segunda directiva se denomina la directiva de Proxy. En este ejemplo, la directiva de Proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la directiva de Proxy, la solicitud de conexión se reenvía al servidor de radio en el grupo de servidores RADIUS remotos. Si la solicitud de conexión no coincide con la directiva de Proxy pero coincide con la directiva predeterminada de solicitud de conexión, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con cualquiera de ellas, se descarta.

**NPS como un servidor RADIUS con servidores remotos de contabilidad**. En este ejemplo, el servidor NPS local no está configurado para administrar cuentas y la directiva predeterminada de solicitud de conexión se revisa para que se envían mensajes de cuentas RADIUS a un servidor NPS u otro servidor de radio en un grupo de servidores RADIUS remotos. Aunque se envían mensajes de cuentas, no se envían mensajes de autenticación y autorización y el servidor NPS local realiza las siguientes funciones para el dominio local y todos los dominios de confianza.

**NPS con RADIUS remoto para la asignación de usuario de Windows**. En este ejemplo, NPS actúa como servidor RADIUS, como un proxy de radio para cada solicitud de conexión en particular reenviando la solicitud de autenticación a un servidor remoto RADIUS mientras usando una cuenta de usuario local de Windows en la autorización. Esta configuración se implementa mediante la configuración de la radio remoto al atributo de asignación de usuarios de Windows como una condición de la directiva de solicitud de conexión. \ (Además, una cuenta de usuario debe crearse localmente en el servidor de radio que tiene el mismo nombre que la cuenta de usuario remoto con los que la autenticación se realiza mediante el servidor remoto RADIUS. \)

## <a name="configuration"></a>Configuración

Para configurar NPS como un servidor RADIUS, puedes usar configuración estándar o configuración avanzada en el administrador del servidor o en la consola NPS. Para configurar NPS como un proxy RADIUS, debes usar configuración avanzada.

### <a name="standard-configuration"></a>Configuración estándar

Con la configuración estándar se proporcionan asistentes para que te ayudarán a configurar NPS para los siguientes escenarios:

- Servidor RADIUS para telefónico o conexiones VPN
- Servidor RADIUS para conexiones con cable o de redes inalámbricas 802.1X

Para configurar NPS usar a un asistente, abre la consola NPS, selecciona uno de los escenarios anteriores y, a continuación, haz clic en el vínculo que se abre al asistente.

### <a name="advanced-configuration"></a>Configuración avanzada

Cuando usas la configuración avanzada, configurar manualmente NPS como un servidor RADIUS o proxy RADIUS. 

Para configurar NPS mediante configuración avanzada, abre la consola NPS y, a continuación, haz clic en la flecha situada junto a **configuración avanzada** para expandir la sección.

Se proporcionan los siguientes elementos de configuración avanzada.

#### <a name="configure-radius-server"></a>Configurar el servidor de radio

Para configurar NPS como un servidor RADIUS, debe configurar clientes RADIUS, directiva de red y cuentas de RADIUS.

Para obtener instrucciones sobre cómo hacer estas configuraciones, consulta los siguientes temas.

- [Configurar a los clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar las directivas de red](nps-np-configure.md)
- [Configurar las cuentas de servidor de directivas de red](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurar el proxy de radio

Para configurar NPS como un proxy RADIUS, debes configurar los clientes RADIUS, grupos de servidores RADIUS remotos y directivas de solicitud de conexión.

Para obtener instrucciones sobre cómo hacer estas configuraciones, consulta los siguientes temas.

- [Configurar a los clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar los grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)
- [Configurar las directivas de solicitud de conexión](nps-crp-configure.md)

## <a name="bkmk_logging"></a>Registro NPS

Registro NPS también se denominan cuentas RADIUS. Configurar el registro de NPS para tus requisitos si NPS se usa como un servidor RADIUS, el proxy o cualquier combinación de estas configuraciones.

Para configurar el registro de NPS, debes configurar los eventos que inició sesión y ver con el Visor de eventos y, a continuación, determinar qué otra información que quieres iniciar sesión. Además, debes decidir si quiere iniciar la sesión de autenticación de usuario y la información de cuentas para los archivos de registro de texto almacenados en el equipo local o en una base de datos de SQL Server en el equipo local o en un equipo remoto.

Para obtener más información, consulta [configurar red directiva de servidor de contabilidad](nps-accounting-configure.md).
