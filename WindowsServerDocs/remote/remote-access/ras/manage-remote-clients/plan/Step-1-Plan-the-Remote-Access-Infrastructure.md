---
title: Paso 1 planear la infraestructura de acceso remoto
description: En este tema forma parte de la Guía de los clientes de DirectAccess de administrar remotamente en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f3b1837145dee5767741052c548a4b44da56659b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792332"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Paso 1 planear la infraestructura de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combina DirectAccess y enrutamiento y acceso remoto (RRAS) en un solo rol de acceso remoto.  
  
En este tema se describe los pasos para planear una infraestructura que puede usar para configurar un único servidor de acceso remoto para la administración remota de los clientes de DirectAccess. En la tabla siguiente se enumera los pasos, pero no es necesario que estas tareas de implementación se realiza en un orden específico.  
  
|Tarea|Descripción|  
|----|--------|  
|[Planear la configuración de servidor y la topología de red](#plan-network-topology-and-settings)|Decide dónde colocar el servidor de acceso remoto (en el borde o detrás de un dispositivo de traducción de direcciones de red (NAT) o un firewall), y planea el direccionamiento IP y enrutamiento.|  
|[Planear los requisitos de firewall](#plan-firewall-requirements)|Planea la configuración de los firewalls perimetrales para permitir el paso de tráfico de Acceso remoto.|  
|[Planear los requisitos de certificado](#plan-certificate-requirements)|Decidir si se usa el protocolo Kerberos o certificados para la autenticación de cliente y planear los certificados del sitio Web.<br/><br/>IP-HTTPS es un protocolo de transición que los clientes de DirectAccess usan para tunelizar el tráfico IPv6 en redes IPv4. Decidir si la autenticación IP-HTTPS para el servidor mediante un certificado emitido por una entidad de certificación (CA) o mediante un certificado autofirmado emitido automáticamente por el servidor de acceso remoto.|  
|[Planear los requisitos de DNS](#plan-dns-requirements)|Planear la configuración del sistema de nombres de dominio (DNS) para el servidor de acceso remoto, los servidores de infraestructura, las opciones de resolución de nombre local y conectividad de cliente.| 
|[Planear la configuración del servidor de ubicación de red](#plan-the-network-location-server-configuration)|Decidir dónde colocar el sitio Web servidor de ubicación de red de su organización (en el servidor de acceso remoto o un servidor alternativo) y planear los requisitos de certificado si el servidor de ubicación de red se ubicará en el servidor de acceso remoto. **Nota:** Los clientes de DirectAccess usan el servidor de ubicación de red para determinar si están ubicados en la red interna.|  
|[Planear las configuraciones de los servidores de administración](#plan-management-servers-configuration)|Plan para los servidores de administración (tales como los servidores de actualización) que se usan durante la administración de clientes remotos. **Nota:** Los administradores pueden administrar de forma remota equipos cliente de DirectAccess que estén ubicados fuera de la red corporativa mediante Internet.|  
|[Planear los requisitos de Active Directory](#plan-active-directory-requirements)|Planee la estructura de dominios múltiples, los requisitos de Active Directory, autenticación de cliente y los controladores de dominio.|  
|[Planear la creación de objetos de directiva de grupo](#plan-group-policy-object-creation)|Decidir qué GPO se necesitan en su organización y cómo crear y editar los GPO.|  
  
## <a name="plan-network-topology-and-settings"></a>Planear la topología de red y la configuración  
Cuando planee la red, debe tener en cuenta la topología de adaptador de red, la configuración para el direccionamiento IP y los requisitos de ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planear los adaptadores de red y el direccionamiento IP  
  
1.  Identificar la topología de adaptador de red que desea usar. Acceso remoto puede configurarse con cualquiera de las siguientes topologías:  
  
    -   Con dos adaptadores de red: El servidor de acceso remoto se instala en el perímetro con un adaptador de red conectado a Internet y el otro a la red interna.  
  
    -   Con dos adaptadores de red: El servidor de acceso remoto se instala detrás de un dispositivo NAT, firewall o enrutador, con un adaptador de red conectado a una red perimetral y el otro a la red interna.  
  
    -   Con un adaptador de red: El servidor de acceso remoto se instala detrás de un dispositivo NAT y el único adaptador de red está conectado a la red interna.  
  
2.  Identifica tus requisitos de direccionamiento IP:  
  
    DirectAccess usa IPv6 con IPsec para crear una conexión segura entre los equipos cliente de DirectAccess y la red corporativa interna. Sin embargo, DirectAccess no requiere necesariamente conectividad con Internet IPv6 ni compatibilidad nativa con IPv6 en las redes internas. En su lugar, configura automáticamente y usa tecnologías de transición IPv6 para tunelizar el tráfico IPv6 a través de Internet IPv4 (6to4, Teredo o IP-HTTPS) y a través de la intranet solo IPv4 (NAT64 o ISATAP). Para obtener información general acerca de estas tecnologías de transición, consulta los siguientes recursos:  
  
    -   [Tecnologías de transición IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificación del protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configura los adaptadores y el direccionamiento obligatorios según la tabla siguiente. Para las implementaciones que están detrás de un dispositivo NAT con un único adaptador de red, configure las direcciones IP usando solo el **adaptador de red interno** columna.  
  
    ||Adaptador de red externo|Adaptador de red interno<sup>1, más arriba</sup>|Requisitos de enrutamiento|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 e intranet IPv4|Configura lo siguiente:<br/><br/>: Estático dos direcciones IPv4 públicas consecutivas con las máscaras de subred adecuadas (necesarias solo para Teredo).<br/>-Puerta de enlace predeterminada dirección IPv4 para el firewall de Internet o el enrutador local de proveedor (ISP) de servicio de Internet. **Nota:** El servidor de acceso remoto requiere dos direcciones IPv4 públicas consecutivas para que pueda actuar como un servidor Teredo y los clientes Teredo basados en Windows pueden usar el servidor de acceso remoto para detectar el tipo de dispositivo NAT.|Configura lo siguiente:<br/><br/>-Una dirección de intranet IPv4 con la máscara de subred adecuada.<br/>-Un sufijo DNS específico de la conexión para el espacio de nombres de intranet. También se debería configurar un servidor DNS en la interfaz interna. **Atención:** No configure ninguna puerta de enlace predeterminada en ninguna interfaz de la intranet.|Para configurar el servidor de acceso remoto para llegar a todas las subredes de la red IPv4 interna, realice lo siguiente:<br/><br/>-Enumere los espacios de direcciones IPv4 para todas las ubicaciones de la intranet.<br/>-Use el `route add -p` o `netsh interface ipv4 add route` espacios de direcciones de comandos para agregar el IPv4 como rutas estáticas en la tabla de enrutamiento IPv4 del servidor de acceso remoto.|  
    |Internet IPv6 e intranet IPv6|Configura lo siguiente:<br/><br/>-Use la configuración de dirección configurada automáticamente proporcionada por su ISP.<br/>-Use el `route print` comando para asegurarse de que existe una ruta IPv6 predeterminada que apunta al enrutador del ISP en la tabla de enrutamiento de IPv6.<br/>-Determine si los enrutadores del ISP y de intranet están usando preferencias del enrutador predeterminadas como se describe en RFC 4191 y si son usar una preferencia predeterminada mayor que los enrutadores de la intranet local. Si ambas condiciones se cumplen, no se necesita ninguna otra configuración para la ruta predeterminada. La preferencia mayor para el enrutador del ISP asegura que la ruta IPv6 predeterminada activa del servidor de acceso remoto apunta a Internet IPv6.<br/><br/>Como el servidor de acceso remoto es un enrutador IPv6, si tienes una infraestructura IPv6 nativa, la interfaz de Internet también puede tener acceso a los controladores de dominio de la intranet. En este caso, puede agregar filtros de paquetes al controlador de dominio en la red perimetral que impidan que la dirección IPv6 de la interfaz de Internet del servidor de acceso remoto.|Configura lo siguiente:<br/><br/>Si no usas los niveles de preferencia de forma predeterminada, configura las interfaces de intranet usando el `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` comando. Este comando garantiza que las rutas predeterminadas adicionales que apunten a enrutadores de la intranet no se agregarán a la tabla de enrutamiento IPv6. Puede obtener el InterfaceIndex de las interfaces de intranet de la presentación de la `netsh interface show interface` comando.|Si la intranet es IPv6, haz lo siguiente para configurar el servidor de acceso remoto para que tenga acceso a todas las ubicaciones IPv6:<br/><br/>-Enumere los espacios de direcciones IPv6 para todas las ubicaciones de la intranet.<br/>-Use el `netsh interface ipv6 add route` comando para agregar los espacios de direcciones IPv6 como rutas estáticas en la tabla de enrutamiento IPv6 del servidor de acceso remoto.|  
    |Internet IPv4 e intranet IPv6|El servidor de acceso remoto reenvía el tráfico de la ruta IPv6 predeterminada mediante el uso de la interfaz de adaptador 6to4 de Microsoft a una retransmisión 6to4 en Internet por IPv4. Cuando no se implementa IPv6 nativa en la red corporativa, puede usar el comando siguiente para configurar un servidor de acceso remoto para la dirección IPv4 de la retransmisión 6to4 de Microsoft en Internet IPv4: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Si el cliente de DirectAccess se ha asignado una dirección IPv4 pública, usará la tecnología de retransmisión 6to4 para conectarse a la intranet. Si el cliente se asigna una dirección IPv4 privada, empleará Teredo. Si el cliente de DirectAccess no puede conectarse al servidor de DirectAccess mediante 6to4 o Teredo, usará IP-HTTPS.  
    > -   Para usar Teredo, debes configurar dos direcciones IP consecutivas en el adaptador de red accesible desde el exterior.  
    > -   No se puede usar Teredo si el servidor de acceso remoto tiene solo un adaptador de red.  
    > -   Los equipos cliente IPv6 nativos pueden conectarse al servidor de acceso remoto a través de IPv6 nativo, y no se necesita ninguna tecnología de transición.  
  
### <a name="plan-isatap-requirements"></a>Planear los requisitos de ISATAP  
ISATAP es necesario para la administración remota de DirectAccessclients, para que los servidores de administración de DirectAccess pueden conectarse a los clientes de DirectAccess ubicados en Internet. ISATAP no es necesario para admitir conexiones iniciadas por los equipos cliente de DirectAccess a los recursos de IPv4 en la red corporativa. Para esto se usa NAT64/DNS64. Si la implementación requiere ISATAP, utilice la siguiente tabla para identificar sus requisitos.  
  
|Escenario de implementación de ISATAP|Requisitos|  
|---------------|--------|  
|Existente intranet IPv6 nativa (no ISATAP es necesario)|Con una infraestructura IPv6 nativa existente, especifique el prefijo de la organización durante la implementación de acceso remoto y el servidor de acceso remoto no se configura como un enrutador ISATAP. Haga lo siguiente:<br/><br/>1.  Para asegurarse de que los clientes de DirectAccess son accesibles desde la intranet, debe modificar el enrutamiento de IPv6 para que enrutar el tráfico de forma predeterminada se reenvíe al servidor de acceso remoto. Si el espacio de direcciones IPv6 de la intranet usa una dirección que no sea un prefijo de dirección de IPv6 de 48 bits único, debe especificar el prefijo IPv6 de la organización pertinente durante la implementación.<br/>2.  Si actualmente está conectado a Internet por IPv6, debe configurar el tráfico de la ruta predeterminada para que se reenvía al servidor de acceso remoto y, a continuación, configurar las conexiones adecuadas y las rutas en el servidor de acceso remoto, para que la ruta predeterminada el tráfico se reenvía al dispositivo que está conectado a Internet por IPv6.|  
|Implementación de ISATAP existente|Si tiene una infraestructura ISATAP existente, durante la implementación se le pedirá el prefijo de 48 bits de la organización, y el servidor de acceso remoto no se configura como un enrutador ISATAP. Para asegurarse de que los clientes de DirectAccess son accesibles desde la intranet, debe modificar la infraestructura de enrutamiento IPv6 para que enrutar el tráfico de forma predeterminada se reenvíe al servidor de acceso remoto. Este cambio debe hacerse en el enrutador ISATAP existente a la que los clientes de la intranet deben ya a reenviar el tráfico de forma predeterminada.|  
|No hay conectividad de IPv6 existente|Cuando el Asistente para instalación de acceso remoto detecta que el servidor no tiene ninguna conectividad IPv6 nativa o basada en ISATAP, automáticamente se deriva un prefijo de 48 bits basado en 6to4 para la intranet y configura el servidor de acceso remoto como un enrutador ISATAP para proporcionar IPv6 conectividad a los hosts ISATAP en toda la intranet. (Un prefijo basado en 6to4 se usa únicamente si el servidor tiene direcciones públicas, en caso contrario, se genera automáticamente el prefijo de un intervalo de direcciones local único).<br/><br/>Para usar ISATAP realice lo siguiente:<br/><br/>1.  Registre el nombre ISATAP en un servidor DNS para cada dominio en el que desea habilitar la conectividad basada en ISATAP, para que el nombre ISATAP es capaz de resolver el servidor DNS interno para la dirección IPv4 interna del servidor de acceso remoto.<br/>2.  De forma predeterminada, servidores DNS que ejecutan Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 bloquean la resolución del nombre ISATAP mediante el uso de la lista global de consultas bloqueadas. Para habilitar ISATAP, debe quitar el nombre ISATAP de la lista de bloqueo. Para obtener más información, vea [Quitar ISATAP de la lista global de consultas bloqueadas de DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Los hosts ISATAP basados en Windows que se pueden resolver el nombre ISATAP automáticamente configuración una dirección con el servidor de acceso remoto como sigue:<br/><br/>1.  Una dirección IPv6 basada en ISATAP en una interfaz de tunelización de ISATAP<br/>2.  Una ruta de 64 bits que proporciona conectividad con los demás hosts ISATAP en la intranet<br/>3.  Una ruta IPv6 predeterminada que señala al servidor de acceso remoto. La ruta predeterminada garantiza que los hosts ISATAP de la intranet pueden llegar a los clientes de DirectAccess<br/><br/>Cuando los hosts ISATAP basados en Windows obtienen una dirección IPv6 basada en ISATAP, empiezan a usar tráfico encapsulado por ISATAP para comunicarse si el destino es también un host ISATAP. Puesto que ISATAP emplea una única subred 64 bits para toda la intranet, la comunicación pasa de un modelo de IPv4 segmentado de comunicación, un modelo de comunicación de una única subred con IPv6. Esto puede afectar al comportamiento de algunos servicios de dominio de Active Directory (AD DS) y las aplicaciones que dependen de la configuración de servicios y sitios de Active Directory. Por ejemplo, si usa los sitios de Active Directory y el complemento Servicios para configurar sitios, subredes basadas en IPv4 y transportes entre sitios para reenviar las solicitudes a servidores dentro de los sitios, esta configuración no se utiliza con los hosts ISATAP.<br/><br/><ol><li>Para configurar sitios de Active Directory y servicios para el reenvío dentro de los sitios para los hosts ISATAP, para cada objeto de subred IPv4, debe configurar un objeto de subred IPv6 equivalente, en el que el prefijo de dirección IPv6 para la subred expresa el mismo host de intervalo de ISATAP direcciones de subred IPv4. Por ejemplo, para IPv4 192.168.99.0/24 subred y la dirección ISATAP de 64 bits del prefijo 2002:836b:1:8000:: / 64, el prefijo de dirección IPv6 equivalente para el objeto de subred IPv6 es 2002:836b:1:8000:0:5efe:192.168.99.0/120. Para una longitud de prefijo de IPv4 arbitraria (establecida en 24 en el ejemplo), puede determinar la longitud del prefijo IPv6 correspondiente de la fórmula 96 + IPv4PrefixLength.</li><li>Para las direcciones IPv6 de los clientes de DirectAccess, agregue lo siguiente:<br/><br/><ul><li>Para los clientes de DirectAccess basados en Teredo: Una subred IPv6 para el intervalo de 2001:0:WWXX:YYZZ:: / 64, donde WWXX: YYZZ es la versión hexadecimal con dos puntos de la primera dirección IPv4 a través de Internet del servidor de acceso remoto. .</li><li>Para los clientes de DirectAccess basados en IP-HTTPS: Una subred IPv6 para el intervalo de 2002:WWXX:YYZZ:8100:: / 56, donde WWXX: YYZZ es la versión hexadecimal con dos puntos de la primera dirección IPv4 a través de Internet (w.x.y.z) del servidor de acceso remoto. .</li><li>Basado en 6to4 para clientes de DirectAccess: Una serie de prefijos IPv6 basados en 6to4 que comienzan con 2002: y representan los prefijos regionales de dirección IPv4 pública administrados por la autoridad de asignación de números de Internet (IANA) y los registros regionales. El prefijo basado en 6to4 para un w.x.y.z/n de prefijo de dirección IPv4 pública es 2002:: / [16 + n], en donde WWXX: YYZZ es la versión hexadecimal con dos puntos w.x.y.z.<br/><br/>        Por ejemplo, ARIN (American Registry for Internet Numbers) administra el intervalo 7.0.0.0/8 para Norteamérica. El prefijo basado en 6to4 correspondiente para este intervalo de direcciones IPv6 públicas es 2002:700:: / 24. Para obtener información sobre el espacio de direcciones públicas IPv4, vea [registro del espacio de direcciones de IPv4 de IANA](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Asegúrese de que no tiene direcciones IP públicas en la interfaz interna del servidor de DirectAccess. Si tiene la dirección IP pública en la interfaz interna, puede producir un error de conectividad a través de ISATAP.  
  
### <a name="plan-firewall-requirements"></a>Planear requisitos de firewall  
Si el servidor de acceso remoto está detrás de un firewall perimetral, se necesitan las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentre en Internet IPv4:  
  
-   Para IP-HTTPS: Puerto de destino del protocolo de Control (TCP) de transmisión 443 y el puerto de origen TCP 443 de salida.  
  
-   Para el tráfico Teredo: Puerto de destino del protocolo de datagramas (UDP) de usuario 3544 de entrada y el puerto de origen UDP 3544 de salida.  
  
-   Para el tráfico 6to4: Protocolo de IP 41 entrante y saliente.  
  
    > [!NOTE]  
    > Para el tráfico Teredo y 6to4, estas excepciones se deben aplicar para las dos direcciones IPv4 públicas consecutivas con conexión a Internet del servidor de acceso remoto.  
    >   
    > Las excepciones deben aplicarse en la dirección que está registrada en el servidor DNS público para IP-HTTPS.  
  
-   Si va a implementar el acceso remoto con un único adaptador de red e instalar al servidor de ubicación de red en el servidor de acceso remoto, el puerto TCP 62000.  
  
    > [!NOTE]  
    > Esta exención está en el servidor de acceso remoto y las excepciones anteriores se encuentran en el firewall perimetral.  
  
Las excepciones siguientes son necesarias para el tráfico de acceso remoto cuando el servidor de acceso remoto en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
-   Tráfico ICMPv6 entrante y saliente (solo cuando se usa Teredo).  
  
Cuando usas firewalls adicionales, se aplican las siguientes excepciones de firewall de red interna para el tráfico de acceso remoto:  
  
-   Para ISATAP: Protocolo 41 de entrada y de salida  
  
-   Para todo el tráfico IPv4/IPv6: TCP/UD  
  
-   Teredo: ICMP para todo el tráfico IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planear requisitos de certificados  
Hay tres escenarios que requieren certificados cuando se implementa un único servidor de acceso remoto.  
  
-   **Autenticación IPsec**: Requisitos de certificados para IPsec incluyen un certificado de equipo que se usa por equipos cliente de DirectAccess al establecer la conexión IPsec con el servidor de acceso remoto y un certificado de equipo que es utilizado por los servidores de acceso remoto para establecer Conexiones IPsec con los clientes de DirectAccess.  
  
    Para DirectAccess en Windows Server 2012, no es obligatorio el uso de estos certificados IPsec. Como alternativa, el servidor de acceso remoto puede actuar como un proxy para la autenticación Kerberos sin necesidad de certificados. Si se usa la autenticación Kerberos, funciona a través de SSL y el protocolo Kerberos usa el certificado que se ha configurado para IP-HTTPS. En algunos escenarios empresariales (incluida la implementación multisitio y autenticación de cliente de una contraseña) requieren el uso de autenticación de certificado y no la autenticación Kerberos.  
  
-   **Servidor IP-HTTPS**: Al configurar el acceso remoto, el servidor de acceso remoto se configura automáticamente para que actúe como el agente de escucha IP-HTTPS. El sitio IP-HTTPS requiere un certificado de sitio web y los equipos cliente deben poder ponerse en contacto con el sitio de la lista de revocación de certificados (CRL) para consultar el certificado.  
  
-   **Servidor de ubicación de red**: El servidor de ubicación de red es un sitio web que se utiliza para detectar si los equipos cliente están ubicados en la red corporativa. El servidor de ubicación de red requiere un certificado de sitio web. Los clientes de DirectAccess tienen que poder contactar con el sitio de la CRL para obtener el certificado.  
  
Los requisitos de certificación emisora (CA) para cada uno de estos escenarios se resume en la tabla siguiente.  
  
|Autenticación IPsec|Servidor IP-HTTPS|Servidor de ubicación de red|  
|------------|----------|--------------|  
|Una CA interna es necesaria para emitir certificados de equipo para el servidor de acceso remoto y los clientes para la autenticación IPsec si no se usa el protocolo Kerberos para la autenticación.|Entidad de certificación interna: Puedes usar una entidad de certificación interna para emitir el certificado IP-HTTPS; sin embargo, debes asegurarte de que el punto de distribución CRL esté disponible externamente.|Entidad de certificación interna: Puedes usar una entidad de certificación interna para emitir el certificado de sitio web del servidor de ubicación de red. Asegúrate de que el punto de distribución de CRL tenga alta disponibilidad desde la red interna.|  
||Certificado autofirmado: Puede usar un certificado autofirmado para el servidor IP-HTTPS. Los certificados autofirmados no se pueden usar en implementaciones multisitio.|Certificado autofirmado: Puede usar un certificado autofirmado para el sitio Web servidor de ubicación de red; Sin embargo, no puede usar un certificado autofirmado en implementaciones multisitio.|  
||Entidad de certificación pública: Se recomienda usar una CA pública para emitir el certificado IP-HTTPS, esto garantiza que el punto de distribución CRL esté disponible externamente.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Planear certificados de equipo para la autenticación IPsec  
Si usa la autenticación IPsec basada en certificados, el servidor de acceso remoto y los clientes deben obtener un certificado de equipo. La manera más sencilla de instalar los certificados es usar la directiva de grupo para configurar la inscripción automática de certificados de equipo. De esta manera se garantiza que todos los miembros del dominio obtengan un certificado de una entidad de certificación empresarial. Si no tiene una empresa entidad emisora de certificados en su organización, consulte [Active Directory Certificate Services](https://technet.microsoft.com/library/cc770357.aspx).  
  
Este certificado tiene los siguientes requisitos:  
  
-   El certificado debe tener el uso mejorada de clave (EKU) de autenticación de cliente.  
  
-   El cliente y los certificados de servidor deben estar relacionada con el mismo certificado raíz. Este certificado raíz se debe seleccionar en la configuración de DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Planear certificados para IP-HTTPS  
El servidor de acceso remoto actúa como agente de escucha de IP-HTTPS y tienes que instalar manualmente un certificado de sitio web HTTPS en el servidor. Ten en cuenta lo siguiente en la planificación:  
  
-   Se recomienda usar una entidad de certificación pública para que haya CRL disponibles.  
  
-   En el campo Asunto, especifique la dirección IPv4 del adaptador Internet del servidor de acceso remoto o el FQDN de la dirección URL de IP-HTTPS (la dirección ConnectTo). Si el servidor de acceso remoto está ubicado detrás de un dispositivo NAT, hay que especificar el nombre público o la dirección del dispositivo NAT.  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
-   Para el **uso mejorado de clave** campo, use el identificador de objeto (OID) de autenticación de servidor.  
  
-   En el campo **Puntos de distribución CRL**, especifique un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
    > [!NOTE]  
    > Esto solo es necesario para los clientes que ejecutan Windows 7.  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
-   Los certificados IP-HTTPS pueden contener caracteres comodín en el nombre.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planear certificados de sitio web para el servidor de ubicación de red  
Cuando planee el sitio Web servidor de ubicación de red, tenga en cuenta lo siguiente:  
  
-   En el campo **Asunto**, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
-   Para el **uso mejorado de clave** , a continuación, usa el OID autenticación de servidor.  
  
-   Para el **puntos de distribución CRL** campo, utilice un punto de distribución de CRL que sea accesible para los clientes de DirectAccess que están conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
> [!NOTE]  
> Asegúrese de que los certificados de servidor de ubicación de red e IP-HTTPS tienen un nombre de sujeto. Si el certificado usa un nombre alternativo, no se aceptarán mediante el Asistente para acceso remoto.  
  
#### <a name="plan-dns-requirements"></a>Planear los requisitos de DNS  
En esta sección se explica los requisitos de DNS para clientes y servidores en una implementación de acceso remoto.  
  
##### <a name="directaccess-client-requests"></a>Solicitudes de cliente de DirectAccess  
DNS se usa para resolver las solicitudes de equipos cliente de DirectAccess que no están ubicados en la red interna. Los clientes de DirectAccess intentan conectarse al servidor de ubicación de red de DirectAccess para determinar si están ubicados en Internet o en la red corporativa.  
  
-   Si la conexión es correcta, los clientes se determinan que en la intranet, no se usa DirectAccess y se resuelven las solicitudes de cliente con el servidor DNS que está configurado en el adaptador de red del equipo cliente.  
  
-   Si se producen errores de conexión, se da por hecho que los clientes están en Internet. Los clientes de DirectAccess usarán la tabla de directivas de resolución de nombres (NRPT) para determinar el servidor DNS que usarán para resolver solicitudes de nombres. Puedes especificar que los clientes tengan que usar DNS64 de DirectAccess para resolver nombres, o bien un servidor DNS interno alternativo.  
  
Para llevar a cabo la resolución de nombres, los clientes de DirectAccess usan la tabla NRPT para identificar cómo gestionar una solicitud. Los clientes solicitan un FQDN o nombre de etiqueta única, como <https://internal>. Si se solicita un nombre de etiqueta única, se anexa un sufijo DNS para crear un FQDN. Si la consulta DNS coincide con una entrada en la tabla NRPT y se especifica DNS4 o un servidor DNS de intranet para la entrada, la consulta se envía para resolución de nombres mediante el servidor especificado. Si existe una coincidencia, pero no se especifica ningún servidor DNS, una regla de exención y normal se aplica la resolución de nombres.  
  
Cuando se agrega un nuevo sufijo a la tabla NRPT en la consola de administración de acceso remoto, los servidores DNS predeterminados para el sufijo se pueden detectar automáticamente haciendo clic en el **detectar** botón. La detección automática funciona del siguiente modo:  
  
-   Si la red corporativa está basado en IPv4 o usa IPv4 e IPv6, la dirección predeterminada es la dirección DNS64 del adaptador interno en el servidor de acceso remoto.  
  
-   Si la red corporativa se basa en IPv6, la dirección predeterminada es la dirección IPv6 de los servidores DNS en la red corporativa.  
  
##### <a name="infrastructure-servers"></a>Servidores de infraestructura  
  
-   **Servidor de ubicación de red**  
  
    Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben poder resolver el nombre del servidor de ubicación de red y deben impedirse que resuelvan el nombre cuando están ubicados en Internet. Para que esto suceda, de forma predeterminada se agrega el FQDN del servidor de ubicación de red como regla de exención a la tabla NRPT. Además, cuando se configura el acceso remoto, se crean automáticamente las siguientes reglas:  
  
    -   Un sufijo DNS regla para el dominio raíz o el nombre de dominio del servidor de acceso remoto y las direcciones IPv6 que corresponden a los servidores DNS de intranet que están configurados en el servidor de acceso remoto. Por ejemplo, si el servidor de acceso remoto es miembro del dominio corp.contoso.com, se crea una regla para el sufijo DNS .corp.contoso.com.  
  
    -   Una regla de exención para el FQDN del servidor de ubicación de red. Por ejemplo, si la URL del servidor de ubicación de red es <https://nls.corp.contoso.com>, se crea una regla de exención para el FQDN nls.corp.contoso.com.  
  
-   **Servidor IP-HTTPS**  
  
    El servidor de acceso remoto actúa como agente de escucha IP-HTTPS y usa su certificado de servidor para autenticar a los clientes IP-HTTPS. El nombre de IP-HTTPS debe ser capaz de resolver los clientes de DirectAccess que usan servidores DNS públicos.  
  
##### <a name="connectivity-verifiers"></a>Comprobadores de la conectividad  
El acceso remoto crea un sondeo web predeterminado que los equipos cliente de DirectAccess usan para comprobar la conectividad de la red interna. Para comprobar si el sondeo funciona correctamente es necesario registrar de forma manual los nombres siguientes en DNS:  
  
-   **DirectAccess-webprobehost** debe proporcionar la dirección IPv4 interna del servidor de acceso remoto, o la dirección IPv6 en un entorno de solo IPv6.  
  
-   **DirectAccess-corpconnectivityhost** debe proporcionar la dirección de host local (bucle invertido). Debe crear registros A y AAAA. El valor del registro es 127.0.0.1 y el valor del registro AAAA se construye a partir del prefijo NAT64 con los últimos 32 bits como 127.0.0.1. El prefijo NAT64 se puede recuperar ejecutando el **Get-netnatTransitionConfiguration** cmdlet de Windows PowerShell.  
  
    > [!NOTE]  
    > Esto es válido únicamente en entornos de sólo IPv4. En IPv4 y IPv6 o un entorno de solo IPv6, cree solo un registro AAAA con la dirección IP de bucle invertido:: 1.  
  
Puede crear comprobadores de conectividad adicionales usando otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
##### <a name="dns-server-requirements"></a>Requisitos del servidor DNS  
  
-   Para los clientes de DirectAccess, debe usar un servidor DNS con Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o cualquier servidor DNS que admita IPv6.  
  
-   Debe usar un servidor DNS que admita actualizaciones dinámicas. Puede usar servidores DNS que no admiten las actualizaciones dinámicas, pero, a continuación, se deben actualizar manualmente las entradas.  
  
-   El FQDN de los puntos de distribución de CRL debe poder resolver usando servidores DNS de Internet. Por ejemplo, si URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> está en el **puntos de distribución CRL** campo del certificado IP-HTTPS del servidor de acceso remoto, debe asegurarse de que el FQDN CRL.contoso.com sea capaz de resolver usando servidores DNS de Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Plan para la resolución local  
Al planear la resolución de nombres local, tenga en cuenta lo siguiente:  
  
##### <a name="nrpt"></a>NRPT  
Es posible que deba crear reglas de tabla (NRPT) de directiva de resolución de nombres adicionales en las situaciones siguientes:  
  
-   Deberá agregar más sufijos DNS para el espacio de nombres de intranet.  
  
-   Si el FQDN de los puntos de distribución de CRL se basan en el espacio de nombres de intranet, debe agregar reglas de exención para el FQDN de los puntos de distribución de CRL.  
  
-   Si tiene un entorno DNS de cerebro dividido, debe agregar reglas de exención para los nombres de recursos para el que desea que los clientes de DirectAccess que se encuentran en Internet para tener acceso a la versión de Internet, en lugar de la versión de la intranet.  
  
-   Si estás redirigiendo el tráfico a un sitio Web externo a través de los servidores de proxy web de intranet, el sitio Web externo solo está disponible desde la intranet. Usa las direcciones de los servidores proxy web para permitir las solicitudes entrantes. En esta situación, agregue una regla de exención para el FQDN del sitio Web externo y especificar que la regla usa el servidor de proxy web de intranet en lugar de las direcciones IPv6 de servidores DNS de intranet.  
  
    Por ejemplo, supongamos que está probando un sitio Web externo denominado test.contoso.com. Este nombre no se puede resolver a través de los servidores DNS de Internet, pero el servidor de proxy web de Contoso sabe cómo resolver el nombre y dirigir las solicitudes para el sitio Web al servidor web externo. Para impedir que los usuarios que no están en la intranet de Contoso tengan acceso al sitio, el sitio web externo solo admite solicitudes de la dirección de Internet IPv4 del proxy web de Contoso. Por lo tanto, los usuarios de la intranet pueden acceder al sitio Web porque están usando al proxy web de Contoso, pero los usuarios de DirectAccess no pueden porque no están usando al proxy web de Contoso. Configurar una regla de exención de NRPT para test.contoso.com que use el proxy web de Contoso permite que las solicitudes de páginas web para test.contoso.com se enruten al servidor proxy web de la intranet a través de Internet IPv4.  
  
##### <a name="single-label-names"></a>Nombres de una sola etiqueta  
Único, como nombres de etiqueta, <https://paycheck>, a veces se usan para los servidores de intranet. Si se solicita un nombre de etiqueta única y una lista de búsqueda de sufijos DNS está configurada, los sufijos DNS en la lista se anexará al nombre de una sola etiqueta. Por ejemplo, cuando un usuario en un equipo que es miembro del dominio corp.contoso.com escribe <https://paycheck> en el explorador web, el FQDN que se construye como el nombre es paycheck.corp.contoso.com. De forma predeterminada, el sufijo anexado se basa en el sufijo DNS principal del equipo cliente.  
  
> [!NOTE]  
> En un escenario de espacio de nombres separado (donde uno o más equipos de dominio tiene un sufijo DNS que no coincide con el dominio de Active Directory para que los equipos son miembros), debe asegurarse de que la lista de búsqueda se personalice para incluir todos los sufijos necesarios. De forma predeterminada, el Asistente para acceso remoto, configura el nombre de DNS de Active Directory como sufijo DNS principal en el cliente. Asegúrate de agregar el sufijo DNS que los clientes usan para la resolución de nombres.  
  
Si se implementan varios dominios y servicio de nombres Internet de Windows (WINS) en su organización, y se conecta de forma remota, se pueden resolver los nombres únicos como sigue:  
  
-   Mediante la implementación de una zona de búsqueda directa de WINS en DNS. Al intentar resolver computername.dns.zone1.corp.contoso.com, la solicitud se dirige al servidor WINS que solo usa el nombre del equipo. El cliente piensa que está emitiendo una solicitud de registros DNS A regular, pero es realmente una solicitud NetBIOS.  
  
    Para obtener más información, consulte [administrar una zona de búsqueda directa](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   Mediante la adición de un sufijo DNS (por ejemplo, dns.zone1.corp.contoso.com) para el GPO del dominio predeterminado.  
  
##### <a name="split-brain-dns"></a>DNS de cerebro dividido  
DNS de cerebro dividido se refiere al uso del mismo dominio DNS para la resolución de nombres de Internet e intranet.  
  
Las implementaciones DNS de cerebro dividido, debe enumerar los FQDN que se duplican en Internet e intranet y decidir qué recursos el cliente de DirectAccess debe alcance, la intranet o la versión de Internet. Cuando desee que los clientes de DirectAccess para llegar a la versión de Internet, debe agregar el FQDN correspondiente como regla de exención a la tabla NRPT para cada recurso.  
  
En un entorno DNS de cerebro dividido, si desea que ambas versiones del recurso esté disponible, configurar los recursos de intranet con nombres que no duplican los nombres que se usan en Internet. A continuación, indique a los usuarios que empleen el nombre alternativo cuando acceden a los recursos de la intranet. Por ejemplo, configurar www\.internal.contoso.com para el nombre interno de World Wide Web\.contoso.com.  
  
En un entorno DNS que no sea de cerebro dividido, el espacio de nombres de Internet es diferente del espacio de nombres de la intranet. Por ejemplo, Contoso Corporation usa contoso.com en Internet y corp.contoso.com en la intranet. Puesto que todos los recursos de la intranet usan el sufijo DNS corp.contoso.com, la regla de la tabla NRPT para corp.contoso.com enruta todas las consultas de nombres DNS para recursos de la intranet a servidores DNS de la intranet. Las consultas DNS de nombres con el sufijo contoso.com no coinciden con la regla de espacio de nombres de intranet corp.contoso.com de la tabla NRPT y se envían a servidores DNS de Internet. Con una implementación DNS que no sea de cerebro dividido, puesto que no hay ninguna duplicación de FQDN para los recursos de la intranet y de Internet, no es necesario realizar ninguna configuración adicional para la tabla NRPT. Los clientes de DirectAccess pueden tener acceso a los recursos de Internet y de la intranet de su organización.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Planear el comportamiento de la resolución de nombre local para los clientes de DirectAccess  
Si un nombre no se puede resolver con DNS, el servicio cliente DNS en Windows Server 2012, Windows 8, Windows Server 2008 R2 y Windows 7 puede utilizar la resolución de nombres local, con la resolución de nombres de multidifusión Local de vínculos (LLMNR) y NetBIOS a través de protocolos TCP/IP para resolver el  nombre de la subred local. La resolución local de nombres suele ser necesaria para la conectividad punto a punto cuando el equipo se encuentra en redes privadas, como redes domésticas de una única subred.  
  
Cuando el servicio cliente DNS realiza la resolución local de nombres de servidores de intranet y el equipo está conectado a una subred compartida en Internet, los usuarios malintencionados pueden capturar LLMNR y NetBIOS a través de los mensajes de TCP/IP para determinar los nombres de servidor de intranet. En la página DNS del Asistente para la instalación del servidor de infraestructura, puede configurar el comportamiento de resolución de nombre local según los tipos de respuestas recibidos de los servidores DNS de intranet. Están disponibles las opciones siguientes:  
  
-   **Usar resolución local si el nombre no existe en DNS**: Esta opción es la más segura porque el cliente de DirectAccess realiza la resolución local solo para los nombres de servidor que los servidores DNS de la intranet no puedan resolver. Si se puede tener acceso a los servidores DNS de la intranet, se resuelven los nombres de los servidores de la intranet. Si los servidores DNS de la intranet no son accesibles o si hay otros tipos de errores de DNS, los nombres de los servidores de la intranet no se filtran a la subred a través de la resolución local de nombres.  
  
-   **Usar resolución local si el nombre no existe en DNS o servidores DNS no están disponibles cuando el equipo cliente está en una red privada (recomendada)** : Esta opción es la recomendada porque permite el uso de la resolución local de nombres en una red privada cuando los servidores DNS de la intranet no están disponibles.  
  
-   **Usar resolución local para cualquier tipo de error de resolución DNS (menos seguro)** : Esta es la opción menos segura porque los nombres de los servidores de red de la intranet se pueden filtrar a la subred local a través de la resolución local de nombres.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Planear la configuración del servidor de ubicación de red  
El servidor de ubicación de red es un sitio web que se utiliza para detectar si los clientes de DirectAccess están ubicados en la red corporativa. Los clientes de la red corporativa no usan DirectAccess para llegar a los recursos internos; pero en su lugar, se conectan directamente.  
  
El sitio Web servidor de ubicación de red puede hospedarse en el servidor de acceso remoto o en otro servidor en su organización. Si hospeda el servidor de ubicación de red en el servidor de acceso remoto, el sitio Web se crea automáticamente al implementar acceso remoto. Si hospeda el servidor de ubicación de red en otro servidor que ejecuta un sistema operativo de Windows, debe asegurarse de que Internet Information Services (IIS) está instalado en ese servidor, y que se ha creado el sitio Web. Acceso remoto no configurar opciones en el servidor de ubicación de red.  
  
Asegúrese de que el sitio Web servidor de ubicación de red cumple los requisitos siguientes:  
  
-   Tiene un certificado de servidor HTTPS.  
  
-   Tiene alta disponibilidad a los equipos de la red interna.  
  
-   No es accesible para los equipos cliente de DirectAccess en Internet.  
  
-  
  
Además, tenga en cuenta los siguientes requisitos para los clientes cuando se configura su sitio Web servidor de ubicación de red:  
  
-   Los equipos cliente de DirectAccess deben confiar en la entidad de certificación que emitió el certificado de servidor para el sitio web del servidor de ubicación de red.  
  
-   Los equipos cliente de DirectAccess de la red interna deben poder resolver el nombre del sitio del servidor de ubicación de red.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Planear certificados para el servidor de ubicación de red  
Al obtener el certificado del sitio Web que se usará para el servidor de ubicación de red, tenga en cuenta lo siguiente:  
  
-   En el campo **Asunto**, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
-   Para el **uso mejorado de clave** , a continuación, usa el OID autenticación de servidor.  
  
-   El certificado de servidor de ubicación de red debe comprobarse en una lista de revocación de certificados (CRL). Para el **puntos de distribución CRL** campo, utilice un punto de distribución de CRL que sea accesible para los clientes de DirectAccess que están conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Planear DNS para el servidor de ubicación de red  
Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben ser capaces de resolver el nombre del servidor de ubicación de red, pero debe evitarse que resuelvan el nombre si se encuentran en Internet. Para que esto suceda, de manera predeterminada, el FQDN del servidor de ubicación de red se agregará como una regla de exención a la NRPT.  
  
### <a name="plan-management-servers-configuration"></a>Planear la configuración de los servidores de administración  
Los clientes de DirectAccess inician comunicaciones con servidores de administración que proporcionan servicios como Windows Update y actualizaciones de antivirus. Los clientes de DirectAccess también usan el protocolo Kerberos para autenticar a los controladores de dominio antes de acceder a la red interna. Durante la administración remota de los clientes de DirectAccess, los servidores de administración se comunican con los equipos cliente para realizar funciones de administración, como evaluaciones de inventario de software o hardware. El acceso remoto puede detectar automáticamente algunos servidores de administración, entre otros:  
  
-   Controladores de dominio: Se realiza la detección automática de controladores de dominio para los dominios que contienen los equipos cliente y para todos los dominios en el mismo bosque que el servidor de acceso remoto.  
  
-   Servidores de System Center Configuration Manager  
  
Los controladores de dominio y System Center Configuration Manager los servidores se detectan automáticamente la primera vez que DirectAccess se configura. Los controladores de dominio detectados no se muestran en la consola, pero la configuración se puede recuperar mediante cmdlets de Windows PowerShell. Si se modifican el controlador de dominio o servidores de System Center Configuration Manager, haga clic en **servidores de administración de actualización** en la consola se actualiza la lista de servidores de administración.  
  
**Requisitos del servidor de administración**  
  
-   Servidores de administración deben ser accesibles a través del túnel de infraestructura. Cuando se configura el acceso remoto, al agregar servidores a la lista de servidores de administración automáticamente son accesibles a través de este túnel.  
  
-   Servidores de administración que inician conexiones con los clientes de DirectAccess deben admitir totalmente IPv6, por medio de una dirección IPv6 nativa o mediante una dirección asignada por ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Planear los requisitos de Active Directory  
Acceso remoto usa Active Directory como sigue:  
  
-   **Autenticación**: El túnel de infraestructura usa la autenticación NTLMv2 para la cuenta de equipo que se está conectando al servidor de acceso remoto y la cuenta debe estar en un dominio de Active Directory. El túnel de intranet usa la autenticación Kerberos para el usuario para crear el túnel de intranet.  
  
-   **Objetos de directiva de grupo**: Acceso remoto recopila las opciones de configuración en objetos de directiva de grupo (GPO), que se aplican a servidores de acceso remoto, clientes y servidores de aplicaciones internos.  
  
-   **Grupos de seguridad**: Acceso remoto usa grupos de seguridad para recopilar e identificar equipos cliente de DirectAccess. Los GPO se aplican a los grupos de seguridad necesarios.  
  
Al planear un entorno de Active Directory para una implementación de acceso remoto, tenga en cuenta los siguientes requisitos:  
  
-   Al menos un controlador de dominio está instalado en el sistema operativo Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 o Windows Server 2003.  
  
    Si el controlador de dominio está en una red perimetral (y, por tanto, es accesible desde el adaptador de red a través de Internet del servidor de acceso remoto), evitar que el servidor de acceso remoto acceda a él;. Deberá agregar filtros de paquetes en el controlador de dominio para impedir la conectividad a la dirección IP del adaptador de Internet.  
  
-   El servidor de acceso remoto debe ser un miembro del dominio.  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los clientes pueden pertenecer a:  
  
    -   Cualquier dominio del mismo bosque que el servidor de acceso remoto.  
  
    -   Cualquier dominio que tenga una confianza bidireccional con el dominio del servidor de acceso remoto.  
  
    -   Cualquier dominio en un bosque que tenga una confianza bidireccional con el bosque del dominio del servidor de acceso remoto.  
  
> [!NOTE]  
> -   El servidor de acceso remoto no puede ser un controlador de dominio.  
> -   El controlador de dominio de Active Directory que se usa para el acceso remoto no debe ser accesible desde el adaptador de Internet externo del servidor de acceso remoto (el adaptador no debe estar en el perfil de dominio del Firewall de Windows).  
  
#### <a name="plan-client-authentication"></a>Planear la autenticación de clientes  
En el acceso remoto en Windows Server 2012, puede elegir entre usar autenticación integrada de Kerberos, que usa nombres de usuario y contraseñas, o mediante certificados para la autenticación IPsec de equipo.  
  
**La autenticación Kerberos**: Si decide usar credenciales de Active Directory para la autenticación, DirectAccess usa primero la autenticación Kerberos para el equipo y, a continuación, usa la autenticación Kerberos para el usuario. Al usar este modo de autenticación, DirectAccess usa un túnel de seguridad única que proporciona acceso a cualquier otro servidor, el controlador de dominio y el servidor DNS en la red interna  
  
**Autenticación IPsec**: Si elige utilizar autenticación de dos factores o protección de acceso de red, DirectAccess usa dos túneles de seguridad. El Asistente para instalación de acceso remoto configura reglas de seguridad de conexión en el Firewall de Windows con seguridad avanzada. Estas reglas permiten especificar las siguientes credenciales al negociar la seguridad de IPsec en el servidor de acceso remoto:  
  
-   El túnel de infraestructura usa credenciales de certificado de equipo para la primera autenticación y credenciales de usuario (NTLMv2) para la segunda autenticación. Las credenciales de usuario forzar el uso del protocolo de Internet autenticado (AuthIP), y proporcionan acceso a un servidor DNS y controlador de dominio antes de que el cliente de DirectAccess pueda usar credenciales Kerberos para el túnel de intranet.  
  
-   El túnel de intranet usa credenciales de certificado de equipo para la primera autenticación y credenciales de usuario (Kerberos V5) para la segunda autenticación.  
  
#### <a name="plan-multiple-domains"></a>Planear varios dominios  
La lista de servidores de administración debe incluir controladores de dominio de todos los dominios que contengan grupos de seguridad que incluyan equipos cliente de DirectAccess. Debe contener todos los dominios que contienen cuentas de usuario que pueden usar los equipos configurados como clientes de DirectAccess. Esto garantiza que los usuarios que no están ubicados en el mismo dominio que el equipo cliente que están usando se autentican con un controlador de dominio en el dominio del usuario.  
  
Esta autenticación es automática si los dominios están en el mismo bosque. Si hay un grupo de seguridad con los equipos cliente o servidores de aplicaciones que están en bosques diferentes, los controladores de dominio de esos bosques no se detectan automáticamente. Bosques también no se detectan automáticamente. Puede ejecutar la tarea **servidores de administración de actualización** en el **administración de acceso remoto** para detectar estos controladores de dominio.  
  
Siempre que sea posible, se deben agregar sufijos de nombre de dominio comunes a la tabla NRPT durante la implementación de acceso remoto. Por ejemplo, si tienes dos dominios, domain1.corp.contoso.com y domain2.corp.contoso.com, en lugar de agregar dos entradas a la tabla NRPT, puedes agregar una entrada de sufijo DNS común, con el sufijo de nombre de dominio corp.contoso.com. Esto ocurre automáticamente para dominios en la misma raíz. Dominios que no están en la misma raíz deben agregarse manualmente.  
  
### <a name="plan-group-policy-object-creation"></a>Planear la creación de objetos de directiva de grupo  
Al configurar el acceso remoto, la configuración de DirectAccess se recopila en objetos de directiva de grupo (GPO). Se rellenan dos GPO con la configuración de DirectAccess, y se distribuyen como sigue:  
  
-   **GPO de cliente de DirectAccess**: Este GPO contiene la configuración de cliente, incluida la configuración de la tecnología de transición IPv6, las entradas NRPT y las reglas de seguridad para Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad que se especifican para los equipos cliente.  
  
-   **Servidor de DirectAccess GPO**: Este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor que ha configurado como servidor de acceso remoto en la implementación. También contiene las reglas de seguridad para Firewall de Windows con seguridad avanzada.  
  
> [!NOTE]  
> Configuración de servidores de aplicaciones no se admite en la administración remota de los clientes de DirectAccess porque los clientes no pueden acceder a la red interna del servidor de DirectAccess donde residen los servidores de aplicaciones. Paso 4 en la pantalla de configuración de instalación de acceso remoto no está disponible para este tipo de configuración.  
  
Puede configurar los GPO automática o manualmente.  
  
**Automáticamente**: Cuando se especifica que los GPO se crean automáticamente, se especifica un nombre predeterminado para cada GPO.  
  
**Manualmente**: Puede usar los GPO que predefinió el Administrador de Active Directory.  
  
Al configurar los GPO, tenga en cuenta las siguientes advertencias:  
  
-   Después de configurar DirectAccess para que use unos GPO específicos, no se puede configurar para que use otros GPO.  
  
-   Use el procedimiento siguiente para realizar copias de seguridad de todos los objetos de directiva de grupo de acceso remoto antes de ejecutar los cmdlets de DirectAccess:  
  
    [Copia de seguridad y restaurar la configuración de acceso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   GPO configurados manual o si se utiliza automáticamente, deberá agregar una directiva de detección de vínculos lentos si los clientes usarán 3G. La ruta de acceso **directiva: Configurar la detección de vínculo de baja velocidad de directiva de grupo** es:  
  
    **Equipo configuración/Políticas/Plantillas administrativas/Sistema/Directiva de grupo**.  
  
-   Si no dispone de los permisos correctos para vincular GPO, se emite una advertencia. La operación de acceso remoto continuará, pero no se producirá la vinculación. Si aparece esta advertencia, vínculos no se creará automáticamente, incluso si se agregan los permisos más adelante. En su lugar, el administrador tiene que crear los vínculos manualmente.  
  
#### <a name="automatically-created-gpos"></a>GPO creados automáticamente  
Tenga en cuenta lo siguiente cuando se usa automáticamente los GPO creados:  
  
Crea automáticamente los GPO se aplican según la ubicación y el destino de vínculo, como sigue:  
  
-   Para el GPO de servidor de DirectAccess, la ubicación y el destino de vínculo apuntan al dominio que contiene el servidor de acceso remoto.  
  
-   Cuando se crean GPO de servidor de cliente y la aplicación, la ubicación se establece en un único dominio. El nombre del GPO se busca en cada dominio y el dominio se rellena con la configuración de DirectAccess, si existe.  
  
-   El destino del vínculo se establece en la raíz del dominio donde se creó el GPO. Se crea un GPO para cada dominio que contiene equipos cliente o servidores de aplicación, y el GPO se vincula a la raíz de su dominio respectivo.  
  
Al usar GPO creados automáticamente para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto requiere los permisos siguientes:  
  
-   Permisos para crear GPO para cada dominio.  
  
-   Permisos para vincular a todas las raíces de dominio del cliente seleccionado.  
  
-   Permisos para vincular a las raíces de dominio de GPO de servidor.  
  
-   Permisos de seguridad para crear, editar, eliminar y modificar los GPO.  
  
-   GPO permisos de lectura para todos los dominios necesarios. Este permiso no es necesario, pero se recomienda porque permite el acceso remoto comprobar que no existen los GPO con nombres duplicados cuando se crean los GPO.  
  
#### <a name="manually-created-gpos"></a>GPO creados manualmente  
Ten en cuenta lo siguiente cuando uses GPO creados manualmente:  
  
-   Los GPO deben existir antes de ejecutar el Asistente para instalación de acceso remoto.  
  
-   Para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto requiere permisos de seguridad completa para crear, editar, eliminar y modificar los GPO creados manualmente.  
  
-   Se realiza una búsqueda de un vínculo al GPO en todo el dominio. Si no hay un vínculo al GPO en el dominio, se crea uno automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperación de un GPO eliminado  
Si un GPO en un servidor, cliente o servidor de aplicaciones de acceso remoto se ha eliminado por accidente, aparecerá el mensaje de error siguiente: **No se puede encontrar el GPO (nombre del GPO)** .  
  
Si hay una copia de seguridad disponible, puedes usarla para restaurar el GPO. Si no hay disponible ninguna copia de seguridad, debe quitar los valores de configuración y configurar de nuevo.  
  
###### <a name="to-remove-configuration-settings"></a>Para quitar valores de configuración  
  
1.  Ejecute el cmdlet de Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Abra **administración de acceso remoto**.  
  
3.  Verás un mensaje de error que indica que no se encontró el GPO. Haz clic en **Quitar opciones de configuración**. Cuando haya finalizado, el servidor se restaurará a un estado no configurado y puede volver a configurar la configuración.  
  


