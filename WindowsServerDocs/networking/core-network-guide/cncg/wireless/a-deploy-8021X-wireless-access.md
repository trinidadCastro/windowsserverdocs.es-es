---
title: Implementación del acceso inalámbrico autenticado mediante 802.1X basado en contraseña
description: Este tema forma parte de la Guía de redes de Windows Server 2016 "Implementar mediante 802.1X basado en contraseña X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f34580f4a0fd92c6f059d19d09a6926fc2775960
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840016"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Implementación de contraseña\-en función de acceso inalámbrico autenticado mediante 802.1X

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta es una guía complementaria a Windows Server&reg; Guía de red principal de 2016. La Guía de red principal proporciona instrucciones para planear e implementar los componentes necesarios para una red plenamente funcional y un nuevo dominio Active Directory® en un bosque nuevo.

Esta guía explica cómo basarse en una red principal proporcionando instrucciones sobre cómo implementar Institute of Electrical and Electronics Engineers \(IEEE\) 802.1X\-uso de acceso inalámbrico IEEE 802.11 autenticado Protocolo de autenticación Extensible: protocolo de autenticación de Microsoft desafío mutuo versión 2 protegido \(PEAP\-MS\-CHAP v2\).

Dado que PEAP\-MS\-CHAP v2 requiere que los usuarios proporcionen contraseña\-en función de las credenciales en lugar de un certificado durante el proceso de autenticación, es normalmente más fácil y menos costoso de implementar que EAP\-TLS o PEAP\-TLS.

>[!NOTE]
>En esta guía, IEEE 802.1X Authenticated Wireless Access con PEAP\-MS\-CHAP v2 se abrevia a "acceso inalámbrico" y "Acceso Wi-Fi".

## <a name="about-this-guide"></a>Acerca de esta guía
Esta guía, junto con las guías de requisitos previos que se describen a continuación, proporciona instrucciones sobre cómo implementar la siguiente infraestructura de acceso Wi-Fi.  

- Uno o más 802.1X\-puntos de acceso inalámbrico 802.11 compatibles con \(APs\).

- Servicios de dominio de Active Directory \(AD DS\) usuarios y equipos.

- Administración de directivas de grupo.

- Uno o varios servidores de directivas de red \(NPS\) servidores.

- Certificados de servidor para equipos con NPS.

- Equipos cliente inalámbricos con Windows&reg; 10, Windows 8.1 o Windows 8.

### <a name="dependencies-for-this-guide"></a>Dependencias en esta guía

Para implementar correctamente inalámbricas autenticadas con esta guía, debe tener un entorno de red y el dominio con todas las tecnologías necesarias implementado. También debe tener implementados en sus NPSs autenticación de certificados de servidor.

Las secciones siguientes proporcionan vínculos a documentación que se muestra cómo implementar estas tecnologías.

#### <a name="network-and-domain-environment-dependencies"></a>Dependencias del entorno de red y dominio

Esta guía está diseñada para los administradores de red y del sistema que han seguido las instrucciones de Windows Server 2016 **Guía de red principal** para implementar una red principal, o para aquellos que han implementado previamente las tecnologías básicas incluido en la red principal, como AD DS, sistema de nombres de dominio \(DNS\), Dynamic Host Configuration Protocol \(DHCP\), TCP\/IP, NPS y servicio de nombres Internet de Windows \(WINS\).  

La Guía de red principal está disponible en las siguientes ubicaciones:

- Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponible en la biblioteca técnica de Windows Server 2016. 

- El [Guía de red principal](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) también está disponible en formato Word en la Galería de TechNet en [ https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683 ](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dependencias de certificado de servidor
Hay dos opciones disponibles para la inscripción de los servidores de autenticación con certificados de servidor para su uso con la autenticación 802.1X - implementar su propia infraestructura de clave pública mediante el uso de servicios de certificados de Active Directory \(AD CS\) o usar certificados de servidor que están inscritos por una entidad de certificación pública \(CA\).

##### <a name="ad-cs"></a>AD CS
Los administradores de red y sistema de implementación inalámbrica autenticada deben seguir las instrucciones en el Windows Server 2016 Core Network Companion Guide, **implementar certificados de servidor para las implementaciones inalámbricas y cableadas 802.1X**. Esta guía explica cómo implementar y usar AD CS para inscribir automáticamente certificados de servidor para equipos que ejecutan NPS.

Esta guía está disponible en la siguiente ubicación.

- Windows Server 2016 Core Network Companion Guide [implementar certificados de servidor para las implementaciones inalámbricas y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) en formato HTML en la biblioteca técnica. 

##### <a name="public-ca"></a>CA pública
Puede adquirir certificados de servidor de una CA pública, como VeriSign, que ya confían los equipos cliente. 

Un equipo cliente confía en una entidad de certificación cuando se instala el certificado de CA en el almacén de certificados entidades emisoras raíz de confianza. De forma predeterminada, los equipos que ejecutan Windows tienen varios certificados de CA públicos instalados en su certificado entidades emisoras raíz de confianza almacenar.  

Se recomienda que revises las guías de diseño e implementación de todas las tecnologías que se usan en este escenario de implementación. Estas guías pueden ayudarte a determinar si este escenario de implementación ofrece los servicios y la configuración que necesitas para la red de tu organización.

### <a name="requirements"></a>Requisitos
Siguientes son los requisitos para implementar una infraestructura de acceso inalámbrico mediante el escenario que se documentan en esta guía:

- Antes de implementar este escenario, primero debe adquirir 802.1X\-puntos de acceso inalámbricos para proporcionar cobertura inalámbrica en las ubicaciones adecuadas en el sitio. La sección de planeación de esta guía ayuda a determinar las características que deben ser compatibles con los puntos de acceso.

- Servicios de dominio de Active Directory \(AD DS\) está instalado, como son las tecnologías de red necesario, según las instrucciones de la Guía de red de Windows Server 2016 Core.  

- Se implementa AD CS y certificados de servidor se inscriben en NPSs. Estos certificados son necesarios al implementar el PEAP\-MS\-CHAP v2 certificado\-según el método de autenticación que se usa en esta guía.

- Un miembro de su organización está familiarizado con los estándares IEEE 802.11 compatibles con tus AP inalámbricos y los adaptadores de red inalámbrica que se instalan en los equipos cliente y dispositivos de la red. Por ejemplo, alguien de su organización está familiarizado con los tipos de radiofrecuencia, autenticación inalámbrica 802.11 \(WPA o WPA2\)y cifrados \(AES o TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Qué no incluye esta guía  
Estos son algunos elementos de que esta guía no proporciona:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Orientación integral para la selección de 802.1X\-puntos de acceso inalámbricos

Dado que existen muchas diferencias entre las marcas y modelos de 802.1X\-AP inalámbricos compatibles con, esta guía no proporciona información detallada acerca de:  

- Determinar qué marca o el modelo de AP inalámbrico es la mejor se adapte a sus necesidades.

- La implementación física de AP inalámbricos en la red.

- Advanced configuración AP inalámbricos, como para redes de área Local virtual inalámbricas \(VLAN\).

- Instrucciones sobre cómo configurar los proveedores de AP inalámbricos\-atributos específicos en NPS.

Además, terminología y los nombres de configuración varían entre los modelos y las marcas de AP inalámbricas y podrían no coincidir con los nombres de configuración genérica que se usan en esta guía. Para detalles de configuración del AP inalámbricos, debe revisar la documentación del producto proporcionada por el fabricante de tus AP inalámbricos.

### <a name="instructions-for-deploying-nps-certificates"></a>Instrucciones para implementar certificados NPS
  
Existen dos alternativas para implementar certificados NPS. Esta guía no proporciona una orientación completa para ayudarle a determinar qué alternativa mejor adapta a sus necesidades. En general, sin embargo, las opciones que se enfrenta a son:

- Compra de certificados de una CA pública, como VeriSign, que ya confían Windows\-los clientes basados en. Normalmente, se recomienda esta opción para redes más pequeñas.

- Implementar una infraestructura de clave pública \(PKI\) en la red mediante el uso de AD CS. Esto se recomienda para la mayoría de las redes y las instrucciones sobre cómo implementar certificados de servidor con AD CS están disponibles en la Guía de implementación se ha mencionado anteriormente.

### <a name="nps-network-policies-and-other-nps-settings"></a>Las directivas de red NPS y otras configuraciones de NPS

Excepto para las opciones de configuración que se realizan al ejecutar el **configurar 802.1X** asistente, tal como se describe en esta guía, esta guía no proporciona información detallada para configurar manualmente las condiciones, restricciones u otros NPS de NPS Configuración.

### <a name="dhcp"></a>DHCP

Esta guía de implementación no proporciona información sobre el diseño o implementación de subredes DHCP para redes LAN inalámbricas.

## <a name="technology-overviews"></a>Introducción a las tecnologías
Estos son información general de tecnología para la implementación de acceso inalámbrico:

### <a name="ieee-8021x"></a>IEEE 802.1X
El estándar IEEE 802.1x define el puerto\-en función de control de acceso de red que se usa para proporcionar acceso de red autenticado a redes Ethernet. Este puerto\-control de acceso de red basada en usa las características físicas de la infraestructura de LAN conmutada para autenticar los dispositivos conectados a un puerto de LAN. Se puede denegar el acceso al puerto si se produce un error en el proceso de autenticación. Aunque este estándar se diseñó para redes Ethernet cableadas, se ha adaptado para usarlo en LAN inalámbricas 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-puntos de acceso inalámbricos \(APs\)
Este escenario requiere la implementación de uno o más 802.1X\-AP inalámbricos compatibles con que son compatibles con el marcado de autenticación remota\-en el servicio de usuario \(RADIUS\) protocolo.

802.1X y RADIUS\-conformes APs, cuando se implementa en una infraestructura de RADIUS con un servidor RADIUS, como un NPS, se denominan *clientes RADIUS*.

### <a name="wireless-clients"></a>Clientes inalámbricos
Esta guía proporciona los detalles de configuración completa para proporcionar acceso autenticado para dominio 802.1X\-usuarios de miembro que se conectan a la red con equipos cliente inalámbricos que ejecutan Windows 10, Windows 8.1 y Windows 8. Los equipos deben estar unidos al dominio con el fin de establecer correctamente el acceso autenticado.

>[!NOTE]
>También puede usar los equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como clientes inalámbricos.

### <a name="support-for-ieee-80211-standards"></a>Compatibilidad con IEEE 802.11 estándares
Sistemas operativos de Windows y Windows Server compatibles proporcionan creado\-en la compatibilidad de 802.11 inalámbrica de red. En estos sistemas operativos, un adaptador de red inalámbrica 802.11 instalado aparece como una conexión de red inalámbrica en el centro de redes y recursos compartidos. 

Aunque no existe, se compila\-en compatibilidad con redes inalámbricas 802.11, los componentes inalámbricos de Windows dependen de lo siguiente:

- Las capacidades del adaptador de red inalámbrica. El adaptador de red inalámbrica instalado debe admitir la LAN inalámbrica o estándares de seguridad inalámbrica que necesite. Por ejemplo, si el adaptador de red inalámbrica no admite Wi\-Fi Protected Access \(WPA\), no se puede habilitar o configurar las opciones de seguridad de WPA.

- Las capacidades del controlador del adaptador de red inalámbrica. Para que pueda configurar las opciones de red inalámbrica, el controlador del adaptador de red inalámbrico debe admitir los informes de toda su funcionalidad a Windows. Compruebe que el controlador para el adaptador de red inalámbrica está escrito para las capacidades del sistema operativo. Asegúrese también de que el controlador es la versión más reciente mediante la comprobación de Microsoft Update o el sitio Web del fabricante del adaptador de red inalámbrica.

En la tabla siguiente se muestra la velocidad de transmisión y las frecuencias de estándares comunes de IEEE 802.11 inalámbricas.

|Estándares|Frecuencias|Transmisión velocidades de bits|Uso|
|-------------|---------------|--------------------------|---------|  
|802.11|S\-banda Industrial, científico y médico \(ISM\) el intervalo de frecuencia \(2.4 a 2,5 GHz\)|2 megabits por segundo \(Mbps\)|Obsoleto. No se usan habitualmente.|  
|802.11b|S\-banda ISM|11 Mbps|Se utiliza habitualmente.|  
|802.11a|C\-banda ISM \(5,725 a 5.875 GHz\)|54 Mbps|No se usan habitualmente debido a los gastos y rango limitado.|  
|802.11g|S\-banda ISM|54 Mbps|Se usa ampliamente. los dispositivos 802.11g son compatibles con 802. 11b dispositivos.|  
|802. 11n \2.4 y 5.0 GHz|C\-banda y S\-banda ISM|250 Mbps|Los dispositivos según el pre\-ratificación IEEE 802. 11n estándar empezó a estar disponible en agosto de 2007. Muchos 802. 11n dispositivos son compatibles con 802. 11a, b y g dispositivos.|
|802.11ac |5 GHz |6,93 Gbps |802. 11ac, aprobada por el IEEE en 2014, es más escalable y más rápido que 802. 11n y se implementa en puntos de acceso y los clientes inalámbricos admiten.|

### <a name="wireless-network-security-methods"></a>Métodos de seguridad de red inalámbrica
**Inalámbrica los métodos de seguridad de red** es una agrupación de la autenticación inalámbrica informal \(a veces se denomina seguridad inalámbrica\) y el cifrado de seguridad inalámbrica. Cifrado y la autenticación inalámbrica se usan en pares para impedir que los usuarios no autorizados accedan a la red inalámbrica y para proteger las transmisiones inalámbricas. 

Al configurar la configuración de seguridad inalámbrica en las directivas de grupo Directiva de red inalámbrica, hay varias combinaciones para elegir. Sin embargo, solo el WPA2\-Enterprise, WPA\-Enterprise y abierto con 802.1X estándares de autenticación son compatibles con 802.1X autenticado de las implementaciones inalámbricas.

>[!NOTE]
>Durante la configuración de directivas de red inalámbrica, debe seleccionar **WPA2\-Enterprise**, **WPA\-Enterprise**, o **abierto con 802.1X** en a fin de obtener acceso a la configuración de EAP que se requieren para 802.1X autentica las implementaciones inalámbricas.  

#### <a name="wireless-authentication"></a>Autenticación inalámbrica
Esta guía recomienda que el uso de los siguientes estándares de autenticación inalámbrica 802.1X autentica las implementaciones inalámbricas.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)**  WPA es un estándar provisional desarrollado por Wi-Fi Alliance para cumplir con el protocolo de seguridad inalámbrica 802.11. El protocolo de WPA se ha desarrollado en respuesta a un número de errores graves que se detectaron en el anterior privacidad equivalente por cable \(WEP\) protocolo.

WPA\-Enterprise proporciona seguridad mejorada a través de WEP por:  

1. Requerir la autenticación que usa el marco de trabajo X EAP 802.1X como parte de la infraestructura que garantiza la autenticación mutua centralizada y administración de clave dinámica  

2. Mejorar el valor de comprobación de integridad \(ICV\) con una comprobación de integridad del mensaje \(MIC\), para proteger el encabezado y la carga  

3. Implementación de un contador de marco para impedir que los ataques de reproducción  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)**  como the WPA\-estándar de Enterprise, WPA2\-empresa utiliza la 802.1X y un marco de EAP. WPA2\-Enterprise proporciona mayor protección de datos para varios usuarios y grandes redes administradas. WPA2\-Enterprise es un protocolo eficaz diseñado para impedir el acceso de red no autorizados mediante la comprobación de los usuarios de red a través de un servidor de autenticación.  

#### <a name="wireless-security-encryption"></a>Cifrado de seguridad inalámbrica
Cifrado de seguridad inalámbrica se usa para proteger las transmisiones inalámbricas que se envían entre el cliente inalámbrico y el AP inalámbrico. Cifrado de seguridad inalámbrica se usa junto con el método de autenticación de seguridad de red seleccionada. De forma predeterminada, los equipos que ejecutan Windows 10, Windows 8.1 y Windows 8 admiten dos estándares de cifrado:

1. **Temporal Key Integrity Protocol** \(TKIP\) es un protocolo de cifrado anterior que originalmente se diseñó para proporcionar cifrado inalámbrico más seguro que la proporcionada por la privacidad equivalente por cable poco \(WEP\) protocolo. TKIP fue diseñado por IEEE 802. 11i tareas grupo y el Wi\-Fi Alliance para reemplazar WEP sin necesidad de realizar la sustitución de hardware heredado. TKIP es un conjunto de algoritmos que encapsula la carga WEP y permite a los usuarios del equipo heredado de Wi-Fi actualizar a TKIP sin reemplazar el hardware. Al igual que WEP, TKIP usa el algoritmo de cifrado RC4 stream como su base. El nuevo protocolo, sin embargo, cifra cada paquete de datos con una clave de cifrado única, y las claves son mucho más seguras que las de WEP. Aunque es útil para la actualización de seguridad en dispositivos más antiguos que se han diseñado para usar solo WEP TKIP, no resuelve todos los problemas de seguridad con conexión a redes LAN inalámbricas y en la mayoría de los casos no es lo suficientemente estable para proteger government confidencial o datos corporativos transmisiones.  

2. **Estándar de cifrado avanzado** \(AES\) es el protocolo de cifrado preferido para el cifrado de datos comerciales y del gobierno. AES ofrece un mayor nivel de seguridad de la transmisión inalámbrica que TKIP o WEP. A diferencia de TKIP y WEP, AES requiere hardware inalámbrico que admite el estándar AES. AES es simétrica\-clave estándar de cifrado que utiliza tres cifrados de bloques, AES\-128, AES\-192 y AES\-256.

En Windows Server 2016, el siguiente AES\-métodos de cifrado inalámbrico basado en están disponibles para la configuración en las propiedades de perfil inalámbrico cuando se selecciona un método de autenticación de WPA2\-Enterprise, que se recomienda.

1. **AES\-CCMP**. El modo Cipher bloque encadenamiento mensaje código protocolo de autenticación de contador \(CCMP\) implementa el estándar 802. 11i está diseñado para el cifrado de seguridad mayor que la proporcionada por WEP y usa claves de cifrado de AES de 128 bits.
2. **AES\-GCMP**. Protocolo de modo contador Galois \(GCMP\) es compatible con 802. 11ac, es más eficaz que AES\-CCMP y proporciona un mejor rendimiento para los clientes inalámbricos. GCMP usa claves de cifrado de AES de 256 bits.

> [!IMPORTANT]
> Cableado Equivalency Privacy \(WEP\) era el estándar de seguridad inalámbrica original que se usó para cifrar el tráfico de red. No debería implementar WEP en la red porque hay bien\-vulnerabilidades conocidas de este formulario obsoleta de seguridad.

### <a name="active-directorydoman-services-adds"></a>Servicios de dominio de Active Directory \(AD DS\)
AD DS proporciona una base de datos distribuida que almacena y administra información acerca de los recursos de red y aplicación\-datos específicos de directorio\-aplicaciones habilitadas. Los administradores pueden usar AD DS para organizar los elementos de una red (por ejemplo, los usuarios, los equipos y otros dispositivos) en una estructura de contención jerárquica. La estructura de contención jerárquica incluye el bosque de Active Directory, dominios del bosque y unidades organizativas \(unidades organizativas\) en cada dominio. Un servidor que ejecuta AD DS se denomina un *controlador de dominio*.  

AD DS contiene las cuentas de usuario, las cuentas de equipo y las propiedades de cuenta que son necesarios por IEEE 802.1X y PEAP\-MS\-CHAP v2 para autenticar las credenciales de usuario y para evaluar la autorización para las conexiones inalámbricas.

### <a name="active-directory-users-and-computers"></a>Usuarios y equipos de Active Directory
Active Directory a los usuarios y equipos es un componente de AD DS que contiene las cuentas que representan entidades físicas, como un equipo, una persona o un grupo de seguridad. Un *grupo de seguridad* es una colección de cuentas de usuario o equipo que los administradores pueden administrar como una sola unidad. Cuentas de usuario y equipo que pertenecen a un grupo determinado se conocen como *miembros del grupo*.  

### <a name="group-policy-management"></a>Administración de directivas de grupo
Administración de directivas de grupo permite directory\-en función de administración de cambios y la configuración de la configuración del equipo y usuario, incluida la información de usuario y de seguridad. Usar Directiva de grupo para definir configuraciones de grupos de usuarios y equipos. Con la directiva de grupo, puede especificar valores para las entradas del registro, seguridad, instalación de software, las secuencias de comandos, redirección de carpetas, servicios de instalación remota y mantenimiento de Internet Explorer. La configuración de directiva de grupo que cree se encuentran en un objeto de directiva de grupo \(GPO\). Al asociar un GPO con contenedores de sistema seleccionados de Active Directory, sitios, dominios y unidades organizativas, puede aplicar la configuración del GPO a los usuarios y equipos de estos contenedores de Active Directory. Para administrar objetos de directiva de grupo en toda la empresa, puede usar el Editor de administración de directivas de grupo de Microsoft Management Console \(MMC\).  

Esta guía proporciona instrucciones detalladas sobre cómo especificar la configuración de la red inalámbrica \(IEEE 802.11\) extensión de directivas de administración de directivas de grupo. La red inalámbrica \(IEEE 802.11\) las directivas de configuración de dominio\-acceso inalámbrico autentican de equipos cliente inalámbricos de miembros con la configuración inalámbrica 802.1X y la conectividad necesaria.

### <a name="server-certificates"></a>Certificados de servidor
Este escenario de implementación requiere certificados de servidor para cada NPS que realiza la autenticación 802.1X.  

Un certificado de servidor es un documento digital que se usa normalmente para la autenticación y para proteger la información en redes abiertas. Un certificado enlaza de manera segura una clave pública a la entidad que contiene la clave privada correspondiente. Los certificados están firmados digitalmente por la CA emisora y que puedan emitirse para un usuario, un equipo o un servicio.  

Una entidad de certificación \(CA\) es una entidad responsable de establecer y garantizar la autenticidad de las claves públicas que pertenecen a los sujetos \(normalmente los usuarios o equipos\) o a otras CA. Las actividades de una entidad de certificación pueden incluir el enlace de claves públicas a nombres completos mediante certificados autofirmados, números de serie del certificado de administración y la revocación de certificados.  

Servicios de certificados de Active Directory \(AD CS\) es un rol de servidor que emite certificados como una CA de red. Un AD CS de certificados de infraestructura, también conocido como un *infraestructura de clave pública \(PKI\)*, ofrece servicios personalizables para emitir y administrar certificados para la empresa.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>PEAP, PEAP y EAP\-MS\-CHAP v2
Protocolo de autenticación extensible \(EAP\) extiende punto\-a\-Protocolo punto \(PPP\) permitiendo métodos de autenticación adicionales que usan la información de credenciales y intercambios de longitudes arbitrarias. Con la autenticación de EAP, tanto la red acceder a cliente y el autenticador \(como NPS\) debe admitir el mismo tipo EAP para la autenticación correcta para que se produzca. Windows Server 2016 incluye una infraestructura EAP, admite dos tipos de EAP y la capacidad de pasar mensajes EAP a NPSs. Mediante el uso de EAP, puede admitir otros esquemas de autenticación, conocidos como *tipos de EAP*. Los tipos EAP que son compatibles con Windows Server 2016 son:  

- Seguridad de la capa de transporte \(TLS\)

- Protocolo de autenticación de Microsoft desafío mutuo versión 2 \(MS\-CHAP v2\)

>[!IMPORTANT]
>Tipos de EAP seguros \(como las que se basan en certificados\) ofrecen una mayor seguridad contra fuerza bruta\-forzar ataques, los ataques de diccionario y ataques de contraseña de averiguación de contraseñas\-basado protocolos de autenticación \(como CHAP o MS\-CHAP versión 1\).

EAP protegido \(PEAP\) usa TLS para crear un canal cifrado entre un cliente de autenticación PEAP, como un equipo inalámbrico y un autenticador PEAP, como un NPS u otros servidores RADIUS. PEAP no especifica un método de autenticación, pero proporciona seguridad adicional para otros protocolos de autenticación EAP \(, como EAP\-MS\-CHAP v2\) que pueden operar a través del canal cifrado TLS proporcionado por PEAP. PEAP se utiliza como un método de autenticación para los clientes de acceso que se conectan a la red de su organización a través de los siguientes tipos de servidores de acceso de red \(NAS\):

- 802.1X\-puntos de acceso inalámbricos

- 802.1X\-capaz de conmutadores de autenticación

- Los equipos que ejecutan Windows Server 2016 y el servicio de acceso remoto \(RAS\) que están configurados como red privada virtual \(VPN\) servidores, servidores de DirectAccess o ambos

- Equipos que ejecutan Windows Server 2016 y servicios de escritorio remoto

PEAP\-MS\-CHAP v2 es más fácil de implementar que EAP\-TLS ya que se realiza la autenticación de usuario con contraseña\-credenciales basadas en \(nombre de usuario y contraseña\), en lugar de los certificados o tarjetas inteligentes. Solo NPS u otros servidores RADIUS deben tener un certificado. El certificado de NPS se usa el NPS durante el proceso de autenticación para demostrar su identidad a los clientes PEAP.

Esta guía proporciona instrucciones para configurar los clientes inalámbricos y NPS\(s\) usar PEAP\-MS\-CHAP v2 para 802.1X acceso autenticado.

### <a name="network-policy-server"></a>Servidor de directivas de redes
Servidor de directivas de red \(NPS\) , podrá configurar y administrar las directivas de red mediante el uso de acceso telefónico de autenticación remota centralmente\-en el servicio de usuario \(RADIUS\) servidor y proxy RADIUS. NPS es necesario al implementar acceso inalámbrico 802.1X.

Al configurar los puntos de acceso inalámbrico 802.1X como clientes RADIUS en NPS, NPS procesa las solicitudes de conexión enviadas por los puntos de acceso. Durante el procesamiento de solicitud de conexión, NPS realiza la autenticación y autorización. La autenticación determina si el cliente ha presentado credenciales válidas. Si NPS autentica correctamente al cliente solicitante, NPS determina si el cliente está autorizado para realizar la conexión solicitada, y permite o rechaza la conexión. Esto se explica con más detalle como sigue:

#### <a name="authentication"></a>Autenticación

PEAP mutua correcta\-MS\-autenticación CHAP v2 tiene dos partes principales:

1.  El cliente autentica el NPS.  Durante esta fase de la autenticación mutua, NPS envía su certificado de servidor en el equipo cliente para que el cliente pueda comprobar la identidad de NPS con el certificado. Para autenticar correctamente el NPS, el equipo cliente debe confiar en la entidad de certificación que emitió el certificado NPS. El cliente confía en esta entidad de certificación al certificado de la entidad de certificación está presente en el almacén de certificados entidades emisoras raíz de confianza en el equipo cliente.

    Si implementa su propia entidad de certificación privada, el certificado de CA se instala automáticamente en el almacén de certificados entidades emisoras raíz de confianza para el usuario actual y para el equipo Local cuando se actualiza la directiva de grupo en el equipo cliente miembro del dominio. Si decide implementar certificados de servidor de una CA pública, asegúrese de que el certificado de entidad de certificación pública ya está en el almacén de certificados entidades emisoras raíz de confianza.  

2.  NPS autentica al usuario. Cuando el cliente autentica correctamente el NPS, el cliente envía la contraseña del usuario\-credenciales al NPS, que comprueba las credenciales del usuario en la base de datos de cuentas de usuario en servicios de dominio de Active Directory basadas en \(AD DS\).

Si las credenciales son válidas y la autenticación se realiza correctamente, el NPS comienza la fase de autorización del procesamiento de la solicitud de conexión. Si las credenciales no son válidas y se produce un error de autenticación, NPS envía un mensaje de rechazo de acceso y se deniega la solicitud de conexión.  

#### <a name="authorization"></a>Autorización

El servidor que ejecuta NPS realiza la autorización como sigue:  

1. NPS comprueba si hay restricciones en el marcado de cuenta de usuario o equipo\-en las propiedades en AD DS. Cada cuenta de usuario y equipo en equipos y usuarios de Active Directory incluye varias propiedades, los que se encuentran en incluidos el **marcado\-en** ficha. En esta pestaña, en **permiso de acceso de red**, si el valor es **permitir el acceso**, el usuario o el equipo está autorizado para conectarse a la red. Si el valor es **denegar el acceso**, el usuario o equipo no está autorizado para conectarse a la red. Si el valor es **controlar el acceso a través de la directiva de red NPS**, NPS evalúa las directivas de red configuradas para determinar si el usuario o el equipo está autorizado para conectarse a la red. 

2. A continuación, NPS procesa sus directivas de red para buscar una directiva que coincida con la solicitud de conexión. Si se encuentra una directiva de coincidencia, NPS concede o deniega la conexión según la configuración de la directiva.  

Si la autenticación y autorización se realizan correctamente, y si la directiva de red coincidente concede acceso, NPS concede acceso a la red y el usuario y el equipo pueden conectarse a recursos de red para los que tienen permisos.  

>[!NOTE]
>Para implementar acceso inalámbrico, debe configurar las directivas de NPS. Esta guía proporciona instrucciones para usar el **Asistente de configuración 802.1X de X** en NPS para crear directivas de NPS para acceso inalámbrico autenticado mediante 802.1X.  

### <a name="bootstrap-profiles"></a>Perfiles de bootstrap
En 802.1X\-autenticado a redes inalámbricas, los clientes inalámbricos deben proporcionar las credenciales de seguridad que se autentican mediante un servidor RADIUS con el fin de conectarse a la red. Para EAP protegido \[PEAP\]\-protocolo de autenticación de Microsoft desafío mutuo versión 2 \[MS\-CHAP v2\], las credenciales de seguridad son un nombre de usuario y la contraseña. Para EAP\-Transport Layer Security \[TLS\] o PEAP\-TLS, las credenciales de seguridad son los certificados, como certificados de usuarios y equipos de cliente o tarjetas inteligentes.

Cuando se conecta a una red que está configurada para llevar a cabo PEAP\-MS\-CHAP v2, PEAP\-TLS o EAP\-autenticación TLS, de forma predeterminada, los clientes inalámbricos también deben validar un certificado de equipo que sea de Windows enviado por el servidor RADIUS. El certificado de equipo que se envía el servidor RADIUS para cada sesión de autenticación se conoce normalmente como un certificado de servidor.

Como se mencionó anteriormente, puede emitir los servidores RADIUS su certificado de servidor en uno de dos maneras: desde una CA comercial \(como VeriSign, Inc.,\), o desde una entidad de certificación privada que puede implementar en la red. Si el servidor RADIUS envía un certificado de equipo que fue emitido por una CA comercial que ya tiene un certificado raíz instalado en el almacén de certificados entidades emisoras raíz de confianza del cliente, el cliente inalámbrico puede validar el servidor RADIUS certificado de equipo, independientemente de si el cliente inalámbrico una al dominio de Active Directory. En este caso el cliente inalámbrico puede conectarse a la red inalámbrica y, a continuación, puede unir el equipo al dominio.

>[!NOTE]
>Se puede deshabilitar el comportamiento de exigir al cliente validar el certificado de servidor, pero no se recomienda deshabilitar la validación de certificado de servidor en entornos de producción.

Perfiles inalámbricos de arranque son perfiles temporales que se configuran de tal forma que permiten a los usuarios del cliente inalámbrico para conectarse a la 802.1X\-autenticado a redes inalámbricas antes de que el equipo está unido al dominio, y\/o antes de el usuario ha iniciado sesión correctamente en el dominio mediante el uso de un determinado equipo inalámbrico por primera vez.  En esta sección se resume qué problema se encuentra al intentar unirse a un equipo inalámbrico al dominio, o para un usuario para que use un dominio\-equipo unido a un inalámbrica por primera vez iniciar sesión en el dominio.

Para las implementaciones en el que el usuario o administrador de TI no se puede conectar físicamente un equipo a la red Ethernet cableada para unir el equipo al dominio y el equipo no tiene la emisora es necesario instalado en el certificado de CA de raíz su  **Entidades de certificación raíz de confianza** almacén de certificados, puede configurar los clientes inalámbricos con un perfil de conexión inalámbrica temporal, denominado un *arrancar perfil*para conectarse a la red inalámbrica.

Un *arrancar perfil* elimina el requisito para validar el certificado de equipo del servidor RADIUS. Esta configuración temporal permite al usuario unir el equipo al dominio, momento en el que la conexión inalámbrica de red inalámbrico \(IEEE 802.11\) las directivas se aplican y se instala automáticamente el certificado de CA de raíz adecuado en el equipo.

Cuando se aplica la directiva de grupo, se aplican uno o varios perfiles de conexión inalámbrica que exigen el requisito para la autenticación mutua en el equipo; el perfil de bootstrap ya no es necesario y se quita. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede usar una conexión inalámbrica para iniciar sesión en el dominio.

Para obtener información general del proceso de implementación de acceso inalámbrico con estas tecnologías, consulte [información general de implementación de acceso inalámbrico](b-wireless-access-deploy-overview.md).
