---
title: Paso 1 planeación de la infraestructura de acceso remoto
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 71a6d38b9c77b3b8c24b28f78114daa63f5bd527
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822538"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Paso 1 planeación de la infraestructura de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
En este tema se describen los pasos para planear una infraestructura que puede usar para configurar un solo servidor de acceso remoto para la administración remota de clientes de DirectAccess. En la tabla siguiente se enumeran los pasos, pero no es necesario realizar estas tareas de planeación en un orden específico.  
  
|Tarea|Descripción|  
|----|--------|  
|[Planear la configuración del servidor y la topología de red](#plan-network-topology-and-settings)|Decida dónde colocar el servidor de acceso remoto (en el perímetro o detrás de un firewall o un dispositivo de traducción de direcciones de red (NAT)) y planee el direccionamiento IP y el enrutamiento.|  
|[Planear los requisitos del firewall](#plan-firewall-requirements)|Planea la configuración de los firewalls perimetrales para permitir el paso de tráfico de Acceso remoto.|  
|[Planear los requisitos de certificado](#plan-certificate-requirements)|Decida si va a usar el protocolo Kerberos o certificados para la autenticación de cliente, y planee los certificados de sitio Web.<br/><br/>IP-HTTPS es un protocolo de transición que los clientes de DirectAccess usan para tunelizar el tráfico IPv6 en redes IPv4. Decida si desea autenticar IP-HTTPS para el servidor con un certificado emitido por una entidad de certificación (CA) o mediante un certificado autofirmado emitido automáticamente por el servidor de acceso remoto.|  
|[Planear los requisitos de DNS](#plan-dns-requirements)|Planee la configuración del sistema de nombres de dominio (DNS) para el servidor de acceso remoto, los servidores de infraestructura, las opciones de resolución local de nombres y la conectividad de cliente.| 
|[Planear la configuración del servidor de ubicación de red](#plan-the-network-location-server-configuration)|Decide dónde colocar el sitio web del servidor de ubicación de red en tu organización (en el servidor de acceso remoto o en un servidor alternativo) y planea los requisitos de certificado si el servidor de ubicación de red se ubicará en el servidor de acceso remoto. **Nota:** Los clientes de DirectAccess usan el servidor de ubicación de red para determinar si están ubicados en la red interna.|  
|[Planear configuraciones de servidores de administración](#plan-management-servers-configuration)|Plan para los servidores de administración (tales como los servidores de actualización) que se usan durante la administración de clientes remotos. **Nota:** Los administradores pueden administrar de forma remota los equipos cliente de DirectAccess que se encuentran fuera de la red corporativa mediante Internet.|  
|[Planear los requisitos de Active Directory](#plan-active-directory-requirements)|Planifique los controladores de dominio, los requisitos de Active Directory, la autenticación del cliente y la estructura de varios dominios.|  
|[Planear la creación de directiva de grupo objeto](#plan-group-policy-object-creation)|Decide qué GPO se necesitan en tu organización y cómo crear y modificar los GPO.|  
  
## <a name="plan-network-topology-and-settings"></a>Planear la topología de red y la configuración  
Al planear la red, debe tener en cuenta la topología del adaptador de red, la configuración de direcciones IP y los requisitos de ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planear los adaptadores de red y el direccionamiento IP  
  
1.  Identifique la topología del adaptador de red que desea usar. El acceso remoto se puede configurar con cualquiera de las topologías siguientes:  
  
    -   Con dos adaptadores de red: el servidor de acceso remoto se instala en el perímetro con un adaptador de red conectado a Internet y el otro a la red interna.  
  
    -   Con dos adaptadores de red: el servidor de acceso remoto se instala detrás de un dispositivo NAT, firewall o enrutador, con un adaptador de red conectado a una red perimetral y el otro a la red interna.  
  
    -   Con un adaptador de red: el servidor de acceso remoto se instala detrás de un dispositivo NAT y el único adaptador de red está conectado a la red interna.  
  
2.  Identifica tus requisitos de direccionamiento IP:  
  
    DirectAccess usa IPv6 con IPsec para crear una conexión segura entre los equipos cliente de DirectAccess y la red corporativa interna. Sin embargo, DirectAccess no requiere necesariamente conectividad con Internet IPv6 ni compatibilidad nativa con IPv6 en las redes internas. En su lugar, configura y usa automáticamente tecnologías de transición IPv6 para Tunelizar el tráfico IPv6 a través de Internet IPv4 (6to4, Teredo o IP-HTTPS) y a través de la intranet solo IPv4 (NAT64 o ISATAP). Para obtener información general acerca de estas tecnologías de transición, consulta los siguientes recursos:  
  
    -   [Tecnologías de transición IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificación del Protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configura los adaptadores y el direccionamiento obligatorios según la tabla siguiente. En el caso de las implementaciones que están detrás de un dispositivo NAT con un solo adaptador de red, configure las direcciones IP utilizando únicamente la columna **adaptador de red interno** .  
  
    ||Adaptador de red externo|Adaptador de red interno<sup>1, arriba</sup>|Requisitos de enrutamiento|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 e intranet IPv4|Configura lo siguiente:<br/><br/>-Dos direcciones IPv4 públicas consecutivas estáticas con las máscaras de subred adecuadas (solo se requiere para Teredo).<br/>-Una dirección IPv4 de puerta de enlace predeterminada para el Firewall de Internet o el enrutador del proveedor de servicios Internet (ISP) local. **Nota:** El servidor de acceso remoto requiere dos direcciones IPv4 públicas consecutivas para que pueda actuar como un servidor Teredo y los clientes Teredo basados en Windows puedan usar el servidor de acceso remoto para detectar el tipo de dispositivo NAT.|Configura lo siguiente:<br/><br/>-Una dirección de intranet IPv4 con la máscara de subred adecuada.<br/>-Un sufijo DNS específico de la conexión para el espacio de nombres de la intranet. También se debería configurar un servidor DNS en la interfaz interna. **PRECAUCIÓN:** No configure una puerta de enlace predeterminada en ninguna interfaz de la intranet.|Para configurar el servidor de acceso remoto para que tenga acceso a todas las subredes de la red IPv4 interna, haga lo siguiente:<br/><br/>-Enumere los espacios de direcciones IPv4 para todas las ubicaciones de la intranet.<br/>-Use los comandos `route add -p` o `netsh interface ipv4 add route` para agregar los espacios de direcciones IPv4 como rutas estáticas en la tabla de enrutamiento IPv4 del servidor de acceso remoto.|  
    |Internet IPv6 e intranet IPv6|Configura lo siguiente:<br/><br/>-Use la configuración de la dirección configurada de forma automática proporcionada por su ISP.<br/>-Use el comando `route print` para asegurarse de que existe una ruta IPv6 predeterminada que apunta al enrutador de ISP en la tabla de enrutamiento de IPv6.<br/>-Determine si los enrutadores del ISP y de la intranet están usando las preferencias de enrutador predeterminadas como se describe en RFC 4191 y si usan una preferencia predeterminada más alta que los enrutadores de la Intranet local. Si ambas condiciones se cumplen, no se necesita ninguna otra configuración para la ruta predeterminada. La preferencia mayor para el enrutador del ISP asegura que la ruta IPv6 predeterminada activa del servidor de acceso remoto apunta a Internet IPv6.<br/><br/>Como el servidor de acceso remoto es un enrutador IPv6, si tienes una infraestructura IPv6 nativa, la interfaz de Internet también puede tener acceso a los controladores de dominio de la intranet. En este caso, agregue filtros de paquetes al controlador de dominio de la red perimetral que impidan la conectividad con la dirección IPv6 de la interfaz de Internet del servidor de acceso remoto.|Configura lo siguiente:<br/><br/>Si no usa los niveles de preferencia predeterminados, configure las interfaces de la intranet con el comando `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled`. Este comando garantiza que las rutas predeterminadas adicionales que apunten a enrutadores de la intranet no se agregarán a la tabla de enrutamiento IPv6. Puede obtener el InterfaceIndex de las interfaces de la intranet en la pantalla del comando `netsh interface show interface`.|Si la intranet es IPv6, haz lo siguiente para configurar el servidor de acceso remoto para que tenga acceso a todas las ubicaciones IPv6:<br/><br/>-Enumere los espacios de direcciones IPv6 para todas las ubicaciones de la intranet.<br/>-Use el comando `netsh interface ipv6 add route` para agregar los espacios de direcciones IPv6 como rutas estáticas en la tabla de enrutamiento IPv6 del servidor de acceso remoto.|  
    |Internet IPv4 e intranet IPv6|El servidor de acceso remoto reenvía el tráfico de enrutamiento IPv6 predeterminado mediante la interfaz del adaptador 6to4 de Microsoft a una retransmisión 6to4 en Internet por IPv4. Cuando IPv6 nativo no está implementado en la red corporativa, puede usar el siguiente comando para configurar un servidor de acceso remoto para la dirección IPv4 de la retransmisión 6to4 de Microsoft en Internet IPv4: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Si se ha asignado una dirección IPv4 pública al cliente de DirectAccess, usará la tecnología de retransmisión 6to4 para conectarse a la intranet. Si al cliente se le asigna una dirección IPv4 privada, utilizará Teredo. Si el cliente de DirectAccess no puede conectarse al servidor de DirectAccess mediante 6to4 o Teredo, usará IP-HTTPS.  
    > -   Para usar Teredo, debes configurar dos direcciones IP consecutivas en el adaptador de red accesible desde el exterior.  
    > -   No se puede usar Teredo si el servidor de acceso remoto solo tiene un adaptador de red.  
    > -   Los equipos cliente IPv6 nativos pueden conectarse al servidor de acceso remoto a través de IPv6 nativo, y no se necesita ninguna tecnología de transición.  
  
### <a name="plan-isatap-requirements"></a>Planear los requisitos de ISATAP  
ISATAP es necesario para la administración remota de Clientesdirectaccess, de modo que los servidores de administración de DirectAccess puedan conectarse a los clientes de DirectAccess ubicados en Internet. ISATAP no es necesario para admitir las conexiones iniciadas por equipos cliente de DirectAccess con los recursos IPv4 de la red corporativa. Para esto se usa NAT64/DNS64. Si la implementación requiere ISATAP, use la tabla siguiente para identificar sus requisitos.  
  
|Escenario de implementación de ISATAP|Requisitos|  
|---------------|--------|  
|Intranet IPv6 nativa existente (no se requiere ISATAP)|Con una infraestructura IPv6 nativa existente, se especifica el prefijo de la organización durante la implementación de acceso remoto y el servidor de acceso remoto no se configura a sí mismo como un enrutador ISATAP. Haz lo siguiente:<br/><br/>1. para asegurarse de que los clientes de DirectAccess son accesibles desde la intranet, debe modificar el enrutamiento IPv6 para que el tráfico de la ruta predeterminada se reenvíe al servidor de acceso remoto. Si el espacio de direcciones IPv6 de la intranet usa una dirección que no sea un único prefijo de dirección IPv6 de 48 bits, debe especificar el prefijo IPv6 de la organización pertinente durante la implementación.<br/>2. Si está conectado actualmente a Internet por IPv6, debe configurar el tráfico de la ruta predeterminada para que se reenvíe al servidor de acceso remoto y, a continuación, configurar las conexiones y rutas adecuadas en el servidor de acceso remoto para que la ruta predeterminada el tráfico se reenvía al dispositivo que está conectado a la red Internet IPv6.|  
|Implementación de ISATAP existente|Si tiene una infraestructura de ISATAP existente, durante la implementación se le pedirá el prefijo de 48 bits de la organización y el servidor de acceso remoto no se configurará a sí mismo como un enrutador ISATAP. Para asegurarse de que los clientes de DirectAccess son accesibles desde la intranet, debe modificar la infraestructura de enrutamiento IPv6 para que el tráfico de la ruta predeterminada se reenvíe al servidor de acceso remoto. Este cambio debe realizarse en el enrutador ISATAP existente al que los clientes de la intranet ya deben reenviar el tráfico predeterminado.|  
|No hay conectividad IPv6 existente|Cuando el Asistente para la instalación de acceso remoto detecta que el servidor no tiene conectividad IPv6 nativa o basada en ISATAP, deriva automáticamente un prefijo de 48 bits basado en 6to4 para la intranet y configura el servidor de acceso remoto como un enrutador ISATAP para proporcionar IPv6 Conectividad a los hosts ISATAP en la intranet. (Un prefijo basado en 6to4 solo se utiliza si el servidor tiene direcciones públicas; de lo contrario, el prefijo se genera automáticamente a partir de un intervalo de direcciones locales único).<br/><br/>Para usar ISATAP, haga lo siguiente:<br/><br/>1. Registre el nombre de ISATAP en un servidor DNS para cada dominio en el que desee habilitar la conectividad basada en ISATAP, de modo que el servidor DNS interno pueda resolver el nombre de ISATAP en la dirección IPv4 interna del servidor de acceso remoto.<br/>2. de forma predeterminada, los servidores DNS que ejecutan Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 bloquean la resolución del nombre ISATAP mediante la lista global de consultas bloqueadas. Para habilitar ISATAP, debe quitar el nombre ISATAP de la lista de bloques. Para obtener más información, vea [Quitar ISATAP de la lista global de consultas bloqueadas de DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Los hosts ISATAP basados en Windows que pueden resolver el nombre ISATAP configuran automáticamente una dirección con el servidor de acceso remoto de la siguiente manera:<br/><br/>1. una dirección IPv6 basada en ISATAP en una interfaz de tunelización ISATAP<br/>2. una ruta de 64 bits que proporciona conectividad con los demás hosts ISATAP de la intranet<br/>3. una ruta IPv6 predeterminada que señala al servidor de acceso remoto. La ruta predeterminada garantiza que los hosts ISATAP de la intranet puedan llegar a los clientes de DirectAccess<br/><br/>Cuando los hosts ISATAP basados en Windows obtienen una dirección IPv6 basada en ISATAP, empiezan a usar el tráfico encapsulado por ISATAP para comunicarse si el destino también es un host ISATAP. Dado que ISATAP utiliza una única subred de 64 bits para toda la intranet, la comunicación va de un modelo de comunicación IPv4 segmentado a un modelo de comunicación de una sola subred con IPv6. Esto puede afectar al comportamiento de algunos Active Directory Domain Services (AD DS) y las aplicaciones que se basan en la configuración de los sitios y servicios de Active Directory. Por ejemplo, si usó el complemento sitios y servicios de Active Directory para configurar sitios, subredes basadas en IPv4 y transportes entre sitios para reenviar solicitudes a servidores dentro de sitios, los hosts ISATAP no usan esta configuración.<br/><br/><ol><li>Para configurar Active Directory sitios y servicios para el reenvío dentro de sitios para hosts ISATAP, para cada objeto de subred IPv4, debe configurar un objeto de subred IPv6 equivalente, en el que el prefijo de dirección IPv6 de la subred expresa el mismo intervalo de host ISATAP se dirige como la subred IPv4. Por ejemplo, para la subred IPv4 192.168.99.0/24 y el prefijo de dirección ISATAP de 64 bits, 2002:836b: 1:8000::/64, el prefijo de dirección IPv6 equivalente para el objeto subred IPv6 es 2002:836b: 1:8000:0: 5EFE: 192.168.99.0/120. En el caso de una longitud de prefijo IPv4 arbitraria (establecida en 24 en el ejemplo), puede determinar la longitud del prefijo IPv6 correspondiente en la fórmula 96 + IPv4PrefixLength.</li><li>Para las direcciones IPv6 de los clientes de DirectAccess, agregue lo siguiente:<br/><br/><ul><li>En el caso de los clientes de DirectAccess basados en Teredo: una subred IPv6 para el intervalo 2001:0: WWXX: YYZZ::/64, donde WWXX: YYZZ es la versión hexadecimal con dos puntos de la primera dirección IPv4 accesible desde Internet del servidor de acceso remoto. .</li><li>En el caso de los clientes de DirectAccess basados en IP-HTTPS: una subred IPv6 para el intervalo 2002: WWXX: YYZZ: 8100::/56, donde WWXX: YYZZ es la versión hexadecimal con dos puntos de la primera dirección IPv4 accesible desde Internet (w.x.y. z) del servidor de acceso remoto. .</li><li>En el caso de los clientes de DirectAccess basados en 6to4: una serie de prefijos IPv6 basados en 6to4 que comienzan por 2002: y representan los prefijos de dirección IPv4 pública y regional administrados por Internet Assigned Numbers Authority (IANA) y los registros regionales. El prefijo basado en 6to4 para un prefijo de dirección IPv4 pública w.x.y. z/n es 2002: WWXX: YYZZ::/[16 + n], donde WWXX: YYZZ es la versión hexadecimal con dos puntos de w.x.y.z.<br/><br/>        Por ejemplo, ARIN (American Registry for Internet Numbers) administra el intervalo 7.0.0.0/8 para Norteamérica. El prefijo basado en 6to4 correspondiente para este intervalo de direcciones IPv6 público es 2002:700::/24. Para obtener más información sobre el espacio de direcciones públicas IPv4, consulte el [registro del espacio de direcciones IPv4 de IANA](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Asegúrese de que no tiene direcciones IP públicas en la interfaz interna del servidor de DirectAccess. Si tiene una dirección IP pública en la interfaz interna, se puede producir un error en la conectividad a través de ISATAP.  
  
### <a name="plan-firewall-requirements"></a>Planear requisitos de firewall  
Si el servidor de acceso remoto está detrás de un firewall perimetral, se necesitan las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentre en Internet IPv4:  
  
-   Para IP-HTTPS: Puerto de destino 443 del Protocolo de control de transmisión (TCP) y puerto de origen TCP 443 de salida.  
  
-   Para el tráfico Teredo: Puerto de destino 3544 del Protocolo de datagramas de usuario (UDP) de entrada y puerto de origen UDP 3544 de salida.  
  
-   Para el tráfico 6to4: protocolo IP 41 entrante y saliente.  
  
    > [!NOTE]  
    > Para el tráfico Teredo y 6to4, estas excepciones se deben aplicar para las dos direcciones IPv4 públicas consecutivas con conexión a Internet del servidor de acceso remoto.  
    >   
    > En el caso de IP-HTTPS, las excepciones deben aplicarse en la dirección que está registrada en el servidor DNS público.  
  
-   Si está implementando el acceso remoto con un único adaptador de red e instalando el servidor de ubicación de red en el servidor de acceso remoto, el puerto TCP 62000.  
  
    > [!NOTE]  
    > Esta exención está en el servidor de acceso remoto y las exenciones anteriores están en el firewall perimetral.  
  
Se necesitan las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de acceso remoto está en Internet por IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
-   Tráfico ICMPv6 entrante y saliente (solo cuando se usa Teredo).  
  
Cuando use firewalls adicionales, aplique las siguientes excepciones de Firewall de red interna para el tráfico de acceso remoto:  
  
-   Para ISATAP: protocolo 41 de entrada y salida  
  
-   Para todo el tráfico IPv4/IPv6: TCP/UD  
  
-   Para Teredo: ICMP para todo el tráfico IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planear requisitos de certificados  
Hay tres escenarios que requieren certificados cuando se implementa un único servidor de acceso remoto.  
  
-   **Autenticación IPSec**: los requisitos de certificado para IPsec incluyen un certificado de equipo que los equipos cliente de DirectAccess usan al establecer la conexión IPsec con el servidor de acceso remoto, y un certificado de equipo que los servidores de acceso remoto usan para establecer conexiones IPsec con los clientes de DirectAccess.  
  
    Para DirectAccess en Windows Server 2012, no es obligatorio usar estos certificados IPsec. Como alternativa, el servidor de acceso remoto puede actuar como un proxy para la autenticación Kerberos sin necesidad de certificados. Si se usa la autenticación Kerberos, funciona a través de SSL y el protocolo Kerberos usa el certificado que se configuró para IP-HTTPS. Algunos escenarios empresariales (incluida la implementación multisitio y la autenticación de cliente de contraseña de un solo uso) requieren el uso de la autenticación de certificados y no la autenticación Kerberos.  
  
-   **Servidor IP-https**: al configurar el acceso remoto, el servidor de acceso remoto se configura automáticamente para que actúe como agente de escucha de IP-https. El sitio IP-HTTPS requiere un certificado de sitio web y los equipos cliente deben poder ponerse en contacto con el sitio de la lista de revocación de certificados (CRL) para consultar el certificado.  
  
-   **Servidor de ubicación de red**: el servidor de ubicación de red es un sitio web que se usa para detectar si los equipos cliente se encuentran en la red corporativa. El servidor de ubicación de red requiere un certificado de sitio web. Los clientes de DirectAccess tienen que poder contactar con el sitio de la CRL para obtener el certificado.  
  
En la tabla siguiente se resumen los requisitos de la entidad de certificación (CA) para cada uno de estos escenarios.  
  
|Autenticación IPsec|Servidor IP-HTTPS|Servidor de ubicación de red|  
|------------|----------|--------------|  
|Se requiere una entidad de certificación interna para emitir certificados de equipo al servidor de acceso remoto y los clientes para la autenticación IPsec cuando no se usa el protocolo Kerberos para la autenticación.|CA interna: puede usar una CA interna para emitir el certificado IP-HTTPS. sin embargo, debe asegurarse de que el punto de distribución de CRL está disponible externamente.|CA interna: puede usar una CA interna para emitir el certificado de sitio web del servidor de ubicación de red. Asegúrate de que el punto de distribución de CRL tenga alta disponibilidad desde la red interna.|  
||Certificado autofirmado: puede usar un certificado autofirmado para el servidor IP-HTTPS. Los certificados autofirmados no se pueden usar en implementaciones multisitio.|Certificado autofirmado: puede usar un certificado autofirmado para el sitio web del servidor de ubicación de red. sin embargo, no puede usar un certificado autofirmado en implementaciones multisitio.|  
||CA pública: se recomienda usar una entidad de certificación pública para emitir el certificado IP-HTTPS, lo que garantiza que el punto de distribución de CRL esté disponible externamente.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Planear certificados de equipo para la autenticación IPsec  
Si usa la autenticación IPsec basada en certificados, el servidor de acceso remoto y los clientes deben obtener un certificado de equipo. La manera más sencilla de instalar los certificados es usar directiva de grupo para configurar la inscripción automática para los certificados de equipo. De esta manera se garantiza que todos los miembros del dominio obtengan un certificado de una entidad de certificación empresarial. Si no tiene configurada una CA empresarial en su organización, consulte [Active Directory servicios de Certificate Server](https://technet.microsoft.com/library/cc770357.aspx).  
  
Este certificado tiene los siguientes requisitos:  
  
-   El certificado debe tener un uso mejorado de clave (EKU) de autenticación del cliente.  
  
-   Los certificados de cliente y de servidor deben estar relacionados con el mismo certificado raíz. Este certificado raíz se debe seleccionar en la configuración de DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Planear certificados para IP-HTTPS  
El servidor de acceso remoto actúa como agente de escucha de IP-HTTPS y tienes que instalar manualmente un certificado de sitio web HTTPS en el servidor. Ten en cuenta lo siguiente en la planificación:  
  
-   Se recomienda usar una entidad de certificación pública para que haya CRL disponibles.  
  
-   En el campo asunto, especifique la dirección IPv4 del adaptador de Internet del servidor de acceso remoto o el FQDN de la dirección URL de IP-HTTPS (la dirección ConnectTo). Si el servidor de acceso remoto está ubicado detrás de un dispositivo NAT, hay que especificar el nombre público o la dirección del dispositivo NAT.  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
-   En el campo **uso mejorado de clave** , utilice el identificador de objeto (OID) de autenticación de servidor.  
  
-   En el campo **Puntos de distribución CRL**, especifique un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
    > [!NOTE]  
    > Esto solo es necesario para los clientes que ejecutan Windows 7.  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
-   Los certificados IP-HTTPS pueden contener caracteres comodín en el nombre.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planear certificados de sitio web para el servidor de ubicación de red  
Tenga en cuenta lo siguiente al planear el sitio web del servidor de ubicación de red:  
  
-   En el campo **Asunto**, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
-   Para el campo **uso mejorado de clave** , utilice el OID de autenticación del servidor.  
  
-   En el campo **puntos de distribución CRL** , use un punto de distribución CRL al que puedan tener acceso los clientes de DirectAccess que estén conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
> [!NOTE]  
> Asegúrese de que los certificados de IP-HTTPS y el servidor de ubicación de red tengan un nombre de sujeto. Si el certificado usa un nombre alternativo, el Asistente para acceso remoto no lo aceptará.  
  
#### <a name="plan-dns-requirements"></a>Planear los requisitos de DNS  
En esta sección se explican los requisitos de DNS para clientes y servidores en una implementación de acceso remoto.  
  
##### <a name="directaccess-client-requests"></a>Solicitudes de cliente de DirectAccess  
DNS se usa para resolver las solicitudes de equipos cliente de DirectAccess que no están ubicados en la red interna. Los clientes de DirectAccess intentan conectarse al servidor de ubicación de red de DirectAccess para determinar si están ubicados en Internet o en la red corporativa.  
  
-   Si la conexión es correcta, los clientes se determinan en la intranet, no se usa DirectAccess y las solicitudes de cliente se resuelven mediante el servidor DNS configurado en el adaptador de red del equipo cliente.  
  
-   Si se producen errores de conexión, se da por hecho que los clientes están en Internet. Los clientes de DirectAccess usarán la tabla de directivas de resolución de nombres (NRPT) para determinar el servidor DNS que usarán para resolver solicitudes de nombres. Puedes especificar que los clientes tengan que usar DNS64 de DirectAccess para resolver nombres, o bien un servidor DNS interno alternativo.  
  
Para llevar a cabo la resolución de nombres, los clientes de DirectAccess usan la tabla NRPT para identificar cómo gestionar una solicitud. Los clientes solicitan un FQDN o un nombre de etiqueta única, como <https://internal>. Si se solicita un nombre de etiqueta única, se anexa un sufijo DNS para crear un FQDN. Si la consulta DNS coincide con una entrada de la NRPT y DNS4 o se especifica un servidor DNS de la intranet para la entrada, la consulta se envía para la resolución de nombres mediante el servidor especificado. Si existe una coincidencia pero no se especifica ningún servidor DNS, se aplica una regla de exención y una resolución de nombres normal.  
  
Cuando se agrega un nuevo sufijo a la NRPT en la consola de administración de acceso remoto, los servidores DNS predeterminados para el sufijo se pueden detectar automáticamente haciendo clic en el botón **detectar** . La detección automática funciona de la siguiente manera:  
  
-   Si la red corporativa se basa en IPv4 o usa IPv4 e IPv6, la dirección predeterminada es la dirección DNS64 del adaptador interno en el servidor de acceso remoto.  
  
-   Si la red corporativa se basa en IPv6, la dirección predeterminada es la dirección IPv6 de los servidores DNS en la red corporativa.  
  
##### <a name="infrastructure-servers"></a>Servidores de infraestructura  
  
-   **Servidor de ubicación de red**  
  
    Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben poder resolver el nombre del servidor de ubicación de red y se les debe impedir que resuelvan el nombre cuando se encuentran en Internet. Para que esto suceda, de forma predeterminada se agrega el FQDN del servidor de ubicación de red como regla de exención a la tabla NRPT. Además, cuando se configura el acceso remoto, se crean automáticamente las siguientes reglas:  
  
    -   Una regla de sufijo DNS para el dominio raíz o el nombre de dominio del servidor de acceso remoto, y las direcciones IPv6 que corresponden a los servidores DNS de la intranet que están configurados en el servidor de acceso remoto. Por ejemplo, si el servidor de acceso remoto es miembro del dominio corp.contoso.com, se crea una regla para el sufijo DNS .corp.contoso.com.  
  
    -   Una regla de exención para el FQDN del servidor de ubicación de red. Por ejemplo, si la dirección URL del servidor de ubicación de red es <https://nls.corp.contoso.com>, se crea una regla de exención para el FQDN nls.corp.contoso.com.  
  
-   **Servidor IP-HTTPS**  
  
    El servidor de acceso remoto actúa como agente de escucha IP-HTTPS y usa su certificado de servidor para autenticarse en los clientes IP-HTTPS. Los clientes de DirectAccess que usan servidores DNS públicos deben poder resolver el nombre IP-HTTPS.  
  
##### <a name="connectivity-verifiers"></a>Comprobadores de la conectividad  
El acceso remoto crea un sondeo web predeterminado que los equipos cliente de DirectAccess usan para comprobar la conectividad de la red interna. Para comprobar si el sondeo funciona correctamente es necesario registrar de forma manual los nombres siguientes en DNS:  
  
-   **DirectAccess-webprobehost** debe resolverse en la dirección IPv4 interna del servidor de acceso remoto o en la dirección IPv6 en un entorno de solo IPv6.  
  
-   **DirectAccess-corpconnectivityhost** debe resolverse en la dirección de host local (bucle invertido). Debe crear los registros A y AAAA. El valor del registro A es 127.0.0.1 y el valor del registro AAAA se construye a partir del prefijo NAT64 con los últimos 32 bits como 127.0.0.1. El prefijo NAT64 se puede recuperar ejecutando el cmdlet **Get-netnatTransitionConfiguration** de Windows PowerShell.  
  
    > [!NOTE]  
    > Esto solo es válido en entornos de solo IPv4. En una IPv4 más IPv6 o en un entorno de solo IPv6, cree solo un registro AAAA con la dirección IP de bucle invertido:: 1.  
  
Puede crear comprobadores de conectividad adicionales mediante otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
##### <a name="dns-server-requirements"></a>Requisitos del servidor DNS  
  
-   En el caso de los clientes de DirectAccess, debe usar un servidor DNS que ejecute Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o cualquier servidor DNS que admita IPv6.  
  
-   Debe usar un servidor DNS que admita actualizaciones dinámicas. Puede usar servidores DNS que no admitan actualizaciones dinámicas, pero las entradas deben actualizarse manualmente.  
  
-   El FQDN de los puntos de distribución de CRL debe poderse resolver mediante el uso de servidores DNS de Internet. Por ejemplo, si la dirección URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> está en el campo **puntos de distribución de CRL** del certificado IP-https del servidor de acceso remoto, debe asegurarse de que el FQDN crld.contoso.com se pueda resolver mediante el uso de servidores DNS de Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Planear la resolución local de nombres  
Tenga en cuenta lo siguiente al planear la resolución local de nombres:  
  
##### <a name="nrpt"></a>NRPT  
Es posible que tenga que crear reglas adicionales de la tabla de directivas de resolución de nombres (NRPT) en las siguientes situaciones:  
  
-   Debe agregar más sufijos DNS para el espacio de nombres de la intranet.  
  
-   Si los FQDN de los puntos de distribución de CRL están basados en el espacio de nombres de la intranet, debe agregar reglas de exención para los FQDN de los puntos de distribución de CRL.  
  
-   Si tiene un entorno DNS de cerebro dividido, debe agregar reglas de exención para los nombres de los recursos para los que desea que los clientes de DirectAccess ubicados en Internet tengan acceso a la versión de Internet, en lugar de a la versión de la intranet.  
  
-   Si está redirigiendo el tráfico a un sitio web externo a través de los servidores proxy Web de la intranet, el sitio web externo solo está disponible desde la intranet. Usa las direcciones de los servidores proxy web para permitir las solicitudes entrantes. En esta situación, agregue una regla de exención para el FQDN del sitio web externo y especifique que la regla usa el servidor proxy Web de la intranet en lugar de las direcciones IPv6 de los servidores DNS de la intranet.  
  
    Por ejemplo, supongamos que está probando un sitio web externo denominado test.contoso.com. Este nombre no se resuelve a través de los servidores DNS de Internet, pero el servidor proxy Web de Contoso sabe cómo resolver el nombre y cómo dirigir las solicitudes para el sitio web al servidor Web externo. Para impedir que los usuarios que no están en la intranet de Contoso tengan acceso al sitio, el sitio web externo solo admite solicitudes de la dirección de Internet IPv4 del proxy web de Contoso. Por lo tanto, los usuarios de la intranet pueden tener acceso al sitio web porque están usando el proxy Web de Contoso, pero los usuarios de DirectAccess no pueden hacerlo porque no usan el proxy Web de contoso. Configurar una regla de exención de NRPT para test.contoso.com que use el proxy web de Contoso permite que las solicitudes de páginas web para test.contoso.com se enruten al servidor proxy web de la intranet a través de Internet IPv4.  
  
##### <a name="single-label-names"></a>Nombres de una sola etiqueta  
A veces se usan nombres de una sola etiqueta, como <https://paycheck>, para los servidores de la intranet. Si se solicita un nombre de una sola etiqueta y se configura una lista de búsqueda de sufijos DNS, los sufijos DNS de la lista se anexarán al nombre de una sola etiqueta. Por ejemplo, cuando un usuario de un equipo que es miembro del <https://paycheck> tipos de dominio corp.contoso.com en el explorador Web, el FQDN que se construye como el nombre es paycheck.corp.contoso.com. De forma predeterminada, el sufijo anexado se basa en el sufijo DNS principal del equipo cliente.  
  
> [!NOTE]  
> En un escenario de espacio de nombres separado (en el que uno o varios equipos de dominio tienen un sufijo DNS que no coincide con el dominio de Active Directory al que son miembros los equipos), debe asegurarse de que la lista de búsqueda se personalice para incluir todos los sufijos necesarios. De forma predeterminada, el Asistente para acceso remoto configura el nombre DNS Active Directory como el sufijo DNS principal en el cliente. Asegúrate de agregar el sufijo DNS que los clientes usan para la resolución de nombres.  
  
Si se implementan varios dominios y el servicio de nombres Internet de Windows (WINS) en su organización y se conecta de forma remota, los nombres únicos se pueden resolver de la siguiente manera:  
  
-   Mediante la implementación de una zona de búsqueda directa WINS en el DNS. Al intentar resolver computername.dns.zone1.corp.contoso.com, la solicitud se dirige al servidor WINS que solo usa el nombre del equipo. El cliente considera que está emitiendo una solicitud de registros A de DNS normal, pero en realidad es una solicitud NetBIOS.  
  
    Para obtener más información, consulte [Administración de una zona de búsqueda directa](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   Agregando un sufijo DNS (por ejemplo, dns.zone1.corp.contoso.com) al GPO de dominio predeterminado.  
  
##### <a name="split-brain-dns"></a>DNS de cerebro dividido  
DNS de cerebro dividido hace referencia al uso del mismo dominio DNS para la resolución de nombres de Internet y de la intranet.  
  
En el caso de las implementaciones de DNS de cerebro dividido, debe enumerar los FQDN que están duplicados en Internet y en la intranet, y decidir a qué recursos debe tener acceso el cliente de DirectAccess: la intranet o la versión de Internet. Si desea que los clientes de DirectAccess lleguen a la versión de Internet, debe agregar el FQDN correspondiente como regla de exención a la tabla NRPT para cada recurso.  
  
En un entorno de DNS de cerebro dividido, si desea que estén disponibles ambas versiones del recurso, configure los recursos de la intranet con nombres que no dupliquen los nombres que se usan en Internet. A continuación, indique a los usuarios que usen el nombre alternativo cuando accedan al recurso en la intranet. Por ejemplo, configure www\.internal.contoso.com para el nombre interno de www\.contoso.com.  
  
En un entorno DNS que no sea de cerebro dividido, el espacio de nombres de Internet es diferente del espacio de nombres de la intranet. Por ejemplo, Contoso Corporation usa contoso.com en Internet y corp.contoso.com en la intranet. Puesto que todos los recursos de la intranet usan el sufijo DNS corp.contoso.com, la regla de la tabla NRPT para corp.contoso.com enruta todas las consultas de nombres DNS para recursos de la intranet a servidores DNS de la intranet. Las consultas DNS de nombres con el sufijo contoso.com no coinciden con la regla de espacio de nombres de la intranet corp.contoso.com de la tabla NRPT y se envían a servidores DNS de Internet. Con una implementación DNS que no sea de cerebro dividido, puesto que no hay ninguna duplicación de FQDN para los recursos de la intranet y de Internet, no es necesario realizar ninguna configuración adicional para la tabla NRPT. Los clientes de DirectAccess pueden tener acceso a los recursos de Internet y de la intranet de su organización.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Planear el comportamiento de la resolución local de nombres para los clientes de DirectAccess  
Si un nombre no se puede resolver con DNS, el servicio cliente DNS en Windows Server 2012, Windows 8, Windows Server 2008 R2 y Windows 7 pueden usar la resolución local de nombres, con los protocolos de resolución de nombres de multidifusión local de vínculo (LLMNR) y NetBIOS sobre TCP/IP, para resolver el nombre en la subred local. La resolución local de nombres suele ser necesaria para la conectividad punto a punto cuando el equipo se encuentra en redes privadas, como redes domésticas de una única subred.  
  
Cuando el servicio cliente DNS realiza la resolución local de nombres de servidores de la intranet y el equipo está conectado a una subred compartida en Internet, los usuarios malintencionados pueden capturar los mensajes de LLMNR y NetBIOS sobre TCP/IP para determinar los nombres de los servidores de la intranet. En la página DNS del Asistente para la instalación del servidor de infraestructura, puede configurar el comportamiento de la resolución local de nombres según los tipos de respuestas recibidos de los servidores DNS de la intranet. Están disponibles las opciones siguientes:  
  
-   **Usar resolución local de nombres si el nombre no existe en DNS**: esta opción es la más segura porque el cliente de DirectAccess realiza la resolución local de nombres solo para los nombres de servidor que los servidores DNS de la intranet no pueden resolver. Si se puede tener acceso a los servidores DNS de la intranet, se resuelven los nombres de los servidores de la intranet. Si los servidores DNS de la intranet no son accesibles o si hay otros tipos de errores de DNS, los nombres de los servidores de la intranet no se filtran a la subred a través de la resolución local de nombres.  
  
-   **Usar la resolución local de nombres si el nombre no existe en DNS o no se puede tener acceso a los servidores DNS cuando el equipo cliente está en una red privada (recomendado)** : esta opción es la recomendada porque permite el uso de la resolución local de nombres en una red privada solo cuando no se puede tener acceso a los servidores DNS de la intranet.  
  
-   **Usar la resolución local de nombres para cualquier tipo de error de resolución de DNS (menos seguro)** : esta es la opción menos segura porque los nombres de los servidores de red de la intranet se pueden filtrar a la subred local a través de la resolución local de nombres.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Planear la configuración del servidor de ubicación de red  
El servidor de ubicación de red es un sitio web que se utiliza para detectar si los clientes de DirectAccess están ubicados en la red corporativa. Los clientes de la red corporativa no usan DirectAccess para acceder a los recursos internos. pero en su lugar, se conectan directamente.  
  
El sitio web del servidor de ubicación de red se puede hospedar en el servidor de acceso remoto o en otro servidor de la organización. Si hospeda el servidor de ubicación de red en el servidor de acceso remoto, el sitio web se crea automáticamente al implementar el acceso remoto. Si hospeda el servidor de ubicación de red en otro servidor que ejecuta un sistema operativo Windows, debe asegurarse de que Internet Information Services (IIS) esté instalado en ese servidor y de que se haya creado el sitio Web. El acceso remoto no configura los valores en el servidor de ubicación de red.  
  
Asegúrese de que el sitio web del servidor de ubicación de red cumpla los siguientes requisitos:  
  
-   Tiene un certificado de servidor HTTPS.  
  
-   Tiene alta disponibilidad en los equipos de la red interna.  
  
-   No es accesible para los equipos cliente de DirectAccess en Internet.  
  
-  
  
Además, tenga en cuenta los siguientes requisitos para los clientes cuando esté configurando el sitio web del servidor de ubicación de red:  
  
-   Los equipos cliente de DirectAccess deben confiar en la entidad de certificación que emitió el certificado de servidor para el sitio web del servidor de ubicación de red.  
  
-   Los equipos cliente de DirectAccess de la red interna deben poder resolver el nombre del sitio del servidor de ubicación de red.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Planear certificados para el servidor de ubicación de red  
Cuando obtenga el certificado de sitio web que se usará para el servidor de ubicación de red, tenga en cuenta lo siguiente:  
  
-   En el campo **Asunto**, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
-   Para el campo **uso mejorado de clave** , utilice el OID de autenticación del servidor.  
  
-   El certificado del servidor de ubicación de red se debe comprobar en una lista de revocación de certificados (CRL). En el campo **puntos de distribución CRL** , use un punto de distribución CRL al que puedan tener acceso los clientes de DirectAccess que estén conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Planear DNS para el servidor de ubicación de red  
Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben ser capaces de resolver el nombre del servidor de ubicación de red, pero debe evitarse que resuelvan el nombre si se encuentran en Internet. Para que esto suceda, de manera predeterminada, el FQDN del servidor de ubicación de red se agregará como una regla de exención a la NRPT.  
  
### <a name="plan-management-servers-configuration"></a>Planear la configuración de los servidores de administración  
Los clientes de DirectAccess inician la comunicación con servidores de administración que proporcionan servicios como Windows Update y actualizaciones de antivirus. Los clientes de DirectAccess también usan el protocolo Kerberos para autenticarse en los controladores de dominio antes de tener acceso a la red interna. Durante la administración remota de los clientes de DirectAccess, los servidores de administración se comunican con los equipos cliente para realizar funciones de administración, como evaluaciones de inventario de software o hardware. El acceso remoto puede detectar automáticamente algunos servidores de administración, entre otros:  
  
-   Controladores de dominio: la detección automática de controladores de dominio se realiza para los dominios que contienen equipos cliente y para todos los dominios del mismo bosque que el servidor de acceso remoto.  
  
-   Servidores de Configuration Manager de puntos de conexión de Microsoft  
  
Los controladores de dominio y los servidores de Configuration Manager se detectan automáticamente la primera vez que se configura DirectAccess. Los controladores de dominio detectados no se muestran en la consola, pero la configuración se puede recuperar con los cmdlets de Windows PowerShell. Si se modifica el controlador de dominio o los servidores de Configuration Manager, al hacer clic en **Update Management servidores** en la consola de se actualiza la lista de servidores de administración.  
  
**Requisitos del servidor de administración**  
  
-   Los servidores de administración deben ser accesibles a través del túnel de infraestructura. Cuando se configura el acceso remoto, al agregar servidores a la lista de servidores de administración automáticamente son accesibles a través de este túnel.  
  
-   Los servidores de administración que inician conexiones a los clientes de DirectAccess deben ser totalmente compatibles con IPv6, por medio de una dirección IPv6 nativa o mediante una dirección asignada por ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Planear los requisitos de Active Directory  
El acceso remoto utiliza Active Directory como se indica a continuación:  
  
-   **Autenticación**: el túnel de infraestructura usa la autenticación NTLMv2 para la cuenta de equipo que se está conectando al servidor de acceso remoto y la cuenta debe estar en un dominio de Active Directory. El túnel de intranet usa la autenticación Kerberos para que el usuario cree el túnel de intranet.  
  
-   **Objetos Directiva de grupo**: el acceso remoto recopila valores de configuración en objetos Directiva de grupo (GPO), que se aplican a servidores de acceso remoto, clientes y servidores de aplicaciones internos.  
  
-   **Grupos de seguridad**: el acceso remoto utiliza grupos de seguridad para recopilar e identificar equipos cliente de DirectAccess. Los GPO se aplican a los grupos de seguridad necesarios.  
  
Al planear un entorno de Active Directory para una implementación de acceso remoto, tenga en cuenta los siguientes requisitos:  
  
-   Al menos un controlador de dominio está instalado en el sistema operativo Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 o Windows Server 2003.  
  
    Si el controlador de dominio está en una red perimetral (y, por tanto, es accesible desde el adaptador de red accesible desde Internet del servidor de acceso remoto), evite que el servidor de acceso remoto llegue a él. Debe agregar filtros de paquetes en el controlador de dominio para impedir la conectividad con la dirección IP del adaptador de Internet.  
  
-   El servidor de acceso remoto debe ser un miembro del dominio.  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los clientes pueden pertenecer a:  
  
    -   Cualquier dominio del mismo bosque que el servidor de acceso remoto.  
  
    -   Cualquier dominio que tenga una confianza bidireccional con el dominio del servidor de acceso remoto.  
  
    -   Cualquier dominio de un bosque que tenga una relación de confianza bidireccional con el bosque del dominio del servidor de acceso remoto.  
  
> [!NOTE]  
> -   El servidor de acceso remoto no puede ser un controlador de dominio.  
> -   La Active Directory controlador de dominio que se usa para el acceso remoto no debe ser accesible desde el adaptador de Internet externo del servidor de acceso remoto (el adaptador no debe estar en el perfil de dominio del firewall de Windows).  
  
#### <a name="plan-client-authentication"></a>Planear la autenticación de clientes  
En el acceso remoto en Windows Server 2012, puede elegir entre usar la autenticación Kerberos integrada, que usa nombres de usuario y contraseñas, o el uso de certificados para la autenticación de equipos con IPsec.  
  
**Autenticación Kerberos**: cuando decide usar credenciales de Active Directory para la autenticación, DirectAccess usa primero la autenticación Kerberos para el equipo y, a continuación, usa la autenticación Kerberos para el usuario. Al usar este modo de autenticación, DirectAccess usa un solo túnel de seguridad que proporciona acceso al servidor DNS, al controlador de dominio y a cualquier otro servidor de la red interna.  
  
**Autenticación IPSec**: cuando decida usar la autenticación de dos factores o la protección de acceso a redes, DirectAccess usa dos túneles de seguridad. El Asistente para la instalación de acceso remoto configura reglas de seguridad de conexión en firewall de Windows con seguridad avanzada. Estas reglas especifican las siguientes credenciales al negociar la seguridad de IPsec en el servidor de acceso remoto:  
  
-   El túnel de infraestructura usa credenciales de certificado de equipo para la primera autenticación y las credenciales de usuario (NTLMv2) para la segunda autenticación. Las credenciales de usuario fuerzan el uso de protocolo de Internet autenticado (AuthIP) y proporcionan acceso a un servidor DNS y a un controlador de dominio antes de que el cliente de DirectAccess pueda usar credenciales Kerberos para el túnel de intranet.  
  
-   El túnel de intranet usa credenciales de certificado de equipo para las credenciales de primera autenticación y usuario (Kerberos V5) para la segunda autenticación.  
  
#### <a name="plan-multiple-domains"></a>Planear varios dominios  
La lista de servidores de administración debe incluir controladores de dominio de todos los dominios que contengan grupos de seguridad que incluyan equipos cliente de DirectAccess. Debe contener todos los dominios que contengan cuentas de usuario que puedan usar equipos configurados como clientes de DirectAccess. Esto garantiza que los usuarios que no están ubicados en el mismo dominio que el equipo cliente que están usando se autentican con un controlador de dominio en el dominio del usuario.  
  
Esta autenticación es automática si los dominios están en el mismo bosque. Si hay un grupo de seguridad con equipos cliente o servidores de aplicaciones que se encuentran en bosques diferentes, los controladores de dominio de esos bosques no se detectan automáticamente. Los bosques tampoco se detectan automáticamente. Puede ejecutar la tarea **Update Management servidores** en la **Administración de acceso remoto** para detectar estos controladores de dominio.  
  
Siempre que sea posible, se deben agregar sufijos de nombre de dominio comunes a la tabla NRPT durante la implementación de acceso remoto. Por ejemplo, si tienes dos dominios, domain1.corp.contoso.com y domain2.corp.contoso.com, en lugar de agregar dos entradas a la tabla NRPT, puedes agregar una entrada de sufijo DNS común, con el sufijo de nombre de dominio corp.contoso.com. Esto se produce automáticamente para los dominios de la misma raíz. Los dominios que no están en la misma raíz deben agregarse manualmente.  
  
### <a name="plan-group-policy-object-creation"></a>Planear la creación de directiva de grupo objeto  
Cuando se configura el acceso remoto, la configuración de DirectAccess se recopila en objetos de directiva de grupo (GPO). Dos GPO se rellenan con la configuración de DirectAccess y se distribuyen de la siguiente manera:  
  
-   **GPO de cliente de DirectAccess**: este GPO contiene la configuración de cliente, incluida la configuración de tecnología de transición IPv6, las entradas de NRPT y las reglas de seguridad de conexión para Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad que se especifican para los equipos cliente.  
  
-   **GPO de servidor de DirectAccess**: este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor configurado como un servidor de acceso remoto en la implementación. También contiene reglas de seguridad de conexión para Firewall de Windows con seguridad avanzada.  
  
> [!NOTE]  
> La configuración de los servidores de aplicaciones no se admite en la administración remota de los clientes de DirectAccess, ya que los clientes no pueden tener acceso a la red interna del servidor de DirectAccess en el que residen los servidores de aplicaciones. El paso 4 de la pantalla de configuración de la instalación de acceso remoto no está disponible para este tipo de configuración.  
  
Puede configurar los GPO de forma automática o manual.  
  
**Automáticamente**: cuando se especifica que los GPO se crean automáticamente, se especifica un nombre predeterminado para cada GPO.  
  
**Manualmente**: puede usar los GPO predefinidos por el administrador de Active Directory.  
  
Al configurar los GPO, tenga en cuenta las siguientes advertencias:  
  
-   Después de configurar DirectAccess para que use unos GPO específicos, no se puede configurar para que use otros GPO.  
  
-   Use el procedimiento siguiente para realizar una copia de seguridad de todos los objetos de directiva de grupo de acceso remoto antes de ejecutar los cmdlets de DirectAccess:  
  
    [Realizar copias de seguridad y restaurar la configuración de acceso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   Si usa GPO configurados de forma automática o manual, debe agregar una directiva para la detección de vínculos de baja velocidad si los clientes van a usar 3G. La ruta de acceso de la **Directiva: configurar directiva de grupo detección de vínculos de baja velocidad** es:  
  
    **Equipo configuración/Políticas/Plantillas administrativas/Sistema/Directiva de grupo**.  
  
-   Si los permisos correctos para vincular GPO no existen, se emite una advertencia. La operación de acceso remoto continuará, pero no se producirá la vinculación. Si se emite esta advertencia, los vínculos no se crearán automáticamente, aunque los permisos se agreguen posteriormente. En su lugar, el administrador tiene que crear los vínculos manualmente.  
  
#### <a name="automatically-created-gpos"></a>GPO creados automáticamente  
Tenga en cuenta lo siguiente al usar GPO creados automáticamente:  
  
Los GPO creados automáticamente se aplican según la ubicación y el destino del vínculo, como se indica a continuación:  
  
-   Para el GPO de servidor de DirectAccess, la ubicación y el destino del vínculo apuntan al dominio que contiene el servidor de acceso remoto.  
  
-   Cuando se crean GPO de servidor de aplicaciones y de cliente, la ubicación se establece en un único dominio. El nombre del GPO se busca en cada dominio y el dominio se rellena con la configuración de DirectAccess, si existe.  
  
-   El destino del vínculo se establece en la raíz del dominio donde se creó el GPO. Se crea un GPO para cada dominio que contiene equipos cliente o servidores de aplicación, y el GPO se vincula a la raíz de su dominio respectivo.  
  
Cuando se usan GPO creados automáticamente para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto requiere los siguientes permisos:  
  
-   Permisos para crear GPO para cada dominio.  
  
-   Permisos para vincular todas las raíces de dominio de cliente seleccionadas.  
  
-   Permisos para vincular a las raíces de dominio de GPO de servidor.  
  
-   Permisos de seguridad para crear, editar, eliminar y modificar los GPO.  
  
-   Permisos de lectura de GPO para cada dominio requerido. Este permiso no es necesario, pero se recomienda porque permite el acceso remoto para comprobar que no existen GPO con nombres duplicados cuando se crean los GPO.  
  
#### <a name="manually-created-gpos"></a>GPO creados manualmente  
Ten en cuenta lo siguiente cuando uses GPO creados manualmente:  
  
-   Los GPO deben existir antes de ejecutar el Asistente para instalación de acceso remoto.  
  
-   Para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto necesita permisos de seguridad completos para crear, editar, eliminar y modificar los GPO creados manualmente.  
  
-   Se realiza una búsqueda de un vínculo al GPO en todo el dominio. Si no hay un vínculo al GPO en el dominio, se crea uno automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperación de un GPO eliminado  
Si un GPO en un servidor de acceso remoto, un cliente o un servidor de aplicaciones se ha eliminado accidentalmente, aparecerá el siguiente mensaje de error: **no se encuentra el GPO (nombre de GPO)** .  
  
Si hay una copia de seguridad disponible, puedes usarla para restaurar el GPO. Si no hay ninguna copia de seguridad disponible, debe quitar las opciones de configuración y configurarlas de nuevo.  
  
###### <a name="to-remove-configuration-settings"></a>Para quitar las opciones de configuración  
  
1.  Ejecute el cmdlet de Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Abra **Administración de acceso remoto**.  
  
3.  Verás un mensaje de error que indica que no se encontró el GPO. Haz clic en **Quitar opciones de configuración**. Una vez finalizado, el servidor se restaurará a un estado sin configurar y podrá volver a configurar los valores.  
  


