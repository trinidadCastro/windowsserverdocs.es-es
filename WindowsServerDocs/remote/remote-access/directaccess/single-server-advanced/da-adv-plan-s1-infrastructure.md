---
title: Paso 1 Plan la infraestructura de DirectAccess avanzada
description: Este tema forma parte de la Guía de implementación de un único servidor de DirectAccess con avanzada configuración para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa3174f3-42af-4511-ac2d-d8968b66da87
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3b7751ca6d0b62ee078d5da7084cbc007edc155
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805170"
---
# <a name="step-1-plan-the-advanced-directaccess-infrastructure"></a>Paso 1 Plan la infraestructura de DirectAccess avanzada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El primer paso para planear una implementación avanzada de DirectAccess en un único servidor es planear la infraestructura necesaria para la implementación. En este tema se detallan los pasos de planeación de la infraestructura. No es necesario realizar estas tareas de implementación en un orden específico.  
  
|Tarea|Descripción|
|----|--------|  
|[1.1 planear la configuración y topología de red](#11-plan-network-topology-and-settings)|Decide dónde colocar el servidor de DirectAccess (en el perímetro, detrás de un firewall o detrás de un dispositivo de traducción de direcciones de red [NAT]), y planea el direccionamiento IP, el enrutamiento y el túnel forzado.|  
|[1.2 Planear los requisitos de firewall](#12-plan-firewall-requirements)|Planea permitir el paso de tráfico de DirectAccess a través de los firewalls perimetrales.|  
|[1.3 planear los requisitos de certificado](#13-plan-certificate-requirements)|Decide si quieres usar Kerberos o certificados para la autenticación de clientes, y planea los certificados de sitio web. IP-HTTPS es un protocolo de transición que los clientes de DirectAccess usan para tunelizar el tráfico IPv6 en redes IPv4. Decide si la autenticación en el servidor IP-HTTPS se realizará mediante un certificado emitido por una entidad de certificación (CA) o mediante un certificado autofirmado que el servidor de DirectAccess emite automáticamente.|  
|[1.4 planear los requisitos de DNS](#14-plan-dns-requirements)|Planea la configuración del Sistema de nombres de dominio (DNS) para el servidor de DirectAccess, servidores de infraestructura, opciones de resolución local de nombres y conectividad de clientes.|  
|[1.5 planear el servidor de ubicación de red](#15-plan-the-network-location-server)|Los clientes de DirectAccess usan el servidor de ubicación de red para determinar si están ubicados en la red interna. Decide si colocarás el sitio web del servidor de ubicación de red en tu organización (en el servidor de DirectAccess o en un servidor alternativo), y planea los requisitos de certificados si el servidor de ubicación de red está ubicado en el servidor de DirectAccess.|  
|[1.6 planear los servidores de administración](#16-plan-management-servers)|Puedes administrar de forma remota equipos cliente de DirectAccess que estén ubicados fuera de la red corporativa en Internet. Plan para los servidores de administración (tales como los servidores de actualización) que se usan durante la administración de clientes remotos.|  
|[1.7 planear Active Directory Domain Services](#17-plan-active-directory-domain-services)|Planea los controladores de dominio, los requisitos de Active Directory, la autenticación de clientes y múltiples dominios.|  
|[1.8 objetos de directiva de grupo de planes de](#18-plan-group-policy-objects)|Decide qué GPO se necesitan en tu organización y cómo crearlos o editarlos.|  
  
## <a name="11-plan-network-topology-and-settings"></a>1.1 Planear la topología de red y la configuración

En esta sección se explica cómo planear tu red, incluido:  
  
- [1.1.1 planear los adaptadores de red y el direccionamiento IP](#111-plan-network-adapters-and-ip-addressing)  
  
- [1.1.2 Planear la conectividad de intranet IPv6](#112-plan-ipv6-intranet-connectivity)  
  
- [1.1.3 planear el túnel forzado](#113-plan-for-force-tunneling)  
  
### <a name="111-plan-network-adapters-and-ip-addressing"></a>1.1.1 Planear los adaptadores de red y el direccionamiento IP  
  
1. Identifica la topología de adaptadores de red que quieres usar. DirectAccess se puede configurar con cualquiera de las siguientes topologías:  
  
    - **Dos adaptadores de red**. El servidor de DirectAccess se puede instalar en el perímetro con un adaptador de red conectado a Internet y otro a la red interna, o se puede instalar detrás de un NAT, firewall o enrutador, con un adaptador de red conectado a una red perimetral y otro a la red interna.  
  
    - **Un adaptador de red**. El servidor de DirectAccess se instala detrás de un dispositivo NAT, y el único adaptador de red se conecta a la red interna.  
  
2. Identifica tus requisitos de direccionamiento IP:  
  
    DirectAccess usa IPv6 con IPsec para crear una conexión segura entre los equipos cliente de DirectAccess y la red corporativa interna. Sin embargo, DirectAccess no requiere necesariamente conectividad con Internet IPv6 ni compatibilidad nativa con IPv6 en las redes internas. En su lugar, configura y usa automáticamente tecnologías de transición IPv6 para tunelizar el tráfico IPv6 a través de Internet IPv4 (mediante 6to4, Teredo o IP-HTTPS) y a través de la intranet solo IPv4 (mediante NAT64 o ISATAP). Para obtener información general acerca de estas tecnologías de transición, consulta los siguientes recursos:  
  
    - [Tecnologías de transición IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Especificación del protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3. Configura los adaptadores y las direcciones necesarios conforme a la tabla siguiente. Para implementaciones que usan un único adaptador de red y se configuran detrás de un dispositivo NAT, configura tus direcciones IP usando solo la columna **Adaptador de red interno**.  
  
    ||Adaptador de red externo|Adaptador de red interno|Requisitos de enrutamiento|  
    |-|--------------|--------------|------------|  
    |Internet IPv4 e intranet IPv4|Configura dos direcciones IPv4 públicas estáticas consecutivas con las máscaras de subred adecuadas (solo se necesita para Teredo).<br/><br/>Configura también la dirección IPv4 de la puerta de enlace predeterminada del firewall de Internet o del enrutador del proveedor de acceso a Internet (ISP). **Nota:** El servidor de DirectAccess necesita dos direcciones IPv4 públicas consecutivas para que pueda actuar como un servidor Teredo y para que los clientes basados en Windows puedan usar el servidor de DirectAccess para detectar el tipo de NAT detrás del que están.|Configura lo siguiente:<br/><br/>-Una dirección de intranet IPv4 con la máscara de subred adecuada.<br/>-El sufijo DNS específico de la conexión del espacio de nombres de intranet. También se debería configurar un servidor DNS en la interfaz interna. **Precaución:** No configure ninguna puerta de enlace predeterminada en ninguna interfaz de la intranet.|Para configurar el servidor de DirectAccess de manera que tenga acceso a todas las subredes de la red IPv4 interna, haz lo siguiente:<br/><br/>-Enumere los espacios de direcciones IPv4 para todas las ubicaciones de la intranet.<br/>-Use el **ruta agregar -p** o**netsh interface ipv4 Agregar ruta** comando para agregar los espacios de direcciones IPv4 como rutas estáticas en la tabla de enrutamiento IPv4 del servidor de DirectAccess.|  
    |Internet IPv6 e intranet IPv6|Configura lo siguiente:<br/><br/>-Usar la configuración de dirección suministrada por su ISP.<br/>-Use el **Route Print** comando para asegurarse de que existe una ruta IPv6 predeterminada y apunta al enrutador del ISP en la tabla de enrutamiento de IPv6.<br/>-Determine si los enrutadores del ISP y de intranet están usando las preferencias de enrutador predeterminadas descritas en RFC 4191 y usando una preferencia predeterminada mayor que los enrutadores de la intranet local.<br/>    Si ambas condiciones se cumplen, no se necesita ninguna otra configuración para la ruta predeterminada. La preferencia mayor para el enrutador del ISP asegura que la ruta IPv6 predeterminada activa del servidor de DirectAccess señala a Internet por IPv6.<br/><br/>Como el servidor de DirectAccess es un enrutador IPv6, si tienes una infraestructura IPv6 nativa, la interfaz de Internet también puede tener acceso a los controladores de dominio de la intranet. En este caso, agrega filtros de paquetes al controlador de dominio de la red perimetral que impidan que el servidor de DirectAccess conecte con la dirección IPv6 de la interfaz accesible desde Internet.|Configura lo siguiente:<br/><br/>-Si no usas los niveles de preferencia predeterminados, puede configurar las interfaces de la intranet mediante el comando siguiente**netsh interface ipv6 establecer InterfaceIndex ignoredefaultroutes = habilitado**.<br/>    Este comando garantiza que las rutas predeterminadas adicionales que apunten a enrutadores de la intranet no se agregarán a la tabla de enrutamiento IPv6. Puedes obtener el índice de interfaz de las interfaces de la intranet con el siguiente comando: **netsh interface ipv6 show interface**.|Si la intranet es IPv6, haz lo siguiente para configurar el servidor de DirectAccess para que tenga acceso a todas las ubicaciones IPv6:<br/><br/>-Enumere los espacios de direcciones IPv6 para todas las ubicaciones de la intranet.<br/>-Use el **netsh interface ipv6 Agregar ruta** comando para agregar los espacios de direcciones IPv6 como rutas estáticas en la tabla de enrutamiento IPv6 del servidor de DirectAccess.|  
    |Internet IPv4 e intranet IPv6|El servidor de DirectAccess reenvía el tráfico de la ruta IPv6 predeterminada a través del adaptador 6to4 de Microsoft con una retransmisión 6to4 en Internet IPv4. Puede configurar un servidor de DirectAccess para la dirección IPv4 del adaptador 6to4 de Microsoft con el siguiente comando: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > - Si se ha asignado una dirección IPv4 pública al cliente de DirectAccess, este usará la tecnología de transición 6to4 para conectarse a la intranet. Si tiene asignada una dirección IPv4 privada, empleará Teredo. Si el cliente de DirectAccess no puede conectarse al servidor de DirectAccess mediante 6to4 o Teredo, usará IP-HTTPS.  
    > - Para usar Teredo, debes configurar dos direcciones IP consecutivas en el adaptador de red accesible desde el exterior.  
    > - No puedes usar Teredo si el servidor de DirectAccess solo tiene un adaptador de red.  
    > - Los equipos cliente IPv6 nativos pueden conectar con el servidor de DirectAccess a través de IPv6 nativo, y no se necesita ninguna tecnología de transición.  
  
### <a name="112-plan-ipv6-intranet-connectivity"></a>1.1.2 Planear la conectividad con la intranet IPv6

Para administrar clientes de DirectAccess remotos, se necesita IPv6. IPv6 permite a los servidores de administración de DirectAccess conectarse a clientes de DirectAccess que están ubicados en Internet con fines de administración remota.  
  
> [!NOTE]  
> - No es necesario usar IPv6 en la red para admitir conexiones iniciadas por equipos cliente de DirectAccess con los recursos IPv4 de la red de tu organización. Para esto se usa NAT64/DNS64.  
> - Si no vas a administrar clientes de DirectAccess remotos, no necesitas implementar IPv6.  
> - ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) no se admite en las implementaciones de DirectAccess.  
> - Si usas IPv6, puedes habilitar las consultas de registros de recursos (AAAA) de host IPv6 para DNS64 mediante el siguiente comando de Windows PowerShell:   **Set-NetDnsTransitionConfiguration -OnlySendAQuery $false**.  
  
### <a name="113-plan-for-force-tunneling"></a>1.1.3 Planear el túnel forzado

Con IPv6 y la tabla de directivas de resolución de nombres (NRPT), los clientes de DirectAccess separan, de forma predeterminada, el tráfico de intranet y de Internet de la siguiente manera:  
  
- Las consultas de nombres DNS para los nombres de dominio completo (FQDN) de la intranet y todo el tráfico de la intranet se intercambian a través de los túneles creados con el servidor de DirectAccess o directamente con servidores de la intranet. El tráfico de intranet de los clientes de DirectAccess es tráfico IPv6.  
  
- Las consultas de nombres FQDN de DNS que corresponden a reglas de exención o no coinciden con el espacio de nombres de la intranet y todo el tráfico a los servidores de Internet se intercambian a través de la interfaz física que está conectada a Internet. El tráfico de Internet de los clientes de DirectAccess normalmente es tráfico IPv4.  
  
Por el contrario, algunas implementaciones de red privada virtual (VPN) de acceso remoto, incluido el cliente VPN, envían de forma predeterminada todo el tráfico de la intranet y de Internet a través de la conexión VPN de acceso remoto. El servidor VPN enruta el tráfico de Internet a los servidores web proxy IPv4 de la intranet para el acceso a recursos de Internet por IPv4. Es posible separar el tráfico de la intranet y de Internet para los clientes VPN de acceso remoto usando túnel dividido. Para ello, se configura la tabla de enrutamiento del protocolo de Internet (IP) en clientes de VPN para que el tráfico a las ubicaciones de la intranet se envíe a través de la conexión VPN y el tráfico a todas las demás ubicaciones se envíe mediante la interfaz física conectada a Internet.  
  
Puede configurar los clientes de DirectAccess para que envíen todo su tráfico a través de los túneles al servidor de DirectAccess con túnel forzado. Cuando el túnel forzado está configurado, los clientes de DirectAccess detectan que están en Internet y quitan su ruta predeterminada de IPv4. A excepción del tráfico de la subred local, todo el tráfico enviado por el cliente de DirectAccess es tráfico IPv6 que pasa por los túneles al servidor de DirectAccess.  
  
> [!IMPORTANT]  
> Si tienes pensado usar túnel forzado o quizás quieras incorporarlo en el futuro, debes implementar una configuración de dos túneles. Debido a las consideraciones de seguridad, no se admite el túnel forzado en una configuración de un solo túnel.  
  
Habilitar el túnel forzado tiene las siguientes consecuencias:  
  
- Los clientes de DirectAccess solo utilizan protocolo de Internet sobre protocolo seguro de transferencia de hipertexto (IP-HTTPS) para obtener conectividad IPv6 al servidor de DirectAccess sobre Internet por IPv4.  
  
- Las únicas ubicaciones a las que un cliente de DirectAccess puede tener acceso de forma predeterminada con tráfico IPv4 son las de su subred local. Todo el tráfico restante enviado por las aplicaciones y los servicios que se ejecutan en el cliente de DirectAccess es tráfico IPv6 enviado a través de la conexión de DirectAccess. Por tanto, las aplicaciones solo IPv4 del cliente de DirectAccess no se pueden usar para tener acceso a recursos de Internet, excepto las de la subred local.  
  
> [!IMPORTANT]  
> Para aplicar el túnel forzado a través de DNS64 y NAT64, se debe implementar conectividad con Internet IPv6. Una manera de lograrlo es hacer que el prefijo IP-HTTPS sea enrutable globalmente, de manera que ipv6.msftncsi.com sea accesible a través de IPv6, y que la respuesta desde el servidor de Internet a los clientes IP-HTTPS pueda volver a través del servidor de DirectAccess.  
>
> Como esto no es factible en la mayoría de los casos, la mejor opción es crear servidores NCSI virtuales dentro de la red corporativa, de la siguiente manera:  
>
> 1. Agrega una entrada NRPT para ipv6.msftncsi.com y resuélvela mediante DNS64 en un sitio web interno (puede ser un sitio web IPv4).  
> 2. Agrega una entrada NRPT para dns.msftncsi.com y resuélvela mediante un servidor DNS corporativo para que devuelva el registro de recursos (AAAA) de host IPv6 (AAAA) fd3e:4f5a:5b81::1. (Puede que usar DNS64 para enviar solo consultas de registros de recursos [A] de host para este FQDN no funcione porque está configurado en implementaciones solo IPv4, por lo que tienes que configurarlo para que se resuelva directamente mediante DNS corporativo).  
  
## <a name="12-plan-firewall-requirements"></a>1.2 Planear los requisitos del firewall

Si el servidor de DirectAccess está detrás de un firewall perimetral, se necesitan las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de DirectAccess se encuentre en Internet IPv4:  
  
- Puerto 3544 de destino (UDP) de protocolo de datagramas de usuario de tráfico Teredo entrante y el puerto de origen UDP 3544 de salida.  
  
- 6to4 tráfico IP protocolo 41 de entrada y salida.  
  
- Puerto de destino 443 de protocolo de Control de transmisión de IP-HTTPS (TCP) y el puerto de origen TCP 443 de salida.  
  
- Si implementas el acceso remoto con un único adaptador de red y lo instalas con el servidor de ubicación de red en el servidor de DirectAccess, también deberás agregar el puerto TCP 62000 a las excepciones.  
  
    > [!NOTE]  
    > Esta exención está en el servidor de DirectAccess, y todas las demás exenciones están en el firewall perimetral.  
  
Para el tráfico de Teredo y 6to4, estas excepciones se deben aplicar para las dos direcciones IPv4 públicas, consecutivas y accesibles desde Internet del servidor de DirectAccess. En el caso de IP-HTTPS, las excepciones deben aplicarse a la dirección que está registrada en el servidor DNS público.  
  
Se necesitan las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de DirectAccess está en Internet IPv6:  
  
- Protocolo IP ID 50  
  
- Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
- Tráfico ICMPv6 de entrada y de salida (solo cuando se usa Teredo).  
  
Si usas firewalls adicionales, aplica las siguientes excepciones de firewall de la red interna para el tráfico de acceso remoto:  
  
- ISATAP: protocolo 41 de entrada y salida  
  
- TCP/UDP para todo el tráfico IPv4 e IPv6  
  
- ICMP para todo el tráfico IPv4 e IPv6 (solo cuando se usa Teredo)  
  
## <a name="13-plan-certificate-requirements"></a>1.3 Planear los requisitos de certificados

Hay tres escenarios que requieren certificados cuando se implementa un único servidor de DirectAccess:  
  
- [1.3.1 planear certificados de equipo para la autenticación IPsec](#131-plan-computer-certificates-for-ipsec-authentication)  
  
    Entre los requisitos de certificados para IPsec se incluye un certificado de equipo que los equipos cliente de DirectAccess usan al establecer la conexión IPsec entre el cliente y el servidor de DirectAccess, y un certificado de equipo que los servidores de DirectAccess usan para establecer conexiones IPsec con los clientes de DirectAccess.  
  
    Para DirectAccess en Windows Server 2012 no es obligatorio el uso de estos certificados IPsec. Como alternativa, el servidor de DirectAccess puede actuar como proxy Kerberos para realizar la autenticación de IPsec sin necesidad de certificados. Si se usa el protocolo Kerberos, funciona a través de SSL, y el proxy Kerberos usa el certificado que está configurado para IP-HTTPS con este fin. En algunos escenarios empresariales (incluida la implementación multisitio y la autenticación de clientes mediante contraseña de un solo uso) es necesario usar la autenticación de certificados y no el protocolo Kerberos.  
  
-   [1.3.2 Planear certificados para IP-HTTPS](#132-plan-certificates-for-ip-https)  
  
    Cuando se configura el acceso remoto, el servidor de DirectAccess se configura automáticamente para actuar como agente de escucha IP-HTTPS. El sitio IP-HTTPS requiere un certificado de sitio web y los equipos cliente deben poder ponerse en contacto con el sitio de la lista de revocación de certificados (CRL) para consultar el certificado.  
  
-   [1.3.3 planear certificados de sitio Web para el servidor de ubicación de red](#133-plan-website-certificates-for-the-network-location-server)  
  
    El servidor de ubicación de red es un sitio web que se utiliza para detectar si los equipos cliente están ubicados en la red corporativa. El servidor de ubicación de red requiere un certificado de sitio web. Los clientes de DirectAccess tienen que poder contactar con el sitio de la CRL para obtener el certificado.  
  
En la tabla siguiente se resumen los requisitos de entidad de certificación (CA) para cada escenario.  
  
|Autenticación IPsec|Servidor IP-HTTPS|Servidor de ubicación de red|  
|------------|----------|--------------|  
|Una CA interna es necesaria para emitir certificados de equipo en el servidor de DirectAccess y los clientes para la autenticación IPsec si no se usa el proxy Kerberos para la autenticación|Entidad de certificación interna:<br/><br/>Puedes usar una entidad de certificación interna para emitir el certificado IP-HTTPS; sin embargo, debes asegurarte de que el punto de distribución CRL esté disponible externamente.|Entidad de certificación interna:<br/><br/>Puedes usar una entidad de certificación interna para emitir el certificado de sitio web del servidor de ubicación de red. Asegúrate de que el punto de distribución CRL tenga alta disponibilidad en la red interna.|  
||Certificado autofirmado:<br/><br/>Puedes usar un certificado autofirmado para el servidor IP-HTTPS; sin embargo, debes asegurarte de que el punto de distribución CRL esté disponible externamente.<br/><br/>En las implementaciones multisitio no se pueden usar certificados autofirmados.|Certificado autofirmado:<br/><br/>Puedes usar un certificado autofirmado para el sitio web del servidor de ubicación de red.<br/><br/>En las implementaciones multisitio no se pueden usar certificados autofirmados.|  
||**Recomienda**<br/><br/>Entidad de certificación pública:<br/><br/>Se recomienda usar una entidad de certificación pública para emitir el certificado IP-HTTPS. Esto garantiza que el punto de distribución CRL esté disponible externamente.|  
  
### <a name="131-plan-computer-certificates-for-ipsec-authentication"></a>1.3.1 Planear certificados de equipo para la autenticación IPsec  
Si usas la autenticación IPsec basada en certificado, el servidor y los clientes de DirectAccess deben obtener un certificado de equipo. La manera más sencilla de instalar los certificados es configurar la inscripción automática basada en la directiva de grupo para los certificados de equipo. De esta manera se garantiza que todos los miembros del dominio obtengan un certificado de una entidad de certificación empresarial. Si no tiene una empresa entidad emisora de certificados en su organización, consulte [Active Directory Certificate Services](https://technet.microsoft.com/library/cc770357.aspx).  
  
Este certificado tiene los siguientes requisitos:  
  
-   El certificado debe tener un uso mejorado de clave (EKU) en la autenticación de cliente.  
  
-   El certificado de cliente y el certificado de servidor deben encadenarse al mismo certificado raíz. Este certificado raíz se debe seleccionar en la configuración de DirectAccess.  
  
### <a name="132-plan-certificates-for-ip-https"></a>1.3.2 Planear certificados para IP-HTTPS  
El servidor de DirectAccess actúa como agente de escucha de IP-HTTPS y tienes que instalar manualmente un certificado de sitio web HTTPS en el servidor. Ten en cuenta lo siguiente en la planificación:  
  
-   Se recomienda usar una entidad de certificación pública para que haya listas de revocación de certificados (CRL) disponibles.  
  
-   En el campo **Asunto**, especifica la dirección IPv4 del adaptador de Internet del servidor de DirectAccess o el FQDN de la dirección URL de IP-HTTPS (la dirección ConnectTo). Si el servidor de DirectAccess está ubicado detrás de un dispositivo NAT, hay que especificar el nombre público o la dirección del dispositivo NAT.  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
-   En el campo **Uso mejorado de clave**, usa el identificador de objeto (OID) de autenticación de servidor.  
  
-   En el campo **Puntos de distribución CRL**, especifique un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
-   Los certificados IP-HTTPS pueden contener caracteres comodín en el nombre.  
  
Si tienes planeado usar IP-HTTPS en un puerto no estándar, sigue estos pasos en el servidor de DirectAccess:  
  
1.  Quita el enlace de certificado existente para 0.0.0.0:443 y reemplázalo con un enlace de certificado para el puerto elegido. Para este ejemplo hemos usado el puerto 44500. Antes de eliminar el enlace de certificado, muestra y copia el **appid**.  
  
    1.  Para eliminar el enlace de certificado, escribe:  
  
        ```  
        netsh http delete ssl ipport=0.0.0.0:443  
        ```  
  
    2.  Para agregar el nuevo enlace de certificado, escribe:  
  
        ```  
        netsh http add ssl ipport=0.0.0.0:44500 certhash=<use the thumbprint from the DirectAccess server SSL cert> appid=<use the appid from the binding that was deleted>  
        ```  
  
2.  Para modificar la dirección URL de IP-HTTPS en el servidor, escribe:  
  
    ```  
    Netsh int http set int url=https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS  
    ```  
  
    ```  
    Net stop iphlpsvc & net start iphlpsvc  
    ```  
  
3.  Cambia la reserva de URL para kdcproxy.  
  
    1.  Para eliminar la reserva de URL existente, escribe:  
  
        ```  
        netsh http del urlacl url=https://+:443/KdcProxy/  
        ```  
  
    2.  Para agregar una nueva reserva de URL, escribe:  
  
        ```  
        netsh http add urlacl url=https://+:44500/KdcProxy/ sddl=D:(A;;GX;;;NS)  
        ```  
  
4.  Agrega la opción para que kppsvc escuche en el puerto no estándar. Para agregar la entrada del Registro, escribe:  
  
    ```  
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\KPSSVC\Settings /v HttpsUrlGroup /t REG_MULTI_SZ /d +:44500 /f  
    ```  
  
5.  Para reiniciar el servicio kdcproxy en el controlador de dominio, escribe:  
  
    ```  
    net stop kpssvc & net start kpssvc  
    ```  
  
Para usar IP-HTTPS en un puerto no estándar, sigue estos pasos en el controlador de dominio:  
  
1.  Modifica la configuración de IP-HTTPS en el GPO de cliente.  
  
    1.  Abre el editor de directivas de grupo.  
  
    2.  Ve a Configuración del equipo=>Directivas=>Plantillas administrativas=> Red=>Configuración de TCPIP=>Tecnologías de transición IPv6.  
  
    3.  Abre la configuración de estado de IP-HTTPS y cambia la dirección URL por **https://<nombre del servidor de DirectAccess (por ejemplo, server.contoso.com)>:44500/IPHTTPS**.  
  
    4.  Haga clic en **Aplicar**.  
  
2.  Modifica la configuración del cliente de proxy Kerberos en el GPO de cliente.  
  
    1.  En el editor de directivas de grupo, ve a Configuración del equipo=>Directivas=>Plantillas administrativas=>Sistema=>Kerberos=> Especifica los servidores proxy de KDC para los clientes Kerberos.  
  
    2.  Abre la configuración de estado de IPHTTPS y cambia la dirección URL por **https://<nombre del servidor de DirectAccess (por ejemplo, server.contoso.com)>:44500/IPHTTPS**.  
  
    3.  Haga clic en **Aplicar**.  
  
3.  Modifica la configuración de la directiva de IPsec del cliente para usar ComputerKerb y UserKerb.  
  
    1.  En el editor de directivas de grupo, ve a Configuración del equipo=>Directivas=> Configuración de Windows=> Configuración de seguridad=> Firewall de Windows con seguridad avanzada.  
  
    2.  Haz clic en **Reglas de seguridad de conexión** y haz doble clic en **Regla IPsec**.  
  
    3.  En la pestaña **Autenticación**, haz clic en **Opciones avanzadas**.  
  
    4.  Para Auth1: quita el método de autenticación existente y reemplázalo por ComputerKerb. Para Auth2: quita el método de autenticación existente y reemplázalo por UserKerb.  
  
    5.  Haz clic en **Aplicar** y después en **Aceptar**.  
  
Para completar el proceso manual para usar un puerto no estándar IP-HTTPS, ejecuta **gpupdate /force** en los equipos cliente y en el servidor de DirectAccess.  
  
### <a name="133-plan-website-certificates-for-the-network-location-server"></a>1.3.3 Planear certificados de sitio web para el servidor de ubicación de red  
A la hora de planear el sitio web del servidor de ubicación de red, ten en cuenta lo siguiente:  
  
-   En el campo **Asunto**, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
-   En el campo **Uso mejorado de clave**, usa el OID Autenticación de servidor.  
  
-   En el campo **Puntos de distribución CRL**, usa un punto de distribución CRL que sea accesible para los clientes de DirectAccess que estén conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
-   Si más adelante tienes previsto configurar una implementación multisitio o en clúster, el nombre del certificado no debe coincidir con el nombre interno de ninguno de los servidores de DirectAccess que se agreguen a la implementación.  
  
    > [!NOTE]  
    > Asegúrate de que los certificados de IP-HTTPS y el servidor de ubicación de red tengan un **Nombre de sujeto**. Si el certificado no tiene un **Nombre de sujeto** sino que tiene un **Nombre alternativo**, el Asistente para acceso remoto no lo aceptará.  
  
## <a name="14-plan-dns-requirements"></a>1.4 Planear los requisitos de DNS  
En esta sección se explican los requisitos de DNS para las solicitudes de cliente de DirectAccess y de servidores de infraestructura en una implementación de acceso remoto. Incluye las siguientes subsecciones:  
  
-   [1.4.1 planear los requisitos de servidor DNS](#141-plan-for-dns-server-requirements)  
  
-   [1.4.2 Planear la resolución de nombres local](#142-plan-for-local-name-resolution)  
  
**Solicitudes de cliente de DirectAccess**  
  
DNS se usa para resolver las solicitudes de equipos cliente de DirectAccess que no están ubicados en la red interna (o corporativa). Los clientes de DirectAccess intentan conectarse al servidor de ubicación de red de DirectAccess para determinar si están ubicados en Internet o en la red interna.  
  
-   Si la conexión es correcta, los clientes se identifican como ubicados en la red interna, no se usa DirectAccess y las solicitudes de cliente se resuelven usando el servidor DNS que está configurado en el adaptador de red del equipo cliente.  
  
-   Si la conexión no se realiza correctamente, se da por hecho que los clientes están en Internet y los clientes de DirectAccess usarán la tabla de directivas de resolución de nombres (NRPT) para determinar qué servidor DNS se usará para resolver las solicitudes de nombres.  
  
Puedes especificar que los clientes usen DirectAccess DNS64 para resolver los nombres o un servidor DNS interno alternativo. Para llevar a cabo la resolución de nombres, los clientes de DirectAccess usan la tabla NRPT para identificar cómo gestionar una solicitud. Los clientes solicitan un FQDN o nombre de etiqueta única, como <https://internal>. Si se solicita un nombre de etiqueta única, se anexa un sufijo DNS para crear un FQDN. Si la consulta DNS coincide con una entrada de la tabla NRPT y se ha especificado DNS64 o un servidor DNS en la red interna para la entrada, la consulta se envía para resolución de nombres con el servidor especificado. Si existe una coincidencia pero no se ha especificado un servidor DNS, esto indica una regla de exención y se aplica la resolución de nombres normal.  
  
> [!NOTE]  
> Ten en cuenta que cuando se agrega un nuevo sufijo a la tabla NRPT en la Consola de administración de acceso remoto, los servidores DNS predeterminados para el sufijo se pueden detectar automáticamente haciendo clic en **Detectar**.  
  
La detección automática funciona de la siguiente manera:  
  
-   Si la red corporativa se basa en IPv4 o usa IPv4 e IPv6, la dirección predeterminada es la dirección DNS64 del adaptador interno en el servidor de DirectAccess.  
  
-   Si la red corporativa se basa en IPv6, la dirección predeterminada es la dirección IPv6 de los servidores DNS en la red corporativa.  
  
**Servidores de infraestructura**  
  
-   **Servidor de ubicación de red**  
  
    Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben poder resolver el nombre del servidor de ubicación de red, pero debe impedirse que resuelvan el nombre cuando están ubicados en Internet. Para que esto suceda, de forma predeterminada se agrega el FQDN del servidor de ubicación de red como regla de exención a la tabla NRPT. Además, cuando se configura el acceso remoto, se crean automáticamente las siguientes reglas:  
  
    -   Una regla de sufijo de DNS para el dominio raíz o el nombre de dominio del servidor de DirectAccess, y las direcciones IPv6 que corresponden con la dirección DNS64. En las redes corporativas de solo IPv6, los servidores DNS de intranet se configuran en el servidor de DirectAccess. Por ejemplo, si el servidor de DirectAccess pertenece al dominio corp.contoso.com, se creará una regla para el sufijo DNS corp.contoso.com.  
  
    -   Una regla de exención para el FQDN del servidor de ubicación de red. Por ejemplo, si la URL del servidor de ubicación de red es <https://nls.corp.contoso.com>, se crea una regla de exención para el FQDN nls.corp.contoso.com.  
  
-   **Servidor IP-HTTPS**  
  
    El servidor de DirectAccess actúa como agente de escucha IP-HTTPS y usa su certificado de servidor para autenticarse en los clientes IP-HTTPS. Los clientes de DirectAccess que usan servidores DNS públicos deben poder resolver el nombre IP-HTTPS.  
  
-   **Comprobación de revocación de CRL**  
  
    DirectAccess usa la comprobación de revocación de certificados para la conexión IP-HTTPS entre los clientes de DirectAccess y el servidor de DirectAccess, y para la conexión basada en HTTPS entre el cliente de DirectAccess y el servidor de ubicación de red. En ambos casos, los clientes de DirectAccess deben ser capaces de resolver y acceder a la ubicación del punto de distribución de CRL.  
  
-   **ISATAP**  
  
    ISATAP permite a los equipos corporativos adquirir una dirección IPv6 y encapsula los paquetes IPv6 dentro de un encabezado IPv4. El servidor de DirectAccess lo usa para proporcionar conectividad IPv6 a los hosts ISATAP en toda la intranet. En un entorno de red IPv6 no nativo, el servidor de DirectAccess se configura a sí mismo automáticamente como enrutador ISATAP.  
  
    Como DirectAccess ya no admite ISATAP, debes asegurarte de que tus servidores DNS estén configurados para no responder a las consultas ISATAP. De forma predeterminada, el servicio Servidor DNS bloquea la resolución de nombres para el nombre ISATAP mediante la lista global de consultas bloqueadas de DNS. No quites el nombre ISATAP de la lista global de consultas bloqueadas.  
  
-   **Comprobadores de conectividad**  
  
    El acceso remoto crea un sondeo web predeterminado que los equipos cliente de DirectAccess usan para comprobar la conectividad de la red interna. Para comprobar si el sondeo funciona correctamente es necesario registrar de forma manual los nombres siguientes en DNS:  
  
    -   **DirectAccess-webprobehost**-debe proporcionar la dirección IPv4 interna del servidor de DirectAccess, o la dirección IPv6 en un entorno de solo IPv6.  
  
    -   **DirectAccess-corpconnectivityhost**-debe proporcionar la dirección de host local (bucle invertido). Deben crearse los siguientes registros de recursos (A) y (AAAA) de host: un registro de recursos (A) de host con el valor 127.0.0.1, y un registro de recursos (AAAA) de host con el valor formado por el prefijo NAT64 con los últimos 32 bits como 127.0.0.1. El prefijo NAT64 se puede recuperar ejecutando el comando de Windows PowerShell **get-netnattransitionconfiguration**.  
  
        > [!NOTE]  
        > Esto solo es válido en un entorno de solo IPv4. En un entorno de IPv4 más IPv6 o en un entorno de solo IPv6, solo se debe crear un registro de recursos (AAAA) de host con la dirección IP de bucle invertido ::1.  
  
    Puedes crear comprobadores de la conectividad adicionales usando otras direcciones web a través de HTTP o usando **ping**. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
### <a name="141-plan-for-dns-server-requirements"></a>1.4.1 Planear los requisitos de servidores DNS  
Los siguientes son los requisitos de DNS cuando se implementa DirectAccess.  
  
-   Para los clientes de DirectAccess, debe usar un servidor DNS que se está ejecutando Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 o cualquier otro servidor DNS que admita IPv6.  
  
    > [!NOTE]  
    > No se recomienda que utilices los servidores DNS que ejecutan Windows Server 2003 al implementar DirectAccess. Aunque los servidores DNS de Windows Server 2003 admiten registros IPv6, Windows Server 2003 ya no es compatible con Microsoft. Además, no deberías implementar DirectAccess si los controladores de dominio ejecutan Windows Server 2003 debido a un problema con el servicio de replicación de archivos. Para obtener más información, consulte [configuraciones no admitidas de DirectAccess](https://technet.microsoft.com/library/dn464274.aspx).  
  
-   Usa un servidor DNS que admita actualizaciones dinámicas. Puedes usar servidores DNS que no admitan actualizaciones dinámicas, pero debes actualizar manualmente las entradas en esos servidores.  
  
-   El FQDN de los puntos de distribución CRL accesibles por Internet deben poder resolverse con servidores DNS de Internet. Por ejemplo, si URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> está en el **puntos de distribución CRL** campo del certificado IP-HTTPS del servidor de DirectAccess, debe asegurarse de que el FQDN CRL.contoso.com sea capaz de resolver usando servidores DNS de Internet.  
  
### <a name="142-plan-for-local-name-resolution"></a>1.4.2 Planear la resolución local de nombres  
A la hora de planear la resolución local de nombres, ten en cuenta los siguientes aspectos:  
  
**NRPT**  
  
Puede que tengas que crear reglas de NRPT adicionales en los siguientes casos:  
  
-   Si necesitas agregar más sufijos DNS para el espacio de nombres de la intranet.  
  
-   Si los nombres de dominio completos (FDQN) de los puntos de distribución CRL están basados en el espacio de nombres de la intranet, debes agregar reglas de exención para los FQDN de los puntos de distribución CRL.  
  
-   Si tienes un entorno DNS de cerebro dividido, debes agregar reglas de exención para los nombres de los recursos a cuya versión de Internet quieres que tengan acceso los clientes de DirectAccess situados en Internet, en lugar de la versión de la intranet.  
  
-   Si estás redirigiendo el tráfico a un sitio web externo a través de los servidores proxy web de la intranet, el sitio web externo solo está disponible desde la intranet y usa las direcciones de los servidores proxy web para permitir las solicitudes de entrada. En este caso, agrega una regla de exención para el FQDN del sitio web externo y especifica que la regla usa el servidor proxy web de la intranet en lugar de las direcciones IPv6 de los servidores DNS de la intranet.  
  
    Por ejemplo, si estás probando un sitio web externo llamado test.contoso.com, este nombre no se puede resolver mediante los servidores DNS de Internet, pero el servidor proxy web de Contoso sabe cómo resolver el nombre y dirigir las solicitudes para el sitio web al servidor web externo. Para impedir que los usuarios que no están en la intranet de Contoso tengan acceso al sitio, el sitio web externo solo admite solicitudes de la dirección de Internet IPv4 del proxy web de Contoso. Por tanto, los usuarios de la intranet pueden acceder al sitio web porque están usando el proxy web de Contoso, pero los usuarios de DirectAccess no pueden acceder al sitio porque no están usando el proxy web de Contoso. Configurar una regla de exención de NRPT para test.contoso.com que use el proxy web de Contoso permite que las solicitudes de páginas web para test.contoso.com se enruten al servidor proxy web de la intranet a través de Internet IPv4.  
  
**Nombres de etiqueta única**  
  
Único, como nombres de etiqueta, <https://paycheck>, a veces se usan para los servidores de intranet. Si se solicita un nombre de una sola etiqueta y hay configurada una lista de sufijos DNS, los sufijos DNS de la lista se anexarán al nombre de una sola etiqueta. Por ejemplo, cuando un usuario en un equipo que es miembro del dominio corp.contoso.com escribe <https://paycheck> en el explorador web, el FQDN que se construye como el nombre es paycheck.corp.contoso.com. De forma predeterminada, el sufijo anexado se basa en el sufijo DNS principal del equipo cliente.  
  
> [!NOTE]  
> En un escenario de espacio de nombres no contiguo (en el que uno o varios equipos del dominio tienen un sufijo DSN que no coincide con el dominio de Active Directory al que pertenecen los equipos), debes asegurarte de que la lista de búsqueda se personalice para incluir todos los sufijos necesarios. De forma predeterminada, el Asistente para acceso remoto configurará el nombre DNS de Active Directory como sufijo DNS principal en el cliente. Asegúrate de agregar el sufijo DNS que los clientes usan para la resolución de nombres.  
  
Si hay implementados varios dominios y Servicios de nombres de Internet de Windows (WINS) en tu organización y te estás conectando de forma remota, los nombres únicos se pueden resolver de la siguiente manera:  
  
-   Implementa una zona de búsqueda directa de WINS en el DNS. Al intentar resolver computername.dns.zone1.corp.contoso.com, la solicitud se dirige al servidor WINS que usa solo computername. El cliente piensa que está emitiendo un registro de recursos (A) de host DNS normal, pero en realidad es una solicitud NetBIOS. Para obtener más información, consulta Administrar una zona de búsqueda directa.  
  
-   Agrega un sufijo DNS, por ejemplo, dns.zone1.corp.contoso.com, al GPO de directiva de dominio predeterminado.  
  
**DNS de cerebro dividido**  
  
DNS de cerebro dividido es el uso del mismo dominio DNS tanto para la resolución de nombres de Internet como de la intranet.  
  
Las implementaciones DNS de cerebro dividido, debe enumerar los FQDN que se duplican en Internet e intranet y decidir qué recursos el cliente de DirectAccess debe alcance, la intranet o la versión de Internet. Para cada nombre correspondiente a un recurso a cuya versión de Internet quieres que tengan acceso los clientes de DirectAccess, debes agregar el FQDN correspondiente como regla de exención a la tabla NRPT para los clientes de DirectAccess.  
  
En un entorno de DNS de cerebro dividido, si quieres que estén disponibles ambas versiones del recurso, configura los recursos de la intranet con nombres alternativos que no sean duplicados de los nombres que se usan en Internet y pide a los usuarios que empleen el nombre alternativo cuando estén en la intranet. Por ejemplo, configura y usa el nombre alternativo www.internal.contoso.com para el nombre interno www.contoso.com.  
  
En un entorno DNS que no sea de cerebro dividido, el espacio de nombres de Internet es diferente del espacio de nombres de la intranet. Por ejemplo, Contoso Corporation usa contoso.com en Internet y corp.contoso.com en la intranet. Puesto que todos los recursos de la intranet usan el sufijo DNS corp.contoso.com, la regla de la tabla NRPT para corp.contoso.com enruta todas las consultas de nombres DNS para recursos de la intranet a servidores DNS de la intranet. Las consultas DNS de nombres que tienen el sufijo contoso.com no coinciden con la regla del espacio de nombres de la intranet corp.contoso.com de la tabla NRPT y se envían a servidores DNS de Internet. Con una implementación DNS que no sea de cerebro dividido, puesto que no hay ninguna duplicación de FQDN para los recursos de la intranet y de Internet, no es necesario realizar ninguna configuración adicional para la tabla NRPT. Los clientes de DirectAccess pueden tener acceso a los recursos de Internet y de la intranet de la organización.  
  
**Comportamiento de la resolución local de nombres para los clientes de DirectAccess**  
  
Si no se puede resolver un nombre DNS para resolver el nombre de la subred local, el servicio cliente DNS en Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8, y Windows 7 puede usar la resolución local, con el nombre de multidifusión Local de vínculo R esolution (LLMNR) y NetBIOS a través de protocolos TCP/IP.  
  
La resolución local de nombres suele ser necesaria para la conectividad punto a punto cuando el equipo se encuentra en redes privadas, como redes domésticas de una única subred. Cuando el servicio Cliente DNS realiza la resolución local de los nombres de servidores de la intranet y el equipo está conectado a una subred compartida en Internet, los usuarios malintencionados pueden capturar los mensajes de LLMNR y de NetBIOS sobre TCP/IP para determinar los nombres de los servidores de la intranet. En la página DNS del Asistente para configuración de DirectAccess se configura el comportamiento de la resolución local de nombres según los tipos de respuestas recibidos de los servidores DNS de la intranet. Están disponibles las opciones siguientes:  
  
-   **Usar la resolución local de nombres si el nombre no existe en DNS**. Esta opción es la más segura porque el cliente de DirectAccess realiza la resolución local solo para los nombres de servidor que los servidores DNS de la intranet no puedan resolver. Si se puede tener acceso a los servidores DNS de la intranet, se resuelven los nombres de los servidores de la intranet. Si los servidores DNS de la intranet no son accesibles o si hay otros tipos de errores de DNS, los nombres de los servidores de la intranet no se filtran a la subred a través de la resolución local de nombres.  
  
-   **Usar resolución local de nombres si el nombre no existe en DNS o los servidores de DNS no están disponibles cuando el equipo cliente está en una red privada (recomendado)** . Esta opción es la recomendada porque permite el uso de la resolución local de nombres en una red privada cuando los servidores DNS de la intranet no están disponibles.  
  
-   **Usar la resolución local de nombres para errores de resolución de DNS (menos restrictivo)** . Esta es la opción menos segura porque los nombres de los servidores de red de la intranet se pueden filtrar a la subred local a través de la resolución local de nombres.  
  
## <a name="15-plan-the-network-location-server"></a>1.5 Planear el servidor de ubicación de red  
El servidor de ubicación de red es un sitio web que se utiliza para detectar si los clientes de DirectAccess están ubicados en la red corporativa. Los clientes que están en la red corporativa no usan DirectAccess para acceder a los recursos internos; en su lugar, se conectan directamente.  
  
El sitio web del servidor de ubicación de red se puede hospedar en el servidor de DirectAccess o en otro servidor de la organización. Si hospedas el servidor de ubicación de red en el servidor de DirectAccess, el sitio web se crea automáticamente cuando instalas el rol de servidor Acceso remoto. Si hospedas el servidor de ubicación de red en otro servidor de la organización que ejecuta un sistema operativo Windows, debes asegurarte de que Internet Information Services (IIS) esté instalado en ese servidor y de que el sitio web se cree. DirectAccess no configura las opciones en un servidor de ubicación de red remoto.  
  
Asegúrate de que el sitio web del servidor de ubicación de red cumpla los siguientes requisitos:  
  
-   Es un sitio web con un certificado de servidor HTTPS.  
  
-   Los equipos cliente de DirectAccess deben confiar en la entidad de certificación que emitió el certificado de servidor para el sitio web del servidor de ubicación de red.  
  
-   Los equipos cliente de DirectAccess de la red interna deben poder resolver el nombre del sitio del servidor de ubicación de red.  
  
-   El sitio del servidor de ubicación de red debe tener una disponibilidad alta para los equipos de la red interna.  
  
-   El servidor de ubicación de red no debe ser accesible para los equipos cliente de DirectAccess en Internet.  
  
-   El certificado de servidor debe cotejarse con una CRL.  
  
### <a name="151-plan-certificates-for-the-network-location-server"></a>1.5.1 Planear certificados para el servidor de ubicación de red  
A la hora de obtener el certificado de sitio web para usarlo para el servidor de ubicación de red, ten en cuenta lo siguiente:  
  
1.  En el campo **Asunto**, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
2.  En el campo **Uso mejorado de clave**, usa el OID Autenticación de servidor.  
  
3.  En el campo **Puntos de distribución CRL**, usa un punto de distribución CRL que sea accesible para los clientes de DirectAccess que estén conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
### <a name="152-plan-dns-for-the-network-location-server"></a>1.5.2 Planear DNS para el servidor de ubicación de red  
Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben poder resolver el nombre del servidor de ubicación de red, pero debe impedirse que resuelvan el nombre cuando están ubicados en Internet. Para que esto suceda, de forma predeterminada se agrega el FQDN del servidor de ubicación de red como regla de exención a la tabla NRPT.  
  
## <a name="16-plan-management-servers"></a>1.6 Planear los servidores de administración  
Los clientes de DirectAccess inician las comunicaciones con los servidores de administración que proporcionan servicios como Windows Update y actualizaciones de antivirus. Los clientes de DirectAccess también usan el protocolo Kerberos para ponerse en contacto con los controladores de dominio para autenticarse antes de acceder a la red interna. Durante la administración remota de los clientes de DirectAccess, los servidores de administración se comunican con los equipos cliente para realizar funciones de administración, como evaluaciones de inventario de software o hardware. El acceso remoto puede detectar automáticamente algunos servidores de administración, entre otros:  
  
-   Dominio controladores-detección automática de controladores de dominio se realiza para todos los dominios en el mismo bosque que los equipos cliente y servidor de DirectAccess.  
  
-   System Center Configuration Manager servidores-detección automática de servidores de System Center Configuration Manager se realiza para todos los dominios en el mismo bosque que los equipos cliente y servidor de DirectAccess.  
  
Los controladores de dominio y los servidores de System Center Configuration Manager se detectan automáticamente la primera vez que se configura DirectAccess. Los controladores de dominio detectados no se muestran en la consola, pero se puede recuperar la configuración mediante el cmdlet de Windows PowerShell **Get-DAMgmtServer-Type All**. Si el controlador de dominio o los servidores de System Center Configuration Manager se modifican, al hacer clic en **Actualizar servidores de administración** en la Consola de administración de acceso remoto se actualiza la lista de servidores de administración.  
  
**Requisitos del servidor de administración**  
  
-   Los servidores de administración deben ser accesibles a través del primer túnel (infraestructura). Cuando se configura el acceso remoto, al agregar servidores a la lista de servidores de administración automáticamente son accesibles a través de este túnel.  
  
-   Los servidores de administración que inician conexiones con los clientes de DirectAccess deben ser totalmente compatibles con IPv6, mediante una dirección IPv6 nativa o mediante una asignada por ISATAP.  
  
## <a name="17-plan-active-directory-domain-services"></a>1.7 Planear Active Directory Domain Services  
En esta sección se explica cómo usa DirectAccess los Servicios de dominio de Active Directory (AD DS), e incluye las siguientes subsecciones:  
  
-   [1.7.1 planear la autenticación de cliente](#171-plan-client-authentication)  
  
-   [1.7.2 Planear varios dominios](#172-plan-multiple-domains)  
  
DirectAccess usa objetos de directiva de grupo de Active Directory y de AD DS (GPO) de la siguiente manera:  
  
-   **Autenticación**  
  
    AD DS se usa para autenticación. El túnel de infraestructura usa la autenticación NTLMv2 para la cuenta de equipo que se está conectando al servidor de DirectAccess, y la cuenta debe incluirse en un dominio de Active Directory. El túnel de intranet usa la autenticación Kerberos para que el usuario cree el segundo túnel.  
  
-   **Objetos de directiva de grupo**  
  
    DirectAccess recopila las opciones de configuración en los GPO que se aplican a los servidores, clientes y servidores de aplicaciones internos de DirectAccess.  
  
-   **Grupos de seguridad**  
  
    DirectAccess usa grupos de seguridad para recopilar e identificar equipos cliente de DirectAccess. Los GPO se aplican al grupo de seguridad necesario.  
  
-   **Directivas IPsec extendidas**  
  
    DirectAccess puede usar cifrado y autenticación IPsec entre los clientes y el servidor de DirectAccess. Puedes extender el cifrado y la autenticación IPsec desde el cliente a los servidores de aplicaciones internos especificados. Para ello, agrega los servidores de aplicaciones a un grupo de seguridad.  
  
**Requisitos de AD DS**  
  
A la hora de planear AD DS para una implementación de DirectAccess, ten en cuenta los requisitos siguientes:  
  
-   Al menos un controlador de dominio debe instalarse con el sistema operativo Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.  
  
    Si el controlador de dominio está en una red perimetral (y, por lo tanto, es accesible desde el adaptador de red con conexión a Internet del servidor de DirectAccess), debes impedir que el servidor de DirectAccess acceda a él; para ello, agrega filtros de paquetes al controlador de dominio para impedir que el adaptador de Internet conecte con la dirección IP.  
  
-   El servidor de DirectAccess debe ser miembro del dominio.  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los clientes pueden pertenecer a:  
  
    -   Cualquier dominio del mismo bosque que el servidor de DirectAccess.  
  
    -   Cualquier dominio que tenga una confianza bidireccional con el dominio del servidor de DirectAccess.  
  
    -   Cualquier dominio de un bosque que tenga una confianza bidireccional con el bosque al que pertenece el dominio de DirectAccess.  
  
> [!NOTE]  
> -   El servidor de DirectAccess no puede ser un controlador de dominio.  
> -   El controlador de dominio de AD DS que se usa para DirectAccess no debe ser accesible desde el adaptador de Internet externo del servidor de DirectAccess (es decir, el adaptador no debe estar en el perfil de dominio del Firewall de Windows).  
  
### <a name="171-plan-client-authentication"></a>1.7.1 Planear la autenticación de clientes  
DirectAccess permite elegir entre usar certificados para la autenticación de equipos IPsec o usar un proxy Kerberos integrado que autentique mediante nombres de usuario y contraseñas.  
  
Si se decide usar credenciales de AD DS para la autenticación, DirectAccess usa un túnel de seguridad que usa Equipo Kerberos para la primera autenticación y Usuario Kerberos para la segunda. Con este modo de autenticación, DirectAccess usa un solo túnel de seguridad que proporciona acceso al servidor DNS, al controlador de dominio y a los demás servidores de la red interna.  
  
Si se elige usar una autenticación de dos factores o Protección de acceso a redes, DirectAccess usa dos túneles de seguridad. El Asistente para configuración de acceso remoto configura reglas de seguridad de conexión de Firewall de Windows con seguridad avanzada que especifican el uso de los siguientes tipos de credenciales al negociar las asociaciones de seguridad IPsec para los túneles al servidor de DirectAccess:  
  
-   El túnel de infraestructura usa credenciales Equipo Kerberos para la primera autenticación y Usuario Kerberos para la segunda autenticación.  
  
-   El túnel de intranet usa credenciales de certificado de equipo para la primera autenticación y Usuario Kerberos para la segunda autenticación.  
  
Cuando DirectAccess está decidiendo si permitir el acceso a los clientes que ejecutan Windows 7 o en una implementación multisitio, usa dos túneles de seguridad. El Asistente para configuración de acceso remoto configura reglas de seguridad de conexión de Firewall de Windows con seguridad avanzada que especifican el uso de los siguientes tipos de credenciales al negociar las asociaciones de seguridad IPsec para los túneles al servidor de DirectAccess:  
  
-   El túnel de infraestructura usa credenciales de certificado de equipo para la primera autenticación y NTLMv2 para la segunda autenticación. Las credenciales NTLMv2 obligan a usar el protocolo de Internet autenticado (AuthIP) y proporcionan acceso a un servidor DNS y al controlador de dominio antes de que el cliente de DirectAccess pueda usar credenciales Kerberos para el túnel de intranet.  
  
-   El túnel de intranet usa credenciales de certificado de equipo para la primera autenticación y Usuario Kerberos para la segunda autenticación.  
  
### <a name="172-plan-multiple-domains"></a>1.7.2 Planear varios dominios  
La lista de servidores de administración debe incluir controladores de dominio de todos los dominios que contengan grupos de seguridad que incluyan equipos cliente de DirectAccess. Debe contener todos los dominios que contengan cuentas de usuario que pudieran usar equipos que están configurados como clientes de DirectAccess. Esto garantiza que los usuarios que no están ubicados en el mismo dominio que el equipo cliente que están usando se autentican con un controlador de dominio en el dominio del usuario. Esto se hace automáticamente si los dominios están en el mismo bosque.  
  
> [!NOTE]  
> Si en los grupos de seguridad hay equipos que se usan para los equipos cliente o los servidores de aplicaciones en diferentes bosques, los controladores de dominio de esos bosques no se detectan automáticamente. Puedes ejecutar la tarea **Actualizar servidores de administración** en la Consola de administración de acceso remoto para detectar estos controladores de dominio.  
  
Cuando sea posible, se deben agregar sufijos de nombres de dominio comunes a la tabla de directivas de resolución de nombres (NRPT) durante la implementación de acceso remoto. Por ejemplo, si tienes dos dominios, domain1.corp.contoso.com y domain2.corp.contoso.com, en lugar de agregar dos entradas a la tabla NRPT, puedes agregar una entrada de sufijo DNS común, con el sufijo de nombre de dominio corp.contoso.com. Esto se produce automáticamente para los dominios de la misma raíz, pero los dominios que no están en la misma raíz deben agregarse manualmente.  
  
Si el Servicio de nombres de Internet de Windows (WINS) se implementa en un entorno de varios dominios, debe implementar una zona de búsqueda directa de WINS en DNS. Para obtener más información, consulte **único de los nombres de etiqueta** en el [1.4.2 Planear la resolución de nombres local](#142-plan-for-local-name-resolution) sección anteriormente en este documento.  
  
## <a name="18-plan-group-policy-objects"></a>1.8 Planear objetos de directiva de grupo  
En esta sección se explica el rol de los objetos de directiva de grupo (GPO) en la infraestructura de acceso remoto, e incluye las siguientes subsecciones:  
  
-   [1.8.1 configurar GPO creados automáticamente](#181-configure-automatically-created-gpos)  
  
-   [1.8.2 configurar GPO creados manualmente](#182-configure-manually-created-gpos)  
  
-   [1.8.3 administrar GPO en un entorno de controladores multidominio](#183-manage-gpos-in-a-multi-domain-controller-environment)  
  
-   [1.8.4 administrar GPO de acceso remoto con permisos limitados](#184-manage-remote-access-gpos-with-limited-permissions)  
  
-   [1.8.5 recuperación de un GPO eliminado](#185-recover-from-a-deleted-gpo)  
  
Las opciones de DirectAccess que se configuran al configurar el acceso remoto se recopilan en GPO. Los siguientes tipos de GPO se rellenan con las opciones de configuración de DirectAccess, y se distribuyen como sigue:  
  
-   **GPO de cliente de DirectAccess**  
  
    Este GPO contiene las opciones de configuración de clientes, incluida la configuración de las tecnologías de transición IPv6, las entradas de la tabla NRPT y las reglas de seguridad de Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad que se especifican para los equipos cliente.  
  
-   **GPO de servidor de DirectAccess**  
  
    Este GPO contiene las opciones de configuración de DirectAccess que se aplican a los servidores configurados como servidor de DirectAccess en tu implementación. Asimismo, contiene las reglas de seguridad de conexión del Firewall de Windows con seguridad avanzada.  
  
-   **GPO de servidores de aplicaciones**  
  
    Este GPO contiene la configuración de los servidores de aplicación seleccionados a los que opcionalmente extiendes la autenticación y el cifrado de los clientes de DirectAccess. Si la autenticación y el cifrado no están extendidos, no se usa este GPO.  
  
Los GPO se pueden configurar de dos maneras:  
  
-   **Automáticamente**-puede especificar que se crean automáticamente. Se especifica un nombre predeterminado para cada GPO.  
  
-   **Manualmente**: puede usar los GPO que predefinió el Administrador de Active Directory.  
  
> [!NOTE]  
> Después de configurar DirectAccess para que use unos GPO específicos, no se puede configurar para que use otros GPO.  
  
Tanto si usas GPO configurados manual o automáticamente, tienes que agregar una directiva para la detección de vínculos de baja velocidad si tus clientes van a usar redes 3G. La ruta de acceso **directiva: Configurar la detección de vínculo de baja velocidad de directiva de grupo** es: **Equipo configuración/Políticas/Plantillas administrativas/Sistema/Directiva de grupo**.  
  
> [!CAUTION]  
> Usa el procedimiento siguiente para hacer una copia de seguridad de todos los GPO de acceso remoto antes de ejecutar los cmdlets de DirectAccess: [Copia de seguridad y restaurar la configuración de acceso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
Si los GPO de vinculación no cuentan con los permisos correctos (que se indican en las siguientes secciones), se emite una advertencia. La operación de acceso remoto continuará pero no se producirá la vinculación. Si aparece esta advertencia, los vínculos no se crearán automáticamente, aunque los permisos se agreguen más tarde. En su lugar, el administrador tiene que crear los vínculos manualmente.  
  
### <a name="181-configure-automatically-created-gpos"></a>1.8.1 Configurar GPO creados automáticamente  
Ten en cuenta lo siguiente cuando uses GPO creados automáticamente.  
  
Los GPO creados automáticamente se aplican según los parámetros de ubicación y destino del vínculo, de la siguiente manera:  
  
-   Para el GPO de servidor de DirectAccess, el parámetro de ubicación y el parámetro de vínculo apuntan al dominio que contiene el servidor de DirectAccess.  
  
-   Cuando se crean GPO de servidor de aplicaciones y de cliente, la ubicación se establece en un único dominio en el que se creará el GPO. El nombre del GPO se busca en todos los dominios y se rellena con la configuración de DirectAccess, si existe. El destino del vínculo se establece en la raíz del dominio donde se creó el GPO. Se crea un GPO para cada dominio que contiene equipos cliente o servidores de aplicación, y el GPO se vincula a la raíz de su dominio respectivo.  
  
Cuando se usan GPO creados automáticamente para aplicar la configuración de DirectAccess, el administrador de acceso remoto requiere los siguientes permisos:  
  
-   Permisos de creación de GPO en todos los dominios  
  
-   Permisos de vinculación para todas las raíces de dominio de cliente seleccionadas  
  
-   Permisos de vinculación para todas las raíces de dominio de GPO de servidor  
  
Además, se necesitan los siguientes permisos:  
  
-   Los GPO necesitan los permisos de seguridad Crear, Editar, Eliminar y Modificar.  
  
-   Es recomendable que el administrador de acceso remoto tenga permisos de lectura en los GPO en todos los dominios necesarios. Esto permite que el acceso remoto compruebe que no haya GPO con nombres duplicados al crearlos.  
  
### <a name="182-configure-manually-created-gpos"></a>1.8.2 Configurar GPO creados manualmente  
Ten en cuenta lo siguiente cuando uses GPO creados manualmente:  
  
-   Los GPO deben existir antes de ejecutar el Asistente para instalación de acceso remoto.  
  
-   Para aplicar la configuración de DirectAccess, el administrador de acceso remoto necesita permisos totales (permisos de seguridad Editar, Eliminar, Modificar) en los GPO creados manualmente.  
  
-   Se busca en todo el dominio un vínculo al GPO. Si no hay un vínculo al GPO en el dominio, se crea uno automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
### <a name="183-manage-gpos-in-a-multi-domain-controller-environment"></a>1.8.3 Administrar GPO en un entorno de controladores multidominio  
Cada GPO es administrado por un controlador de dominio específico, de la siguiente manera:  
  
-   El GPO de servidor es administrado por uno de los controladores de dominio en el sitio de Active Directory que está asociado con el servidor. Si los controladores de dominio del sitio son de solo lectura, el GPO del servidor es administrado por el controlador de dominio habilitado para escritura que esté más próximo al servidor de DirectAccess.  
  
-   Los GPO de cliente y de servidor de aplicaciones son administrados por el controlador de dominio que se ejecuta como controlador de dominio principal (PDC).  
  
Si quieres modificar manualmente la configuración de los GPO, ten en cuenta lo siguiente:  
  
-   Para el GPO de servidor, para identificar qué controlador de dominio está asociado con el servidor de DirectAccess, en un símbolo del sistema con privilegios elevados del servidor de DirectAccess, ejecuta **nltest /dsgetdc: /writable**.  
  
-   De forma predeterminada, cuando se realizan cambios con los cmdlets de red de Windows PowerShell o se realizan cambios desde la consola de Administración de directivas de grupo, se usa el controlador de dominio que actúa como PDC.  
  
Además, si modificas la configuración en un controlador de dominio que no es el controlador de dominio asociado con el servidor de DirectAccess (para el GPO de servidor) o el PDC (para los GPO de cliente y de servidor de aplicaciones), ten en cuenta lo siguiente:  
  
-   Antes de modificar la configuración, asegúrate de que el controlador de dominio se replica con un GPO actualizado, y haz un copia de seguridad de la configuración de tu GPO. Para obtener más información, consulte [copia de seguridad y restaurar la configuración de acceso remoto](https://go.microsoft.com/fwlink/?LinkID=257928). Si el GPO no está actualizado, se podrían producir conflictos de combinación durante la replicación que podrían dañar la configuración de acceso remoto.  
  
-   Después de modificar la configuración, tienes que esperar a que los cambios se repliquen a los controladores de dominio que están asociados con los GPO. No hagas más cambios con la consola de Administración de acceso remoto ni con los cmdlets de acceso remoto de PowerShell hasta que la replicación haya terminado. Si se edita un GPO en dos controladores de dominio antes de que termine la replicación, se podrían producir conflictos de combinación y dañar la configuración de acceso remoto.  
  
También puedes cambiar la configuración predeterminada en el cuadro de diálogo **Cambiar el controlador de dominio**, en la consola de Administración de directivas de grupo, o con el cmdlet de Windows PowerShell **Open-NetGPO**, para que los cambios usen el controlador de dominio que especifiques.  
  
-   Para hacerlo en la consola de Administración de directivas de grupo, haz clic con el botón derecho en el dominio o en el contenedor de sitios y haz clic en **Cambiar el controlador de dominio**.  
  
-   Para hacerlo en Windows PowerShell, especifica el parámetro **DomainController** para el cmdlet **Open-NetGPO**. Por ejemplo, para habilitar perfiles públicos y privados en Firewall de Windows en un GPO llamado domain1\DA_Server_GPO _Europe usando un controlador de dominio llamado europe-dc.corp.contoso.com, escribe lo siguiente:  
  
    ```powershell
    $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
    Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
    Save-NetGPO -GpoSession $gpoSession  
    ```  
  
### <a name="184-manage-remote-access-gpos-with-limited-permissions"></a>1.8.4 Administrar GPO de acceso remoto con permisos limitados  
Para administrar una implementación de acceso remoto, el administrador de acceso remoto necesita permisos de GPO completos (permisos de seguridad Leer, Editar, Eliminar y Modificar) en los GPO que se usan en la implementación. El motivo es que la consola de Administración de acceso remoto y los módulos de acceso remoto de PowerShell leen y escriben la configuración en los GPO de acceso remoto (es decir, GPO de cliente, de servidor y de servidor de aplicaciones).  
  
En muchas organizaciones, el administrador del dominio que está a cargo de las operaciones de GPO no es la misma persona que el administrador de acceso remoto que está a cargo de la configuración de acceso remoto. Puede que estas organizaciones tengan directivas que impidan que el administrador de acceso remoto tenga permisos completos en los GPO del dominio. Es posible que el administrador del dominio tenga que revisar también la configuración de la directiva antes de aplicarla a los equipos del dominio.  
  
Para satisfacer estas necesidades, el administrador del dominio debe crear dos copias de cada GPO: de almacenamiento provisional y de producción. El administrador de acceso remoto tiene permisos completos en los GPO de almacenamiento provisional. En la consola de Administración de acceso remoto y en los cmdlets de Windows PowerShell, el administrador de acceso remoto especifica los GPO de almacenamiento provisional, como los GPO que se usan para la implementación de acceso remoto. Esto permite al administrador de acceso remoto leer y modificar la configuración de acceso remoto como y cuando sea necesario.  
  
El administrador del dominio debe asegurarse de que los GPO de almacenamiento provisional no estén vinculados a ningún ámbito de administración en el dominio y de que el administrador de acceso remoto no tenga permisos de vinculación de GPO en el dominio. Así se garantiza que los cambios que realice el administrador de acceso remoto a los GPO de almacenamiento provisional no afecten a los equipos del dominio.  
  
El administrador del dominio vincula los GPO de producción con el ámbito de administración requerido y aplica los filtros de seguridad apropiados. Esto garantiza que los cambios que se realicen en estos GPO se aplican a los equipos del dominio (equipos cliente, servidores de DirectAccess y servidores de aplicación). El administrador de acceso remoto no tiene permisos en los GPO de producción.  
  
Cuando se realizan cambios en los GPO de almacenamiento provisional, el administrador del dominio puede revisar la configuración de la directiva de estos GPO para asegurarse de que satisface los requisitos de seguridad de la organización. Después, el administrador del dominio exporta la configuración de los GPO de almacenamiento provisional usando la característica de copia de seguridad, e importa la configuración a los GPO de producción correspondientes, que se aplicarán a los equipos del dominio.  
  
En el siguiente diagrama se ilustra esta configuración.  
  
![Administrar GPO de acceso remoto](../../../media/Step-1-Plan-the-DirectAccess-Infrastructure/DA_Plan_Advanced_Step1_GPOS.png)  
  
### <a name="185-recover-from-a-deleted-gpo"></a>1.8.5 Recuperación de un GPO eliminado  
Si un GPO de cliente, de servidor de DirectAccess o de servidor de aplicaciones se elimina accidentalmente y no hay una copia de seguridad disponible, tienes que quitar la configuración y volver a configurarlo. Si hay una copia de seguridad disponible, puedes usarla para restaurar el GPO.  
  
La consola de Administración de acceso remoto mostrará el siguiente mensaje de error: **No se puede encontrar el GPO (nombre del GPO)** . Para quitar las opciones de configuración, sigue estos pasos:  
  
1.  Ejecuta el cmdlet de Windows PowerShell **Uninstall-remoteaccess**.  
  
2.  Abre la consola de Administración de acceso remoto.  
  
3.  Verás un mensaje de error que indica que no se encontró el GPO. Haz clic en **Quitar opciones de configuración**. Cuando se complete la operación, el servidor se restaurará a un estado sin configurar.  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   [Paso 2: Planear las implementaciones de DirectAccess](da-adv-plan-s2-deployments.md)  
  


