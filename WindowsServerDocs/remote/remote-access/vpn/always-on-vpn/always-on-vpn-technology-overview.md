---
title: Información general sobre la tecnología VPN de AlwaysOn
description: 'Este rewriter página proporciona una breve introducción a las tecnologías de VPN de Always On con vínculos a documentos detallados. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 65a575b24ea3c70ad7eedd95fe287d955ccaeea6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749675"
---
# <a name="always-on-vpn-technology-overview"></a>Información general sobre la tecnología de VPN de Always On

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Obtenga información sobre las mejoras de VPN de Always On](always-on-vpn-enhancements.md)
- [**Siguiente:** Obtenga información sobre las características avanzadas de VPN de Always On](deploy/always-on-vpn-adv-options.md)

Para esta implementación, debe instalar a un nuevo servidor de acceso remoto que ejecuta Windows Server 2016, así como modificar parte de su infraestructura existente para la implementación.

La siguiente ilustración muestra la infraestructura necesaria para implementar la VPN de Always On.

![Siempre en la infraestructura VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

El proceso de conexión que se muestra en esta ilustración se compone de los pasos siguientes:

1. Usar servidores DNS públicos, el cliente de VPN de Windows 10 realiza una consulta de resolución de nombres para la dirección IP de la puerta de enlace VPN.

2. Con la dirección IP devuelta por DNS, el cliente VPN envía una solicitud de conexión a la puerta de enlace VPN.

3. La puerta de enlace VPN también está configurado como un cliente de servicio de autenticación remota telefónica de usuario (RADIUS); el cliente de VPN RADIUS envía la solicitud de conexión al servidor NPS o corporativas de la organización para el procesamiento de solicitud de conexión.

4. El servidor NPS procesa la solicitud de conexión, incluida la realización de autorización y autenticación y determina si se debe permitir o denegar la solicitud de conexión.

5. El servidor NPS reenvía una respuesta de aceptación de acceso o denegar el acceso a la puerta de enlace VPN.

6. Se inicia o finaliza la conexión en función de la respuesta que recibió el servidor VPN desde el servidor NPS.

Para obtener más información sobre cada componente de la infraestructura que se muestra en la ilustración anterior, consulte las secciones siguientes.

>[!NOTE]
>Si ya tiene algunas de estas tecnologías implementadas en la red, puede usar las instrucciones que aparecen en esta guía de implementación para realizar una configuración adicional de las tecnologías para este propósito de implementación.

## <a name="domain-name-system-dns"></a>Sistema de nombres de dominio (DNS)

Zonas del sistema de nombres de dominio (DNS) internos y externos son necesarias, que se da por supuesto que la zona interna es un subdominio delegado de la zona externa (por ejemplo, corp.contoso.com y contoso.com).

Obtenga más información sobre [Domain Name System (DNS)](../../../../networking/dns/dns-top.md) o [Guía de red principal](../../../../networking/core-network-guide/core-network-guide.md).

>[!NOTE]
>Otros DNS diseña, por ejemplo, DNS de cerebro dividido (con el mismo nombre de dominio interna y externamente de zonas DNS independientes) o no relacionada con interno y dominios externos (por ejemplo, contoso.local y contoso.com) también son posibles. Para obtener más información sobre la implementación de DNS de cerebro dividido, consulte [uso de directiva DNS para la implementación de DNS Split-Brain](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Firewalls

Asegúrese de que los firewalls permiten el tráfico que es necesario para las comunicaciones de VPN y RADIUS funcionar correctamente.

Para obtener más información, consulte [configurar Firewalls para el tráfico RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Acceso remoto como un servidor VPN de puerta de enlace RAS

En Windows Server 2016, el rol de servidor de acceso remoto está diseñado para realizar bien como un enrutador y un servidor de acceso remoto. por lo tanto, admite una amplia gama de características. Para esta guía de implementación, necesita solo un pequeño subconjunto de estas características: compatibilidad con conexiones VPN de IKEv2 y enrutamiento LAN.

IKEv2 es una protocolo que se describe en la solicitud de Internet Engineering Task Force para comentarios 7296 de túnel de VPN. La ventaja principal de IKEv2 es que tolera interrupciones en la conexión de red subyacente. Por ejemplo, si se pierde temporalmente la conexión o si un usuario mueve un equipo cliente de una red a otra, IKEv2 restaura automáticamente la conexión VPN cuando se restablece la conexión de red, todo ello sin intervención del usuario.

Mediante el uso de puerta de enlace RAS, puede implementar conexiones VPN para proporcionar a los usuarios finales con acceso remoto a la red y los recursos de su organización. Implementación de VPN de Always On mantiene una conexión persistente entre los clientes y la red de su organización, siempre que los equipos remotos están conectados a Internet. Con la puerta de enlace de RAS, también puede crear una conexión VPN de sitio a sitio entre dos servidores en ubicaciones diferentes, como entre la oficina principal y una sucursal y usar la traducción de direcciones de red (NAT) para que los usuarios dentro de la red pueden tener acceso externo recursos, como Internet. Además, la puerta de enlace RAS admite Border Gateway Protocol (BGP), que proporciona servicios de enrutamiento dinámicos cuando sus oficinas remotas también tienen las puertas de enlace de borde que admiten BGP.

Puede administrar las puertas de enlace de servicio de acceso remoto (RAS) mediante el uso de comandos de Windows PowerShell y remotos acceso a Microsoft Management Console (MMC).

## <a name="network-policy-server-nps"></a>Servidor de directivas de redes (NPS)

NPS permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación de solicitudes de conexión y autorización. Cuando se usa NPS como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), configure los servidores de acceso de red, como servidores VPN, como clientes RADIUS en NPS.

Además, se configuran las directivas de redes que usa NPS para autorizar las solicitudes de conexión. También puede configurar la administración de cuentas RADIUS para que NPS registre la información de las cuentas en archivos de registro del disco duro local o en una base de datos de Microsoft SQL Server.

Para obtener más información, consulte [servidor de directivas de redes (NPS)](../../../../networking/technologies/nps/nps-top.md).

## <a name="active-directory-certificate-services"></a>Servicios de certificados de Active Directory

El servidor de la entidad de certificación (CA) es una entidad de certificación que se está ejecutando servicios de certificados de Active Directory. La configuración de VPN requiere una basada en Active Directory infraestructura de clave pública (PKI).

Las organizaciones pueden usar AD CS para mejorar la seguridad al enlazar la identidad de una persona, dispositivo o servicio con la clave pública correspondiente. AD CS también incluye características que permiten administrar la inscripción y revocación de una variedad de entornos escalables. Para obtener más información, consulte [Active Directory Certificate Services Overview](https://technet.microsoft.com/library/hh831740.aspx) y [Guía de diseño de infraestructura de clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante la finalización de la implementación, configurará las siguientes plantillas de certificado en la CA.

- La plantilla de certificado de autenticación de usuario

- La plantilla de certificado de autenticación de servidor VPN

- La plantilla de certificado de autenticación del servidor NPS

### <a name="certificate-templates"></a>Plantillas de certificado

Las plantillas de certificado pueden simplificar notablemente la tarea de administración de una entidad de certificación (CA) por lo que le permite emitir certificados que están preconfigurados para las tareas seleccionadas. El complemento MMC de plantillas de certificado permite realizar las siguientes tareas.

- Ver las propiedades de cada plantilla de certificado.

- Copiar y modificar plantillas de certificado.

- Controlar qué usuarios y equipos pueden leer las plantillas e inscribirse para certificados.

- Realizar otras tareas administrativas relacionadas con las plantillas de certificado.

Las plantillas de certificado son una parte integral de una entidad de certificación (CA) empresarial. Son un elemento importante de la directiva de certificado para un entorno, que es el conjunto de reglas y formatos para la inscripción de certificados, el uso y administración.

Para obtener más información, consulte [plantillas de certificado](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificados de servidor digital

Esta guía de implementación proporciona instrucciones para usar los servicios de certificados de Active Directory (AD CS) para inscribir tanto inscripción automática de certificados para el acceso remoto y los servidores de infraestructura NPS. AD CS le permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, certificados digitales y las capacidades de firma digital de su organización.

Cuando se usan certificados de servidor digital para la autenticación entre los equipos de la red, los certificados proporcionan:

1. Confidencialidad mediante cifrado.

2. Integridad mediante firmas digitales.

3. Autenticación mediante la asociación de claves de certificado con una cuenta de equipo, usuario o dispositivo en una red de equipo.

Para obtener más información, consulte [guía paso a paso de AD CS: Implementación de la jerarquía PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## <a name="active-directory-domain-services-ad-ds"></a>Servicios de dominio de Active Directory (AD DS)

AD DS proporciona una base de datos distribuida que almacena y administra información acerca de los recursos de red y datos específicos de las aplicaciones habilitadas para el uso de directorios. Los administradores pueden usar AD DS para organizar los elementos de una red (por ejemplo, los usuarios, los equipos y otros dispositivos) en una estructura de contención jerárquica. La estructura de contención jerárquica incluye el bosque de Active Directory, los dominios del bosque y las unidades organizativas de cada dominio. Un servidor que ejecuta AD DS se denomina controlador de dominio.

AD DS contiene las cuentas de usuario, las cuentas de equipo y propiedades de la cuenta que son necesarios por protegido Extensible Authentication Protocol (PEAP) para autenticar las credenciales de usuario y para evaluar la autorización para las solicitudes de conexión de VPN. Para obtener información sobre la implementación de AD DS, vea Windows Server 2016 [Guía de red principal](../../../../networking/core-network-guide/Core-Network-Guide.md).

Durante la realización de los pasos descritos en esta implementación, configurará los siguientes elementos en el controlador de dominio.

- Habilitar la inscripción automática de certificados en la directiva de grupo para equipos y usuarios

- Crear el grupo de usuarios de VPN

- Crear el grupo de servidores de VPN

- Crear el grupo de servidores de NPS

### <a name="active-directory-users-and-computers"></a>Usuarios y equipos de Active Directory

Active Directory a los usuarios y equipos es un componente de AD DS que contiene las cuentas que representan entidades físicas, como un equipo, una persona o un grupo de seguridad. Un grupo de seguridad es una colección de cuentas de usuario o equipo que los administradores pueden administrar como una sola unidad. Cuentas de usuario y equipo que pertenecen a un grupo determinado se conocen como miembros del grupo.

Cuentas de usuario en usuarios y equipos de usuarios de Active Directory tienen propiedades de acceso telefónico que se evalúa como NPS durante el proceso de autorización - a menos que el **permiso de acceso de red** propiedad de la cuenta de usuario está establecida en **Control acceso a través de la directiva de red NPS**. Se trata de la configuración predeterminada para todas las cuentas de usuario. Sin embargo, en algunos casos, esta configuración podría tener una configuración diferente que impide que el usuario de la conexión mediante VPN. Para protegerse contra esta posibilidad, puede configurar el servidor NPS para que omita las propiedades de acceso telefónico de la cuenta de usuario.

Para obtener más información, consulte [configurar NPS para la cuenta de usuario Omitir propiedades de acceso telefónico](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).

### <a name="group-policy-management"></a>Administración de directivas de grupo

Administración de directivas de grupo permite la administración de cambios y la configuración basada en Active de configuración del equipo y usuario, incluida la información de usuario y de seguridad. Usar Directiva de grupo para definir configuraciones de grupos de usuarios y equipos.

Con la directiva de grupo, puede especificar valores para las entradas del registro, seguridad, instalación de software, las secuencias de comandos, redirección de carpetas, servicios de instalación remota y mantenimiento de Internet Explorer. La configuración de directiva de grupo que cree se encuentran en un objeto de directiva de grupo (GPO). Al asociar un GPO con contenedores de sistema seleccionados de Active Directory, sitios, dominios y unidades organizativas, puede aplicar la configuración del GPO a los usuarios y equipos de estos contenedores de Active Directory. Para administrar objetos de directiva de grupo en toda la empresa, puede usar el grupo de directiva de administración Editor de Microsoft Management Console (MMC).

## <a name="windows-10-vpn-clients"></a>Clientes VPN de Windows 10

Además de los componentes de servidor, asegúrese de que los equipos cliente configure para que usen la VPN se está ejecutando Windows 10 Anniversary Update (versión 1607). Los clientes de VPN de Windows 10 deben estar unidos al dominio de Active Directory.


El cliente de VPN de Windows 10 es altamente configurable y ofrece muchas opciones. Para ilustrar mejor las características específicas de que este escenario se usa, la tabla 1 identifica las categorías de características VPN y configuraciones específicas que hace referencia esta implementación. Configurará los valores individuales para estas características utilizando el proveedor de servicios de configuración (CSP) de VPNv2 tratan más adelante en esta implementación. 

Tabla 1. Características de VPN y las configuraciones tratadas en esta implementación

| Característica VPN     |     Configuración del escenario de implementación         |
|-----------------|-----------------------------------------------|
| Tipo de conexión |                 IKEv2 nativo                  |
|     Enrutamiento     |                Tunelización dividida                |
| Resolución de nombres |  Sufijo de la lista de información de nombre de dominio y DNS  |
|   Desencadenar    |    Detección de red siempre encendido y confianza    |
| Autenticación  | PEAP-TLS con certificados de usuarios protegidos de TPM: |

>[!NOTE]
>PEAP-TLS y TPM son "Protected Extensible Authentication Protocol con Transport Layer Security" y "Módulo de plataforma segura,", respectivamente.

### <a name="vpnv2-csp-nodes"></a>Nodos CSP de VPNv2

En esta implementación, utilice el nodo de CSP de VPNv2 ProfileXML para crear el perfil VPN que se entrega a los equipos cliente de Windows 10. Proveedores de servicios de configuración (CSP) son interfaces que exponen diversas funcionalidades de administración en el cliente de Windows; Conceptualmente, CSP funcionan de manera similar a cómo funciona la directiva de grupo. Cada CSP tiene nodos de configuración que representan las opciones de configuración individuales. También como la configuración de directiva de grupo, se puede enlazar la configuración de CSP a las claves del registro, archivos, permisos y así sucesivamente. De forma similar a cómo usar el Editor de administración de directivas de grupo para configurar objetos de directiva de grupo (GPO), configure los nodos CSP mediante el uso de una solución de administración (MDM) de dispositivos móviles como Microsoft Intune. Productos de MDM como Intune ofrecen una opción de configuración fácil de usar que configura el CSP en el sistema operativo.

![Administración de dispositivos móviles a la configuración del CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Sin embargo, no se puede configurar algunos nodos CSP directamente a través de una interfaz de usuario (UI), como la consola de administración de Intune. En estos casos, debe configurar la configuración de Open Mobile Alliance identificador uniforme de recursos (OMA-URI) manualmente. Configuración de OMA-URI mediante el protocolo de administración de dispositivos de OMA (OMA-DM), una especificación de administración de dispositivo universales más modernos dispositivos de Apple, Android y Windows son compatibles con. Siempre y cuando cumple con las especificaciones de OMA-DM, todos los productos MDM deben interactuar con estos sistemas operativos de la misma manera.

Windows 10 ofrece muchos de los CSP, pero esta implementación se centra en usar el CSP de VPNv2 para configurar al cliente VPN. El CSP de VPNv2 permite la configuración de cada configuración de perfil VPN en Windows 10 a través de un único nodo CSP. También está incluida en el CSP de VPNv2 es un nodo denominado *ProfileXML*, lo que permite configurar todos los parámetros en un nodo en lugar de individualmente. Para obtener más información acerca de ProfileXML, consulte la sección "Información general de ProfileXML" más adelante en esta implementación. Para obtener más información acerca de cada nodo de CSP de VPNv2, consulte el [CSP de VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información sobre algunas de las características avanzadas de VPN de Always On](deploy/always-on-vpn-adv-options.md)

- [Comience a planear la implementación de VPN de Always On](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>Temas relacionados

- [Soporte de software de servidor de Microsoft para las máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Este artículo describe la directiva de compatibilidad para ejecutar software de servidor de Microsoft en el entorno de máquina virtual de Microsoft Azure (infraestructura como-servicio).

- [Acceso remoto](../../Remote-Access.md): Este tema proporciona información general sobre el rol de servidor de acceso remoto en Windows Server 2016.

- [Guía técnica de VPN de Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Esta guía le guiará a través de las decisiones que tomará para que clientes de Windows 10 en su solución de VPN de la empresa y cómo configurar la implementación. Esta guía hace referencia a VPNv2 Configuration Service Provider (CSP) y proporciona administración de dispositivos móviles en las instrucciones de configuración (MDM) con Microsoft Intune y la plantilla de perfil de VPN para Windows 10.

- [Guía de red principal](../../../../networking/core-network-guide/Core-Network-Guide.md): Esta guía proporciona instrucciones sobre cómo planear e implementar los componentes principales necesarios para una red plenamente funcional y un nuevo dominio de Active Directory en un bosque nuevo.

- [Sistema de nombres de dominio (DNS)](../../../../networking/dns/dns-top.md): En este tema se proporciona información general de los sistemas de nombres de dominio (DNS). En Windows Server 2016, el DNS es un rol de servidor que se puede instalar mediante los comandos del administrador del servidor o Windows PowerShell. Si va a instalar un nuevo bosque de Active Directory y dominio, DNS se instala automáticamente con Active Directory que el servidor de catálogo Global para el bosque y dominio.

- [Información general de servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx): Este documento proporciona información general de servicios de certificados de Active Directory (AD CS) en Windows Server® 2012. AD CS es el rol de servidor que permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, certificados digitales y capacidad de forma digital a su organización.

- [Guía de diseño de infraestructura de clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):  Este wiki proporciona instrucciones sobre el diseño de infraestructuras de clave pública (PKI). Antes de configurar una jerarquía PKI y certificación emisora (CA), debe tener en cuenta de seguridad certificados y directivas de prácticas instrucción su organización (CPS).

- [Guía paso a paso de AD CS: Implementación de la jerarquía PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): Esta guía paso a paso describe los pasos necesarios para configurar una configuración básica de servicios de certificados de Active Directory® (AD CS) en un entorno de laboratorio. AD CS en Windows Server® 2008 R2 ofrece servicios personalizables para crear y administrar certificados de clave pública utilizados en sistemas de seguridad de software que emplean tecnologías de clave pública.

- [Red (NPS) del servidor de directivas](../../../../networking/technologies/nps/nps-top.md): En este tema se proporciona información general del servidor de directivas de redes en Windows Server 2016. Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión.
