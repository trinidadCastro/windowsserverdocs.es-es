---
title: Paso 1 configurar la infraestructura de acceso remoto
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 549150b10dede7dca9786fe38da40e9b7dea706f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308153"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>Paso 1 configurar la infraestructura de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el Servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
En este tema se describe cómo configurar la infraestructura necesaria para una implementación de acceso remoto avanzada con un solo servidor de acceso remoto en un entorno mixto de IPv4 e IPv6. Antes de comenzar con los pasos de implementación, asegúrese de haber completado los pasos de planificación descritos en [paso 1: planear la infraestructura de acceso remoto](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tarea|Descripción|  
|----|--------|  
|Configurar los valores de red del servidor|Configura los valores de red del servidor en el servidor de acceso remoto.|  
|Configurar el enrutamiento en la red corporativa|Configura el enrutamiento en la red corporativa para asegurarte de que el tráfico se enruta correctamente.|  
|Configurar los firewalls|Configure los firewalls adicionales, si es necesario.|  
|Configurar las entidades de certificación y los certificados|Configure una entidad de certificación (CA), si es necesario, y cualquier otra plantilla de certificado necesaria en la implementación.|  
|Configurar el servidor DNS|Configura los valores de DNS para el servidor de acceso remoto.|  
|Configurar Active Directory|Unir los equipos cliente y el servidor de acceso remoto al dominio de Active Directory.|  
|Configurar GPO|Configure objetos de directiva de grupo (GPO) para la implementación, si es necesario.|  
|Configurar grupos de seguridad|Configura los grupos de seguridad que contendrán los equipos cliente de DirectAccess, así como otros grupos de seguridad necesarios para la implementación.|  
|Configurar el servidor de ubicación de red|Configura el servidor de ubicación de red y, además, instala el certificado de sitio web del servidor de ubicación de red.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="configure-server-network-settings"></a><a name="BKMK_ConfigNetworkSettings"></a>Configurar las opciones de red del servidor  
Dependiendo de si decide colocar el servidor de acceso remoto en el perímetro o detrás de un dispositivo de traducción de direcciones de red (NAT), se requiere la siguiente configuración de dirección de interfaz de red para una implementación de un solo servidor en un entorno con IPv4 e IPv6. Todas las direcciones IP se configuran mediante **Cambiar configuración del adaptador** en el **Centro de redes y recursos compartidos de Windows**.  
  
**Topología perimetral**:  
  
Requiere lo siguiente:  
  
-   Dos direcciones IPv4 o IPv6 estáticas públicas consecutivas a través de Internet.  
  
    > [!NOTE]  
    > Se requieren dos direcciones IPv4 públicas consecutivas para Teredo. Si no usas Teredo, puedes configurar una sola dirección IPv4 estática y pública.  
  
-   Una sola dirección IPv4 o IPv6 estática interna.  
  
**Detrás de un dispositivo NAT (dos adaptadores de red)** :  
  
Requiere una sola dirección IPv4 o IPv6 estática accesible a la red interna.  
  
**Detrás de un dispositivo NAT (un adaptador de red)** :  
  
Requiere una sola dirección IPv4 o IPv6 estática.  
  
Si el servidor de acceso remoto tiene dos adaptadores de red (uno para el perfil de dominio y el otro para un perfil público o privado), pero utiliza una topología de adaptador de red única, la recomendación es la siguiente:  
  
1.  Asegúrese de que el segundo adaptador de red también esté clasificado en el perfil de dominio.  
  
2.  Si el segundo adaptador de red no se puede configurar para el perfil de dominio por alguna razón, el ámbito de la directiva IPsec de DirectAccess debe establecerse de forma manual en todos los perfiles mediante el siguiente comando de Windows PowerShell:  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    Los nombres de las directivas IPsec que se usarán en este comando son **DirectAccess-DaServerToInfra** y **DirectAccess-DaServerToCorp**.  
  
## <a name="configure-routing-in-the-corporate-network"></a><a name="BKMK_ConfigRouting"></a>Configurar el enrutamiento en la red corporativa  
Configura el enrutamiento en la red corporativa de la siguiente manera:  
  
-   Si se implementa IPv6 nativa en la organización, agrega una ruta para que los enrutadores de la red interna enruten el tráfico IPv6 a través del servidor de acceso remoto.  
  
-   Configura manualmente las rutas de IPv4 e IPv6 de la organización en los servidores de acceso remoto. Agregue una ruta publicada para que todo el tráfico con un prefijo IPv6 (/48) se reenvíe a la red interna. Además, para el tráfico IPv4, agrega rutas explícitas para que el tráfico IPv4 se reenvíe a la red interna.  
  
## <a name="configure-firewalls"></a><a name="BKMK_ConfigFirewalls"></a>Configuración de firewalls  
En función de la configuración de red que elija, al usar firewalls adicionales en la implementación, aplique las siguientes excepciones de Firewall para el tráfico de acceso remoto:  
  
### <a name="remote-access-server-on-ipv4-internet"></a>Servidor de acceso remoto en Internet IPv4  
Aplique las siguientes excepciones de firewall con conexión a Internet para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentre en Internet IPv4:  
  
-   **Tráfico Teredo**  
  
    Puerto de destino 3544 del Protocolo de datagramas de usuario (UDP) de entrada y puerto de origen UDP 3544 de salida. Aplique esta exención para las dos direcciones IPv4 públicas consecutivas orientadas a Internet en el servidor de acceso remoto.  
  
-   **tráfico 6to4**  
  
    Protocolo IP 41 entrante y saliente. Aplique esta exención para las dos direcciones IPv4 públicas consecutivas orientadas a Internet en el servidor de acceso remoto.  
  
-   **Tráfico IP-HTTPS**  
  
    Puerto de destino 443 del Protocolo de control de transmisión (TCP) y puerto de origen TCP 443 de salida. Si el servidor de acceso remoto tiene un único adaptador de red y el servidor de ubicación de red se encuentra en el servidor de acceso remoto, el puerto TCP 62000 también es obligatorio. Aplique estas exenciones solo para la dirección en la que se resuelve el nombre externo del servidor.  
  
    > [!NOTE]  
    > Esta exención se configura en el servidor de acceso remoto. Todas las demás exenciones están configuradas en el firewall perimetral.  
  
### <a name="remote-access-server-on-ipv6-internet"></a>Servidor de acceso remoto en Internet por IPv6  
Aplique las siguientes excepciones de firewall con conexión a Internet para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentre en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
-   Tráfico entrante y saliente del Protocolo de mensajes de control de Internet para IPv6 (ICMPv6) solo para implementaciones de Teredo.  
  
### <a name="remote-access-traffic"></a>Tráfico de acceso remoto  
Aplique las siguientes excepciones de Firewall de la red interna para el tráfico de acceso remoto:  
  
-   ISATAP: protocolo 41 de entrada y de salida  
  
-   TCP/UDP para todo el tráfico IPv4 o IPv6  
  
-   ICMP para todo el tráfico IPv4 o IPv6  
  
## <a name="configure-cas-and-certificates"></a><a name="BKMK_ConfigCAs"></a>Configurar entidades de certificación y certificados  
Con el acceso remoto en Windows Server 2012, puede elegir entre usar certificados para la autenticación del equipo o usar una autenticación Kerberos integrada que use nombres de usuario y contraseñas. También debe configurar un certificado IP-HTTPS en el servidor de acceso remoto. En esta sección se explica cómo configurar estos certificados.  
  
Para obtener información acerca de cómo configurar una infraestructura de clave pública (PKI), consulte [Active Directory servicios de Certificate Server](https://technet.microsoft.com/library/cc770357.aspx).  
  
### <a name="configure-ipsec-authentication"></a><a name="BKMK_ConfigIPsec"></a>Configurar la autenticación IPsec  
Se requiere un certificado en el servidor de acceso remoto y todos los clientes de DirectAccess para que puedan usar la autenticación IPsec. El certificado debe ser emitido por una entidad de certificación (CA) interna. Los servidores de acceso remoto y los clientes de DirectAccess deben confiar en la CA que emite los certificados raíz e intermedio.  
  
##### <a name="to-configure-ipsec-authentication"></a>Para configurar la autenticación IPsec  
  
1.  En la CA interna, decida si usará la plantilla de certificado de equipo predeterminada, o si va a crear una nueva plantilla de certificado como se describe en [creación de plantillas de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
    > [!NOTE]  
    > Si crea una nueva plantilla, debe configurarse para la autenticación del cliente.  
  
2.  Implemente la plantilla de certificado si es necesario. Para obtener más información, vea [implementar plantillas de certificado](https://technet.microsoft.com/library/cc770794.aspx).  
  
3.  Si es necesario, configure la plantilla para la inscripción automática.  
  
4.  Configure la inscripción automática de certificados si es necesario. Para obtener más información, consulte Configuración de la [inscripción automática de certificados](https://technet.microsoft.com/library/cc731522.aspx).  
  
### <a name="configure-certificate-templates"></a><a name="BKMK_ConfigCertTemp"></a>Configurar plantillas de certificado  
Al usar una CA interna para emitir certificados, debe configurar plantillas de certificado para el certificado IP-HTTPS y el certificado de sitio web del servidor de ubicación de red.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar una plantilla de certificado  
  
1.  En la CA interna, cree una plantilla de certificado del modo descrito en el tema sobre [creación de plantillas de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Implemente la plantilla de certificado según se indica en el tema sobre [implementación de plantillas de certificado](https://technet.microsoft.com/library/cc770794.aspx).  
  
Después de preparar las plantillas, puede usarlas para configurarlas. Consulte los procedimientos siguientes para obtener más información:  
  
-   [Configurar el certificado IP-HTTPS](#BKMK_IPHTTPS)  
  
-   [Configurar el servidor de ubicación de red](#BKMK_ConfigNLS)  
  
### <a name="configure-the-ip-https-certificate"></a><a name="BKMK_IPHTTPS"></a>Configurar el certificado IP-HTTPS  
El acceso remoto requiere que un certificado IP-HTTPS autentique las conexiones IP-HTTPS con el servidor de acceso remoto. Hay tres opciones de certificado para el certificado IP-HTTPS:  
  
-   **Pública**  
  
    Proporcionado por terceros.  
  
-   **Privada**  
  
    El certificado se basa en la plantilla de certificado que creó en [configuración de plantillas de certificado](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp). Requiere un punto de distribución de lista de revocación de certificados (CRL) que sea accesible desde un FQDN que pueda resolverse públicamente.  
  
-   **Con firma automática**  
  
    Este certificado requiere un punto de distribución de CRL que sea accesible desde un FQDN que pueda resolverse públicamente.  
  
    > [!NOTE]  
    > Los certificados autofirmados no pueden usarse en implementaciones multisitio.  
  
Asegúrate de que el certificado de sitio web utilizado para la autenticación IP-HTTPS reúna estos requisitos:  
  
-   El nombre del firmante del certificado debe ser el nombre de dominio completo (FQDN) que se puede resolver externamente de la dirección URL de IP-HTTPS (la dirección ConnectTo) que se usa solo para las conexiones IP-HTTPS del servidor de acceso remoto.  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
-   En el campo asunto, especifique la dirección IPv4 del adaptador orientado externamente del servidor de acceso remoto o el FQDN de la dirección URL IP-HTTPS.  
  
-   En el campo **uso mejorado de clave** , utilice el identificador de objeto (OID) de autenticación de servidor.  
  
-   En el campo **Puntos de distribución CRL**, especifique un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
-   Los certificados IP-HTTPS pueden contener caracteres comodín en el nombre.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Cómo instalar el certificado IP-HTTPS desde una CA interna  
  
1.  En el servidor de acceso remoto: en la pantalla **Inicio** , escriba**MMC. exe**y, a continuación, presione Entrar.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, en **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, en **Finalizar** y, por último, en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas**, haga clic en **solicitar nuevo certificado**y, a continuación, haga clic en **siguiente** dos veces.  
  
6.  En la página **solicitar certificados** , active la casilla de la plantilla de certificado que creó en configuración de plantillas de certificado y, si es necesario, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
7.  En el cuadro de diálogo **Propiedades de certificado**, en la pestaña **Sujeto**, en el área **Nombre de sujeto**, en **Tipo**, selecciona **Nombre común**.  
  
8.  En **valor**, especifique la dirección IPv4 del adaptador accesible desde el exterior del servidor de acceso remoto, o el FQDN de la dirección URL de IP-https y, a continuación, haga clic en **Agregar**.  
  
9. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
10. En **valor**, especifique la dirección IPv4 del adaptador accesible desde el exterior del servidor de acceso remoto, o el FQDN de la dirección URL de IP-https y, a continuación, haga clic en **Agregar**.  
  
11. En la pestaña **General**, en **Nombre descriptivo**, puedes escribir un nombre que te ayude a identificar el certificado.  
  
12. En la pestaña **Extensiones**, junto a **Uso mejorado de clave**, haz clic en la flecha y asegúrate de que Autenticación de servidor se encuentra en la lista de **Opciones seleccionadas**.  
  
13. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
14. En el panel de detalles del complemento certificados, compruebe que el nuevo certificado se ha inscrito con el propósito previsto de la autenticación del servidor.  
  
## <a name="configure-the-dns-server"></a><a name="BKMK_ConfigDNS"></a>Configurar el servidor DNS  
Debes configurar manualmente una entrada DNS para el sitio web del servidor de ubicación de red para la red interna de tu implementación.  
  
### <a name="to-add-the-network-location-server-and-web-probe"></a><a name="NLS_DNS"></a>Para agregar el servidor de ubicación de red y el sondeo Web  
  
1.  En el servidor DNS de la red interna: en la pantalla **Inicio** , escriba**DNSMgmt. msc**y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo de la consola del **Administrador del DNS**, expande la zona de búsqueda directa de tu dominio. Haga clic con el botón secundario en el dominio y haga clic en **nuevo host (A o aaaa)** .  
  
3.  En el cuadro de diálogo **host nuevo** , en el cuadro **nombre (si está en blanco, se usa el nombre del dominio primario)** , escriba el nombre DNS del sitio web del servidor de ubicación de red (este es el nombre que los clientes de DirectAccess usan para conectarse al servidor de ubicación de red). En el cuadro **dirección IP** , escriba la dirección IPv4 del servidor de ubicación de red, haga clic en **Agregar host**y, a continuación, haga clic en **Aceptar**.  
  
4.  En el cuadro de diálogo **nuevo host** , en el cuadro **nombre (si está en blanco se usa el nombre del dominio primario)** , escriba el nombre DNS del sondeo web (el nombre del sondeo Web predeterminado es DirectAccess-webprobehost). En el cuadro **Dirección IP**, escribe la dirección IPv4 del sondeo web y haz clic en **Agregar host**.  
  
5.  Repite este proceso para directaccess-corpconnectivityhost y para los comprobadores de conectividad creados de forma manual. En el cuadro de diálogo **DNS** , haga clic en **Aceptar**.  
  
6.  Haga clic en **Done** (Listo).  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
También debes configurar entradas DNS para los siguiente:  
  
-   **El servidor IP-HTTPS**  
  
    Los clientes de DirectAccess deben ser capaces de resolver el nombre DNS del servidor de acceso remoto desde Internet.  
  
-   **Comprobación de revocación de CRL**  
  
    DirectAccess usa la comprobación de revocación de certificados para la conexión IP-HTTPS entre los clientes de DirectAccess y el servidor de acceso remoto, y para la conexión basada en HTTPS entre el cliente de DirectAccess y el servidor de ubicación de red. En ambos casos, los clientes de DirectAccess deben ser capaces de resolver y acceder a la ubicación del punto de distribución de CRL.  
  
-   **ISATAP**  
  
    El protocolo de direccionamiento automático de túnel (ISATAP) Intrasite utiliza túneles para permitir que los clientes de DirectAccess se conecten al servidor de acceso remoto a través de Internet por IPv4, encapsulando los paquetes IPv6 dentro de un encabezado IPv4. Acceso remoto lo usa para proporcionar conectividad IPv6 a hosts ISATAP a través de una intranet. En un entorno de red IPv6 no nativo, el servidor de acceso remoto se configura automáticamente como un enrutador ISATAP. La resolución debe ser compatible con el nombre de ISATAP.  
  
## <a name="configure-active-directory"></a><a name="BKMK_ConfigAD"></a>Configurar Active Directory  
El servidor de acceso remoto y todos los equipos cliente de DirectAccess deben estar unidos a un dominio de Active Directory. Los equipos cliente de DirectAccess deben pertenecer a uno de los siguientes tipos de dominio:  
  
-   Dominios que pertenecen al mismo bosque que el servidor de acceso remoto.  
  
-   Dominios que pertenecen a bosques con confianza bidireccional con el bosque del servidor de acceso remoto.  
  
-   Dominios con confianza de dominio bidireccional con el dominio del servidor de acceso remoto.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Para unir el servidor de acceso remoto a un dominio  
  
1.  En el Administrador del servidor, haga clic en **Servidor local**. En el panel de detalles, haga clic en el vínculo que aparece junto al **Nombre de equipo**.  
  
2.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en la pestaña **Nombre de equipo** y después haga clic en **Cambiar**.  
  
3.  En el cuadro **nombre de equipo** , escriba el nombre del equipo si también va a cambiar el nombre del equipo al unir el servidor al dominio. En **miembro de**, haga clic en **dominio**y, a continuación, escriba el nombre del dominio al que desea unir el servidor (por ejemplo, Corp.contoso.com) y, a continuación, haga clic en **Aceptar**.  
  
4.  Cuando se le pida un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con permisos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema**, haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Cómo unir equipos cliente al dominio  
  
1.  En la pantalla **Inicio** , escriba**Explorer. exe**y, a continuación, presione Entrar.  
  
2.  Haz clic con el botón secundario en el icono del equipo y, a continuación, haz clic en **Propiedades**.  
  
3.  En la página **Sistema**, haz clic en **Configuración avanzada del sistema**.  
  
4.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cambiar**.  
  
5.  En el cuadro **nombre de equipo** , escriba el nombre del equipo si también va a cambiar el nombre del equipo al unir el servidor al dominio. En **Miembro de**, haz clic en **Dominio** y, después, escribe el nombre del dominio al que quieras que se una el servidor (por ejemplo, corp.contoso.com) y haz clic en **Aceptar**.  
  
6.  Cuando se le pida un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con permisos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
7.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio, haga clic en **Aceptar**.  
  
8.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
9. En el cuadro de diálogo **propiedades del sistema** , haga clic en cerrar.  
  
10. Haz clic en **Reiniciar ahora** cuando se te solicite.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
> [!NOTE]  
> Debe proporcionar las credenciales de dominio después de escribir el siguiente comando.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="configure-gpos"></a><a name="BKMK_ConfigGPOs"></a>Configurar GPO  
Para implementar el acceso remoto, se requiere un mínimo de dos objetos directiva de grupo. Un objeto de directiva de grupo contiene la configuración del servidor de acceso remoto y otra contiene la configuración de los equipos cliente de DirectAccess. Al configurar el acceso remoto, el asistente crea automáticamente los objetos directiva de grupo necesarios. Sin embargo, si su organización exige una Convención de nomenclatura, o si no tiene los permisos necesarios para crear o editar objetos directiva de grupo, se deben crear antes de configurar el acceso remoto.  
  
Para crear directiva de grupo objetos, vea [crear y editar un objeto Directiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
Un administrador puede vincular manualmente los objetos de directiva de grupo de DirectAccess a una unidad organizativa (OU). Tenga en cuenta lo siguiente:  
  
1.  Vincule los GPO creados a las unidades organizativas correspondientes antes de configurar DirectAccess.  
  
2.  Al configurar DirectAccess, especifica un grupo de seguridad para los equipos cliente.  
  
3.  Los GPO se configuran automáticamente, independientemente de si el administrador tiene permisos para vincular los GPO al dominio.  
  
4.  Si los GPO ya están vinculados a una unidad organizativa, los vínculos no se quitarán, pero no se vincularán al dominio.  
  
5.  En el caso de los GPO de servidor, la unidad organizativa debe contener el objeto de equipo del servidor; de lo contrario, el GPO se vinculará a la raíz del dominio.  
  
6.  Si la unidad organizativa no se ha vinculado previamente mediante la ejecución del Asistente para configuración de DirectAccess, una vez completada la configuración, el administrador puede vincular los GPO de DirectAccess a las unidades organizativas necesarias y quitar el vínculo al dominio.  
  
    Para obtener más información, vea [vincular un objeto de directiva de grupo](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Si un objeto de directiva de grupo se creó manualmente, es posible que el objeto de directiva de grupo no esté disponible durante la configuración de DirectAccess. Es posible que el objeto de directiva de grupo no se haya replicado en el controlador de dominio más cercano al equipo de administración. El administrador puede esperar a que se complete la replicación o forzar la replicación.  
  
## <a name="configure-security-groups"></a><a name="BKMK_ConfigSGs"></a>Configurar grupos de seguridad  
La configuración de DirectAccess contenida en el equipo cliente directiva de grupo objeto solo se aplica a los equipos que son miembros de los grupos de seguridad que se especifican al configurar el acceso remoto.  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>Para crear un grupo de seguridad para clientes de DirectAccess  
  
1.  En la pantalla **Inicio** , escriba**DSA. msc**y, a continuación, presione Entrar.  
  
2.  En la consola **Usuarios y equipos de Active Directory**, en el panel izquierdo, expande el dominio que contendrá el grupo de seguridad, haz clic con el botón secundario en **Usuarios**, elige **Nuevo** y haz clic en **Grupo**.  
  
3.  En el cuadro de diálogo **Nuevo objeto - Grupo**, en **Nombre de grupo**, escribe el nombre del grupo de seguridad.  
  
4.  En **Ámbito del grupo**, haz clic en **Global** y, en **Tipo de grupo**, haz clic en **Seguridad** y, después, en **Aceptar**.  
  
5.  Haga doble clic en el grupo de seguridad equipos cliente de DirectAccess y, en el cuadro de diálogo **propiedades** , haga clic en la pestaña **miembros** .  
  
6.  En la pestaña **Miembros**, haga clic en **Agregar**.  
  
7.  En el cuadro de diálogo **Seleccionar usuarios, contactos, equipos o cuentas de servicio**, selecciona los equipos cliente que quieras habilitar para DirectAccess y, después, haz clic en **Aceptar**.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**comandos equivalentes** de Windows PowerShell Windows PowerShell  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="configure-the-network-location-server"></a><a name="BKMK_ConfigNLS"></a>Configurar el servidor de ubicación de red  
El servidor de ubicación de red debe encontrarse en un servidor con alta disponibilidad y necesita un certificado de Capa de sockets seguros válido (SSL) que sea de confianza para los clientes de DirectAccess.  
  
> [!NOTE]  
> Si el sitio web del servidor de ubicación de red se encuentra en el servidor de acceso remoto, se creará automáticamente un sitio web al configurar el acceso remoto y se enlazará al certificado de servidor que proporcione.  
  
Hay dos opciones de certificados para el certificado de servidor de ubicación de red:  
  
-   **Privada**  
  
    > [!NOTE]  
    > El certificado se basa en la plantilla de certificado que creó en [configuración de plantillas de certificado](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp).  
  
-   **Con firma automática**  
  
    > [!NOTE]  
    > Los certificados autofirmados no pueden usarse en implementaciones multisitio.  
  
Tanto si usa un certificado privado como un certificado autofirmado, requiere lo siguiente:  
  
-   Un certificado de sitio web que se use para el servidor de ubicación de red. El firmante del certificado debe ser la dirección URL del servidor de ubicación de red.  
  
-   Un punto de distribución de CRL que tiene alta disponibilidad en la red interna.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Cómo instalar el certificado de servidor de ubicación de red desde una CA interna  
  
1.  En el servidor que hospedará el sitio web del servidor de ubicación de red: en la pantalla **Inicio** , escriba**MMC. exe**y, a continuación, presione Entrar.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos**, haz clic en **Certificados**, en **Agregar**, **Cuenta de equipo**, **Siguiente**, **Equipo local**, en **Finalizar** y, por último, en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas**, haga clic en **solicitar nuevo certificado**y, a continuación, haga clic en **siguiente** dos veces.  
  
6.  En la página **solicitar certificados** , active la casilla de la plantilla de certificado que creó en configuración de plantillas de certificado y, si es necesario, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
7.  En el cuadro de diálogo **Propiedades de certificado**, en la pestaña **Sujeto**, en el área **Nombre de sujeto**, en **Tipo**, selecciona **Nombre común**.  
  
8.  En **Valor**, escribe el FQDN del sitio web del servidor de ubicación de red y, a continuación, haz clic en **Agregar**.  
  
9. En la zona **Nombre alternativo**, en **Tipo**, selecciona **DNS**.  
  
10. En **Valor**, escribe el FQDN del sitio web del servidor de ubicación de red y, a continuación, haz clic en **Agregar**.  
  
11. En la pestaña **General**, en **Nombre descriptivo**, puedes escribir un nombre que te ayude a identificar el certificado.  
  
12. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
13. En el panel de detalles del complemento certificados, compruebe que el nuevo certificado se ha inscrito con el propósito previsto de la autenticación del servidor.  
  
#### <a name="to-configure-the-network-location-server"></a>Para configurar el servidor de ubicación de red  
  
1.  Instala un sitio web en un servidor de alta disponibilidad. No es necesario que el sitio web tenga contenidos; pero, cuando lo pruebes, puedes definir una página predeterminada donde se muestre un mensaje a los clientes cuando se conecten.  
  
    Este paso no es necesario si el sitio web del servidor de ubicación de red se hospeda en el servidor de acceso remoto.  
  
2.  Enlaza un certificado de servidor HTTPS al sitio web. El nombre común del certificado debe coincidir con el nombre del sitio del servidor de ubicación de red. Comprueba que los clientes de DirectAccess confíen en la CA emisora.  
  
    Este paso no es necesario si el sitio web del servidor de ubicación de red se hospeda en el servidor de acceso remoto.  
  
3.  Configure un sitio de CRL que Hass alta disponibilidad en la red interna.  
  
    Puedes acceder a los puntos de distribución CRL mediante:  
  
    -   Servidores web que usan una dirección URL basada en HTTP, como: https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Servidores de archivos a los que se accede a través de una ruta de acceso UNC (Convención de nomenclatura universal), como \\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    Si el punto de distribución de CRL interno solo es accesible a través de IPv6, debe configurar una regla de seguridad de conexión de Firewall de Windows con seguridad avanzada. Esto exime la protección de IPsec del espacio de direcciones IPv6 de la intranet a las direcciones IPv6 de los puntos de distribución de CRL.  
  
4.  Asegúrese de que los clientes de DirectAccess de la red interna pueden resolver el nombre del servidor de ubicación de red y que los clientes de DirectAccess en Internet no pueden resolver el nombre.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vea también  
  
-   [Paso 2: configurar el servidor de acceso remoto](Step-2-Configure-the-Remote-Access-Server.md)

