---
title: Servidor de directivas de redes (NPS)
description: En este tema se proporciona información general sobre el servidor de directivas de redes en Windows Server 2016 y Windows Server 2019, e incluye vínculos a instrucciones adicionales sobre NPS.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 5685d4dae742245c131ff2fee3114e905e94e9bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396008"
---
# <a name="network-policy-server-nps"></a>Servidor de directivas de redes (NPS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2019

Puede usar este tema para obtener información general sobre el servidor de directivas de redes en Windows Server 2016 y Windows Server 2019. NPS se instala cuando se instala la característica servicios de acceso y directivas de redes (NPAS) en Windows Server 2016 y Server 2019.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación de NPS.
> - [Procedimientos recomendados del servidor de directivas de redes](nps-best-practices.md)
> - [Introducción con el servidor de directivas de redes](nps-getstart-top.md)
> - [Planear el servidor de directivas de redes](nps-plan-top.md)
> - [Implementar servidor de directivas de redes](nps-deploy.md)
> - [Administrar el servidor de directivas de redes](nps-manage-top.md)
> - [Cmdlets del servidor de directivas de redes (NPS) en Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para windows Server 2016 y Windows 10
> - [Cmdlets del servidor de directivas de redes (NPS) en Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para windows Server 2012 R2 y Windows 8.1
> - [Cmdlets de NPS en Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para windows Server 2012 y Windows 8

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.

También puede configurar NPS como un proxy de Servicio de autenticación remota telefónica de usuario (RADIUS) para reenviar las solicitudes de conexión a un NPS remoto u otro servidor RADIUS para que pueda equilibrar la carga de las solicitudes de conexión y reenviarlas al dominio correcto para la autenticación. y autorización.

NPS le permite configurar y administrar de forma centralizada la autenticación de acceso a la red, la autorización y las cuentas con las siguientes características:

- **Servidor RADIUS**. NPS realiza la autenticación centralizada, la autorización y la administración de cuentas para el conmutador de autenticación inalámbrico, el acceso remoto y las conexiones de red privada virtual (VPN). Cuando se usa NPS como un servidor RADIUS, se configuran los servidores de acceso a la red, como los puntos de acceso inalámbrico y los servidores VPN, como clientes RADIUS en NPS. Además, se configuran las directivas de redes que usa NPS para autorizar las solicitudes de conexión. También puede configurar la administración de cuentas RADIUS para que NPS registre la información de las cuentas en archivos de registro del disco duro local o en una base de datos de Microsoft SQL Server. Para obtener más información, consulte [servidor RADIUS](#radius-server).
- **Proxy RADIUS**. Cuando se usa NPS como un proxy RADIUS, se configuran directivas de solicitud de conexión que indican al NPS qué solicitudes de conexión se reenvían a otros servidores RADIUS y a qué servidores RADIUS desea reenviar las solicitudes de conexión. También puede configurar NPS para que reenvíe datos de cuentas que deben registrar uno o más equipos en un grupo de servidores RADIUS remotos. Para configurar NPS como un servidor proxy RADIUS, consulte los temas siguientes. Para obtener más información, consulte [proxy RADIUS](#radius-proxy).
    - [Configurar directivas de solicitud de conexión](nps-crp-configure.md)
- **Cuentas de RADIUS**. Puede configurar NPS para registrar eventos en un archivo de registro local o en una instancia local o remota de Microsoft SQL Server. Para obtener más información, consulte [registro de NPS](#nps-logging).

> [!IMPORTANT]
> Protección de acceso a redes \(NAP\), autoridad de registro de mantenimiento \(HRA\)y Protocolo de autorización de credenciales de host \(HCAP\) quedó en desuso en Windows Server 2012 R2 y no están disponibles en Windows Server 2016. Si tiene una implementación de NAP con sistemas operativos anteriores a Windows Server 2016, no puede migrar la implementación de NAP a Windows Server 2016.

Puede configurar NPS con cualquier combinación de estas características. Por ejemplo, puede configurar un NPS como un servidor RADIUS para conexiones VPN y también como proxy RADIUS para reenviar algunas solicitudes de conexión a los miembros de un grupo de servidores RADIUS remotos para la autenticación y autorización en otro dominio.

## <a name="windows-server-editions-and-nps"></a>Windows Server Edition y NPS

NPS proporciona una funcionalidad diferente en función de la edición de Windows Server que se instale.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 o Windows Server 2019 Standard/Datacenter Edition

Con NPS en Windows Server 2016 Standard o Datacenter, puede configurar un número ilimitado de clientes RADIUS y grupos de servidores RADIUS remotos. Además, puede configurar los clientes RADIUS especificando un intervalo de direcciones IP.

> [!NOTE]
> La característica servicios de acceso y directivas de redes de WIndows no está disponible en sistemas instalados con una opción de instalación Server Core.

En las secciones siguientes se proporciona información más detallada sobre NPS como un servidor RADIUS y un proxy.

## <a name="radius-server-and-proxy"></a>Proxy y servidor RADIUS

Puede usar NPS como un servidor RADIUS, un proxy RADIUS o ambos.

### <a name="radius-server"></a>Servidor RADIUS

NPS es la implementación de Microsoft del estándar RADIUS especificado por el equipo de ingeniería de Internet \(IETF\) en RFC 2865 y 2866. Como servidor RADIUS, NPS realiza la autenticación centralizada de la conexión, la autorización y las cuentas para muchos tipos de acceso a la red, como el conmutador de autenticación inalámbrico, de acceso telefónico y de red privada virtual \(VPN\) acceso remoto y las conexiones de enrutador a enrutador.

> [!NOTE]
> Para obtener información acerca de la implementación de NPS como un servidor RADIUS, consulte [implementar el servidor de directivas de redes](nps-deploy.md).

NPS permite el uso de un conjunto heterogéneo de equipos inalámbricos, de conmutación, de acceso remoto o VPN. Puede usar NPS con el servicio de acceso remoto, que está disponible en Windows Server 2016.

NPS usa un Active Directory Domain Services \(AD DS dominio de\) o la base de datos de cuentas de usuario del administrador de cuentas de seguridad (SAM) local para autenticar las credenciales de usuario para los intentos de conexión. Cuando un servidor que ejecuta NPS es miembro de un dominio de AD DS, NPS usa el servicio de directorio como su base de datos de cuentas de usuario y forma parte de una solución de inicio de sesión único. El mismo conjunto de credenciales se usa para el control de acceso a la red \(autenticar y autorizar el acceso a un\) de red e iniciar sesión en un dominio de AD DS.

> [!NOTE]
> NPS usa las propiedades de acceso telefónico de la cuenta de usuario y las directivas de red para autorizar una conexión.

Los proveedores de servicios de Internet \(ISP\) y las organizaciones que mantienen el acceso a la red tienen el mayor desafío de administrar todos los tipos de acceso a la red desde un único punto de administración, independientemente del tipo de equipo de acceso a la red utilizado. El estándar RADIUS es compatible con esta funcionalidad en entornos homogéneos y heterogéneos. RADIUS es un protocolo cliente-servidor que permite que los equipos de acceso a la red (usados como clientes RADIUS) envíen solicitudes de autenticación y cuentas a un servidor RADIUS.

Un servidor RADIUS tiene acceso a la información de la cuenta de usuario y puede comprobar las credenciales de autenticación de acceso a la red. Si se autentican las credenciales de usuario y se autoriza el intento de conexión, el servidor RADIUS autoriza el acceso del usuario en función de las condiciones especificadas y, a continuación, registra la conexión de acceso a la red en un registro de cuentas. El uso de RADIUS permite que los datos de autenticación, autorización y cuentas de usuario de acceso a la red se recopilen y se mantengan en una ubicación central, en lugar de en cada servidor de acceso.

#### <a name="using-nps-as-a-radius-server"></a>Usar NPS como un servidor RADIUS

Puede usar NPS como un servidor RADIUS cuando:

- Está usando un dominio de AD DS o la base de datos de cuentas de usuario SAM local como base de datos de cuentas de usuario para obtener acceso a los clientes. 
- Está usando acceso remoto en varios servidores de acceso telefónico, servidores VPN o enrutadores de marcado a petición y desea centralizar la configuración de las directivas de red y el registro y la administración de conexiones. 
- Está sobrecontratando el acceso telefónico, VPN o inalámbrico a un proveedor de servicios. Los servidores de acceso usan RADIUS para autenticar y autorizar las conexiones realizadas por los miembros de la organización.
- Desea centralizar la autenticación, la autorización y las cuentas para un conjunto heterogéneo de servidores de acceso.

En la siguiente ilustración se muestra NPS como un servidor RADIUS para una variedad de clientes de acceso. 

![NPS como un servidor RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

Como proxy RADIUS, NPS reenvía los mensajes de autenticación y cuentas a NPS y a otros servidores RADIUS. Puede usar NPS como un proxy RADIUS para proporcionar el enrutamiento de mensajes RADIUS entre clientes RADIUS \(también denominados servidores de acceso a la red\) y servidores RADIUS que realizan la autenticación de usuarios, la autorización y las cuentas para el intento de conexión. 

Cuando se usa como proxy RADIUS, NPS es un punto central de conmutación o enrutamiento a través del cual fluyen los mensajes de acceso y cuentas de RADIUS. NPS registra información en un registro de cuentas sobre los mensajes que se reenvían.

#### <a name="using-nps-as-a-radius-proxy"></a>Uso de NPS como un proxy RADIUS

Puede usar NPS como un proxy RADIUS cuando:

- Es un proveedor de servicios que ofrece servicios de acceso telefónico, VPN o red inalámbrica a varios clientes. Los NAS envían solicitudes de conexión al proxy RADIUS NPS. En función de la parte del dominio Kerberos del nombre de usuario en la solicitud de conexión, el proxy RADIUS NPS reenvía la solicitud de conexión a un servidor RADIUS mantenido por el cliente y puede autenticar y autorizar el intento de conexión.
- Quiere proporcionar autenticación y autorización para las cuentas de usuario que no son miembros del dominio en el que el NPS es miembro u otro dominio que tenga una confianza bidireccional con el dominio al que pertenece el NPS. Esto incluye las cuentas de dominios que no son de confianza, dominios de confianza unidireccionales y otros bosques. En lugar de configurar los servidores de acceso para que envíen sus solicitudes de conexión a un servidor RADIUS NPS, puede configurarlos para que envíen sus solicitudes de conexión a un proxy RADIUS NPS. El proxy RADIUS NPS usa la parte del nombre de dominio Kerberos del nombre de usuario y reenvía la solicitud a un NPS en el dominio o bosque correcto. Los intentos de conexión para las cuentas de usuario en un dominio o bosque se pueden autenticar para los NAS en otro dominio o bosque.
- Desea realizar la autenticación y autorización mediante una base de datos que no sea una base de datos de cuentas de Windows. En este caso, las solicitudes de conexión que coinciden con un nombre de dominio Kerberos especificado se reenvían a un servidor RADIUS, que tiene acceso a una base de datos diferente de cuentas de usuario y datos de autorización. Entre los ejemplos de otras bases de datos de usuario se incluyen las bases de datos de servicios de directorio Novell (NDS) y Lenguaje de consulta estructurado (SQL).
- Desea procesar un gran número de solicitudes de conexión. En este caso, en lugar de configurar los clientes RADIUS para intentar equilibrar las solicitudes de conexión y cuentas entre varios servidores RADIUS, puede configurarlos para que envíen sus solicitudes de conexión y cuentas a un proxy RADIUS NPS. El proxy RADIUS NPS equilibra dinámicamente la carga de solicitudes de conexión y cuentas entre varios servidores RADIUS y aumenta el procesamiento de un gran número de clientes RADIUS y autenticaciones por segundo.
- Desea proporcionar autenticación y autorización RADIUS para los proveedores de servicios de origen y minimizar la configuración del firewall de la intranet. Un firewall de intranet se encuentra entre la red perimetral (la red entre la intranet e Internet) y la intranet. Al colocar un NPS en la red perimetral, el Firewall entre la red perimetral y la intranet debe permitir que el tráfico fluya entre el NPS y varios controladores de dominio. Al reemplazar el NPS con un proxy NPS, el Firewall debe permitir que solo fluya el tráfico RADIUS entre el proxy NPS y uno o varios NPSs dentro de la intranet.

En la ilustración siguiente se muestra NPS como un proxy RADIUS entre clientes RADIUS y servidores RADIUS.

![NPS como proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)

Con NPS, las organizaciones también pueden externalizar la infraestructura de acceso remoto a un proveedor de servicios a la vez que conservan el control sobre la autenticación, la autorización y las cuentas de usuario.

Se pueden crear configuraciones de NPS para los siguientes escenarios:

- Acceso inalámbrico
- Acceso remoto de acceso telefónico de la organización o red privada virtual (VPN)
- Acceso telefónico o inalámbrico Outsource
- Acceso a Internet
- Acceso autenticado a los recursos de extranet para socios comerciales

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Ejemplos de configuración de servidor RADIUS y proxy RADIUS

Los siguientes ejemplos de configuración muestran cómo puede configurar NPS como un servidor RADIUS y un proxy RADIUS.

**NPS como un servidor RADIUS**. En este ejemplo, NPS se configura como un servidor RADIUS, la Directiva de solicitud de conexión predeterminada es la única directiva configurada y todas las solicitudes de conexión las procesa el NPS local. NPS puede autenticar y autorizar a los usuarios cuyas cuentas están en el dominio del NPS y en los dominios de confianza.

**NPS como proxy RADIUS**. En este ejemplo, el NPS se configura como un proxy RADIUS que reenvía las solicitudes de conexión a grupos de servidores RADIUS remotos en dos dominios que no son de confianza. Se elimina la Directiva de solicitud de conexión predeterminada y se crean dos nuevas directivas de solicitud de conexión para reenviar las solicitudes a cada uno de los dos dominios que no son de confianza. En este ejemplo, NPS no procesa ninguna solicitud de conexión en el servidor local. 

**NPS como servidor RADIUS y proxy RADIUS**. Además de la Directiva de solicitud de conexión predeterminada, que designa que las solicitudes de conexión se procesan localmente, se crea una nueva Directiva de solicitud de conexión que reenvía las solicitudes de conexión a un NPS u otro servidor RADIUS en un dominio que no es de confianza. Esta segunda Directiva se denomina Directiva de proxy. En este ejemplo, la Directiva de proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la Directiva de proxy, la solicitud de conexión se reenvía al servidor RADIUS en el grupo de servidores RADIUS remotos. Si la solicitud de conexión no coincide con la Directiva de proxy, pero coincide con la Directiva de solicitud de conexión predeterminada, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con ninguna de las dos directivas, se descarta.

**NPS como un servidor RADIUS con servidores de cuentas remotas**. En este ejemplo, el NPS local no está configurado para realizar cuentas y la Directiva de solicitud de conexión predeterminada se revisa para que los mensajes de cuentas RADIUS se reenvíen a un NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos. Aunque se reenvían los mensajes de cuentas, no se reenvían los mensajes de autenticación y autorización, y el NPS local realiza estas funciones para el dominio local y todos los dominios de confianza.

**NPS con asignación de usuarios de RADIUS remotos a Windows**. En este ejemplo, NPS actúa como servidor RADIUS y como proxy RADIUS para cada solicitud de conexión individual mediante el reenvío de la solicitud de autenticación a un servidor RADIUS remoto mientras se usa una cuenta de usuario de Windows local para la autorización. Esta configuración se implementa mediante la configuración del atributo de asignación de RADIUS remota a usuario de Windows como una condición de la Directiva de solicitud de conexión. \(además, se debe crear una cuenta de usuario localmente en el servidor RADIUS que tenga el mismo nombre que la cuenta de usuario remota en la que el servidor RADIUS remoto realiza la autenticación.\)

## <a name="configuration"></a>Configuración

Para configurar NPS como un servidor RADIUS, puede usar la configuración estándar o la configuración avanzada en la consola de NPS o en Administrador del servidor. Para configurar NPS como un proxy RADIUS, debe usar la configuración avanzada.

### <a name="standard-configuration"></a>Configuración estándar

Con la configuración estándar, se proporcionan asistentes para ayudarle a configurar NPS para los siguientes escenarios:

- Servidor RADIUS para conexiones de acceso telefónico o VPN
- Servidor RADIUS para conexiones 802.1X inalámbricas o por cable

Para configurar NPS mediante un asistente, abra la consola de NPS, seleccione uno de los escenarios anteriores y, a continuación, haga clic en el vínculo que abre el asistente.

### <a name="advanced-configuration"></a>Configuración avanzada

Cuando se usa la configuración avanzada, se configura manualmente NPS como un servidor RADIUS o proxy RADIUS. 

Para configurar NPS mediante la configuración avanzada, abra la consola de NPS y, a continuación, haga clic en la flecha situada junto a **Configuración avanzada** para expandir esta sección.

Se proporcionan los siguientes elementos de configuración avanzada.

#### <a name="configure-radius-server"></a>Configurar el servidor RADIUS

Para configurar NPS como un servidor RADIUS, debe configurar clientes RADIUS, directivas de red y cuentas RADIUS.

Para obtener instrucciones sobre cómo establecer estas configuraciones, vea los temas siguientes.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar las directivas de red](nps-np-configure.md)
- [Configurar cuentas de servidor de directivas de redes](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurar el proxy RADIUS

Para configurar NPS como un proxy RADIUS, debe configurar los clientes RADIUS, los grupos de servidores RADIUS remotos y las directivas de solicitud de conexión.

Para obtener instrucciones sobre cómo establecer estas configuraciones, vea los temas siguientes.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)
- [Configurar directivas de solicitud de conexión](nps-crp-configure.md)

## <a name="nps-logging"></a>Registro de NPS

El registro de NPS también se denomina cuentas RADIUS. Configure el registro de NPS en sus requisitos si NPS se usa como un servidor RADIUS, un proxy o cualquier combinación de estas configuraciones.

Para configurar el registro de NPS, debe configurar los eventos que desea registrar y ver con Visor de eventos y, a continuación, determinar qué otra información desea registrar. Además, debe decidir si desea registrar la información de cuentas y autenticación de usuarios en los archivos de registro de texto almacenados en el equipo local o en una base de datos de SQL Server en el equipo local o en un equipo remoto.

Para obtener más información, vea [configurar las cuentas del servidor de directivas de redes](nps-accounting-configure.md).
