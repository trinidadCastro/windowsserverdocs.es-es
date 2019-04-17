---
title: Implementar basada en contraseña autenticado acceso inalámbrico 802.1X
description: Este tema es parte de la Guía de redes de Windows Server 2016 "Implementar basada en contraseña 802.1X X autenticados Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ded8a273a9ad464b44fa7245db58d0fd05f06a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Implementar Password\ 802.1X autenticado acceso inalámbrico

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Esta es una guía de complemento a Windows Server&reg; 2016 guía básica de red. La guía básica de red se proporcionan instrucciones para planear e implementar los componentes necesarios para una red totalmente funcional y un nuevo dominio Active Directory® en un bosque nuevo.

Esta guía explica cómo se estructuran una red principal al proporcionar instrucciones sobre cómo implementar Instituto de electricidad y electrónica ingenieros \(IEEE\) 802.1X\-autenticado con protegido Protocolo de autenticación Extensible versión 2 de protocolo de autenticación por desafío mutuo de Microsoft de acceso inalámbrico IEEE 802.11 \ (v2\ PEAP\-MS\-CHAP).

Dado que PEAP\-MS\-CHAPv2 requiere que los usuarios que proporcionen las credenciales basadas en password\ en lugar de un certificado durante el proceso de autenticación, es suele ser más fácil y económica de implementar que EAP\ TLS o PEAP\ TLS.

>[!NOTE]
>En esta guía, se abrevia IEEE 802.1X autenticados inalámbrica acceso con PEAP\-MS\-CHAPv2 "acceso inalámbrico" y "Acceso Wi-Fi".

## <a name="about-this-guide"></a>Acerca de esta guía
Esta guía, en combinación con las guías de requisito previo que se describe a continuación, proporciona instrucciones sobre cómo implementar la infraestructura de acceso Wi-Fi siguiente.  

- Uno o más 802.1X\-capaz 802.11 acceso inalámbrico puntos \(APs\).

- Servicios de dominio de Active Directory \(AD DS\) usuarios y equipos.

- Administración de directivas de grupo.

- Uno o varios servidores \(NPS\) el servidor de directivas de red.

- Certificados de servidor en equipos que ejecutan NPS.

- Inalámbrica equipos cliente que ejecutan Windows&reg; 10, Windows 8.1 o Windows 8.

### <a name="dependencies-for-this-guide"></a>Dependencias de esta guía

Para implementar correctamente autenticado inalámbrica con esta guía, se debe tener un entorno de red y el dominio con todas las tecnologías necesarias implementar. También debes tener implementados en los servidores NPS autenticación de certificados de servidor.

Las siguientes secciones proporcionan vínculos a documentación que se muestra cómo implementar estas tecnologías.

#### <a name="network-and-domain-environment-dependencies"></a>Dependencias de entorno de red y de dominio

Esta guía está diseñada para los administradores de red y del sistema que han seguido las instrucciones en el equipo con Windows Server 2016 **guía básica de red** para implementar una red principal, o para aquellos que han implementado previamente las tecnologías principales incluidas en la red principal, incluidos los AD DS, el sistema de nombre de dominio \(DNS\), \(DHCP\) protocolo de configuración dinámica de Host, TCP\ TCP/IP, NPS y servicio de nombres de Windows Internet \(WINS\).  

Guía básica de la red está disponible en las siguientes ubicaciones:

- El equipo con Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponible en la biblioteca técnica de Windows Server 2016. 

- La [guía básica de red](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) también está disponible en formato de Word en la Galería de TechNet, en [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dependencias de certificado de servidor
Hay dos opciones disponibles para la inscripción de autenticación de servidores, con certificados de servidor para su uso con la autenticación 802.1X - implementar tu propia infraestructura de clave pública mediante los servicios de certificados de Active Directory \(AD CS\) o usar certificados de servidor inscritos por una entidad de certificación públicas \(CA\).

##### <a name="ad-cs"></a>AD CS
Los administradores de red y del sistema que implementan inalámbrica autenticado deben seguir las instrucciones en el Windows Server 2016 guía básica de red complementaria, **implementar certificados de servidor para implementaciones inalámbricas y cableadas 802.1X**. Esta guía explica cómo implementar y usar los AD CS para inscribir automáticamente los certificados de servidor a equipos que ejecutan NPS.

Esta guía está disponible en la siguiente ubicación.

- La Guía de Windows Server 2016 Core red complementaria [implementar certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) en formato HTML en la biblioteca técnica. 

##### <a name="public-ca"></a>CA pública
Puedes comprar certificados de servidor de una CA pública, como VeriSign, que ya confíen los equipos cliente. 

Un equipo cliente confía en una entidad de certificación cuando se instala el certificado de CA en el almacén de certificados de entidades de certificación raíz de confianza. De manera predeterminada, equipos que ejecutan Windows tienen varios certificados de CA públicos instalados en su certificado de entidades de certificación raíz de confianza de la tienda.  

Se recomienda que revise a las guías de diseño e implementación para cada una de las tecnologías que se usan en este escenario de implementación. Estas guías pueden ayudarte a determinar si este escenario de implementación proporciona los servicios y la configuración que necesitas para la red de su organización.

### <a name="requirements"></a>Requisitos
A continuación se indican los requisitos para implementar una infraestructura de acceso inalámbrico mediante el escenario documentado en esta guía:

- Antes de implementar este escenario, debe adquirir 802.1X\-puntos de acceso inalámbrico compatible con proporcionar cobertura inalámbrica en las ubicaciones deseadas en el sitio. La sección de planeamiento de esta guía ayuda a determinar las características deben ser compatibles con los puntos de acceso.

- Active \(AD DS\) los servicios de dominio de directorio está instalado, como son las otras tecnologías de red necesarios, según las instrucciones de la Guía de red de Windows Server 2016 Core.  

- AD CS se implementa y certificados de servidor están inscritos en los servidores NPS. Estos certificados se requieren al implementar el método de autenticación basada en certificate\ de PEAP\-MS\-CHAP v2 que se usa en esta guía.

- Un miembro de la organización está familiarizado con los estándares de IEEE 802.11 que son compatibles con los puntos de acceso inalámbricos y los adaptadores de red que están instalados en los equipos cliente y los dispositivos de la red. Por ejemplo, alguien de la organización está familiarizado con los tipos de radiofrecuencia, autenticación inalámbrica 802.11 \ (WPA2 o WPA\) y cifrados \(AES or TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Lo que no proporciona esta guía  
Los siguientes son algunos elementos que no proporciona esta guía:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Una completa orientación para seleccionar 802.1X\-puntos de acceso inalámbrico compatible con

Porque existen muchas diferencias entre marcas y modelos de 802.1X\-compatibles con puntos de acceso inalámbricos, esta guía proporciona información detallada sobre:  

- Determinar qué marca o modelo de punto de acceso inalámbrico es la mejor se adapte a tus necesidades.

- La implementación física de los puntos de acceso inalámbricos en la red.

- Advanced configuración de punto de acceso inalámbrica, como para inalámbrica \(VLANs\) redes de área Local virtual.

- Instrucciones sobre cómo configurar los atributos de vendor\ específicos de punto de acceso inalámbricos en NPS.

Además, nombres para la configuración y la terminología varían entre los modelos y marcas de punto de acceso inalámbricas y podrían no coincidir con los nombres de configuración genérico que se usan en esta guía. Para los detalles de la configuración de punto de acceso inalámbricos, debe revisar la documentación del producto proporcionada por el fabricante de los puntos de acceso inalámbricos.

### <a name="instructions-for-deploying-nps-server-certificates"></a>Instrucciones para implementar certificados de servidor NPS
  
Hay dos alternativas para la implementación de certificados de servidor NPS. Esta guía no proporciona una completa orientación para ayudarte a determinar qué alternativa mejor satisfaga sus necesidades. En general, sin embargo, las opciones que se enfrenta son:

- Compra de certificados de una CA pública, como VeriSign, que ya son de confianza para los clientes basados en Windows\. Normalmente se recomienda esta opción para redes más pequeñas.

- Implementar un \(PKI\) infraestructura de clave pública de la red mediante AD CS. Este es el recomendado para la mayoría de las redes y las instrucciones para implementar certificados de servidor con AD CS están disponibles en la Guía de implementación mencionados anteriormente.

### <a name="nps-network-policies-and-other-nps-settings"></a>Las directivas de red NPS y otras opciones de configuración de NPS

Excepto para las opciones de configuración que se crean cuando ejecutas la **configurar 802.1X** asistente, tal como se documenta en esta guía, esta guía proporciona información detallada para configurar manualmente NPS condiciones, las restricciones u otras opciones de configuración de NPS.

### <a name="dhcp"></a>DHCP

Esta guía de implementación no proporciona información sobre cómo diseñar o implementar subredes DHCP para LAN inalámbricas.

## <a name="technology-overviews"></a>Información general de la tecnología
A continuación se muestran información general de la tecnología para la implementación de acceso inalámbrico:

### <a name="ieee-8021x"></a>IEEE 802.1X X
El estándar IEEE 802.1x define el control de acceso de red basada en port\ que se usa para proporcionar acceso a la red autenticados a las redes de Ethernet. Este control de acceso de red basada en port\ usa las características de la infraestructura de LAN conmutada para autenticar los dispositivos conectados a un puerto LAN. Puede ser deniega el acceso al puerto si se produce un error en el proceso de autenticación. Aunque este estándar se diseñó para redes Ethernet por cable, se ha adaptado para su uso en 802.11 inalámbricas.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-\(APs\) de puntos de acceso inalámbrico compatible con
Este escenario requiere la implementación de uno o más 802.1X\-compatibles con puntos de acceso inalámbricos que son compatibles con el protocolo remoto servicio de autenticación de usuario Dial\-In \(RADIUS\).

802.1X y puntos de acceso compatible con RADIUS\, cuando se implementa en una infraestructura de RADIUS con un servidor RADIUS, como un servidor NPS, se denominan *clientes RADIUS*.

### <a name="wireless-clients"></a>Clientes inalámbricos
Esta guía proporciona detalles de la configuración completa para proporcionar autenticación 802.1X para los usuarios domain\ miembro que se conectan a la red con equipos cliente que ejecutan Windows 10, Windows 8.1 y Windows 8. Equipos deben estar unidos al dominio para establecer correctamente el acceso autenticado.

>[!NOTE]
>También puedes usar equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como los clientes inalámbricos.

### <a name="support-for-ieee-80211-standards"></a>Compatibilidad con IEEE 802.11 estándares
Sistemas operativos de Windows y Windows Server compatibles permiten built\ en 802.11 redes inalámbricas. En estos sistemas operativos, un adaptador de red inalámbrica 802.11 instalado aparece como una conexión de red inalámbrica en el centro de redes y recursos compartidos. 

Aunque hay built\ la compatibilidad con redes inalámbricas 802.11, los componentes de Windows inalámbricos dependen de las siguientes acciones:

- Las funcionalidades del adaptador de red inalámbrica. El adaptador de red inalámbrica instalado debe admitir la LAN inalámbrica o los estándares de seguridad inalámbrica que necesita. Por ejemplo, si el adaptador de red inalámbrica no es compatible con \(WPA\) acceso protegido Wi\-Fi, no se puede habilitar o configurar las opciones de seguridad WPA.

- Las funcionalidades del controlador del adaptador de red inalámbrica. Para que te permite configurar las opciones de red inalámbrica, el controlador del adaptador de red inalámbrica debe admitir los informes de todos sus funcionalidades de Windows. Comprueba que el controlador para el adaptador de red inalámbrica se ha escrito para las capacidades del sistema operativo. Asegúrate de que el controlador es la versión más reciente Comprobando Microsoft Update o el sitio Web del fabricante del adaptador de red inalámbrica.

La siguiente tabla muestra la velocidad de transmisión y la frecuencia de los estándares inalámbricos IEEE 802.11 comunes.

|Estándares|Frecuencias|Transmisión velocidades de bits|Uso de|
|-------------|---------------|--------------------------|---------|  
|802.11|Banda S\ Industrial, científica y medicina rango de frecuencia \(ISM\) \ (2.4a 2.5 GHz\)|2 megabits por segundo \(Mbps\)|Obsoleto. No se utiliza frecuentemente.|  
|802. 11b|ISM S\ Band|11 Mbps|Generalmente se usa.|  
|802. 11a|Banda C\ ISM \ (5,725a 5.875 GHz\)|54 Mbps|No se utiliza frecuentemente debido a los gastos e intervalo limitado.|  
|802.11g|ISM S\ Band|54 Mbps|Se usa. 802.11g dispositivos son compatibles con 802. 11b dispositivos.|  
|802. 11n \2.4 y GHz 5.0|ISM C\ y S\ banda|250 Mbps|Está disponibles en agosto de 2007 para dispositivos basados en pre\ ratificación IEEE 802. 11n estándar. Muchos 802. 11n dispositivos son compatibles con 802. 11a, b y dispositivos g.|
|802.11ac |5 GHz |6,93 GB/s |802.11ac, aprobado por el IEEE en 2014, es más escalable y más rápido que 802. 11n y se implementa en puntos de acceso y los clientes inalámbricos lo admiten.|

### <a name="wireless-network-security-methods"></a>Métodos de seguridad de red inalámbrica
**Inalámbrica métodos de seguridad de red** es una agrupación informal de autenticación inalámbrica \ (a veces denominados security\ inalámbrica) y el cifrado de seguridad inalámbrica. El cifrado y la autenticación inalámbrica se usan en pares para evitar que los usuarios no autorizados accedan a la red inalámbrica y para proteger las transmisiones inalámbricas. 

Al configurar la configuración de seguridad inalámbrica en las directivas de grupo Directiva de red inalámbrica, hay varias combinaciones de elegir. Sin embargo, solo los WPA2\ Enterprise, WPA\ Enterprise y abrir con estándares de autenticación de 802.1X son compatibles con implementaciones inalámbricas 802.1X X autenticado.

>[!NOTE]
>Durante la configuración de directivas de red inalámbrica, debes seleccionar **WPA2\ Enterprise**, **WPA\ Enterprise**, o **802.1X** para acceder a la configuración de EAP que son necesarios para 802.1X autenticados implementaciones inalámbricas.  

#### <a name="wireless-authentication"></a>Autenticación inalámbrica
Esta guía, se recomienda que el uso de los siguientes estándares de autenticación inalámbrica para 802.1X autenticado implementaciones inalámbricas.  

**Wi\-Fi protegidas acceso: Enterprise \(WPA\-Enterprise\)** WPA es un estándar provisional desarrollado por Wi-Fi Alliance para cumplir con el protocolo de seguridad inalámbrico 802.11. El protocolo de WPA se desarrolló en respuesta a un número de errores graves que se detectaron en el protocolo de privacidad equivalente por cable \(WEP\) anterior.

WPA\ Enterprise ofrece seguridad mejorada sobre WEP por:  

1. Requerir autenticación que usa el marco X EAP 802.1X como parte de la infraestructura que garantiza la autenticación mutua centralizada y administración dinámica de claves  

2. Lo que mejora la \(ICV\) valor de comprobar la integridad con un mensaje de comprobación de integridad \(MIC\), para proteger el encabezado y la carga  

3. Implementar un contador de fotogramas para impedir que los ataques de reproducción  

**Wi\-Fi protegidas acceso 2: Enterprise \(WPA2\-Enterprise\)** como el WPA\-estándar de la empresa, empresa WPA2\ usa el 802.1X y el marco EAP. WPA2\ Enterprise proporciona mayor protección de datos para varios usuarios y grandes redes administradas. WPA2\ empresa es un protocolo eficaz que está diseñado para impedir el acceso de red no autorizado verificando a los usuarios de red a través de un servidor de autenticación.  

#### <a name="wireless-security-encryption"></a>Cifrado de seguridad inalámbrica
Cifrado de seguridad inalámbrica se usa para proteger las transmisiones inalámbricas que se envían entre el cliente inalámbrico y el punto de acceso inalámbrico. Cifrado de seguridad inalámbrica se usa junto con el método de autenticación de seguridad de red seleccionado. De manera predeterminada, los equipos que ejecutan Windows 10, Windows 8.1 y Windows 8 admiten estándares de cifrado de dos:

1. **Protocolo de integridad de claves temporal** \(TKIP\) es un protocolo más antiguo de cifrado que se diseñó originalmente para proporcionar el cifrado inalámbrico más seguro que lo proporcionó el protocolo de privacidad equivalente por cable \(WEP\) poco. Se ha diseñado TKIP por el IEEE 802. 11i grupo y la Wi\-Fi Alliance para reemplazar WEP sin necesidad de la sustitución del hardware heredado de tareas. TKIP es un conjunto de algoritmos que encapsula la carga WEP y permite a los usuarios de equipos antiguos de Wi-Fi actualizar a TKIP sin reemplazar el hardware. Al igual que WEP, TKIP usa el algoritmo de cifrado de secuencias RC4 como base. El protocolo nuevo, sin embargo, cifra cada paquete de datos con una clave de cifrado único y las claves son mucho más seguras que las por WEP. Aunque TKIP es útil para la actualización de seguridad en dispositivos más antiguos que fueron diseñados para usar solo WEP, no se corrige todos los problemas de seguridad apunta LAN inalámbricas y en la mayoría de los casos no es lo suficientemente sólido como para proteger gobierno con información confidencial o la transmisión de datos corporativos.  

2. **Estándar de cifrado avanzado** \(AES\) es el protocolo de cifrado preferido para el cifrado de datos comerciales y del gobierno. AES ofrece un mayor nivel de seguridad de transmisión inalámbricos que TKIP o WEP. A diferencia de TKIP y WEP, AES requiere hardware inalámbrico que admite el estándar AES. AES es un estándar de cifrado de clave de symmetric\ que usa tres cifrados de bloques, AES\-128, AES\ 192 y AES\-256.

En Windows Server 2016, los siguientes métodos de cifrado AES\ inalámbrico están disponibles para la configuración de propiedades del perfil inalámbrico cuando se selecciona un método de autenticación de empresa WPA2\, lo que se recomienda.

1. **AES\ CCMP**. Modo cifrado bloque encadenamiento mensaje código protocolo de autenticación \(CCMP\) implementa el estándar 802. 11i de contador y está diseñado para el cifrado de seguridad más alto que el proporcionado por WEP y usa las claves de cifrado AES de 128 bits.
2. **AES\ GCMP**. Protocolo de modo de contador Galois \(GCMP\) es compatible con 802.11ac, es más eficaz que AES\ CCMP y proporciona un mejor rendimiento para los clientes inalámbricos. GCMP usa claves de cifrado AES de 256 bits.

> [!IMPORTANT]
> Privacidad de equivalencia por cable \(WEP\) era el original inalámbrica estándar de seguridad que se usó para cifrar el tráfico de red. No debes implementar WEP en la red porque hay well\ conocida vulnerabilidades en este formulario obsoleto de seguridad.

### <a name="active-directory-doman-services-ad-ds"></a>\(AD DS\) de servicios de dominio de Active Directory
AD DS proporciona una base de datos distribuida que almacena y administra la información sobre recursos de red y datos específicos de application\ desde aplicaciones habilitadas directory\. Los administradores pueden usar AD DS para organizar los elementos de una red, como los usuarios, equipos y otros dispositivos, en una estructura jerárquica de contención. La estructura de contención jerárquica incluye el bosque de Active Directory, dominios del bosque y unidades organizativas \(OUs\) en cada dominio. Se llama a un servidor que ejecuta AD DS un *controlador de dominio*.  

AD DS contiene las cuentas de usuario, cuentas de equipo y propiedades de la cuenta que son necesarias para IEEE 802.1X y PEAP\-MS\-CHAP v2 para autenticar las credenciales de usuario y para evaluar la autorización para las conexiones inalámbricas.

### <a name="active-directory-users-and-computers"></a>Equipos y usuarios de active Directory
Directorio de usuarios y equipos de Active es un componente de AD DS que contiene las cuentas que representan entidades físicas, como un equipo, una persona o un grupo de seguridad. Un *grupo de seguridad* es una colección de cuentas de usuario o equipo que los administradores pueden administrar como una sola unidad. Cuentas de usuario y del equipo que pertenecen a un grupo en particular se conocen como *agrupar miembros*.  

### <a name="group-policy-management"></a>Administración de directivas de grupo
Administración de directivas de grupo permite la administración de configuración y los cambios basados en directory\ de configuración de usuario y del equipo, incluida la información de seguridad y del usuario. Usar la directiva de grupo para definir configuraciones de grupos de usuarios y equipos. Con la directiva de grupo, puedes especificar valores para las entradas del registro, seguridad, instalación de software, scripts, redireccionamiento de carpetas, servicios de instalación remota y mantenimiento de Internet Explorer. La configuración que crees se encuentra en una directiva de grupo de directiva de grupo objeto \(GPO\). Mediante la asociación de un GPO con contenedores de sistema de Active Directory seleccionados: sitios, dominios y unidades organizativas, puedes aplicar la configuración del GPO a los usuarios y equipos en los contenedores de Active Directory. Para administrar objetos de directiva de grupo en toda la empresa, puedes usar la \(MMC\) el Editor de administración de directivas de grupo de Microsoft Management Console.  

Esta guía proporciona instrucciones detalladas sobre cómo especificar opciones de configuración en la red inalámbrica \ (IEEE 802.11\) extensión de directivas de administración de directivas de grupo. La red inalámbrica \ (IEEE 802.11\) directivas configurar equipos cliente inalámbrico domain\ con la conectividad necesaria y configuración inalámbrica para 802.1X autenticados acceso inalámbrico.

### <a name="server-certificates"></a>Certificados de servidor
Este escenario de implementación requiere certificados de servidor para cada servidor NPS que realiza la autenticación 802.1X.  

Un certificado de servidor es un documento digital que se usa normalmente para la autenticación y para proteger la información en las redes abiertas. Un certificado enlaza una clave pública de forma segura a la entidad que contiene la clave privada correspondiente. Certificados están firmados digitalmente por la CA emisora, y que pueden ser emitidos para un usuario, un equipo o un servicio.  

Una entidad de certificación \(CA\) es una entidad responsable de establecer y garantizar la autenticidad de las claves públicas pertenecientes a sujetos \ (por lo general, los usuarios o DOS\) o de otras entidades emisoras de certificados. Las actividades de una entidad de certificación pueden incluir enlazar claves públicas a nombres completos mediante certificados firmados, números de serie del certificado de administración y revocación de certificados.  

Active \(AD CS\) servicios de certificados Directory es un rol de servidor que emite certificados como una entidad de certificación de la red. Un AD CS certificados infraestructura, también conocido como un *infraestructura de clave pública \(PKI\)*, proporciona servicios personalizables para emitir y administrar certificados para la empresa.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP, PEAP y PEAP\-MS\-CHAP v2
Extensible \(EAP\) protocolo de autenticación extiende protocolo Point\-to\-punto intercambia \(PPP\) al permitir que otros métodos de autenticación que usen credenciales e información de longitudes arbitrarias. Con la autenticación de EAP, acceder a la red cliente y el autenticador \ (por ejemplo, la server\ NPS) debe ser compatible con el mismo tipo EAP para la autenticación correcta para que se produzca. Windows Server 2016 incluye una infraestructura EAP, admite dos tipos de EAP y la capacidad para pasar mensajes EAP a servidores NPS. Mediante el uso de EAP, puedes admitir esquemas de autenticación adicional, conocidos como *tipos de EAP*. Los tipos EAP que son compatibles con Windows Server 2016 son:  

- Seguridad de la capa de transporte \(TLS\)

- Versión del protocolo de autenticación por desafío mutuo de Microsoft 2 \ (v2\ MS\ CHAP)

>[!IMPORTANT]
>Tipos de EAP seguros \ (como las que se basan en certificates\) ofrecen el mayor seguridad contra los ataques que protocolos de autenticación basada en password\ de adivinación de contraseñas, los ataques de diccionario y ataques de fuerza brute\ \ (por ejemplo, CHAP o MS\ CHAP 1\ de versión).

Protegido \(PEAP\) EAP usa TLS para crear un canal cifrado entre un cliente de autenticación PEAP, como un equipo inalámbrico y un autenticador PEAP, como un servidor NPS u otros servidores RADIUS. PEAP no especifica un método de autenticación, pero proporciona seguridad adicional para otros protocolos de autenticación de EAP \ (por ejemplo, v2\ EAP\-MS\-CHAP) que se pueden utilizar a través del canal cifrado de TLS proporcionado PEAP. PEAP se usa como un método de autenticación para los clientes de acceso que se conectan a la red de tu organización a través de los siguientes tipos de red acceso servidores \(NASs\):

- 802.1X\-puntos de acceso inalámbrico compatible con

- 802.1X\-capaz conmutadores de autenticación

- Servidores \(VPN\), servidores de DirectAccess o ambos de red en equipos que ejecutan Windows Server 2016 y el servicio de acceso remoto de \(RAS\) y que están configurados como privadas virtuales

- Equipos que ejecutan Windows Server 2016 y servicios de escritorio remoto

PEAP\-MS\-CHAPv2 es más fácil de implementar que EAP\ TLS porque la autenticación de usuario se realiza mediante las credenciales basadas en password\ \ (nombre de usuario y password\), en lugar de certificados o tarjetas inteligentes. Solo NPS u otros servidores RADIUS deben tener un certificado. El certificado de servidor NPS se usa el servidor NPS durante el proceso de autenticación para probar su identidad a los clientes PEAP.

Esta guía brinda instrucciones para configurar los clientes inalámbricos y tu server\(s\) NPS para utilizar PEAP\-MS\-CHAPv2 de autenticación 802.1X.

### <a name="network-policy-server"></a>Servidor de directivas de red
Red el servidor de directivas \(NPS\) te permite configurar y administrar directivas de red mediante el servidor remoto servicio de autenticación de usuario Dial\-In \(RADIUS\) y proxy RADIUS de forma centralizada. NPS es necesario cuando se implemente 802.1X de acceso inalámbrico.

Al configurar los puntos de acceso inalámbrico 802.1X como clientes RADIUS en NPS, NPS procesa las solicitudes de conexión enviadas por los puntos de acceso. Durante el proceso de solicitud de conexión, NPS realiza la autenticación y autorización. Autenticación determina si el cliente ha presentado las credenciales válidas. Si NPS autentica correctamente del cliente solicitante, NPS determina si el cliente está autorizado para realizar la conexión solicitada, y permite o deniega la conexión. Esto se explica con más detalle de la siguiente manera:

#### <a name="authentication"></a>Autenticación

Correcta PEAP\-MS\-CHAP v2 la autenticación mutua tiene dos partes principales:

1.  El cliente autentica el servidor NPS.  Durante esta fase de la autenticación mutua, el servidor NPS envía su certificado de servidor en el equipo cliente para que el cliente puede comprobar la identidad del servidor NPS con el certificado. Para autenticar correctamente el servidor NPS, el equipo cliente debe confiar en la CA que emitió el certificado de servidor NPS. El cliente confía en esta CA cuando el certificado de CA está presente en el almacén de certificados de entidades de certificación raíz de confianza en el equipo cliente.

    Si implementas tu propio CA privada, el certificado de CA se instala automáticamente en el almacén de certificados de entidades de certificación raíz de confianza para el usuario actual y para el equipo Local cuando se actualiza la directiva de grupo en el equipo de cliente de miembro de dominio. Si decides implementar los certificados de servidor de una CA pública, asegúrate de que el certificado de CA público ya está en el almacén de certificados de entidades de certificación raíz de confianza.  

2.  El servidor NPS autentica al usuario. Cuando el cliente autentica el servidor NPS correctamente, el cliente envía las credenciales del usuario basados en password\ en el servidor NPS, que comprueba las credenciales del usuario contra la base de datos de cuentas de usuario en servicios de dominio de Active Directory \(AD DS\).

Si las credenciales sean válidas y la autenticación se supera, el servidor NPS empieza la fase de autorización de procesamiento de la solicitud de conexión. Si las credenciales no son válidas y se produce un error de autenticación, NPS envía un mensaje de rechazar el acceso y se deniega la solicitud de conexión.  

#### <a name="authorization"></a>Autorización

El servidor que ejecuta NPS realiza la autorización como sigue:  

1. NPS comprueba si hay restricciones en el equipo o usuario dial\ en Propiedades de la cuenta en AD DS. Cada cuenta de usuario y del equipo en equipos y usuarios de Active Directory incluye varias propiedades, las que se encuentran en incluidas la **Dial\ en** pestaña. En esta pestaña, en **permiso de acceso de red**, si el valor es **permitir el acceso**, el usuario o el equipo está autorizado para conectarse a la red. Si el valor es **denegar el acceso**, el usuario o el equipo no está autorizado para conectarse a la red. Si el valor es **controlar el acceso a través de la directiva de red NPS**, NPS evalúa las directivas de red configurados para determinar si el usuario o el equipo está autorizado para conectarse a la red. 

2. NPS a continuación, procesa sus directivas de red para encontrar una directiva que coincida con la solicitud de conexión. Si se encuentra una directiva coincidente, NPS concede o deniega la conexión basándose en la configuración de la directiva.  

Si la autenticación y autorización son correctos, y si la directiva de red coincidente concede acceso, NPS concede acceso a la red y el usuario y el equipo pueden conectarse a recursos de red para los que tienen permisos.  

>[!NOTE]
>Para implementar acceso inalámbrico, debes configurar las directivas NPS. Esta guía brinda instrucciones para usar la **configurar 802.1X X wizard** en NPS para crear directivas NPS para 802.1X autenticados acceso inalámbrico.  

### <a name="bootstrap-profiles"></a>Perfiles de arranque
En 802.1X\-autenticado redes inalámbricas, los clientes inalámbricos deben proporcionar credenciales de seguridad que se autentican por un servidor de radio para poder conectarse a la red. Para EAP protegido versión del protocolo de autenticación de \[PEAP\]\-Microsoft 2 \[MS\-CHAP v2\], las credenciales de seguridad son un nombre de usuario y contraseña. Para la seguridad de la capa de transporte de EAP\ \[TLS\] o PEAP\-TLS, las credenciales de seguridad son certificados, como certificados de usuario y del equipo cliente o tarjetas inteligentes.

Al conectarse a una red que está configurada para realizar PEAP\-MS\-CHAPv2, PEAP\ TLS o autenticación EAP\ TLS, de manera predeterminada, los clientes inalámbricos de Windows también deben validar un certificado de equipo que se envía por el servidor de radio. El certificado de equipo que se envía por el servidor de radio para cada sesión de autenticación normalmente se conoce como un certificado de servidor.

Como se mencionó anteriormente, puede emitir los servidores RADIUS su certificado de servidor en uno de dos maneras: desde una entidad de certificación comercial \ (como VeriSign, Inc., \), o a una CA privada que se implementa en la red. Si el servidor RADIUS envía un certificado de equipo que fue emitido por una entidad de certificación comercial que ya tiene un certificado raíz instalado en el almacén de certificados de entidades de certificación raíz de confianza del cliente, el cliente inalámbrico puede validar certificado de equipo del servidor RADIUS, independientemente de si el cliente inalámbrico está unido al dominio de Active Directory. En este caso en que el cliente inalámbrico puede conectarse a la red inalámbrica, y, a continuación, el equipo se puede unir al dominio.

>[!NOTE]
>Se puede deshabilitar el comportamiento de solicitar al cliente validar el certificado de servidor, pero no se recomienda deshabilitar validación del certificado de servidor en entornos de producción.

Perfiles inalámbricos de arranque son temporales perfiles que están configurados de manera que los usuarios de cliente inalámbrico conectar con el 802.1X\-red inalámbrica autenticada antes de que el equipo está unido al dominio, y\ / o antes de que el usuario ha iniciado sesión correctamente en el dominio mediante el uso de un equipo determinado inalámbrico por primera vez.  Esta sección resume el problema se encuentra al intentar unirte a un equipo inalámbrico al dominio o un usuario puede usar un equipo unido a domain\ inalámbrico por primera vez para iniciar sesión en el dominio.

Para implementaciones en el que el usuario o administrador de TI físicamente no se puede conectar un equipo a la red Ethernet por cable para unir el equipo al dominio y el equipo no tiene la emisión necesario raíz instalado en el certificado de CA su **entidades de certificación raíz de confianza** almacén de certificados, puede configurar clientes inalámbricos con un perfil temporal conexión inalámbrica, llamado un *arrancar perfil*, para conectarse a la red inalámbrica.

Un *arrancar perfil* elimina la necesidad de validar certificado de equipo del servidor RADIUS. Esta configuración temporal permite al usuario unir el equipo al dominio, en cuyo momento la conexión inalámbrica de red inalámbrico \ (IEEE 802.11\) se aplican directivas y el certificado de CA de raíz correspondiente se instala automáticamente en el equipo.

Cuando se aplica la directiva de grupo, uno o varios perfiles de conexión inalámbrica que aplicar el requisito para la autenticación mutua se aplican en el equipo. el perfil de inicio ya no es necesario y se quita. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede usar una conexión inalámbrica para iniciar sesión en el dominio.

Para obtener información general del proceso de implementación de acceso inalámbrico con estas tecnologías, consulta [introducción de la implementación de acceso inalámbrico](b-wireless-access-deploy-overview.md).
