---
title: Información general sobre la tecnología VPN Always On
description: 'En esta página se proporciona una breve introducción a las tecnologías de VPN Always On con vínculos a documentos detallados. '
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.openlocfilehash: a5d65dd9f8fb6328bd00be8a46af37ee69ccd2bf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313395"
---
# <a name="always-on-vpn-technology-overview"></a>Información general sobre la tecnología VPN Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Más información sobre las mejoras de VPN de Always On](always-on-vpn-enhancements.md)
- [**Siguiente:** Más información sobre las características avanzadas de Always On VPN](deploy/always-on-vpn-adv-options.md)

Para esta implementación, debe instalar un nuevo servidor de acceso remoto que ejecute Windows Server 2016, así como modificar parte de la infraestructura existente para la implementación.

En la ilustración siguiente se muestra la infraestructura necesaria para implementar Always On VPN.

![Always On infraestructura de VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

El proceso de conexión que se representa en esta ilustración consta de los siguientes pasos:

1. Mediante los servidores DNS públicos, el cliente VPN de Windows 10 realiza una consulta de resolución de nombres para la dirección IP de la puerta de enlace de VPN.

2. Con la dirección IP devuelta por DNS, el cliente VPN envía una solicitud de conexión a la puerta de enlace de VPN.

3. La puerta de enlace de VPN también se configura como un cliente de Servicio de autenticación remota telefónica de usuario (RADIUS). el cliente RADIUS de VPN envía la solicitud de conexión al servidor NPS corporativo o de la organización para el procesamiento de solicitudes de conexión.

4. El servidor NPS procesa la solicitud de conexión, incluida la realización de la autorización y la autenticación, y determina si se permite o se deniega la solicitud de conexión.

5. El servidor NPS reenvía una respuesta de aceptación de acceso o acceso denegado a la puerta de enlace de VPN.

6. La conexión se inicia o finaliza en función de la respuesta que el servidor VPN ha recibido del servidor NPS.

Para obtener más información sobre cada componente de la infraestructura que se describe en la ilustración anterior, vea las secciones siguientes.

>[!NOTE]
>Si ya tiene algunas de estas tecnologías implementadas en la red, puede seguir las instrucciones de esta guía de implementación para realizar una configuración adicional de las tecnologías para este propósito de implementación.

## <a name="domain-name-system-dns"></a>Sistema de nombres de dominio (DNS)

Se requieren zonas de sistema de nombres de dominio (DNS) internas y externas, lo que supone que la zona interna es un subdominio delegado de la zona externa (por ejemplo, corp.contoso.com y contoso.com).

Obtenga más información sobre [el sistema de nombres de dominio (DNS)](../../../../networking/dns/dns-top.md) o la [Guía de red principal](../../../../networking/core-network-guide/core-network-guide.md).

>[!NOTE]
>También se pueden usar otros diseños DNS, como DNS de cerebro dividido (con el mismo nombre de dominio interna y externamente en zonas DNS independientes) o dominios internos y externos no relacionados (por ejemplo, contoso. local y contoso.com). Para obtener más información sobre la implementación de DNS de cerebro dividido, consulte [uso de la Directiva de DNS para la implementación de DNS de cerebro dividido](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Firewalls

Asegúrese de que los firewalls permiten que el tráfico que es necesario para que las comunicaciones VPN y RADIUS funcionen correctamente.

Para obtener más información, consulte [configurar firewalls para el tráfico RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Acceso remoto como un servidor VPN de puerta de enlace RAS

En Windows Server 2016, el rol de servidor de acceso remoto está diseñado para funcionar bien como un enrutador y un servidor de acceso remoto; por lo tanto, admite una amplia gama de características. Para esta guía de implementación, solo necesita un pequeño subconjunto de estas características: compatibilidad con conexiones VPN de IKEv2 y enrutamiento de LAN.

IKEv2 es un protocolo de túnel VPN que se describe en solicitud de fuerza de la tarea de ingeniería de Internet para los comentarios 7296. La principal ventaja de IKEv2 es que tolera las interrupciones en la conexión de red subyacente. Por ejemplo, si la conexión se pierde temporalmente o si un usuario mueve un equipo cliente de una red a otra, IKEv2 restaura automáticamente la conexión VPN cuando se restablece la conexión de red, todo ello sin la intervención del usuario.

Mediante el uso de la puerta de enlace RAS, puede implementar conexiones VPN para proporcionar a los usuarios finales acceso remoto a la red y los recursos de la organización. La implementación de Always On VPN mantiene una conexión persistente entre los clientes y la red de la organización siempre que los equipos remotos estén conectados a Internet. Con la puerta de enlace de RAS, también puede crear una conexión VPN de sitio a sitio entre dos servidores en diferentes ubicaciones, como entre la oficina principal y una sucursal, y usar la traducción de direcciones de red (NAT) para que los usuarios dentro de la red puedan acceder a la ubicación externa. recursos, como Internet. Además, la puerta de enlace RAS admite Protocolo de puerta de enlace de borde (BGP), que proporciona servicios de enrutamiento dinámico cuando las ubicaciones de oficinas remotas también tienen puertas de enlace perimetrales que admiten BGP.

Puede administrar puertas de enlace del servicio de acceso remoto (RAS) mediante los comandos de Windows PowerShell y Microsoft Management Console (MMC) de acceso remoto.

## <a name="network-policy-server-nps"></a>Servidor de directivas de redes (NPS)

NPS le permite crear y aplicar directivas de acceso a la red de toda la organización para la autenticación y autorización de solicitudes de conexión. Cuando se usa NPS como un servidor Servicio de autenticación remota telefónica de usuario (RADIUS), se configuran los servidores de acceso a la red, como los servidores VPN, como clientes RADIUS en NPS.

Además, se configuran las directivas de redes que usa NPS para autorizar las solicitudes de conexión. También puede configurar la administración de cuentas RADIUS para que NPS registre la información de las cuentas en archivos de registro del disco duro local o en una base de datos de Microsoft SQL Server.

Para obtener más información, consulte [servidor de directivas de redes (NPS)](../../../../networking/technologies/nps/nps-top.md).

## <a name="active-directory-certificate-services"></a>Servicios de certificados de Active Directory

El servidor de la entidad de certificación (CA) es una entidad de certificación que ejecuta Active Directory servicios de Certificate Server. La configuración de VPN requiere una infraestructura de clave pública (PKI) basada en Active Directory.

Las organizaciones pueden usar AD CS para mejorar la seguridad mediante el enlace de la identidad de una persona, un dispositivo o un servicio con la clave pública correspondiente. AD CS también incluye características que permiten administrar la inscripción y revocación de certificados en una variedad de entornos escalables. Para obtener más información, vea [información general de servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx) y guía de diseño de infraestructura de [clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante la realización de la implementación, configurará las siguientes plantillas de certificado en la entidad de certificación.

- La plantilla de certificado de autenticación de usuario

- La plantilla de certificado de autenticación de servidor VPN

- La plantilla de certificado de autenticación de servidor NPS

### <a name="certificate-templates"></a>Plantillas de certificado

Las plantillas de certificado pueden simplificar en gran medida la tarea de administrar una entidad de certificación (CA) permitiéndole emitir certificados que estén preconfigurados para las tareas seleccionadas. El complemento MMC plantillas de certificado le permite realizar las siguientes tareas.

- Ver las propiedades de cada plantilla de certificado

- Copiar y modificar plantillas de certificado

- Controlar qué usuarios y equipos pueden leer plantillas e inscribirse para certificados.

- Realizar otras tareas administrativas relacionadas con las plantillas de certificado

Las plantillas de certificado son una parte fundamental de las entidades de certificación (CA) de empresa. Constituyen un elemento importante de la directiva de certificado para un entorno, que es el conjunto de reglas y formatos para la administración, uso e inscripción de certificados.

Para obtener más información, consulte [plantillas de certificado](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificados de servidor digital

En esta guía de implementación se proporcionan instrucciones para usar Active Directory servicios de Certificate Server (AD CS) para inscribir e inscribir automáticamente certificados en servidores de infraestructura NPS y de acceso remoto. AD CS le permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, certificados digitales y capacidades de firma digital para su organización.

Cuando se usan certificados de servidor digital para la autenticación entre equipos de la red, los certificados proporcionan:

1. Confidencialidad mediante cifrado.

2. Integridad mediante firmas digitales.

3. Autenticación mediante la Asociación de claves de certificado con cuentas de equipos, usuarios o dispositivos en una red de equipos.

Para obtener más información, consulte [Guía paso a paso de AD CS: implementación de la jerarquía de PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## <a name="active-directory-domain-services-ad-ds"></a>Servicios de dominio de Active Directory (AD DS)

AD DS proporciona una base de datos distribuida que almacena y administra información acerca de los recursos de red y datos específicos de las aplicaciones habilitadas para el uso de directorios. Los administradores pueden usar AD DS para organizar los elementos de una red (por ejemplo, los usuarios, los equipos y otros dispositivos) en una estructura de contención jerárquica. La estructura de contención jerárquica incluye el bosque de Active Directory, los dominios del bosque y las unidades organizativas de cada dominio. Un servidor que ejecuta AD DS se denomina controlador de dominio.

AD DS contiene las cuentas de usuario, las cuentas de equipo y las propiedades de cuenta que necesita el protocolo de autenticación extensible protegido (PEAP) para autenticar las credenciales de usuario y para evaluar la autorización para las solicitudes de conexión VPN. Para obtener información acerca de la implementación de AD DS, consulte la [Guía de red principal](../../../../networking/core-network-guide/Core-Network-Guide.md)de Windows Server 2016.

Durante la realización de los pasos de esta implementación, configurará los siguientes elementos en el controlador de dominio.

- Habilitar la inscripción automática de certificados en directiva de grupo para equipos y usuarios

- Crear el grupo de usuarios de VPN

- Crear el grupo de servidores VPN

- Crear el grupo de servidores NPS

### <a name="active-directory-users-and-computers"></a>Usuarios y equipos de Active Directory

Active Directory usuarios y equipos es un componente de AD DS que contiene cuentas que representan entidades físicas, como un equipo, una persona o un grupo de seguridad. Un grupo de seguridad es una colección de cuentas de usuario o de equipo que los administradores pueden administrar como una sola unidad. Las cuentas de usuario y de equipo que pertenecen a un grupo determinado se denominan miembros del grupo.

Las cuentas de usuario de Active Directory usuarios y equipos tienen propiedades de marcado que NPS evalúa durante el proceso de autorización, a menos que la propiedad **permiso de acceso a la red** de la cuenta de usuario esté configurada para **controlar el acceso a través de la Directiva de red NPS**. Esta es la configuración predeterminada para todas las cuentas de usuario. En algunos casos, sin embargo, esta configuración podría tener una configuración diferente que impida que el usuario se conecte mediante VPN. Para protegerse frente a esta posibilidad, puede configurar el servidor NPS para que omita las propiedades de acceso telefónico de la cuenta de usuario.

Para obtener más información, consulte [configuración de NPS para omitir las propiedades de acceso telefónico de la cuenta de usuario](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).

### <a name="group-policy-management"></a>Administración de directivas de grupo

La administración de directiva de grupo permite la administración de cambios y configuraciones basados en directorios de configuración de usuarios y equipos, incluida la información de seguridad y de usuario. Utilice directiva de grupo para definir las configuraciones de grupos de usuarios y equipos.

Con directiva de grupo, puede especificar la configuración de las entradas del registro, la seguridad, la instalación de software, los scripts, el redireccionamiento de carpetas, los servicios de instalación remota y el mantenimiento de Internet Explorer. La configuración de directiva de grupo que cree está contenida en un objeto de directiva de grupo (GPO). Al asociar un GPO con los contenedores del sistema Active Directory seleccionados (sitios, dominios y unidades organizativas), puede aplicar la configuración del GPO a los usuarios y equipos de esos contenedores de Active Directory. Para administrar directiva de grupo objetos en una empresa, puede usar el Editor de administración de directivas de grupo Microsoft Management Console (MMC).

## <a name="windows-10-vpn-clients"></a>Clientes VPN de Windows 10

Además de los componentes de servidor, asegúrese de que los equipos cliente que configure para usar VPN ejecuten la actualización de aniversario de Windows 10 (versión 1607). Los clientes de VPN de Windows 10 deben estar Unidos a un dominio en el dominio de Active Directory.


El cliente VPN de Windows 10 es muy configurable y ofrece muchas opciones. Para ilustrar mejor las características específicas que usa este escenario, la tabla 1 identifica las categorías de características de VPN y las configuraciones específicas a las que hace referencia esta implementación. Configurará la configuración individual de estas características mediante el proveedor de servicios de configuración de VPNv2 (CSP) que se describe más adelante en esta implementación. 

Tabla 1. Características y configuraciones de VPN descritas en esta implementación

| Característica VPN     |     Configuración del escenario de implementación         |
|-----------------|-----------------------------------------------|
| Tipo de conexión |                 IKEv2 nativo                  |
|     Enrutamiento     |                Tunelización dividida                |
| Resolución de nombres |  Lista de información de nombres de dominio y sufijo DNS  |
|   Desencadenar    |    Always On y detección de redes de confianza    |
| Autenticación  | PEAP-TLS con TPM: certificados de usuario protegidos |

>[!NOTE]
>PEAP-TLS y TPM son "Protocolo de autenticación extensible protegido con seguridad de la capa de transporte" y "Módulo de plataforma segura", respectivamente.

### <a name="vpnv2-csp-nodes"></a>Nodos de CSP VPNv2

En esta implementación, se usa el nodo ProfileXML VPNv2 CSP para crear el perfil de VPN que se entrega a los equipos cliente de Windows 10. Los proveedores de servicios de configuración (CSP) son interfaces que exponen diversas capacidades de administración dentro del cliente de Windows. conceptualmente, los CSP funcionan de manera similar a cómo funciona directiva de grupo. Cada CSP tiene nodos de configuración que representan la configuración individual. Además de la configuración de directiva de grupo, puede asociar la configuración de CSP a las claves del registro, los archivos, los permisos, etc. De forma similar a cómo se usa el Editor de administración de directivas de grupo para configurar objetos de directiva de grupo (GPO), se configuran los nodos de CSP mediante una solución de administración de dispositivos móviles (MDM) como Microsoft Intune. Los productos MDM como Intune ofrecen una opción de configuración fácil de utilizar que configura el CSP en el sistema operativo.

![Configuración de administración de dispositivos móviles a CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Sin embargo, no se pueden configurar algunos nodos de CSP directamente a través de una interfaz de usuario (IU), como la consola de administración de Intune. En estos casos, debe configurar manualmente la configuración del identificador uniforme de recursos de Open Mobile Alliance (OMA-URI). Configure OMA-URI mediante el protocolo de administración de dispositivos OMA (OMA-DM), una especificación de administración de dispositivos universal que admiten la mayoría de los dispositivos de Apple, Android y Windows modernos. Siempre y cuando se adhiere a la especificación OMA-DM, todos los productos MDM deben interactuar con estos sistemas operativos de la misma manera.

Windows 10 ofrece muchos CSP, pero esta implementación se centra en el uso del CSP VPNv2 para configurar el cliente VPN. El CSP VPNv2 permite la configuración de cada configuración de Perfil de VPN en Windows 10 a través de un nodo CSP único. También se incluye en el CSP VPNv2 es un nodo denominado *ProfileXML*, que le permite configurar todos los valores de un nodo en lugar de hacerlo de forma individual. Para obtener más información sobre ProfileXML, consulte la sección "información general sobre ProfileXML" más adelante en esta implementación. Para obtener más información sobre cada nodo de CSP de VPNv2, consulte el [CSP de VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información acerca de algunas de las características avanzadas de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Inicio de la planeación de la implementación de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>Temas relacionados

- [Compatibilidad del software de servidor de Microsoft con máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): en este artículo se describe la Directiva de soporte técnico para ejecutar el software de servidor de Microsoft en el entorno de máquinas virtuales Microsoft Azure (infraestructura como servicio).

- [Acceso remoto](../../Remote-Access.md): en este tema se proporciona información general sobre el rol de servidor de acceso remoto en Windows Server 2016.

- [Guía técnica de VPN de Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): esta guía le guiará a través de las decisiones que realizará para los clientes de Windows 10 en la solución VPN de la empresa y sobre cómo configurar la implementación. En esta guía se hace referencia al proveedor de servicios de configuración de VPNv2 (CSP) y se proporcionan instrucciones de configuración de administración de dispositivos móviles (MDM) mediante Microsoft Intune y la plantilla de Perfil de VPN para Windows 10.

- [Guía de red principal](../../../../networking/core-network-guide/Core-Network-Guide.md): esta guía proporciona instrucciones sobre cómo planear e implementar los componentes principales necesarios para una red totalmente operativa y un nuevo dominio de Active Directory en un bosque nuevo.

- [Sistema de nombres de dominio (DNS)](../../../../networking/dns/dns-top.md): en este tema se proporciona información general sobre los sistemas de nombres de dominio (DNS). En Windows Server 2016, DNS es un rol de servidor que puede instalar mediante Administrador del servidor o comandos de Windows PowerShell. Si va a instalar un nuevo bosque y dominio de Active Directory, DNS se instala automáticamente con Active Directory como el servidor de catálogo global para el bosque y el dominio.

- [Información general sobre servicios de Certificate Server de Active Directory](https://technet.microsoft.com/library/hh831740.aspx): este documento proporciona información general sobre Active Directory servicios de Certificate Server (AD CS) en Windows Server® 2012. AD CS es el rol de servidor que permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, certificados digitales y capacidad de forma digital a su organización.

- [Guía de diseño de infraestructura de clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx): este wiki proporciona instrucciones sobre el diseño de infraestructuras de clave pública (PKI). Antes de configurar una PKI y una jerarquía de entidad de certificación (CA), debe tener en cuenta la Directiva de seguridad de la organización y el informe de prácticas de certificados (CPS).

- [Guía paso a paso de AD CS: implementación de una jerarquía de PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): en esta guía paso a paso se describen los pasos necesarios para configurar una configuración básica de Active Directory® servicios de Certificate Server (AD CS) en un entorno de laboratorio. AD CS en Windows Server® 2008 R2 proporciona servicios personalizables para la creación y administración de certificados de clave pública usados en sistemas de seguridad de software que emplean tecnologías de clave pública.

- [Servidor de directivas de redes (NPS)](../../../../networking/technologies/nps/nps-top.md): en este tema se proporciona información general sobre el servidor de directivas de redes en Windows Server 2016. Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.
