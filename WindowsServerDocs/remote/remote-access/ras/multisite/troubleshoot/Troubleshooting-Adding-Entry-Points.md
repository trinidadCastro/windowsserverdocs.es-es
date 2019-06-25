---
title: Solucionar problemas relacionados con la adición de puntos de entrada
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 51f49364aa4e7a6da6c51b1d8b7da7e37f842190
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282563"
---
# <a name="troubleshooting-adding-entry-points"></a>Solucionar problemas relacionados con la adición de puntos de entrada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema encontrará información para solucionar problemas relacionados con el comando `Add-DAEntryPoint`. Para confirmar que el error recibido está relacionado con la adición de un punto de entrada, consulte el identificador de evento 10067 en el Registro de eventos de Windows.  
  
## <a name="missing-remoteaccessserver-parameter"></a>Falta el parámetro RemoteAccessServer  
**Recibidas el error**. Debe proporcionar un valor para el parámetro RemoteAccessServer.  
  
**Causa**  
  
Al agregar un punto de entrada nuevo a una implementación multisitio, hay que especificar el parámetro *RemoteAccessServer*, que es el nombre del servidor que se quiere agregar como nuevo punto de entrada.  
  
**Solución**  
  
Ejecute el comando y cuide de especificar el parámetro *RemoteAccessServer* con el nombre del servidor que se va a agregar como punto de entrada.  
  
## <a name="remote-access-is-not-configured"></a>Acceso remoto no está configurado  
**Recibidas el error**. Acceso remoto no está configurado en < nombreDeServidor >. Especifique el nombre de un servidor que forme parte de una implementación multisitio.  
  
**Causa**  
  
Acceso remoto no está configurado en el equipo especificado en el parámetro *ComputerName* o en el equipo en el que el comando se ha ejecutado.  
  
Al agregar una nuevo punto de entrada a una implementación multisitio, debe especificar dos parámetros: *ComputerName* y *RemoteAccessServer*. *ComputerName* es el nombre de un servidor que ya forma parte de la implementación multisitio, mientras que *RemoteAccessServer* es el nombre del servidor que se quiere agregar como nuevo punto de entrada. Si realiza este proceso en un equipo que forme parte de una implementación multisitio, el parámetro ComputerName no es necesario.  
  
**Solución**  
  
Ejecute el comando y cuide de especificar el parámetro *ComputerName* con el nombre del servidor que ya está configurado como parte de la implementación multisitio, o bien ejecute el comando desde un equipo que forme parte de la implementación multisitio.  
  
## <a name="multisite-not-enabled"></a>La funcionalidad de multisitio no está habilitada  
**Recibidas el error**. Debe habilitar una implementación multisitio antes de realizar esta operación. Para ello, use el cmdlet `Enable-DAMultiSite`.  
  
**Causa**  
  
La funcionalidad de multisitio no está habilitada en el servidor especificado en el parámetro *ComputerName*. Para agregar un nuevo punto de entrada a una implementación de Acceso remoto, antes hay que habilitar la funcionalidad de multisitio.  
  
**Solución**  
  
Habilite la funcionalidad de multisitio mediante el cmdlet `Enable-DaMultiSite`. Para obtener más información, consulte [implementar acceso remoto multisitio](https://technet.microsoft.com/library/hh831664.aspx).  
  
## <a name="ipv6-prefix-issues"></a>Problemas de prefijo IPv6  
  
-   **Problema 1**  
  
    **Recibidas el error**. IPv6 está implementado en la red interna, pero no ha especificado un prefijo de IPv6 del cliente.  
  
    **Causa**  
  
    IPv6 está implementado en la red corporativa, de modo que se necesita un prefijo IP-HTTPS, pero no se especificó ninguno en el parámetro *ClientIPv6Prefix* del nuevo punto de entrada.  
  
    **Solución**  
  
    1.  Asigne un prefijo IP-HTTPS único al nuevo punto de entrada y asegúrese de que los paquetes destinados a una dirección IP bajo este prefijo se van a enrutar al servidor que está agregando.  
  
    2.  Ejecute el cmdlet `Add-DAEntryPoint` e indique el prefijo IP-HTTPS en el parámetro *ClientIPv6Prefix*.  
  
-   **Issue 2**  
  
    **Recibidas el error**. El prefijo IPv6 del cliente ya está en uso por otro punto de entrada. Especifique un valor alternativo.  
  
    **Causa**  
  
    Ya hay otro punto de entrada usando el prefijo IP-HTTPS especificado en el parámetro *ClientIPv6Prefix*.  
  
    **Solución**  
  
    1.  Asigne un prefijo IP-HTTPS único al nuevo punto de entrada y asegúrese de que los paquetes destinados a una dirección IP bajo este prefijo se van a enrutar al servidor que está agregando.  
  
    2.  Ejecute el cmdlet `Add-DAEntryPoint` e indique el prefijo IP-HTTPS en el parámetro *ClientIPv6Prefix*.  
  
## <a name="connectto-address"></a>Dirección ConnectTo  
**Recibidas el error**. La dirección (< dirección_connect_to >) a la que los clientes de DirectAccess conectan en el servidor de acceso remoto es el mismo que la dirección del servidor de ubicación de red. Especifique un valor alternativo.  
  
**Causa**  
  
La dirección ConnectTo y la dirección del servidor de ubicación de red son la misma.  
  
**Solución**  
  
La dirección ConnectTo se debe poder resolver a través de Internet para que los equipos cliente puedan conectarse mediante IP-HTTPS. Sin embargo, la dirección del servidor de ubicación de red se debe poder resolver a través de la red corporativa, pero no a través de Internet. Asegúrese de que las direcciones ConnectTo y del servidor de ubicación de red son distintas. Seleccione otras direcciones e inténtelo de nuevo.  
  
## <a name="directaccess-or-vpn-already-installed"></a>DirectAccess o VPN ya instalados  
**Recibidas el error**. Se detectó una instalación de VPN en el servidor < nombre_servidor >. Especifique un servidor alternativo que no tenga instalado el acceso remoto o quite la configuración de VPN del servidor.  
  
O bien  
  
Acceso remoto ya está instalado en el servidor < nombreDeServidor >. Especifique un servidor alternativo que no ejecute DirectAccess o quite la configuración de DirectAccess existente del servidor.  
  
**Causa**  
  
DirectAccess o VPN ya se han configurado en el nuevo punto de entrada. No se puede agregar un punto de entrada configurado a una implementación multisitio.  
  
**Solución**  
  
Para agregar un servidor a una implementación multisitio, el rol Acceso remoto debe estar instalado en el servidor, pero ni DirectAccess ni VPN deben estar configurados.  
  
Ejecute el comando y asegúrese de que el servidor que indique en el parámetro *RemoteAccessServer* no tiene DirectAccess o VPN configurados.  
  
## <a name="ipsec-root-certificate"></a>Certificado raíz IPsec  
**Recibidas el error**. No se encuentra el certificado raíz IPsec configurado en el servidor < nombreDeServidor >.  
  
**Causa**  
  
El certificado de la entidad de certificación (CA) raíz o intermedia que emite certificados de equipo no se ha podido encontrar en el servidor que intenta agregar a la implementación.  
  
**Solución**  
  
En el área **Paso 2: Servidor de acceso remoto** de la Consola de administración de acceso remoto, haga clic en **Editar** y en la página **Autenticación**, en **Usar certificados de equipo**, confirme que el certificado seleccionado es válido. Si lo es, asegúrese de que está en la CA raíz de confianza del servidor que quiere agregar y, tras ello, inténtelo de nuevo.  
  
> [!NOTE]  
> Este certificado debe ser el mismo certificado y con la misma huella digital.  
  
Si no es válido, seleccione uno que lo sea y que esté configurado como CA raíz de confianza en todos los servidores de Acceso remoto.  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>Combinar puntos de entrada de IPv6 e IPv4  
Cuando DirectAccess se instala por primera vez, se inspecciona el adaptador de red interno para averiguar si la red contiene únicamente direcciones IPv4 (red solo para IPv4), direcciones IPv6 e IPv4 o únicamente direcciones IPv6 (red solo para IPv6). Esta información se usa para determinar el tipo de implementación (solo para IPv4, IPv6+IPv4 o solo para IPv6).  
  
-   **Problema 1**  
  
    **Advertencia**. El servidor de acceso remoto que se va a agregar está configurado con direcciones IPv4 e IPv6. Esta es una implementación solo para IPv4, por lo que Acceso remoto pasará por alto las direcciones IPv6.  
  
    **Causa**  
  
    Cuando esta implementación se instaló por primera vez, la red interna se detectó como una red solo para IPv4. En una implementación multisitio se da por hecho que los diversos puntos de entrada van a estar en subredes diferentes con características variadas. Por lo tanto, aunque la implementación está configurada como solo para IPv4, puede contener un punto de entrada ubicado en una subred de IPv6+IPv4. Sin embargo, aunque el punto de entrada se agregará a la implementación, DirectAccess omitirá las direcciones IPv6 configuradas en la interfaz interna del punto de entrada nueva.  
  
    **Solución**  
  
    En caso de que toda la red interna esté configurada con direcciones IPv6 e IPv4, considere la posibilidad de pasar a una implementación de IPv6+IPv4 para disfrutar de las ventajas que reportan las tecnologías de IPv6. Consulte la sección "Transición desde un IPv4 pura a una red corporativa de IPv6 + IPv4" en [paso 3: Planear una implementación multisitio](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d).  
  
-   **Issue 2**  
  
    **Recibidas el error**. Los adaptadores de red interna de los servidores de acceso remoto en esta implementación multisitio se configuran con direcciones IPv4. El punto de entrada que intenta agregar también debe estar configurado con una dirección IPv4 en el adaptador de red interno.  
  
    **Causa**  
  
    Cuando esta implementación se instaló por primera vez, la red interna se detectó como una red solo para IPv4. Acceso remoto detectó que el punto de entrada que intenta agregar está configurado exclusivamente con direcciones IPv6 en su red interna, lo cual no es factible en una implementación solo para IPv4.  
  
    **Solución**  
  
    En caso de que toda la red ya esté configurada con direcciones IPv6, convendría pasar a una implementación de IPv6+IPv4 o solo para IPv6. Vea "Planear la transición a IPv6 cuando acceso remoto multisitio está implementado".  
  
-   **Problema 3**  
  
    **Recibidas el error**. Este punto de entrada se encuentra en una red IPv4, pero los puntos de entrada anteriores se encuentran en una red IPv6. Conecte este punto de entrada a la red IPv6 antes de agregarlo a la misma implementación multisitio.  
  
    **Causa**  
  
    Cuando esta implementación se instaló por primera vez, se detectó que la red interna era IPv6+IPv4 o solo para IPv6. También se detectó que las direcciones del nuevo punto de entrada que intenta agregar son solo direcciones IPv4, lo cual no es factible en implementaciones de IPv6+IPv4 o solo para IPv6.  
  
    **Solución**  
  
    Configure el nuevo punto de entrada con direcciones IPv6 y, a continuación, agregue el punto de entrada a la implementación multisitio.  
  
-   **Problema 4**  
  
    **Advertencia**. El adaptador de red interna en el servidor de acceso remoto no está configurado con una dirección IPv4. DNS64 y NAT64 no se configurarán en este servidor y los clientes de DirectAccess solo podrán obtener acceso a servidores internos IPv6.  
  
    **Causa**  
  
    Cuando esta implementación se instaló por primera vez, se detectó que la red interna era IPv6+IPv4. En este tipo de implementación, DNS64 y NAT64 están habilitados para que los equipos cliente puedan acceder a los equipos de la red interna que están configurados únicamente con direcciones IPv4.  
  
    Al agregar el nuevo punto de entrada, Acceso remoto detectó que la interfaz interna en el nuevo equipo solo tiene direcciones IPv6. Para configurar DNS64 y NAT64 se necesita una dirección IPv4, ya que así los paquetes se pueden enrutar desde el servidor de acceso remoto al equipo solo para IPv4. Dado que dicha IP no existe en el nuevo equipo, NAT64 y DNS64 no se configurarán en el servidor de acceso remoto. En consecuencia, los equipos cliente que accedan a la red corporativa mediante DirectAccess a través de este punto de entrada no tendrán acceso a los servidores solo para IPv4 de la red interna. Para obtener información sobre cómo realizar una transición a una red de IPv6 + IPv4 o a una red de sólo IPv6, vea "Planear la transición a IPv6 cuando acceso remoto multisitio está implementado".  
  
    **Solución**  
  
    Agregue una dirección IPv4 al nuevo servidor de acceso remoto para garantizar que DNS64 y NAT64 funcionan correctamente.  
  
## <a name="domain-issues-with-the-servergponame"></a>Problemas de dominio con ServerGpoName  
  
-   **Problema 1**  
  
    **Recibidas el error**. El dominio especificado en el parámetro ServerGpoName < gpo_servidor > no existe. Especifique el dominio < nombreDeDominio > en su lugar.  
  
    **Causa**  
  
    No se encontró la parte del nombre de dominio del nombre de GPO del servidor que el administrador ha enviado.  
  
    **Solución**  
  
    Confirme que ha escrito bien el nombre de dominio. Si está bien escrito, inténtelo de nuevo usando el nombre de dominio completo (FQDN).  
  
-   **Issue 2**  
  
    **Recibidas el error**. El GPO de servidor debe encontrarse en el dominio del servidor de acceso remoto. Especifique el dominio < nombreDeDominio > en el parámetro ServerGpoName.  
  
    **Causa**  
  
    El dominio del GPO de servidor no es el mismo al que el servidor de acceso remoto pertenece.  
  
    **Solución**  
  
    El GPO de servidor debe estar en el mismo dominio que el servidor de acceso remoto. Use el nombre de dominio del servidor correspondiente al GPO de servidor e inténtelo de nuevo.  
  
## <a name="split-brain-dns"></a>DNS de cerebro dividido  
**Advertencia**. La entrada NRPT para el sufijo DNS < sufijoDNS > contiene el nombre público usado por los equipos cliente para conectarse al servidor de acceso remoto. Agregue el nombre < dirección_connect_to > como exención en NRPT.  
  
**Causa**  
  
Está usando DNS de cerebro dividido. Si quiere permitir que los clientes se conecten mediante IP-HTTPS, deberá asegurarse de que la dirección ConnectTo seleccionada esté exenta en las reglas NRPT.  
  
**Solución**  
  
Si tiene una implementación multisitio, confirme que todas las direcciones ConnectTo de los distintos puntos de entrada están exentas de las reglas NRPT.  
  
Para eximir una dirección en las reglas NRPT:  
  
1.  En el área **Paso 3: Servidores de infraestructura** de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
2.  En la página **DNS** del **Asistente para la instalación del servidor de infraestructura**, haga doble clic en la tabla para especificar un nuevo sufijo de nombre.  
  
3.  En el área de sufijo DNS del cuadro de diálogo **Direcciones de servidor DNS**, especifique la dirección ConnectTo del punto de entrada y haga clic en **Aplicar**.  
  
Cuando se agrega un sufijo de nombre sin indicar una dirección de servidor, dicho sufijo se considera una exención de NRPT.  
  
## <a name="saving-server-gpo-settings"></a>Guardar la configuración del GPO de servidor  
**Recibidas el error**. Se produjo un error al guardar la configuración de acceso remoto en el GPO < nombre_gpo >.  
  
Para solucionar este error, consulte guardar la configuración de GPO de servidor en [solución de problemas de habilitar la funcionalidad de multisitio](https://technet.microsoft.com/library/jj591658.aspx).  
  
## <a name="gpo-updates-cannot-be-applied"></a>No se pueden aplicar actualizaciones de GPO  
**Advertencia**. No se puede aplicar actualizaciones de GPO en < nombreDeServidor >. Los cambios no surtirán efecto hasta la siguiente actualización de directiva.  
  
**Causa**  
  
Se ha producido un error al intentar actualizar las directivas en el equipo especificado. Por lo tanto, los cambios realizados no surtirán efecto hasta la siguiente actualización de directiva.  
  
**Solución**  
  
Para aplicar una actualización de directiva, ejecute `gpupdate /force` en el equipo especificado.  
  

