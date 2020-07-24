---
title: Paso 1 configurar la infraestructura de DirectAccess avanzada
description: Este tema forma parte de la guía implementar un único servidor de DirectAccess con configuración avanzada para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 43abc30a-300d-4752-b845-10a6b9f32244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d90005faeb16d81294b498c53cb5119b855f5294
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959297"
---
# <a name="step-1-configure-advanced-directaccess-infrastructure"></a>Paso 1 configurar la infraestructura de DirectAccess avanzada

>Se aplica a: Windows Server 2012 R2, Windows Server 2012

En este tema aprenderás a configurar la infraestructura necesaria para una implementación de acceso remoto avanzado que use un único servidor de DirectAccess en un entorno donde se combinan IPv4 e IPv6. Antes de comenzar con los pasos de implementación, asegúrese de que ha completado los pasos de planeación que se describen en [planear una implementación de DirectAccess avanzada](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Tarea|Descripción|  
|----|--------|  
|1.1 Configurar las opciones de red del servidor|Configura las opciones de red del servidor en el servidor de DirectAccess.|  
|1.2 Configurar el túnel forzado|Configura el túnel forzado.|  
|1.3 Configurar el enrutamiento en la red corporativa|Configura el enrutamiento en la red corporativa.|  
|1.4 Configurar los firewalls|Configure los firewalls adicionales, si es necesario.|  
|1.5 Configurar las entidades de certificación y los certificados|Si es necesario, configura una entidad de certificación (CA), así como el resto de plantillas de certificado necesarias para la implementación.|  
|1.6 Configurar el servidor DNS|Configura las opciones del sistema de nombres de dominio (DNS) para el servidor de DirectAccess.|  
|1.7 Configurar Active Directory|Une los equipos cliente y el servidor de DirectAccess al dominio de Active Directory.|  
|1.8 Configurar los GPO|Configura los GPO para la implementación, si es necesario.|  
|1.9 Configurar los grupos de seguridad|Configura los grupos de seguridad que contendrán los equipos cliente de DirectAccess, así como otros grupos de seguridad necesarios para la implementación.|  
|1.10 Configurar el servidor de ubicación de red|Configura el servidor de ubicación de red y, además, instala el certificado de sitio web del servidor de ubicación de red.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulte [Uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="11-configure-server-network-settings"></a><a name="ConfigNetworkSettings"></a>1.1 Configurar las opciones de red del servidor  
Las siguientes opciones de configuración de interfaz de red son obligatorias para una implementación de un solo servidor en un entorno que use IPv4 e IPv6. Todas las direcciones IP se configuran mediante **Cambiar configuración del adaptador** en el **Centro de redes y recursos compartidos de Windows**.  
  
**Topología perimetral**  
  
-   Dos direcciones IPv4 o IPv6 que sean estáticas, públicas, consecutivas y accesibles desde Internet  
  
    > [!NOTE]  
    > Teredo requiere dos direcciones públicas. Si no usas Teredo, puedes configurar una sola dirección IPv4 estática y pública.  
  
-   Una sola dirección IPv4 o IPv6 estática e interna  
  
**Detrás de un dispositivo NAT (con dos adaptadores de red)**  
  
-   Una sola dirección IPv4 o IPv6 estática y accesible desde Internet  
  
-   Una sola dirección IPv4 o IPv6 estática, interna y accesible desde la red  
  
**Detrás de un dispositivo NAT (con un adaptador de red)**  
  
-   Una sola dirección IPv4 o IPv6 estática, interna y accesible desde la red  
  
> [!NOTE]  
> Si configuras un servidor de DirectAccess con dos o más adaptadores de red (uno clasificado en el perfil de dominio y otro en un perfil público o privado) con una sola topología de adaptador de red, haz lo siguiente:  
>   
> -   Comprueba que el segundo adaptador de red y otros adaptadores de red adicionales estén clasificados en el perfil de dominio.  
> -   Si el segundo adaptador de red no se puede configurar para el perfil de dominio, la directiva IPsec de DirectAccess debe tener el ámbito manual para todos los perfiles mediante el siguiente comando de Windows PowerShell después de configurar DirectAccess:  
>   
>     ```  
>     $gposession = Open-NetGPO "PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule "DisplayName <Name of the IPsec policy> "GPOSession $gposession "Profile Any  
>     Save-NetGPO "GPOSession $gposession  
>     ```  
  
## <a name="12-configure-force-tunneling"></a><a name="BKMK_forcetunnel"></a>1.2 Configurar el túnel forzado  
El túnel forzado se puede configurar mediante el Asistente para la instalación de acceso remoto. Se presenta como una casilla en el Asistente para la configuración de clientes remotos. Esta opción solo afecta a los clientes de DirectAccess. Si se habilita VPN, los clientes VPN usarán el túnel forzado de manera predeterminada. Los administradores pueden cambiar la opción para clientes VPN desde el perfil de cliente.  
  
Al seleccionar la casilla de túnel forzado:  
  
-   Se habilita el túnel forzado en clientes de DirectAccess  
  
-   Se agrega la entrada **Any** en la tabla de directivas de resolución de nombres (NRPT) para clientes de DirectAccess; de esta forma, todo el tráfico DNS será dirigido a los servidores DNS de la red interna  
  
-   Configura los clientes de DirectAccess para que siempre usen la tecnología de transición IP-HTTPS  
  
Para que los recursos de Internet estén disponibles para los clientes de DirectAccess que usen túnel forzado, puedes usar un servidor proxy para que reciba las solicitudes basadas en IPv6 para recursos de Internet y las convierta en solicitudes de recursos de Internet basados en IPv4. Para configurar un servidor proxy para recursos de Internet, tienes que modificar la entrada predeterminada en NRPT para agregar el servidor proxy. Para ello, puedes usar los cmdlets de PowerShell de acceso remoto o los cmdlets de PowerShell de DNS. Por ejemplo, haz lo siguiente para usar el cmdlet de PowerShell de acceso remoto:  
  
```  
Set-DAClientDNSConfiguration "DNSSuffix "." "ProxyServer <Name of the proxy server:port>  
```  
  
> [!NOTE]  
> Si DirectAccess y VPN están habilitados en el mismo servidor, VPN está en el modo de túnel forzado y el servidor está implementado en una topología perimetral o detrás de una topología NAT (con dos adaptadores de red, uno conectado al dominio y otro a una red privada), el tráfico de Internet a través de VPN no se puede reenviar a través de la interfaz externa del servidor de DirectAccess. En este escenario, las organizaciones deben implementar acceso remoto en el servidor protegido por un firewall en una topología de un solo adaptador de red. Como alternativa, las organizaciones pueden usar un servidor proxy separado en la red interna para reenviar el tráfico de Internet de clientes VPN.  
  
> [!NOTE]  
> Si una organización usa un proxy web para que los clientes de DirectAccess accedan a recursos de Internet, y el proxy corporativo no puede administrar los recursos de la red interna, los clientes de DirectAccess no podrán acceder a los recursos internos si se encuentran fuera de la intranet. En dicho escenario, para que los clientes de DirectAccess puedan acceder a recursos internos, necesitarás crear, de forma manual, entradas de NRPT para los sufijos de la red interna mediante la página DNS del asistente para infraestructuras. No apliques la configuración de proxy en estos sufijos de NRPT. Los sufijos deben completarse con las entradas predeterminadas del servidor DNS.  
  
## <a name="13-configure-routing-in-the-corporate-network"></a><a name="ConfigRouting"></a>1.3 Configurar el enrutamiento en la red corporativa  
Configura el enrutamiento en la red corporativa de la siguiente manera:  
  
-   Después de implementar la IPv6 nativa en la organización, agrega una ruta para que los enrutadores de la red interna redirijan el tráfico IPv6 al servidor de DirectAccess.  
  
-   Configure manualmente las rutas IPv4 e IPv6 de la organización en los servidores de DirectAccess. Agrega una ruta publicada para que todo el tráfico con un prefijo IPv6 (/48) de organización se reenvíe a la red interna. Para el tráfico IPv4, agrega rutas explícitas para que el tráfico IPv4 se reenvíe a la red interna.  
  
## <a name="14-configure-firewalls"></a><a name="ConfigFirewalls"></a>1.4 Configurar los firewalls  
Si usas firewalls adicionales en la implementación, aplica las siguientes excepciones del firewall accesible desde Internet para el tráfico de acceso remoto cuando el servidor de DirectAccess se encuentre en Internet IPv4:  
  
-   Tráfico Teredo: Puerto de destino 3544 del Protocolo de datagramas de usuario (UDP) de entrada y puerto de origen UDP 3544 de salida.  
  
-   tráfico 6to4: protocolo IP 41 entrante y saliente.  
  
-   IP-HTTPS "Puerto de destino del Protocolo de control de transmisión (TCP) 443 y puerto de origen TCP 443 de salida. Si el servidor de DirectAccess tiene un único adaptador de red y el servidor de ubicación de red se encuentra en el servidor de DirectAccess, el puerto TCP 62000 también es obligatorio.  
  
    > [!NOTE]  
    > Esta exención se debe configurar en el servidor de DirectAccess, y todas las demás en el firewall perimetral.  
  
> [!NOTE]  
> Para el tráfico de Teredo y 6to4, estas excepciones se deben aplicar para las dos direcciones IPv4 públicas, consecutivas y accesibles desde Internet del servidor de DirectAccess. Para IP-HTTPS, las excepciones solo se deben aplicar a la dirección en la que se resuelve el nombre público del servidor.  
  
Si usas firewalls adicionales, aplica las siguientes excepciones del firewall accesible desde Internet para el tráfico de acceso remoto cuando el servidor de DirectAccess se encuentre en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
-   Tráfico entrante y saliente del Protocolo de mensajes de control de Internet para IPv6 (ICMPv6) solo para implementaciones de Teredo.  
  
Si usa firewalls adicionales, aplique las siguientes excepciones de firewall de la red interna para el tráfico de acceso remoto:  
  
-   ISATAP "Protocolo 41 entrante y saliente  
  
-   TCP/UDP para todo el tráfico IPv4/IPv6  
  
-   ICMP para todo el tráfico IPv4/IPv6  
  
## <a name="15-configure-cas-and-certificates"></a><a name="ConfigCAs"></a>1.5 Configurar las entidades de certificación y los certificados  
El acceso remoto en Windows Server 2012 le permite elegir entre usar certificados para la autenticación de equipos o usar un proxy Kerberos integrado que se autentique mediante nombres de usuario y contraseñas. Además, también deberás configurar un certificado IP-HTTPS en el servidor de DirectAccess.  
  
Para obtener más información, consulte [Active Directory servicios de Certificate Server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10)).  
  
### <a name="151-configure-ipsec-authentication"></a>1.5.1 Configurar la autenticación IPsec  
Para usar la autenticación IPsec debes instalar un certificado de equipo en el servidor de DirectAccess y en todos los clientes de DirectAccess. El certificado debe ser emitido por una entidad de certificación (CA) interna y, además, los servidores de DirectAccess y los clientes de DirectAccess deben confiar en la cadena de CA que emite los certificados raíz e intermedio.  
  
##### <a name="to-configure-ipsec-authentication"></a>Para configurar la autenticación IPsec  
  
1.  En la CA interna, decida si usará la plantilla de certificado **Equipo** o si creará una plantilla de certificado nueva, como se describe en [Creación de plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10)).  
  
    > [!NOTE]  
    > Si creas una plantilla nueva, debe configurarse para la autenticación de cliente.  
  
2.  Si es necesario, implementa la plantilla de certificado. Para obtener más información, consulte [Implementación de plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10)).  
  
3.  Si es necesario, configura la plantilla de certificado para inscripción automática. Para obtener más información, consulte [Configurar inscripción automática de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731522(v=ws.11)).  
  
### <a name="152-configure-certificate-templates"></a><a name="ConfigCertTemp"></a>1.5.2 Configurar las plantillas de certificado  
Cuando uses una CA interna para emitir certificados, debes configurar una plantilla de certificado para el certificado IP-HTTPS y el certificado del sitio web del servidor de ubicación de red.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar una plantilla de certificado  
  
1.  En la CA interna, cree una plantilla de certificado del modo descrito en [Creación de plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10)).  
  
2.  Implemente la plantilla de certificado según se indica en el tema sobre [implementación de plantillas de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10)).  
  
### <a name="153-configure-the-ip-https-certificate"></a>1.5.3 Configurar el certificado IP-HTTPS  
Acceso remoto requiere un certificado IP-HTTPS para autenticar conexiones IP-HTTPS al servidor de DirectAccess. Hay tres opciones de certificado disponibles para la autenticación IP-HTTPS:  
  
**Certificado público**  
  
Los certificados públicos son suministrados por terceros. Si el nombre de sujeto del certificado no contiene caracteres comodín, deberás usar la URL del nombre de dominio completo (FQDN) que pueda resolverse externamente y que se use únicamente para las conexiones IP-HTTPS del servidor de DirectAccess.  
  
**Certificado privado**  
  
Para usar un certificado privado, necesitarás lo siguiente (si no existe ya):  
  
-   Un certificado de sitio web que se use para autenticación IP-HTTPS. El sujeto del certificado debe ser un FQDN que pueda resolverse externamente y accesible desde Internet. El certificado se basa en la plantilla de certificado que creó siguiendo las instrucciones de 1.5.2 configurar plantillas de certificado.  
  
-   Un punto de distribución de lista de revocación de certificados (CRL) que sea accesible desde un FQDN que pueda resolverse públicamente.  
  
**Certificado autofirmado**  
  
Para usar un certificado autofirmado, necesitarás lo siguiente (si no existe ya):  
  
-   Un certificado de sitio web que se use para autenticación IP-HTTPS. El sujeto del certificado debe ser un FQDN que pueda resolverse externamente y accesible desde Internet.  
  
-   Un punto de distribución CRL que sea accesible desde un FQDN que pueda resolverse públicamente.  
  
> [!NOTE]  
> Los certificados autofirmados no pueden usarse en implementaciones multisitio.  
  
Comprueba que el certificado de sitio web usado para la autenticación IP-HTTPS cumpla con estos requisitos:  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
-   En el campo **Asunto**, especifique el FQDN de la dirección URL de IP-HTTPS.  
  
-   En el campo **Uso mejorado de clave**, usa el identificador de objeto (OID) de autenticación de servidor.  
  
-   En el campo **Puntos de distribución CRL**, especifique un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
-   Los certificados IP-HTTPS pueden contener caracteres comodín en el nombre.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Cómo instalar el certificado IP-HTTPS desde una CA interna  
  
1.  En el servidor de DirectAccess: en la pantalla **Inicio** , escriba**mmc.exe**y, a continuación, presione Entrar.  
  
2.  En el menú **Archivo** de la consola MMC, haga clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, en **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, en **Finalizar** y, por último, en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abre **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , active la casilla de la plantilla de certificado que creó anteriormente (para obtener más información, consulte 1.5.2 configurar plantillas de certificado). Si es necesario, haz clic en **Se necesita más información para inscribir este certificado**.  
  
8.  En el cuadro de diálogo **Propiedades de certificado**, en la pestaña **Sujeto**, en el área **Nombre de sujeto**, en **Tipo**, selecciona **Nombre común**.  
  
9. En **Valor**, especifica la dirección IPv4 del adaptador accesible desde el exterior del servidor de DirectAccess o del FQDN de la URL de IP-HTTPS y, después, haz clic en **Agregar**.  
  
10. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
11. En **Valor**, especifica la dirección IPv4 del adaptador accesible desde el exterior del servidor de DirectAccess o del FQDN de la URL de IP-HTTPS y, después, haz clic en **Agregar**.  
  
12. En la pestaña **General**, en **Nombre descriptivo**, puedes escribir un nombre que te ayude a identificar el certificado.  
  
13. En la pestaña **Extensiones**, haz clic en la flecha junto a **Uso mejorado de clave** y comprueba que **Autenticación de servidor** aparezca en la lista **Opciones seleccionadas**.  
  
14. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
15. En el panel de detalles del complemento Certificados, comprueba que el nuevo certificado se ha inscrito con propósitos planteados de autenticación de servidor.  
  
## <a name="16-configure-the-dns-server"></a><a name="ConfigDNS"></a>1.6 Configurar el servidor DNS  
Debes configurar manualmente una entrada DNS para el sitio web del servidor de ubicación de red para la red interna de tu implementación.  
  
### <a name="to-create-the-network-location-server"></a><a name="NLS_DNS"></a>Para crear el servidor de ubicación de red  
  
1.  En el servidor DNS de la red interna: en la pantalla **Inicio** , escriba**DNSMgmt. msc**y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo de la consola del **Administrador del DNS**, expande la zona de búsqueda directa de tu dominio. Haz clic con el botón secundario en el dominio y selecciona **Host nuevo (A o AAAA)**.  
  
3.  En el cuadro de diálogo **Host nuevo**, en el cuadro **Dirección IP**:  
  
    -   En el cuadro **Nombre (si se deja en blanco, se usa el nombre del dominio primario)**, escribe el nombre DNS del sitio web del servidor de ubicación de red (es decir, el nombre que los clientes de DirectAccess usarán para conectarse al servidor de ubicación de red).  
  
    -   Escribe la dirección IPv4 o IPv6 del servidor de ubicación de red, haz clic en **Agregar host** y, después, en **Aceptar**.  
  
4.  En el cuadro de diálogo **Host nuevo**:  
  
    -   En el cuadro **Nombre (si se deja en blanco, se usa el nombre del dominio primario)**, escribe el nombre DNS del sondeo web (el nombre del sondeo web predeterminado es **directaccess-webprobehost**).  
  
    -   En el cuadro **Dirección IP**, escribe la dirección IPv4 o IPv6 del sondeo web y, después, haz clic en **Agregar host**.  
  
    -   Repite este proceso para **directaccess-corpconnectivityhost** y para los comprobadores de conectividad creados de forma manual.  
  
5.  En el cuadro de diálogo **DNS**, haz clic en **Aceptar** y, después, en **Listo**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
También debes configurar entradas DNS para los siguiente:  
  
-   **Servidor IP-HTTPS**  
  
    Los clientes de DirectAccess han de ser capaces de resolver el nombre DNS del servidor de DirectAccess desde Internet.  
  
-   **Comprobación de la revocación de CRL**  
  
    DirectAccess usa la comprobación de revocación de certificados para la conexión IP-HTTPS entre los clientes de DirectAccess y el servidor de DirectAccess, y para la conexión basada en HTTPS entre el cliente de DirectAccess y el servidor de ubicación de red. En ambos casos, los clientes de DirectAccess deben ser capaces de resolver y acceder a la ubicación del punto de distribución de CRL.  
  
-   **ISATAP**  
  
    El protocolo ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) usa la tunelización para permitir a los clientes de DirectAccess conectarse al servidor de DirectAccess a través de Internet IPv4, ya que encapsula los paquetes IPv6 en un encabezado IPv4. Acceso remoto lo usa para proporcionar conectividad IPv6 a hosts ISATAP a través de una intranet. En un entorno de red IPv6 no nativo, el servidor de DirectAccess se configura a sí mismo automáticamente como enrutador ISATAP. La resolución debe ser compatible con el nombre de ISATAP.  
  
## <a name="17-configure-active-directory"></a><a name="ConfigAD"></a>1.7 Configurar Active Directory  
El servidor de DirectAccess y todos los equipos cliente de DirectAccess deben unirse a un dominio de Active Directory. Los equipos cliente de DirectAccess deben pertenecer a uno de los siguientes tipos de dominio:  
  
-   Dominios que pertenecen al mismo bosque que el servidor de DirectAccess.  
  
-   Dominios que pertenecen a bosques con una confianza bidireccional con el bosque del servidor de DirectAccess.  
  
-   Dominios que tengan una confianza bidireccional con el dominio del servidor de DirectAccess.  
  
#### <a name="to-join-the-directaccess-server-to-a-domain"></a>Para unir el servidor de DirectAccess a un dominio  
  
1.  En el Administrador del servidor, haga clic en **Servidor local**. En el panel de detalles, haga clic en el vínculo que aparece junto al **Nombre de equipo**.  
  
2.  En el cuadro de diálogo **Propiedades del sistema**, haga clic en la ficha **Nombre del equipo** y, a continuación, en **Cambiar**.  
  
3.  En **Nombre de equipo**, escriba el nombre del equipo si también está cambiando el nombre del equipo al unir el servidor al dominio. En **Miembro de**, haz clic en **Dominio** y, después, escribe el nombre del dominio al que quieras que se una el servidor (por ejemplo, corp.contoso.com) y haz clic en **Aceptar**.  
  
4.  Cuando se le solicite un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con derechos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando veas el cuadro de diálogo de bienvenida al dominio, haz clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema**, haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Cómo unir equipos cliente al dominio  
  
1.  En la pantalla **Inicio** , escriba**explorer.exe**y, a continuación, presione Entrar.  
  
2.  Haz clic con el botón secundario en el icono del equipo y, a continuación, haz clic en **Propiedades**.  
  
3.  En la página **Sistema**, haz clic en **Configuración avanzada del sistema**.  
  
4.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo**, haz clic en **Cambiar**.  
  
5.  En **Nombre de equipo**, escribe el nombre del equipo si también estás cambiando el nombre del equipo al unir el servidor al dominio. En **Miembro de**, haz clic en **Dominio** y, después, escribe el nombre del dominio al que quieras que se una el servidor (por ejemplo, corp.contoso.com) y haz clic en **Aceptar**.  
  
6.  Cuando se le solicite un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con derechos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
7.  Cuando veas el cuadro de diálogo de bienvenida al dominio, haz clic en **Aceptar**.  
  
8.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
9. En el cuadro de diálogo **Propiedades del sistema**, haga clic en **Cerrar**.  
  
10. Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
> [!NOTE]  
> Debes suministrar credenciales de dominio al escribir el comando siguiente, **Add-Computer**.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="18-configure-gpos"></a><a name="ConfigGPOs"></a>1.8 Configurar los GPO  
Se requiere un mínimo de dos objetos directiva de grupo para implementar el acceso remoto:  
  
-   Uno contiene la configuración para el servidor de DirectAccess  
  
-   Otro contiene la configuración para los equipos cliente de DirectAccess  
  
Al configurar el acceso remoto, el asistente crea automáticamente los objetos directiva de grupo necesarios. No obstante, si la organización obliga a usar una convención de nomenclatura, puedes escribir un nombre en el cuadro de diálogo GPO de la consola de administración de acceso remoto. Para obtener más información, consulta 2.7. Resumen de la configuración y GPO alternativos. Si ya has creado los permisos, se creará el GPO. Si no tienes los permisos necesarios para crear GPO, deberás crearlos antes de configurar el acceso remoto.  
  
Para crear directiva de grupo objetos, vea [crear y editar un objeto Directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11)).  
  
> [!IMPORTANT]  
> Los administradores pueden vincular manualmente los objetos de directiva de grupo de DirectAccess a una unidad organizativa (OU) siguiendo estos pasos:  
>   
> 1.  Antes de configurar DirectAccess, vincula los GPO creados a las unidades organizativas correspondientes.  
> 2.  Al configurar DirectAccess, especifica un grupo de seguridad para los equipos cliente.  
> 3.  Es posible que el administrador de acceso remoto tenga permisos para vincular los objetos de directiva de grupo al dominio. En cualquier caso, los objetos de directiva de grupo se configurarán automáticamente. Si los GPO ya están vinculados a una unidad organizativa, los vínculos no se eliminarán y los GPO no se vincularán al dominio. Para un GPO de servidor, la unidad organizativa debe contener el objeto de equipo del servidor, o el GPO se vinculará a la raíz del dominio.  
> 4.  Si no se ha vinculado a la unidad organizativa antes de ejecutar el Asistente de DirectAccess, una vez completada la configuración, el administrador de dominio puede vincular los objetos de directiva de grupo de DirectAccess a las unidades organizativas necesarias. El vínculo al dominio puede eliminarse. Para obtener más información, consulte [Vincular un objeto de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11)).  
  
> [!NOTE]  
> Si un objeto de directiva de grupo se creó manualmente, es posible que el objeto de directiva de grupo no esté disponible durante la configuración de DirectAccess. Es posible que el objeto de directiva de grupo no se haya replicado en el controlador de dominio más próximo al equipo de administración. En este caso, el administrador puede esperar a que la replicación finalice, o bien forzarla.  
  
### <a name="181-configure-remote-access-gpos-with-limited-permissions"></a>1.8.1 Configurar los GPO de acceso remoto con permisos limitados  
En una implementación que use GPO de almacenamiento provisional y de producción, el administrador de dominio debería hacer lo siguiente:  
  
1.  Obtener la lista de los GPO obligatorios para la implementación de acceso remoto del administrador de acceso remoto. Para obtener más información, consulta 1.8 Planear objetos de directiva de grupo.  
  
2.  Por cada GPO solicitado por el administrador de acceso remoto, crea dos GPO con nombres distintos. El primero se usará como el GPO de almacenamiento provisional, y el segundo, como el GPO de producción.  
  
    Para crear directiva de grupo objetos, vea [crear y editar un objeto Directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11)).  
  
3.  Para vincular los GPO de producción, consulte [Vincular un objeto de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11)).  
  
4.  Asigna al administrador de acceso remoto permisos para **Editar configuración, eliminar y modificar seguridad** en todos los GPO de almacenamiento provisional. Para obtener más información, consulte [Delegar permisos para un grupo o usuario en un objeto de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754542(v=ws.11)).  
  
5.  Denegar permisos de administrador de acceso remoto para vincular GPO en todos los dominios (o comprobar que el administrador de acceso remoto no tiene estos permisos). Para obtener más información, consulte [Delegar permisos para vincular objetos de directiva de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755086(v=ws.11)).  
  
Los administradores de acceso remoto que configuren acceso remoto siempre deberán especificar únicamente los GPO de almacenamiento provisional (no los GPO de producción). Esto es lo correcto durante la configuración inicial de acceso remoto y al realizar operaciones de configuración adicionales que necesitan más GPO (por ejemplo, al agregar puntos de entrada en una implementación multisitio o al habilitar equipos cliente en dominios adicionales).  
  
Cuando el administrador de acceso remoto completa los cambios en la configuración de acceso remoto, el administrador del dominio debe revisar la configuración en los GPO de almacenamiento provisional y completar el procedimiento siguiente para copiar la configuración en los GPO de producción.  
  
> [!TIP]  
> Haz lo siguiente después de realizar cambios en la configuración de acceso remoto.  
  
##### <a name="to-copy-settings-to-the-production-gpos"></a>Para copiar la configuración en los GPO de producción  
  
1.  Comprueba que todos los GPO de almacenamiento provisional de la implementación de acceso remoto se hayan replicado a todos los controladores de dominio del dominio. Esto es necesario para asegurarse de que se importe la configuración más reciente en los GPO de producción. Para obtener más información, consulta Comprobar el estado de la infraestructura de la directiva de grupo.  
  
2.  Exporta la configuración; para ello, realiza una copia de seguridad de todos los GPO de almacenamiento provisional en la implementación de acceso remoto. Para obtener más información, consulte Crear una copia de seguridad de un objeto de directiva de grupo.  
  
3.  Por cada GPO de producción, cambia los filtros de seguridad para que coincidan con los filtros de seguridad del GPO de almacenamiento provisional correspondiente. Para obtener más información, vea Filtrar mediante grupos de seguridad.  
  
    > [!NOTE]  
    > Esto es necesario porque la opción **Importar configuración** no copia el filtro de seguridad del GPO de origen.  
  
4.  Por cada GPO de producción, haz lo siguiente para importar la configuración de la copia de seguridad del GPO de almacenamiento provisional correspondiente:  
  
    1.  En el Consola de administración de directivas de grupo (GPMC), expanda el nodo objetos de directiva de grupo en el bosque y el dominio que contiene el objeto de directiva de grupo de producción en el que se importará la configuración.  
  
    2.  Haz clic con el botón secundario en el GPO y selecciona **Importar configuración**.  
  
    3.  En el **Asistente para importar configuración**, en la **Página principal**, haz clic en **Siguiente**.  
  
    4.  En la página **Hacer copia de seguridad de GPO**, haz clic en **Copia de seguridad**.  
  
    5.  En el cuadro de diálogo **Copia de seguridad de objeto de directiva de grupo**, en el cuadro **Ubicación**, escribe la ruta de la ubicación donde quieras almacenar las copias de seguridad del GPO, o bien haz clic en **Examinar** para buscar la carpeta.  
  
    6.  En el cuadro **Descripción**, escribe una descripción para el GPO de producción y haz clic en **Hacer copia de seguridad**.  
  
    7.  Cuando se complete la copia de seguridad, haz clic en **Aceptar** y, después, en la página **Hacer copia de seguridad de GPO**, haz clic en **Siguiente**.  
  
    8.  En la página **Ubicación de la copia de seguridad**, en el cuadro **Carpeta de copia de seguridad**, escribe la ruta de la ubicación donde se almacenó (en el paso 2) la copia de seguridad del GPO de almacenamiento provisional correspondiente, o bien haz clic en **Examinar** para buscar la carpeta y, después, haz clic en **Siguiente**.  
  
    9. En la página **GPO de origen**, selecciona la casilla **Mostrar solo la versión más reciente de cada GPO** para ocultar las copias de seguridad anteriores y selecciona el GPO de almacenamiento provisional correspondiente. Haz clic en **Ver configuración** para revisar la configuración de acceso remoto antes de aplicarla al GPO de producción y, después, haz clic en **Siguiente**.  
  
    10. En la página **Examinar copia de seguridad**, haz clic en **Siguiente** y, después, en **Finalizar**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
-   Para hacer una copia de seguridad del GPO de cliente de almacenamiento provisional "configuración de cliente de DirectAccess-staging" en el dominio "corp.contoso.com" en la carpeta de copia de seguridad "C:\Backups \" :  
  
    ```  
    $backup = Backup-GPO "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "Path 'C:\Backups\'  
    ```  
  
-   Para ver el filtrado de seguridad del GPO de cliente de almacenamiento provisional "configuración de cliente de DirectAccess-almacenamiento provisional" en el dominio "corp.contoso.com":  
  
    ```  
    Get-GPPermission "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "All | ?{ $_.Permission "eq 'GpoApply'}  
    ```  
  
-   Para agregar el grupo de seguridad ' Corp. contoso. com\DirectAccess clients ' al filtro de seguridad del GPO de cliente de producción "configuración de cliente de DirectAccess" Production "en el dominio" corp.contoso.com ":  
  
    ```  
    Set-GPPermission "Name 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com' "PermissionLevel GpoApply "TargetName 'corp.contoso.com\DirectAccess clients' "TargetType Group  
    ```  
  
-   Para importar la configuración de la copia de seguridad en el GPO de cliente de producción "configuración de cliente de DirectAccess" Production "en el dominio" corp.contoso.com ":  
  
    ```  
    Import-GPO "BackupId $backup.Id "Path $backup.BackupDirectory "TargetName 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com'  
    ```  
  
## <a name="19-configure-security-groups"></a><a name="ConfigSGs"></a>1.9 Configurar los grupos de seguridad  
La configuración de DirectAccess contenida en el equipo cliente directiva de grupo objeto solo se aplica a los equipos que son miembros de los grupos de seguridad que se especifican al configurar el acceso remoto. Además, si usas grupos de seguridad para administrar tus servidores de aplicación, debes crear un grupo de seguridad para dichos servidores.  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>Cómo crear un grupo de seguridad para clientes de DirectAccess  
  
1.  En la pantalla **Inicio** , escriba**DSA. msc**y, a continuación, presione Entrar. En la consola **Usuarios y equipos de Active Directory**, en el panel izquierdo, expande el dominio que contendrá el grupo de seguridad, haz clic con el botón secundario en **Usuarios**, elige **Nuevo** y haz clic en **Grupo**.  
  
2.  En el cuadro de diálogo **Nuevo objeto - Grupo**, en **Nombre de grupo**, escribe el nombre del grupo de seguridad.  
  
3.  En **Ámbito del grupo**, haz clic en **Global** y, en **Tipo de grupo**, haz clic en **Seguridad** y, después, en **Aceptar**.  
  
4.  Haz doble clic en el grupo de seguridad de equipos cliente de DirectAccess y, en el cuadro de diálogo Propiedades, haz clic en la pestaña **Miembros**.  
  
5.  En la pestaña **Miembros**, haga clic en **Agregar**.  
  
6.  En el cuadro de diálogo **Seleccionar usuarios, contactos, equipos o cuentas de servicio**, selecciona los equipos cliente que quieras habilitar para DirectAccess y, después, haz clic en **Aceptar**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**Comandos equivalentes** de Windows PowerShell Windows PowerShell  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="110-configure-the-network-location-server"></a><a name="ConfigNLS"></a>1.10 Configurar el servidor de ubicación de red  
El servidor de ubicación de red debe ser un servidor con alta disponibilidad y, además, debe tener un certificado SSL válido en el que confíen los clientes de DirectAccess. Hay dos opciones de certificados para el certificado de servidor de ubicación de red:  
  
-   **Certificado privado**  
  
    Este certificado se basa en la plantilla de certificado que creó siguiendo las instrucciones de [1.5.2 configurar plantillas de certificado](#ConfigCertTemp).  
  
-   **Certificado autofirmado**  
  
    > [!NOTE]  
    > Los certificados autofirmados no pueden usarse en implementaciones multisitio.  
  
Estos son los requisitos para cada tipo de certificado, si aún no existen:  
  
-   Un certificado de sitio web que se use para el servidor de ubicación de red. El firmante del certificado debe ser la dirección URL del servidor de ubicación de red.  
  
-   Un punto de distribución CRL que tenga alta disponibilidad de la red interna.  
  
> [!NOTE]  
> Si el sitio web del servidor de ubicación de red se encuentra en el servidor de DirectAccess, se creará automáticamente un sitio web cuando configures el acceso remoto. Este sitio está enlazado con el certificado de servidor que indiques.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Cómo instalar el certificado de servidor de ubicación de red desde una CA interna  
  
1.  En el servidor que hospedará el sitio web del servidor de ubicación de red: en la pantalla **Inicio** , escriba**mmc.exe**y, a continuación, presione Entrar.  
  
2.  En el menú **Archivo** de la consola MMC, haga clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, en **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, en **Finalizar** y, por último, en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abre **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , active la casilla de la plantilla de certificado que creó siguiendo las instrucciones de 1.5.2 configurar plantillas de certificado. Si es necesario, haz clic en **Se necesita más información para inscribir este certificado**.  
  
8.  En el cuadro de diálogo **Propiedades de certificado**, en la pestaña **Sujeto**, en el área **Nombre de sujeto**, en **Tipo**, selecciona **Nombre común**.  
  
9. En **Valor**, escribe el FQDN del sitio web del servidor de ubicación de red y, a continuación, haz clic en **Agregar**.  
  
10. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
11. En **Valor**, escribe el FQDN del sitio web del servidor de ubicación de red y, a continuación, haz clic en **Agregar**.  
  
12. En la pestaña **General**, en **Nombre descriptivo**, puedes escribir un nombre que te ayude a identificar el certificado.  
  
13. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
14. En el panel de detalles del complemento Certificados, comprueba que se inscribió un nuevo certificado con el valor Propósitos planteados de autenticación del servidor.  
  
#### <a name="to-configure-the-network-location-server"></a>Para configurar el servidor de ubicación de red  
  
1.  Instala un sitio web en un servidor de alta disponibilidad. No es necesario que el sitio web tenga contenidos; pero, cuando lo pruebes, puedes definir una página predeterminada donde se muestre un mensaje a los clientes cuando se conecten.  
  
    > [!NOTE]  
    > Este paso no es necesario si el sitio web del servidor de ubicación de red está hospedado en el servidor de DirectAccess.  
  
2.  Enlaza un certificado de servidor HTTPS al sitio web. El nombre común del certificado debe coincidir con el nombre del sitio del servidor de ubicación de red. Comprueba que los clientes de DirectAccess confíen en la CA emisora.  
  
    > [!NOTE]  
    > Este paso no es necesario si el sitio web del servidor de ubicación de red está hospedado en el servidor de DirectAccess.  
  
3.  Configura un sitio de CRL que tenga alta disponibilidad de la red interna.  
  
    Puedes acceder a los puntos de distribución CRL mediante:  
  
    -   Servidores Web mediante el uso de una dirección URL basada en HTTP, como:https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Servidores de archivos a los que se accede a través de una ruta de acceso UNC (Convención de nomenclatura universal), como \\ \crl.Corp.contoso.com\crld\corp-app1-CA.CRL  
  
    Si el punto de distribución CRL de la intranet solo es accesible a través de IPv6, deberás configurar una regla de seguridad de conexión de Firewall de Windows con seguridad avanzada para eximir la protección de IPsec de la dirección IPv6 de la intranet a las direcciones IPv6 de los puntos de distribución CRL.  
  
4.  Comprueba que los clientes de DirectAccess de la red interna puedan resolver el nombre del servidor de ubicación de red. Comprueba que el nombre no pueda ser resuelto por clientes de DirectAccess en Internet.  
  
## <a name="next-step"></a><a name="BKMK_Links"></a>Paso siguiente  
  
-   [Paso 2: Configurar los servidores avanzados de DirectAccess](da-adv-configure-s2-servers.md)  
  
