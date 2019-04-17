---
title: Siempre en la descripción de la tecnología VPN
description: 'Esta página se provies una breve introducción de las tecnologías de VPN de Always On con vínculos a documentos detallados. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031329"
---
# Introducción a la tecnología de VPN de Always On
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** más información sobre las mejoras de VPN de Always On](always-on-vpn-enhancements.md)<br>
& #187;  [ **Siguiente:** más información sobre las características avanzadas de VPN de Always On](deploy/always-on-vpn-adv-options.md)

Para esta implementación, debe instalar a un nuevo servidor de acceso remoto que ejecuta Windows Server 2016, así como modificar alguno de la infraestructura existente para la implementación.

La siguiente ilustración muestra la infraestructura necesaria para implementar VPN de Always On.

![Siempre en la infraestructura de VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

El proceso de conexión que se muestra en la siguiente ilustración se compone de los siguientes pasos:

1. Con los servidores DNS públicos, el cliente VPN de Windows 10 realiza una consulta de resolución de nombre para la dirección IP de la puerta de enlace VPN.

2. Con la dirección IP devuelta por DNS, el cliente VPN envía una solicitud de conexión a la puerta de enlace VPN.

3. La puerta de enlace VPN también está configurado como un \(RADIUS\) servicio autenticación remota telefónica de usuario cliente; el cliente de VPN RADIUS envía la solicitud de conexión al servidor NPS organización o empresa para el procesamiento de la solicitud de conexión.

4. El servidor NPS procesa la solicitud de conexión, incluida la realización de autorización y autenticación y determina si se deben permitir o denegar la solicitud de conexión.

5. El servidor NPS envía una respuesta de aceptación de acceso o denegar el acceso a la puerta de enlace VPN.

6. Se inicia o finaliza la conexión en función de la respuesta que el servidor VPN recibido desde el servidor NPS.

Para obtener más información sobre cada componente de infraestructura que se muestra en la ilustración anterior, consulta las siguientes secciones.

>[!NOTE]
>Si ya tienes algunas de estas tecnologías implementadas en la red, puedes usar las instrucciones que aparecen en esta guía de implementación para realizar una configuración adicional de las tecnologías para este propósito de implementación.

## Sistema de nombres de dominio (DNS)

Zonas de sistema de nombres de dominio (DNS) internos y externos son necesarias, que se supone que la zona interna es un subdominio delegado de la zona externo (por ejemplo, corp.contoso.com y contoso.com).

Obtén más información acerca [Del sistema de nombres de dominio (DNS)](../../../../networking/dns/dns-top.md) o [Guía de red principal](../../../../networking/core-network-guide/core-network-guide.md).




>[!NOTE] 
>Otros DNS diseños, como Brain DNS (con el mismo nombre de dominio interna y externamente en diferentes zonas DNS) o no estén relacionados con interno y dominios externos (por ejemplo, contoso.local y contoso.com) también son posibles. Para obtener más información sobre la implementación de DNS de cerebro dividido, consulta el [Uso de directiva de DNS para la implementación de DNS de cerebro/en dos paneles](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## Servidores de seguridad

Asegúrate de que los firewalls permiten el tráfico que es necesario para las comunicaciones RADIUS y VPN funcione correctamente.

Para obtener más información, consulta [Configurar servidores de seguridad para el tráfico RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Acceso remoto como un servidor VPN de puerta de enlace RAS

En Windows Server 2016, el rol de servidor de acceso remoto está diseñado para funcionar bien como un enrutador y un servidor de acceso remoto. por lo tanto, admite una amplia variedad de características. Esta guía de implementación, se requiere solo un pequeño subconjunto de estas características: compatibilidad con conexiones VPN IKEv2 y el enrutamiento de LAN.

IKEv2 es una VPN se describe en la solicitud de grupo de trabajo de ingeniería de Internet para 7296 de comentarios de protocolo de túnel. La principal ventaja de IKEv2 es que tolera interrupciones en la conexión de red subyacente. Por ejemplo, si se pierde temporalmente la conexión o si un usuario desplaza un equipo cliente desde una red a otro, IKEv2 restaura automáticamente la conexión VPN cuando se restablece la conexión de red, todo ello sin intervención del usuario.

Mediante el uso de puerta de enlace RAS, puedes implementar las conexiones de VPN para proporcionar a los usuarios finales acceso remoto a la red y los recursos de la organización. Implementación de VPN de Always On mantiene una conexión persistente entre los clientes y la red de su organización siempre que los equipos remotos estén conectados a Internet. Con la puerta de enlace RAS, también puede crear una conexión de VPN de sitio a sitio entre dos servidores en distintas ubicaciones, como entre la oficina principal y una sucursal y usar \(NAT\) traducción de direcciones de red para que los usuarios dentro de la red pueden acceder externos recursos, como Internet. Además, puerta de enlace RAS admite el protocolo de puerta de enlace de borde (BGP), que proporciona servicios de enrutamiento dinámicos cuando las ubicaciones de oficinas remotas también tienen puertas de enlace de borde que admiten BGP.

Puedes administrar las puertas de enlace de servicio de acceso remoto (RAS) mediante comandos de Windows PowerShell y el remoto acceso a Microsoft Management Console (MMC).



## Servidor de directivas de redes (NPS)

NPS te permite crear y aplicar directivas de acceso de red en toda la organización de autorización y autenticación de la solicitud de conexión. Cuando se usa NPS como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), configuran servidores de acceso de red, como servidores VPN, como clientes RADIUS en NPS.

También configurar las directivas de red que usa NPS para autorizar las solicitudes de conexión y puedes configurar cuentas RADIUS para que NPS registra información de cuentas para registrar los archivos en el disco duro local o en una base de datos de Microsoft SQL Server.

Para obtener más información, consulta el [Servidor de directivas de redes (NPS)](../../../../networking/technologies/nps/nps-top.md).


## Servicios de certificados de Active Directory

El servidor de entidad de certificación (CA) es una entidad de certificación que se está ejecutando los servicios de certificados de Active Directory. La configuración de VPN requiere una Active Directory-based infraestructura de clave pública (PKI).

Las organizaciones pueden usar AD CS para mejorar la seguridad al enlazar la identidad de una persona, el dispositivo o el servicio a una clave pública correspondiente. AD CS también incluye características que te permiten administrar la inscripción de certificados y revocación en una variedad de entornos escalables. Para obtener más información, consulta la [Introducción de servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx) y [Guía de diseño de infraestructura de clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Durante la finalización de la implementación, va a configurar las siguientes plantillas de certificado en la entidad de certificación.

-   La plantilla de certificado de autenticación de usuario

-   La plantilla de certificado de autenticación de servidor VPN

-   La plantilla de certificado de autenticación de servidor NPS

### Plantillas de certificado

Plantillas de certificado pueden simplificar en gran medida la tarea de administración de una entidad de certificación (CA) por lo que te permite para emitir certificados preconfiguradas para tareas seleccionadas. El complemento MMC de plantillas de certificado te permite realizar las siguientes tareas.

-   Ver las propiedades de cada plantilla de certificado.

-   Copiar y modificar las plantillas de certificado.

-   Controlar los usuarios y equipos pueden leer las plantillas e inscribir certificados.

-   Realizar otras tareas administrativas relativas a las plantillas de certificado.

Plantillas de certificado son una parte integral de una entidad de certificación (CA) de empresa. Son un elemento importante de la directiva de certificados en un entorno, que es el conjunto de reglas y formatos para la inscripción de certificados, el uso y la administración.

Para obtener más información, consulta [Las plantillas de certificado](https://technet.microsoft.com/library/cc730705.aspx).

### Certificados de servidor digital

Esta guía de implementación proporciona instrucciones para usar los servicios de certificados de Active Directory (AD CS) para inscribir e inscribir certificados para acceso remoto y servidores de infraestructura NPS automáticamente. AD CS te permite generar una infraestructura de clave pública (PKI) y proporciona capacidades de firma digital, los certificados digitales y criptografía de clave pública para la organización.

Cuando se utilizan certificados de servidor digital para la autenticación entre los equipos de la red, se proporcionan los certificados:

1.  Confidencialidad mediante el cifrado.

2.  Integridad mediante firmas digitales.

3.  Autenticación mediante la asociación de claves de certificado con una cuenta de equipo, el usuario o el dispositivo en una red de equipos.

Para obtener más información, consulta [Guía paso a paso de AD CS: implementación de jerarquía de PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## Servicios de dominio de Active Directory (AD DS)

AD DS proporciona una base de datos distribuida que almacena y administra la información acerca de los recursos de red y datos específicos de la aplicación de aplicaciones habilitadas para directorio. Los administradores pueden usar AD DS para organizar los elementos de una red, como los usuarios, equipos y otros dispositivos, en una estructura de contención jerárquica. La estructura de contención jerárquica incluye el bosque de Active Directory, dominios en el bosque y unidades organizativas (OU) en cada dominio. Un servidor que ejecuta AD DS se denomina un controlador de dominio.

AD DS contiene las cuentas de usuario, las cuentas de equipo y propiedades de la cuenta que son necesarios por Extensible Authentication Protocolo protegido (PEAP) para autenticar las credenciales de usuario y para evaluar la autorización para las solicitudes de conexión VPN. Para obtener información sobre la implementación de AD DS, consulta la [Guía de red principal](../../../../networking/core-network-guide/Core-Network-Guide.md)de Windows Server 2016.



Durante la finalización de los pasos de esta implementación, va a configurar los siguientes elementos en el controlador de dominio.

-   Habilitar la inscripción automática de certificado en la directiva de grupo para equipos y usuarios

-   Crear el grupo de usuarios de VPN

-   Crear el grupo de servidores VPN

-   Crear el grupo de servidores de NPS

### Equipos y usuarios de active Directory

Active Directory a los usuarios y equipos es un componente de AD DS que contiene las cuentas que representan las entidades físicas, como un equipo, una persona o un grupo de seguridad. Un grupo de seguridad es una colección de cuentas de usuario o equipo que los administradores pueden administrar como una sola unidad. Las cuentas de usuario y de equipo que pertenecen a un determinado grupo se conocen como miembros del grupo.

Cuentas de usuario en equipos y usuarios de Active Directory tienen propiedades de acceso telefónico que NPS evalúa durante el proceso de autorización - a menos que se establece la propiedad de **Permiso de acceso de red** de la cuenta de usuario en **controlar el acceso a través de la directiva de red de NPS **. Se trata de la configuración predeterminada para todas las cuentas de usuario. Sin embargo, en algunos casos, esta configuración podría tener una configuración diferente que impide que el usuario conecta mediante la VPN. Para proteger contra esta posibilidad, puedes configurar el servidor NPS para omitir propiedades de marcado de la cuenta de usuario.

Para obtener más información, consulta [Configurar NPS para propiedades de acceso telefónico ignorar la cuenta de usuario](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### Administración de directivas de grupo

Administración de directivas de grupo permite la administración de cambios y la configuración basada en el directorio de configuración de usuario y del equipo, incluida la información de usuario y de seguridad. Usa la directiva de grupo para definir configuraciones para grupos de usuarios y equipos.

Con la directiva de grupo, puedes especificar opciones para las entradas del registro, seguridad, instalación de software, scripts, redireccionamiento de carpetas, servicios de instalación remota y mantenimiento de Internet Explorer. La configuración de directiva de grupo que crees se encuentra en un objeto de directiva de grupo (GPO). Mediante la asociación de un GPO con contenedores de sistema de Active Directory seleccionados: sitios, dominios y unidades organizativas, puedes aplicar la configuración del GPO a los usuarios y equipos de los contenedores de Active Directory. Para administrar objetos de directiva de grupo en una empresa, puedes usar el grupo de directiva de administración de Editor Microsoft Management Console (MMC).


## Clientes VPN de Windows 10

Además de los componentes de servidor, asegúrate de que los equipos cliente se configura para usar VPN con actualización de aniversario de Windows 10 (version1607). Los clientes de VPN de Windows 10 deben estar unidos a un dominio en el dominio de Active Directory.


El cliente VPN de Windows 10 es configurable y ofrece muchas opciones. Para ilustrar mejor las características específicas que se usa en este escenario, Tabla1 identifica las categorías de característica VPN y configuraciones específicas en la que hace referencia a esta implementación. Se configura las opciones de configuración individuales para que estas características mediante el uso del proveedor de servicios de configuración (CSP) VPNv2 se describen más adelante en esta implementación. 

Tabla 1. Características VPN y las configuraciones descritas en esta implementación

| **Característica de VPN** | **Configuración del escenario de implementación**         |
|-----------------|-----------------------------------------------|
| Tipo de conexión | IKEv2 nativo                                  |
| Enrutamiento         | Dividir túnel                               |
| Resolución de nombres | Sufijo DNS y la lista de información de nombre de dominio   |
| Desencadenar      | Detección de red siempre activado y de confianza       |
| Autenticación  | PEAP-TLS con certificados de usuario protegidas por el TPM |
---

>[!NOTE] 
>PEAP-TLS y TPM son "Protected Extensible Authentication Protocol con Transport Layer Security" y "Módulo de plataforma," respectivamente.

### VPNv2 CSP nodos

En esta implementación, usa el nodo ProfileXML VPNv2 CSP para crear el perfil de VPN que se entrega a los equipos cliente de Windows 10. Proveedores de servicios de configuración (CSP) son interfaces que exponen distintas funciones de administración en el cliente de Windows; Conceptualmente, CSP funcionan de manera similar a cómo funciona la directiva de grupo. Cada CSP tiene nodos de configuración que representan las opciones de configuración individuales. También como configuración de directiva de grupo, puede vincular las configuraciones de CSP para las claves del registro, archivos, permisos y así sucesivamente. Similar a cómo usar el Editor de administración de directivas de grupo para configurar objetos de directiva de grupo (GPO), configurar los nodos CSP mediante el uso de una solución de administración (MDM) de dispositivos móviles, como Microsoft Intune. Productos MDM como Intune ofrecen una opción de configuración sencilla que configure el CSP en el sistema operativo.

![Administración de dispositivos móviles a la configuración de CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Sin embargo, no puedes configurar algunos nodos CSP directamente a través de una interfaz de usuario (UI) como la consola de administración de Intune. En estos casos, debes configurar las opciones de configuración de Open Mobile Alliance identificador uniforme de recursos (OMA-URI) manualmente. Configurar a OMA-URI mediante el protocolo de administración de dispositivos de OMA (OMA-DM), que es una especificación de administración de dispositivos universales compatibles con dispositivos más modernos de Apple, Android y Windows. Siempre que cumplen con la especificación de OMA DM, todos los productos MDM deben interactuar con estos sistemas operativos de la misma manera.

Windows 10 ofrece muchas CSP, pero esta implementación se centra en el uso de VPNv2 CSP para configurar al cliente VPN. VPNv2 CSP permite la configuración de cada opción de configuración de perfil VPN en Windows 10 a través de un nodo único de CSP. También se encuentra en el CSP de VPNv2 es un nodo denominado *ProfileXML*, que te permite configurar toda la configuración en un nodo en lugar de forma individual. Para obtener más información sobre ProfileXML, consulta la sección "Información general de ProfileXML" más adelante en esta implementación. Para obtener más información acerca de cada nodo VPNv2 CSP, consulta el [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## Pasos siguientes

- [Obtén información sobre algunas de las características avanzadas de VPN de Always On](deploy/always-on-vpn-adv-options.md)

- [Empezar a planificar la implementación de VPN de Always On](deploy/always-on-vpn-deploy-deployment.md)


---

## Temas relacionados
- [Software de servidor de Microsoft se admiten para las máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): este artículo describe la directiva de soporte técnico para ejecutar software de servidor de Microsoft en el entorno de máquina virtual de Microsoft Azure (infraestructura como-servicio).

- [Acceso remoto](../../Remote-Access.md): en este tema proporciona información general sobre el rol de servidor de acceso remoto en Windows Server 2016.

- [Guía técnica de VPN de Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): esta guía explica las decisiones que tomarás para los clientes de Windows 10 en la solución VPN de la empresa y cómo configurar la implementación. Esta guía hace referencia al Proveedor de servicios de configuración (CSP) VPNv2 y proporciona instrucciones de configuración de la administración de dispositivos móviles (MDM) mediante Microsoft Intune y la plantilla de perfil de VPN para Windows 10.

- [Guía de red principal](../../../../networking/core-network-guide/Core-Network-Guide.md): esta guía proporciona instrucciones sobre cómo planear e implementar los componentes principales necesarios para una red totalmente funcional y un nuevo dominio de Active Directory en un nuevo bosque.

- [Sistema de nombres de dominio (DNS)](../../../../networking/dns/dns-top.md): este tema proporciona una descripción general de sistemas de nombres de dominio (DNS). En Windows Server 2016, DNS es un rol de servidor que puedes instalar mediante comandos de administrador del servidor o Windows PowerShell. Si vas a instalar un nuevo bosque de Active Directory y dominio, DNS se instala automáticamente con Active Directory que el servidor de catálogo Global para el dominio y bosque. 

- [Introducción de servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx): este documento proporciona una visión general de servicios de certificados de Active Directory (AD CS) en Windows Server® 2012. AD CS es el rol de servidor que te permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, los certificados digitales y las capacidades de firma digital para la organización.

- [Guía de diseño de infraestructura de clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx): este wiki proporciona instrucciones sobre cómo diseñar infraestructuras de clave pública (PKI). Antes de configurar una jerarquía PKI y la certificación (CA) de la entidad, debe tener en cuenta de seguridad Directiva y certificado de práctica enunciado la organización.

- [Guía paso a paso de AD CS: implementación de jerarquía de PKI de dos niveles](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): esta guía paso a paso describe los pasos necesarios para establecer una configuración básica de servicios de certificados de Active Directory® (AD CS) en un entorno de laboratorio. AD CS en Windows Server® 2008 R2 ofrece servicios personalizables para crear y administrar certificados de claves públicas que se usa en sistemas de seguridad de software que emplean tecnologías de clave pública.

- [Servidor de directivas de redes (NPS)](../../../../networking/technologies/nps/nps-top.md): este tema proporciona una visión general de servidor de directivas de red en Windows Server 2016. Servidor de directivas de redes (NPS) te permite crear y aplicar directivas de acceso de red de toda la organización para la autenticación y autorización de solicitudes de conexión. 

---
