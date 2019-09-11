---
title: Implementación del acceso inalámbrico autenticado mediante 802.1X basado en contraseña
description: Este tema forma parte de la guía de redes de Windows Server 2016 "implementación de acceso inalámbrico autenticado mediante 802.1 X basado en contraseña".
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ee20e002aa8dad29eefcda87a7b949ffb0bb6e9b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868941"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Implementación del\-acceso inalámbrico autenticado mediante 802.1 x basado en contraseña

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta es una guía complementaria de la guía de&reg; red principal de Windows Server 2016. La guía de red principal proporciona instrucciones para planear e implementar los componentes necesarios para una red totalmente operativa y un nuevo Active Directory® dominio en un bosque nuevo.

En esta guía se explica cómo basarse en una red principal proporcionando instrucciones sobre cómo implementar los ingenieros de Institute of Electrical \(and\) Electronics Engineers IEEE 802.1 x\-authenticatedd IEEE 802,11 Wireless Access Using Protocolo de autenticación extensible protegido: Protocolo de autenticación por desafío mutuo de Microsoft versión \(2\-PEAP\-MS CHAP\)V2.

Dado que\-PEAP\-MS CHAP V2 requiere que los usuarios\-proporcionen credenciales basadas en contraseña en lugar de un certificado durante el proceso de autenticación, normalmente es más fácil y menos costoso implementar que EAP\-.TLS o\-TLS TLS.

>[!NOTE]
>En esta guía, el acceso inalámbrico autenticado mediante IEEE 802.1 x\-con\-PEAP MS CHAP V2 se abrevia como "acceso inalámbrico" y "acceso Wi-Fi".

## <a name="about-this-guide"></a>Acerca de esta guía
En esta guía, junto con las guías de requisitos previos que se describen a continuación, se proporcionan instrucciones sobre cómo implementar la siguiente infraestructura de acceso WiFi.  

- Uno o más\- \(puntosdeacceso inalámbrico 802,11 con capacidad de 802.1 x.\)

- Active Directory Domain Services \(ADDS\) usuarios y equipos.

- Administración de directivas de grupo.

- Uno o más servidores NPS \(\) del servidor de directivas de redes.

- Certificados de servidor para equipos con NPS.

- Equipos cliente inalámbricos que&reg; ejecutan windows 10, Windows 8.1 o Windows 8.

### <a name="dependencies-for-this-guide"></a>Dependencias de esta guía

Para implementar correctamente la conexión inalámbrica autenticada con esta guía, debe tener un entorno de red y de dominio con todas las tecnologías necesarias implementadas. También debe tener certificados de servidor implementados en el NPSs de autenticación.

En las secciones siguientes se proporcionan vínculos a documentación en la que se muestra cómo implementar estas tecnologías.

#### <a name="network-and-domain-environment-dependencies"></a>Dependencias de entorno de dominio y red

Esta guía está diseñada para los administradores de red y de sistema que han seguido las instrucciones de la **Guía de red principal** de Windows Server 2016 para implementar una red principal, o para aquellos que han implementado previamente las tecnologías básicas incluidas en el núcleo. red, incluidos AD DS \(, DNS\)de sistema de nombres de dominio, protocolo \(de\)configuración dinámica\/de host DHCP, IP TCP, NPS y \(servicio de nombres Internet de Windows WINS\).  

La guía de red principal está disponible en las siguientes ubicaciones:

- La [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) de windows Server 2016 está disponible en la biblioteca técnica de windows Server 2016. 

- La [Guía de red principal](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) también está disponible en formato de Word en la Galería [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)de TechNet, en.

#### <a name="server-certificate-dependencies"></a>Dependencias de certificados de servidor
Hay dos opciones disponibles para inscribir servidores de autenticación con certificados de servidor para su uso con la autenticación 802.1 x: implemente su propia infraestructura de clave pública mediante Active Directory \(servicios de\) Certificate Server AD CS o usar certificados de servidor inscritos por una CA \(\)de entidad de certificación pública.

##### <a name="ad-cs"></a>AD CS
Los administradores de redes y sistemas que implementan la red inalámbrica autenticada deben seguir las instrucciones de la guía complementaria de red principal de Windows Server 2016, **implementar certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 x**. En esta guía se explica cómo implementar y usar AD CS para inscribir automáticamente certificados de servidor en equipos que ejecutan NPS.

Esta guía está disponible en la siguiente ubicación.

- La guía complementaria de red principal de Windows Server 2016 [implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) en formato HTML en la biblioteca técnica de. 

##### <a name="public-ca"></a>CA pública
Puede adquirir certificados de servidor de una entidad de certificación pública, como VeriSign, en la que los equipos cliente ya confían. 

Un equipo cliente confía en una CA cuando el certificado de CA está instalado en el almacén de certificados de entidades de certificación raíz de confianza. De forma predeterminada, los equipos que ejecutan Windows tienen varios certificados de CA pública instalados en el almacén de certificados de entidades de certificación raíz de confianza.  

Se recomienda que revises las guías de diseño e implementación de todas las tecnologías que se usan en este escenario de implementación. Estas guías pueden ayudarte a determinar si este escenario de implementación ofrece los servicios y la configuración que necesitas para la red de tu organización.

### <a name="requirements"></a>Requisitos
A continuación se indican los requisitos para implementar una infraestructura de acceso inalámbrico mediante el escenario documentado en esta guía:

- Antes de implementar este escenario, primero debe adquirir puntos de acceso\-inalámbricos compatibles con 802.1 x para proporcionar cobertura inalámbrica en las ubicaciones deseadas en su sitio. La sección de planeación de esta guía ayuda a determinar las características que el APs debe admitir.

- Active Directory Domain Services \(ADDS\) está instalado, al igual que las otras tecnologías de red necesarias, según las instrucciones de la guía de red principal de Windows Server 2016.  

- AD CS está implementado y los certificados de servidor se inscriben en NPSs. Estos certificados son necesarios al implementar el método de\-autenticación\-basado en certificado\-PEAP MS CHAP v2 que se usa en esta guía.

- Un miembro de su organización está familiarizado con los estándares IEEE 802,11 que son compatibles con los AP inalámbricos y los adaptadores de red inalámbrica instalados en los equipos cliente y dispositivos de la red. Por ejemplo, alguien de su organización está familiarizado con los tipos de frecuencia de radio \(, la autenticación\)inalámbrica 802,11 WPA2 o \(WPA, y\)cifra AES o TKIP.

## <a name="what-this-guide-does-not-provide"></a>Qué no incluye esta guía  
Estos son algunos de los elementos que esta guía no proporciona:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Instrucciones completas para seleccionar puntos de\-acceso inalámbricos compatibles con 802.1 x

Dado que existen muchas diferencias entre las marcas y los modelos\-de AP inalámbricos compatibles con 802.1 x, esta guía no proporciona información detallada sobre:  

- Determinar la marca o el modelo de AP inalámbrico que mejor se adapte a sus necesidades.

- La implementación física de AP inalámbricos en la red.

- Configuración avanzada de AP inalámbrico, como para redes \(de área local virtuales de red\)VLAN.

- Instrucciones sobre cómo configurar atributos específicos del proveedor\-de AP inalámbrico en NPS.

Además, la terminología y los nombres de la configuración varían entre las marcas y los modelos de AP inalámbrico, y puede que no coincidan con los nombres de configuración genéricos que se usan en esta guía. Para obtener detalles sobre la configuración de AP inalámbrico, debe revisar la documentación del producto proporcionada por el fabricante de los AP inalámbricos.

### <a name="instructions-for-deploying-nps-certificates"></a>Instrucciones para implementar certificados NPS
  
Hay dos alternativas para implementar certificados NPS. En esta guía no se proporcionan instrucciones completas para ayudarle a determinar qué alternativa se adaptará mejor a sus necesidades. Sin embargo, en general, las opciones a las que se enfrente son:

- Adquirir certificados de una entidad de certificación pública, como VeriSign, que ya son de confianza\-para los clientes basados en Windows. Esta opción se recomienda normalmente para redes más pequeñas.

- Implementar una PKI \(\) de infraestructura de clave pública en la red mediante AD CS. Esto se recomienda para la mayoría de las redes y las instrucciones sobre cómo implementar certificados de servidor con AD CS están disponibles en la guía de implementación mencionada anteriormente.

### <a name="nps-network-policies-and-other-nps-settings"></a>Directivas de red NPS y otra configuración de NPS

A excepción de los valores de configuración que se realizaron al ejecutar el Asistente para **configurar 802.1 x** , como se describe en esta guía, esta guía no proporciona información detallada para configurar manualmente las condiciones de NPS, las restricciones u otra configuración de NPS.

### <a name="dhcp"></a>DHCP

Esta guía de implementación no proporciona información acerca del diseño o la implementación de subredes DHCP para LAN inalámbricas.

## <a name="technology-overviews"></a>Introducción a las tecnologías
A continuación se muestran información general sobre la tecnología para implementar el acceso inalámbrico:

### <a name="ieee-8021x"></a>IEEE 802.1X
El estándar IEEE 802.1 x define el control\-de acceso a la red basado en puerto que se usa para proporcionar acceso de red autenticado a las redes Ethernet. Este control\-de acceso a la red basado en Puerto usa las características físicas de la infraestructura de LAN conmutada para autenticar los dispositivos conectados a un puerto LAN. Se puede denegar el acceso al puerto si se produce un error en el proceso de autenticación. Aunque este estándar se diseñó para redes Ethernet cableadas, se ha adaptado para usarlo en LAN inalámbricas 802,11.

### <a name="8021x-capable-wireless-access-points-aps"></a>AP de\-puntos \(de acceso inalámbricos compatibles con 802.1 x\)
Este escenario requiere la implementación de uno o más puntos de\-acceso inalámbricos compatibles con 802.1 x que son compatibles con\-el protocolo RADIUS \(\) del servicio de autenticación remota telefónica de usuario.

los AP compatibles con\-802.1 x y RADIUS, cuando se implementan en una infraestructura de RADIUS con un servidor RADIUS, como un NPS, se denominan *clientes RADIUS*.

### <a name="wireless-clients"></a>Clientes inalámbricos
En esta guía se proporcionan detalles de configuración completos para proporcionar acceso autenticado mediante\-802.1 x a los usuarios miembros del dominio que se conectan a la red con equipos cliente inalámbricos que ejecutan Windows 10, Windows 8.1 y Windows 8. Los equipos deben estar Unidos al dominio para poder establecer correctamente el acceso autenticado.

>[!NOTE]
>También puede usar equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como clientes inalámbricos.

### <a name="support-for-ieee-80211-standards"></a>Compatibilidad con los estándares IEEE 802,11
Los sistemas operativos Windows y Windows Server admitidos\-proporcionan compatibilidad integrada con redes inalámbricas 802,11. En estos sistemas operativos, un adaptador de red inalámbrica instalado 802,11 aparece como una conexión de red inalámbrica en el centro de redes y recursos compartidos. 

Aunque existe compatibilidad integrada\-con redes inalámbricas 802,11, los componentes inalámbricos de Windows dependen de lo siguiente:

- Las capacidades del adaptador de red inalámbrica. El adaptador de red inalámbrica instalado debe ser compatible con los estándares de seguridad inalámbrica o LAN inalámbrica que necesite. Por ejemplo, si el adaptador de red inalámbrica no admite el\-acceso \(protegido Wi-\)fi WPA, no se pueden habilitar ni configurar las opciones de seguridad de WPA.

- Las capacidades del controlador del adaptador de red inalámbrica. Para que pueda configurar las opciones de red inalámbrica, el controlador del adaptador de red inalámbrica debe admitir la generación de informes de todas sus capacidades en Windows. Compruebe que el controlador del adaptador de red inalámbrica está escrito para las capacidades del sistema operativo. Asegúrese también de que el controlador sea la versión más reciente comprobando Microsoft Update o el sitio web del proveedor del adaptador de red inalámbrica.

En la tabla siguiente se muestran las velocidades y frecuencias de transmisión para los estándares inalámbricos IEEE 802,11 comunes.

|Estándares|Altas|Velocidades de transmisión de bits|Uso|
|-------------|---------------|--------------------------|---------|  
|802,11|\(\) Intervalo\- defrecuenciadeISMindustrial,científicoymédicodelabandade2,4a2,5GHz\(\)|2 megabits por segundo \(Mbps\)|Obsoleto. No se usa habitualmente.|  
|norma|ISM\-de banda S|11 Mbps|Se suele usar.|  
|802.11 a|Banda\-de C \(de ISM 5,725 a 5,875 GHz\)|54 Mbps|No se usa normalmente debido a gastos y a un intervalo limitado.|  
|802.11|ISM\-de banda S|54 Mbps|Ampliamente utilizado. los dispositivos 802.11 g son compatibles con los dispositivos 802.11 b.|  
|802.11 n \ 2,4 y 5,0 GHz|Banda\-de C y\-de banda de S|250 Mbps|Los dispositivos basados en el\-estándar de la versión preliminar IEEE 802.11 n están disponibles en agosto de 2007. Muchos dispositivos 802.11 n son compatibles con los dispositivos 802.11 a, b y g.|
|802.11ac |5 GHz |6,93 Gbps |802.11 AC, aprobado por el IEEE en 2014, es más escalable y más rápido que 802.11 n, y se implementa donde APs y los clientes inalámbricos lo admiten.|

### <a name="wireless-network-security-methods"></a>Métodos de seguridad de red inalámbrica
**Los métodos de seguridad de red inalámbrica** son una agrupación informal \(de la autenticación inalámbrica a veces\) denominada seguridad inalámbrica y cifrado de seguridad inalámbrica. La autenticación inalámbrica y el cifrado se usan en pares para impedir que usuarios no autorizados accedan a la red inalámbrica y para proteger las transmisiones inalámbricas. 

Al configurar las opciones de seguridad inalámbrica en las directivas de red inalámbrica de directiva de grupo, hay varias combinaciones entre las que elegir. Sin embargo, solo se\-admiten los\-estándares de autenticación WPA2 Enterprise, WPA Enterprise y Open with 802.1 x para las implementaciones inalámbricas autenticadas de 802.1 x.

>[!NOTE]
>Al configurar directivas de red inalámbrica, debe seleccionar **WPA2\-Enterprise**, **WPA\-Enterprise**o **abrir con 802.1 x** para obtener acceso a la configuración de EAP que se requiere para 802.1 x Authenticated. implementaciones inalámbricas.  

#### <a name="wireless-authentication"></a>Autenticación inalámbrica
En esta guía se recomienda el uso de los siguientes estándares de autenticación inalámbrica para las implementaciones inalámbricas autenticadas mediante 802.1 X.  

**Acceso\-protegido Wi-Fi: \(Enterprise\-WPA\) Enterprise** WPA es un estándar provisional desarrollado por la Alianza WiFi para cumplir con el protocolo de seguridad inalámbrica 802,11. El protocolo WPA se desarrolló en respuesta a una serie de errores graves detectados en el protocolo WEP \(\) de privacidad equivalente por cable.

WPA\-Enterprise proporciona seguridad mejorada a través de WEP:  

1. Requerir la autenticación que usa el marco EAP 802.1 X como parte de la infraestructura que asegura la autenticación mutua centralizada y la administración de claves dinámica  

2. \(Mejorar el valor de comprobación de\) integridad ICV con\)un mensaje \(de comprobación de integridad de mensajes para proteger el encabezado y la carga  

3. Implementar un contador de fotogramas para evitar ataques de reproducción  

**Acceso\-protegido Wi\) \(-fi 2: Enterprise\-WPA2 Enterprise** como WPA\-Enterprise Standard, WPA2\-Enterprise usa el marco 802.1 x y EAP. WPA2\-Enterprise proporciona una protección de datos más segura para varios usuarios y redes administradas de gran tamaño. WPA2\-Enterprise es un protocolo sólido diseñado para evitar el acceso no autorizado a la red mediante la comprobación de usuarios de red a través de un servidor de autenticación.  

#### <a name="wireless-security-encryption"></a>Cifrado de seguridad inalámbrica
El cifrado de seguridad inalámbrica se usa para proteger las transmisiones inalámbricas que se envían entre el cliente inalámbrico y el punto de conexión inalámbrico. El cifrado de seguridad inalámbrica se usa junto con el método de autenticación de seguridad de red seleccionado. De forma predeterminada, los equipos que ejecutan Windows 10, Windows 8.1 y Windows 8 admiten dos estándares de cifrado:

1. **Protocolo de integridad de clave temporal** TKIP es un protocolo de cifrado anterior que se diseñó originalmente para proporcionar un cifrado inalámbrico más seguro que el proporcionado por el protocolo WEP\) de \(privacidad equivalente por cable inherentemente débil.\) \( TKIP fue diseñado por el grupo de tareas IEEE 802.11 i y la\-Alianza Wi-Fi para reemplazar la WEP sin necesidad de reemplazar el hardware heredado. TKIP es un conjunto de algoritmos que encapsula la carga de WEP y permite a los usuarios del equipo WiFi heredado actualizar a TKIP sin reemplazar el hardware. Al igual que WEP, TKIP usa el algoritmo de cifrado de secuencia RC4 como base. El nuevo protocolo, sin embargo, cifra cada paquete de datos con una clave de cifrado única y las claves son mucho más seguras que las de WEP. Aunque TKIP es útil para actualizar la seguridad en dispositivos antiguos que se diseñaron para usar solo WEP, no soluciona todos los problemas de seguridad que se enfrentan a las LAN inalámbricas y, en la mayoría de los casos, no es lo suficientemente sólido como para proteger los datos corporativos o gubernamentales confidenciales. las transmisiones.  

2. **Estándar de cifrado avanzado** \(AES\) es el protocolo de cifrado preferido para el cifrado de datos comerciales y de administración pública. AES ofrece un mayor nivel de seguridad de transmisión inalámbrica que TKIP o WEP. A diferencia de TKIP y WEP, AES requiere hardware inalámbrico que admita el estándar AES. AES es un estándar\-de cifrado de claves simétricas que usa tres cifrados de bloques\-, AES\-128\-, AES 192 y AES 256.

En Windows Server 2016, los siguientes métodos\-de cifrado inalámbrico basados en AES están disponibles para la configuración en las propiedades de perfil inalámbrico cuando se\-selecciona un método de autenticación de WPA2 Enterprise, que se recomienda.

1. **AES\-CCMP**. El modo de contador de encadenamiento \(de\) bloques de cifrado código de autenticación de mensajes (Mac) protocolo CCMP implementa el estándar 802.11 i y está diseñado para un mayor cifrado de seguridad que el proporcionado por WEP y usa claves de cifrado AES de 128 bits.
2. **GCMP\-AES**. El protocolo \(de modo de\) contador Galois GCMP es compatible con 802.11 AC, es más\-eficaz que AES CCMP y proporciona un mejor rendimiento para los clientes inalámbricos. GCMP usa claves de cifrado AES de 256 bits.

> [!IMPORTANT]
> Wired Equivalent Privacy \(WEP\) era el estándar de seguridad inalámbrica original que se usaba para cifrar el tráfico de red. No debe implementar WEP en la red porque\-existen vulnerabilidades conocidas en esta forma de seguridad obsoleta.

### <a name="active-directorydoman-services-adds"></a>Active Directory dominio Services \(AD DS\)
AD DS proporciona una base de datos distribuida que almacena y administra información acerca de\-los recursos de red\-y los datos específicos de la aplicación en las aplicaciones habilitadas para el directorio. Los administradores pueden usar AD DS para organizar los elementos de una red (por ejemplo, los usuarios, los equipos y otros dispositivos) en una estructura de contención jerárquica. La estructura de contención jerárquica incluye el Active Directory bosque, los dominios de las unidades organizativas del \(bosque\) y las unidades organizativas de cada dominio. Un servidor que ejecuta AD DS se denomina controlador de *dominio*.  

AD DS contiene las cuentas de usuario, las cuentas de equipo y las propiedades de cuenta que requieren IEEE 802.1 x\-y\-PEAP MS CHAP v2 para autenticar las credenciales de usuario y evaluar la autorización para las conexiones inalámbricas.

### <a name="active-directory-users-and-computers"></a>Usuarios y equipos de Active Directory
Active Directory usuarios y equipos es un componente de AD DS que contiene cuentas que representan entidades físicas, como un equipo, una persona o un grupo de seguridad. Un *grupo de seguridad* es una colección de cuentas de usuario o de equipo que los administradores pueden administrar como una sola unidad. Las cuentas de usuario y de equipo que pertenecen a un grupo determinado se denominan *miembros del grupo*.  

### <a name="group-policy-management"></a>Administración de directivas de grupo
La administración de directiva de grupo\-permite la administración de cambios y configuraciones basadas en directorios de configuración de usuarios y equipos, incluida la información de seguridad y de usuario. Utilice directiva de grupo para definir las configuraciones de grupos de usuarios y equipos. Con directiva de grupo, puede especificar la configuración de las entradas del registro, la seguridad, la instalación de software, los scripts, el redireccionamiento de carpetas, los servicios de instalación remota y el mantenimiento de Internet Explorer. La configuración de directiva de grupo que cree está contenida en un GPO \(\)de directiva de grupo objeto. Al asociar un GPO con los contenedores del sistema Active Directory seleccionados (sitios, dominios y unidades organizativas), puede aplicar la configuración del GPO a los usuarios y equipos de esos contenedores de Active Directory. Para administrar Directiva de grupo objetos en una empresa, puede usar el editor de administración de directivas de grupo MMC \(\)de Microsoft Management Console.  

En esta guía se proporcionan instrucciones detalladas sobre cómo especificar la configuración de \(la extensión\) de directivas IEEE 802,11 de red inalámbrica de administración de directiva de grupo. Las directivas de \(red inalámbrica\) IEEE 802,11 configuran los equipos cliente inalámbricos de miembros de dominio\-con la conectividad y la configuración inalámbrica necesarias para el acceso inalámbrico autenticado mediante 802.1 x.

### <a name="server-certificates"></a>Certificados de servidor
Este escenario de implementación requiere certificados de servidor para cada NPS que realiza la autenticación 802.1 X.  

Un certificado de servidor es un documento digital que se usa normalmente para la autenticación y para proteger la información en redes abiertas. Un certificado enlaza de manera segura una clave pública a la entidad que contiene la clave privada correspondiente. Los certificados están firmados digitalmente por la CA emisora y se pueden emitir para un usuario, un equipo o un servicio.  

Una entidad de \(certificación\) es una entidad responsable de establecer y garantizar la autenticidad de las claves públicas que pertenecen a asuntos \(normalmente usuarios o equipos\) u otras CA. Las actividades de una entidad de certificación pueden incluir el enlace de claves públicas a nombres distintivos a través de certificados firmados, la administración de números de serie de certificados y la revocación de certificados.  

Active Directory servicios \(de Certificate\) Server AD CS es un rol de servidor que emite certificados como una CA de red. Una infraestructura de certificados de AD CS, también conocida como *PKI \(\)de infraestructura de clave pública*, proporciona servicios personalizables para la emisión y administración de certificados para la empresa.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP, PEAP y PEAP\-MS\-CHAP V2
El \(protocolo de autenticación extensible EAP\) extiende el\-protocolo\- \(punto a punto\) PPP al permitir métodos de autenticación adicionales que usan credenciales e información intercambios de longitudes arbitrarias. Con la autenticación EAP, tanto el cliente de acceso a redes como \(el autenticador,\) como el NPS, deben admitir el mismo tipo de EAP para que se produzca la autenticación correcta. Windows Server 2016 incluye una infraestructura EAP, admite dos tipos de EAP y la capacidad de pasar mensajes EAP a NPSs. Mediante el uso de EAP, puede admitir esquemas de autenticación adicionales, conocidos como *tipos de EAP*. Los tipos de EAP que son compatibles con Windows Server 2016 son los siguientes:  

- TLS de seguridad \(de la capa de transporte\)

- Protocolo de autenticación por desafío mutuo de Microsoft \(versión\-2 MS CHAP V2\)

>[!IMPORTANT]
>Los tipos \(de EAP seguros, como los basados en certificados\) , ofrecen\-una mayor seguridad frente a ataques por fuerza bruta, ataques de diccionario y ataques de adivinación de contraseñas que en función de la contraseña\- protocolos \(de autenticación como CHAP o MS\-CHAP versión 1\).

EAP \(protegido PEAP\) usa TLS para crear un canal cifrado entre un cliente PEAP de autenticación, como un equipo inalámbrico, y un autenticador PEAP, como un NPS u otros servidores RADIUS. PEAP no especifica ningún método de autenticación, pero proporciona seguridad \(adicional para otros protocolos de autenticación EAP, como EAP\-MS\-CHAP V2\) , que puede funcionar a través del canal cifrado TLS. proporcionado por PEAP. PEAP se usa como método de autenticación para acceder a los clientes que se conectan a la red de la organización a través de los \(siguientes\)tipos de servidores de acceso a la red NAS:

- puntos de\-acceso inalámbricos compatibles con 802.1 x

- conmutadores\-de autenticación con capacidad de 802.1 x

- Equipos que ejecutan Windows Server 2016 y el servicio \(de\) acceso remoto ras que están configurados\) como servidores VPN de red \(privada virtual, servidores de DirectAccess o ambos

- Equipos que ejecutan Windows Server 2016 y Servicios de Escritorio remoto

PEAP\-MS\-\- \(\)CHAP V2 es más fácil de implementar que EAP TLS, ya que la autenticación de usuario se realiza mediante el nombre de usuario y la contraseña de credenciales basadas en contraseña, en lugar de\- certificados o tarjetas inteligentes. Solo se requiere NPS u otros servidores RADIUS para tener un certificado. El NPS usa el certificado NPS durante el proceso de autenticación para demostrar su identidad a los clientes PEAP.

En esta guía se proporcionan instrucciones para configurar los clientes inalámbricos\(y\) los NPS s\-para\-que usen el acceso autenticado PEAP MS CHAP v2 para 802.1 x.

### <a name="network-policy-server"></a>Servidor de directivas de redes
\(El NPS\- \(del servidor de directivas de redes permite configurar y administrar de forma centralizada las directivas de red mediante el uso\) del servidor RADIUS del servicio de usuario y el proxy RADIUS del servicio de autenticación remota.\) NPS es necesario cuando se implementa el acceso inalámbrico 802.1 X.

Cuando se configuran los puntos de acceso inalámbricos de 802.1 X como clientes RADIUS en NPS, NPS procesa las solicitudes de conexión enviadas por el APs. Durante el procesamiento de solicitudes de conexión, NPS realiza la autenticación y la autorización. La autenticación determina si el cliente ha presentado credenciales válidas. Si NPS autentica correctamente el cliente que realiza la solicitud, NPS determina si el cliente está autorizado para realizar la conexión solicitada y permite o deniega la conexión. Esto se explica con más detalle de la manera siguiente:

#### <a name="authentication"></a>Autenticación

La autenticación de\-MS\-CHAP v2 de PEAP mutua correcta tiene dos partes principales:

1.  El cliente autentica el NPS.  Durante esta fase de la autenticación mutua, el NPS envía su certificado de servidor al equipo cliente para que el cliente pueda comprobar la identidad del NPS con el certificado. Para autenticar correctamente el NPS, el equipo cliente debe confiar en la CA que emitió el certificado NPS. El cliente confía en esta entidad de certificación cuando el certificado de la CA está presente en el almacén de certificados de entidades de certificación raíz de confianza en el equipo cliente.

    Si implementa su propia entidad de certificación privada, el certificado de CA se instala automáticamente en el almacén de certificados de entidades de certificación raíz de confianza para el usuario actual y para el equipo local cuando directiva de grupo se actualiza en el equipo cliente miembro del dominio. Si decide implementar certificados de servidor desde una entidad de certificación pública, asegúrese de que el certificado de CA público ya está en el almacén de certificados de entidades de certificación raíz de confianza.  

2.  NPS autentica al usuario. Después de que el cliente autentique correctamente el NPS, el cliente envía las credenciales\-basadas en contraseña del usuario al NPS, que comprueba las credenciales del usuario en la base de datos de cuentas \(de usuario en Active Directory dominio Services ad DS\).

Si las credenciales son válidas y la autenticación se realiza correctamente, el NPS comienza la fase de autorización de procesamiento de la solicitud de conexión. Si las credenciales no son válidas y se produce un error en la autenticación, NPS envía un mensaje de rechazo de acceso y se deniega la solicitud de conexión.  

#### <a name="authorization"></a>Autorización

El servidor que ejecuta NPS realiza la autorización de la siguiente manera:  

1. NPS comprueba las restricciones en las propiedades de acceso telefónico\-de la cuenta de usuario o equipo en AD DS. Cada cuenta de usuario y equipo de Active Directory usuarios y equipos incluye varias propiedades, incluidas las que se encuentran en la pestaña **marcado\-** . En esta pestaña, en **permiso de acceso a la red**, si el valor es **permitir el acceso**, el usuario o el equipo está autorizado para conectarse a la red. Si el valor es **denegar el acceso**, el usuario o el equipo no está autorizado para conectarse a la red. Si el valor es **controlar el acceso a través de la Directiva de red NPS**, NPS evalúa las directivas de red configuradas para determinar si el usuario o el equipo tiene autorización para conectarse a la red. 

2. Después, NPS procesa sus directivas de red para encontrar una directiva que coincida con la solicitud de conexión. Si se encuentra una directiva de coincidencia, NPS concede o deniega la conexión en función de la configuración de la Directiva.  

Si la autenticación y la autorización se realizan correctamente y la Directiva de red coincidente concede acceso, NPS concede acceso a la red y el usuario y el equipo pueden conectarse a los recursos de red para los que tienen permisos.  

>[!NOTE]
>Para implementar el acceso inalámbrico, debe configurar las directivas de NPS. En esta guía se proporcionan instrucciones para usar el **Asistente para configurar 802.1 x** en NPS con el fin de crear directivas de NPS para el acceso inalámbrico autenticado mediante 802.1 x.  

### <a name="bootstrap-profiles"></a>Perfiles de arranque
En las redes\-inalámbricas autenticadas mediante 802.1 x, los clientes inalámbricos deben proporcionar credenciales de seguridad autenticadas por un servidor RADIUS para conectarse a la red. En el caso \[del\]protocolo de autenticación por desafío mutuo de EAP \[PEAP\-\-de Microsoft\], versión 2 MS CHAP V2, las credenciales de seguridad son un nombre de usuario y una contraseña. Para la\-seguridad \[de la capa\] de transporte\-EAP TLS o PEAP TLS, las credenciales de seguridad son certificados, como certificados de usuario y de equipo cliente o tarjetas inteligentes.

Al conectarse a una red que está configurada para\-realizar\-la autenticación PEAP MS\-CHAP V2, PEAP\-TLS o EAP TLS, de forma predeterminada, los clientes inalámbricos de Windows también deben validar un certificado de equipo que sea Enviado por el servidor RADIUS. El certificado de equipo enviado por el servidor RADIUS para cada sesión de autenticación se conoce comúnmente como certificado de servidor.

Como se mencionó anteriormente, puede emitir sus servidores RADIUS en su certificado de servidor de una de las dos maneras siguientes: \(desde una entidad de certificación comercial como\)VeriSign, Inc., o desde una CA privada que implemente en la red. Si el servidor RADIUS envía un certificado de equipo emitido por una CA comercial que ya tiene un certificado raíz instalado en el almacén de certificados de entidades de certificación raíz de confianza del cliente, el cliente inalámbrico puede validar el servidor RADIUS certificado de equipo, independientemente de si el cliente inalámbrico se ha unido al dominio de Active Directory. En este caso, el cliente inalámbrico puede conectarse a la red inalámbrica y, a continuación, puede unir el equipo al dominio.

>[!NOTE]
>El comportamiento que requiere el cliente para validar el certificado de servidor se puede deshabilitar, pero no se recomienda deshabilitar la validación de certificados de servidor en entornos de producción.

Los perfiles de arranque inalámbrico son perfiles temporales que se configuran de tal forma que permiten a los usuarios de clientes inalámbricos\-conectarse a la red inalámbrica autenticada mediante 802.1 x antes de que el\/equipo se una al dominio, y o antes el usuario ha iniciado sesión correctamente en el dominio mediante un equipo inalámbrico determinado por primera vez.  En esta sección se resume el problema que se produce al intentar unir un equipo inalámbrico al dominio o para que un usuario Use un equipo\-inalámbrico unido a un dominio por primera vez para iniciar sesión en el dominio.

En el caso de las implementaciones en las que el usuario o el administrador de ti no puede conectar físicamente un equipo a la red Ethernet cableada para unir el equipo al dominio, y el equipo no tiene instalado el certificado de CA raíz necesario en su **raíz de confianza** Almacén de certificados de entidades de certificación, puede configurar clientes inalámbricos con un perfil de conexión inalámbrica temporal, denominado *Perfil de bootstrap*, para conectarse a la red inalámbrica.

Un *Perfil de arranque* quita el requisito de validar el certificado de equipo del servidor RADIUS. Esta configuración temporal permite al usuario inalámbrico unir el equipo al dominio, momento en el que se aplican las \(directivas IEEE\) 802,11 de la red inalámbrica y se instala automáticamente el certificado de CA raíz adecuado en el computadora.

Cuando se aplica directiva de grupo, se aplican en el equipo uno o más perfiles de conexión inalámbrica que exigen el requisito de autenticación mutua. el perfil de arranque ya no es necesario y se ha quitado. Después de unir el equipo al dominio y reiniciar el equipo, el usuario puede usar una conexión inalámbrica para iniciar sesión en el dominio.

Para obtener información general sobre el proceso de implementación del acceso inalámbrico con estas tecnologías, consulte [información general sobre la implementación de acceso inalámbrico](b-wireless-access-deploy-overview.md).
