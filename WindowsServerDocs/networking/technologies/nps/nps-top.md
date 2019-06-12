---
title: Servidor de directivas de redes (NPS)
description: En este tema proporciona información general sobre servidor de directivas de redes en Windows Server 2016 y Windows Server 2019 e incluye vínculos a guías adicionales acerca de NPS.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 515012a21ba6e90abe2c4db2150fd1feaad06677
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812370"
---
# <a name="network-policy-server-nps"></a>Servidor de directivas de redes (NPS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server de 2019

Puede usar este tema para obtener información general del servidor de directivas de redes en Windows Server 2016 y Windows Server 2019. NPS se instala al instalar la característica Servicios de acceso (NPAS) y directivas de redes en Windows Server 2016 y Server 2019.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación de NPS.
> - [Prácticas recomendadas de servidor de directivas de red](nps-best-practices.md)
> - [Introducción a servidor de directivas de red](nps-getstart-top.md)
> - [Planear el servidor de directivas de red](nps-plan-top.md)
> - [Implementar el servidor de directivas de red](nps-deploy.md)
> - [Administrar el servidor de directivas de red](nps-manage-top.md)
> - [Cmdlets de directiva de servidor (NPS) en Windows PowerShell de red](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2016 y Windows 10
> - [Cmdlets de directiva de servidor (NPS) en Windows PowerShell de red](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 R2 y Windows 8.1
> - [Cmdlets NPS en Windows PowerShell](https://technet.microsoft.com/library/jj872739.aspx) para Windows Server 2012 y Windows 8

Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.

También puede configurar NPS como un proxy de servicio de autenticación remota telefónica de usuario (RADIUS) para reenviar las solicitudes de conexión a un NPS remoto u otro servidor RADIUS para que pueda cargar solicitudes de conexión de equilibrio y reenviarlos al dominio correcto para la autenticación y la autorización.

NPS permite configurar y administrar la autenticación de acceso de red, autorización y las cuentas con las siguientes características de forma centralizada:

- **Servidor RADIUS**. NPS realiza la autenticación centralizada, autorización y contabilidad para conexiones inalámbricas, autenticación de modificador, acceso remoto telefónico y conexiones de red privada virtual (VPN). Cuando se usa NPS como un servidor RADIUS, se configuran los servidores de acceso a la red, como los puntos de acceso inalámbrico y los servidores VPN, como clientes RADIUS en NPS. Además, se configuran las directivas de redes que usa NPS para autorizar las solicitudes de conexión. También puede configurar la administración de cuentas RADIUS para que NPS registre la información de las cuentas en archivos de registro del disco duro local o en una base de datos de Microsoft SQL Server. Para obtener más información, consulte [servidor RADIUS](#radius-server).
- **Proxy RADIUS**. Cuando se usa NPS como proxy RADIUS, configure las directivas de solicitud de conexión que indican el NPS qué solicitudes de conexión para reenviar a otros servidores RADIUS y a qué servidores RADIUS desea reenviar las solicitudes de conexión. También puede configurar NPS para que reenvíe datos de cuentas que deben registrar uno o más equipos en un grupo de servidores RADIUS remotos. Para configurar NPS como un servidor proxy RADIUS, consulte los temas siguientes. Para obtener más información, consulte [proxy RADIUS](#radius-proxy).
    - [Configurar las directivas de solicitud de conexión](nps-crp-configure.md)
- **Administración de cuentas RADIUS**. Puede configurar NPS para registrar eventos en un archivo de registro local o a una instancia local o remota de Microsoft SQL Server. Para obtener más información, consulte [registro NPS](#nps-logging).

> [!IMPORTANT]
> Protección de acceso de red \(NAP\), autoridad de registro de mantenimiento \(HRA\)y el protocolo de autorización de credenciales de Host \(HCAP\) han quedado en desuso en Windows Server 2012 R2, y no están disponibles en Windows Server 2016. Si tiene una implementación de NAP con sistemas operativos anteriores a Windows Server 2016, no puede migrar la implementación de NAP en Windows Server 2016.

Puede configurar NPS con cualquier combinación de estas características. Por ejemplo, puede configurar un servidor NPS como un servidor RADIUS para conexiones VPN y también como un proxy RADIUS para reenviar algunas solicitudes de conexión a los miembros de un grupo de servidores RADIUS remoto para la autenticación y autorización en otro dominio.

## <a name="windows-server-editions-and-nps"></a>NPS y ediciones de Windows Server

NPS ofrece una funcionalidad diferente dependiendo de la edición de Windows Server que instale.

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 o Windows Server 2019 Standard/Datacenter Edition

Con NPS en Windows Server 2016 Standard o Datacenter, puede configurar un número ilimitado de clientes RADIUS y grupos de servidores RADIUS remotos. Además, puede configurar los clientes RADIUS especificando un intervalo de direcciones IP.

> [!NOTE]
> La característica Servicios de acceso y directivas de redes de WIndows no está disponible en sistemas instalados con una opción de instalación Server Core.

Las secciones siguientes proporcionan información más detallada acerca de NPS como servidor RADIUS y proxy.

## <a name="radius-server-and-proxy"></a>Proxy y servidor RADIUS

Puede usar NPS como un servidor RADIUS, proxy RADIUS o ambos.

### <a name="radius-server"></a>Servidor RADIUS

NPS es la implementación de Microsoft del estándar RADIUS especificado por la Internet Engineering Task Force \(IETF\) en RFC 2865 y 2866. Como un servidor RADIUS, NPS realiza la autenticación de la conexión centralizada, autorización y contabilidad para muchos tipos de acceso a la red, incluidos el conmutador inalámbrico, que se autentica, acceso telefónico y red privada virtual \(VPN\) remoto el acceso y conexiones de enrutador a enrutador.

> [!NOTE]
> Para obtener información acerca de cómo implementar NPS como servidor RADIUS, consulte [implementar un servidor de directivas de redes](nps-deploy.md).

NPS habilita el uso de un conjunto heterogéneo de wireless, switch, acceso remoto o equipos de VPN. Puede usar NPS con el servicio de acceso remoto, que está disponible en Windows Server 2016.

NPS usa un Active Directory Domain Services \(AD DS\) las cuentas de usuario de administrador de cuentas de seguridad (SAM) local o de dominio de la base de datos para autenticar las credenciales de usuario para los intentos de conexión. Cuando un servidor que ejecuta NPS es miembro de un dominio de AD DS, NPS usa el servicio de directorio como su base de datos de la cuenta de usuario y forma parte de una solución de inicio de sesión único. Se usa el mismo conjunto de credenciales para el control de acceso de red \(autenticar y autorizar el acceso a una red\) e iniciar sesión en un dominio de AD DS.

> [!NOTE]
> NPS usa las propiedades de acceso telefónico de las directivas de red y cuenta de usuario que autorice una conexión.

Proveedores de servicios Internet \(ISP\) y las organizaciones que mantienen el acceso de red tienen el mayor desafío de administrar todos los tipos de acceso de red desde un único punto de administración, independientemente del tipo de acceso de red equipos utilizados. El estándar RADIUS admite esta funcionalidad en entornos homogéneos y heterogéneos. RADIUS es un protocolo cliente-servidor que permite a equipos de acceso de red (usado como clientes RADIUS) para enviar solicitudes de autenticación y cuentas a un servidor RADIUS.

Un servidor RADIUS tiene acceso a información de la cuenta de usuario y puede comprobar las credenciales de autenticación de acceso de red. Si se autentican las credenciales de usuario y se autoriza el intento de conexión, el servidor RADIUS autoriza el acceso de usuario según las condiciones especificadas y, a continuación, registra la conexión de acceso de red en un registro de cuentas. El uso de RADIUS permite la autenticación de usuario de acceso de red, autorización y datos de cuentas que se recopilen y conserven en una ubicación central, en lugar de en cada servidor de acceso.

#### <a name="using-nps-as-a-radius-server"></a>Usa NPS como servidor RADIUS

Puede usar NPS como servidor RADIUS cuando:

- Está usando un dominio de AD DS o la base de datos de cuentas de usuario de SAM local como la base de datos de la cuenta de usuario para los clientes de acceso. 
- Se usa en varios servidores de acceso telefónico, servidores VPN, acceso remoto o enrutadores de marcado a petición y desea centralizar la configuración de directivas de red y conexión de registro y administración de cuentas. 
- Cuando subcontrate el acceso telefónico, VPN o acceso inalámbrico a un proveedor de servicios. Los servidores de acceso usan RADIUS para autenticar y autorizar conexiones realizadas por miembros de su organización.
- Cuando desee centralizar la autenticación, autorización y contabilidad para un grupo heterogéneo de servidores de acceso.

La siguiente ilustración muestra NPS como servidor RADIUS para una variedad de clientes de acceso. 

![NPS como servidor RADIUS](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>Proxy RADIUS

Como un proxy RADIUS, NPS reenvía mensajes de autenticación y cuentas para NPS y otros servidores RADIUS. Puede usar NPS como proxy RADIUS para el enrutamiento del radio de los mensajes entre clientes RADIUS \(también denominados servidores de acceso de red\) y servidores RADIUS que realizan la autenticación de usuario, autorización y contabilidad para el intento de conexión. 

Cuando se usa como proxy RADIUS, NPS es una central de conmutación o enrutamiento punto a través de la que fluyen los mensajes de acceso y administración de cuentas RADIUS. NPS registra información en un registro de cuentas de los mensajes que se reenvían.

#### <a name="using-nps-as-a-radius-proxy"></a>Usar NPS como proxy RADIUS

Puede usar NPS como proxy RADIUS cuando:

- Son un proveedor de servicios que ofrece servicios de acceso de red inalámbrica, VPN o telefónico externo a varios clientes. Sus NAS envían solicitudes de conexión al proxy RADIUS NPS. En función de la parte del dominio Kerberos del nombre de usuario en la solicitud de conexión, el proxy RADIUS NPS reenvía la solicitud de conexión a un servidor RADIUS que se mantiene el cliente y puede autenticar y autorizar el intento de conexión.
- Desea proporcionar autenticación y autorización para las cuentas de usuario que no son miembros de otro dominio que tenga una confianza bidireccional con el dominio en el que el NPS es un miembro o el dominio en el que el NPS es miembro. Esto incluye las cuentas en dominios sin confianza, dominios de confianza unidireccional y demás bosques. En lugar de configurar los servidores de acceso para enviar las solicitudes de conexión a un servidor RADIUS NPS, puede configurarlos para enviar las solicitudes de conexión a un proxy RADIUS NPS. El proxy RADIUS NPS usa la parte del nombre de dominio Kerberos del nombre de usuario y reenvía la solicitud a un NPS en el bosque o dominio correcto. Intentos de conexión para el usuario se pueden autenticar las cuentas en un dominio o bosque para servidores NAS en otro dominio o bosque.
- Desea realizar la autenticación y autorización mediante el uso de una base de datos que no es una base de datos de cuenta de Windows. En este caso, las solicitudes de conexión que coinciden con un nombre de dominio Kerberos especificado se reenvían a un servidor RADIUS, que tiene acceso a otra base de datos de cuentas de usuario y los datos de autorización. Algunos ejemplos de otras bases de datos de usuario son las bases de datos de servicios de directorio Novell (NDS) y lenguaje de consulta estructurado (SQL).
- Desea procesar un gran número de solicitudes de conexión. En este caso, en lugar de configurar los clientes RADIUS para intentar equilibrar su conexión y las solicitudes de cuentas entre varios servidores RADIUS, puede configurarlos para enviar sus solicitudes de cuentas y la conexión a un proxy RADIUS NPS. El proxy RADIUS NPS equilibra dinámicamente la carga de conexión y contabilidad de solicitudes entre varios servidores RADIUS y aumenta el procesamiento de grandes cantidades de clientes RADIUS y autenticaciones por segundo.
- Desea proporcionar autenticación RADIUS y una autorización para proveedores de servicios externos y minimizar la configuración de firewall de intranet. Es un firewall de intranet entre la red perimetral (la red entre la intranet e Internet) y la intranet. Al colocar un NPS en la red perimetral, el firewall entre la red perimetral y la intranet debe permitir el tráfico entre el NPS y varios controladores de dominio. Al reemplazar el NPS con un proxy NPS, el firewall debe permitir únicamente tráfico RADIUS entre el proxy NPS y uno o varios NPSs dentro de la intranet.

La siguiente ilustración muestra NPS como proxy RADIUS entre clientes RADIUS y servidores RADIUS.

![NPS como Proxy RADIUS](../../media/Nps-Proxy/Nps-Proxy.jpg)

Con NPS, las organizaciones también pueden subcontratar infraestructura de acceso remoto a un proveedor de servicios mientras conserva el control a través de cuentas, autorización y autenticación de usuario.

Para los escenarios siguientes se pueden crear configuraciones de NPS:

- Acceso inalámbrico
- Acceso remoto telefónico o red privada virtual (VPN) de organización
- Acceso inalámbrico o telefónico externo
- Acceso a Internet
- Acceso autenticado a recursos de la extranet para socios comerciales

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>Ejemplos de configuración de proxy RADIUS y servidores RADIUS

Los ejemplos de configuración siguientes muestran cómo puede configurar NPS como un servidor RADIUS y un proxy RADIUS.

**NPS como servidor RADIUS**. En este ejemplo, NPS se configura como servidor RADIUS, la directiva de solicitud de conexión predeterminada es la única directiva configurada y se procesan todas las solicitudes de conexión al NPS local. NPS puede autenticar y autorizar a los usuarios cuyas cuentas están en el dominio de NPS y en dominios de confianza.

**NPS como proxy RADIUS**. En este ejemplo, se configura NPS como un proxy RADIUS que reenvía las solicitudes de conexión a grupos de servidores RADIUS remotos en dos dominios sin confianza. La directiva de solicitud de conexión predeterminada se elimina y se crean dos nuevas directivas de solicitud de conexión para reenviar solicitudes a cada uno de los dos dominios de confianza. En este ejemplo, NPS no procesa las solicitudes de conexión en el servidor local. 

**NPS como servidor RADIUS y proxy RADIUS**. Además de la directiva de solicitud de conexión predeterminado, que designa que las solicitudes de conexión se procesan localmente, se crea una nueva directiva de solicitud de conexión que reenvía las solicitudes de conexión a NPS u otro servidor RADIUS en un dominio de confianza. Esta segunda directiva se denomina directiva de Proxy. En este ejemplo, la directiva de Proxy aparece en primer lugar en la lista ordenada de directivas. Si la solicitud de conexión coincide con la directiva de Proxy, la solicitud de conexión se reenvía al servidor RADIUS en el grupo de servidores RADIUS remotos. Si la solicitud de conexión no coincide con la directiva Proxy pero coincide con la directiva de solicitud de conexión de forma predeterminada, NPS procesa la solicitud de conexión en el servidor local. Si la solicitud de conexión no coincide con cualquiera de ellas, se descarta.

**NPS como servidor RADIUS con servidores de cuentas remotas**. En este ejemplo, el NPS local no está configurado para administrar cuentas y la directiva de solicitud de conexión predeterminada se revisó para que se reenvían los mensajes de cuentas RADIUS a NPS u otro servidor RADIUS en un grupo de servidores RADIUS remotos. Aunque se reenvían los mensajes de cuentas, no se reenvían los mensajes de autenticación y autorización y el NPS local realiza estas funciones para el dominio local y todos los dominios de confianza.

**NPS con RADIUS remota a la asignación de usuario de Windows**. En este ejemplo, NPS actúa como servidor RADIUS y como proxy RADIUS para cada solicitud de conexión individuales mediante el reenvío de la solicitud de autenticación a un servidor RADIUS remoto mientras se usa una cuenta de usuario de Windows local para la autorización. Esta configuración se implementa mediante la configuración de la de RADIUS remota a atributos de asignación de usuario de Windows como una condición de la directiva de solicitud de conexión. \(Además, se debe crear una cuenta de usuario localmente en el servidor RADIUS que tiene el mismo nombre que la cuenta de usuario remoto en el que se realiza autenticación por el servidor RADIUS remoto.\)

## <a name="configuration"></a>Configuración

Para configurar NPS como servidor RADIUS, puede usar configuración estándar o avanzada de configuración en la consola NPS o en el administrador del servidor. Para configurar NPS como proxy RADIUS, debe usar configuración avanzada.

### <a name="standard-configuration"></a>Configuración estándar

Con la configuración estándar se proporcionan asistentes que le ayudarán a configurar NPS para los escenarios siguientes:

- Servidor RADIUS para conexiones VPN o telefónico
- Servidor RADIUS para conexiones 802.1X inalámbricas o por cable

Para configurar NPS con un asistente, abra la consola de NPS, seleccione uno de los escenarios anteriores y, a continuación, haga clic en el vínculo que abre al asistente.

### <a name="advanced-configuration"></a>Configuración avanzada

Cuando se usa la configuración avanzada manualmente configurar NPS como servidor RADIUS o proxy RADIUS. 

Para configurar NPS con configuración avanzada, abra la consola NPS y, a continuación, haga clic en la flecha situada junto a **configuración avanzada** para expandir esta sección.

Se proporcionan los siguientes elementos de configuración avanzada.

#### <a name="configure-radius-server"></a>Configurar el servidor RADIUS

Para configurar NPS como servidor RADIUS, debe configurar clientes RADIUS, directivas de redes y cuentas RADIUS.

Para obtener instrucciones sobre cómo realizar estas configuraciones, vea los temas siguientes.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar las directivas de red](nps-np-configure.md)
- [Configurar las cuentas de servidor de directivas de red](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>Configurar el proxy RADIUS

Para configurar NPS como proxy RADIUS, debe configurar clientes RADIUS, grupos de servidores RADIUS remotos y directivas de solicitud de conexión.

Para obtener instrucciones sobre cómo realizar estas configuraciones, vea los temas siguientes.

- [Configurar clientes RADIUS](nps-radius-clients-configure.md)
- [Configurar grupos de servidores RADIUS remotos](nps-crp-rrsg-configure.md)
- [Configurar las directivas de solicitud de conexión](nps-crp-configure.md)

## <a name="nps-logging"></a>Registro NPS

Registro NPS también se denomina administración de cuentas RADIUS. Configure el registro NPS a sus requisitos si usa NPS como un servidor RADIUS, proxy o cualquier combinación de estas configuraciones.

Para configurar el registro de NPS, debe configurar qué eventos desea registrados y vistos con el Visor de eventos y, a continuación, determine qué otra información que desee registrar. Además, debe decidir si desea registrar información de cuentas y autenticación de usuario para los archivos de registro de texto almacenados en el equipo local o a una base de datos de SQL Server en el equipo local o en un equipo remoto.

Para obtener más información, consulte [configurar red directiva de servidor de contabilidad](nps-accounting-configure.md).
