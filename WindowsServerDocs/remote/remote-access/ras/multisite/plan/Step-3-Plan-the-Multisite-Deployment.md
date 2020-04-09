---
title: Paso 3 planeación de la implementación multisitio
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 97705e3d6f5a4300c32ec98cc59e849862381607
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858328"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Paso 3 planeación de la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura multisitio, planee los requisitos de certificado adicionales, el modo en que los equipos cliente seleccionan los puntos de entrada y las direcciones IPv6 asignadas en la implementación.  

En las secciones siguientes se proporciona información detallada sobre Planeación.
  
## <a name="31-plan-ip-https-certificates"></a><a name="bkmk_3_1_IPHTTPS"></a>3,1 planear certificados IP-HTTPS  
Al configurar los puntos de entrada, configure cada punto de entrada con una dirección ConnectTo específica. El certificado IP-HTTPS para cada punto de entrada debe coincidir con la dirección ConnectTo. Tenga en cuenta lo siguiente al obtener el certificado:  
  
-   Este tipo de certificados no se puede usar en una implementación multisitio.  
  
-   Se recomienda usar una entidad de certificación pública para que haya CRL disponibles.  
  
-   En el campo asunto, especifique la dirección IPv4 del adaptador externo del servidor de acceso remoto (si se ha especificado la dirección ConnectTo como una dirección IP y no un nombre DNS) o el FQDN de la dirección URL de IP-HTTPS.  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio web de IP-HTTPS. También se admite el uso de una dirección URL de comodín que coincida con el nombre DNS de ConnectTo.  
  
-   Los certificados IP-HTTPS pueden usar caracteres comodín en el nombre de sujeto. Se puede usar el mismo certificado comodín para todos los puntos de entrada.  
  
-   En el campo Uso mejorado de claves, use el identificador de objeto (OID) Autenticación de servidor.  
  
-   Si está admitiendo equipos cliente que ejecutan Windows 7 en la implementación multisitio, en el campo puntos de distribución CRL, especifique un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess conectados a Internet. Esto no es necesario para los clientes que ejecutan Windows 8 (de forma predeterminada, la comprobación de revocación de CRL está deshabilitada para IP-HTTPS en estos clientes).  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS se debe importar directamente en el almacén personal del equipo y no en el usuario.  
  
## <a name="32-plan-the-network-location-server"></a><a name="bkmk_3_2_NLS"></a>3,2 planear el servidor de ubicación de red  
El sitio web del servidor de ubicación de red se puede hospedar en el servidor de acceso remoto o en otro servidor de la organización. Si hospeda el servidor de ubicación de red en el servidor de acceso remoto, el sitio web se crea automáticamente al implementar el acceso remoto. Si hospeda el servidor de ubicación de red en otro servidor que ejecuta un sistema operativo de Windows en su organización, debe asegurarse de que Internet Information Services (IIS) está instalado para crear el sitio Web.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 requisitos de certificado para el servidor de ubicación de red  
Asegúrese de que el sitio web del servidor de ubicación de red cumpla los siguientes requisitos para la implementación de certificados:  
  
-   Requiere un certificado de servidor HTTPS.  
  
-   Si el servidor de ubicación de red se encuentra en el servidor de acceso remoto y ha seleccionado usar un certificado autofirmado al implementar el único servidor de acceso remoto, debe volver a configurar la implementación de un solo servidor para que use un certificado emitido por una CA interna.  
  
-   Los equipos cliente de DirectAccess deben confiar en la entidad de certificación que emitió el certificado de servidor para el sitio web del servidor de ubicación de red.  
  
-   Los equipos cliente de DirectAccess de la red interna deben poder resolver el nombre del sitio web del servidor de ubicación de red.  
  
-   El sitio web del servidor de ubicación de red debe tener una alta disponibilidad en los equipos de la red interna.  
  
-   El servidor de ubicación de red no debe ser accesible para los equipos cliente de DirectAccess en Internet.  
  
-   El certificado de servidor debe comprobarse con una lista de revocación de certificados (CRL).  
  
-   No se admiten certificados comodín cuando el servidor de ubicación de red se hospeda en el servidor de acceso remoto.  
  
A la hora de obtener el certificado de sitio web que se va a usar para el servidor de ubicación de red, tenga en cuenta lo siguiente:  
  
1.  En el campo Asunto, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red. Tenga en cuenta que no debe especificar una dirección IP si el servidor de ubicación de red se hospeda en el servidor de acceso remoto. Esto se debe a que el servidor de ubicación de red debe usar el mismo nombre de sujeto para todos los puntos de entrada y no todos los puntos de entrada tienen la misma dirección IP.  
  
2.  Para el campo Uso mejorado de clave, usa el OID Autenticación de servidor.  
  
3.  En el campo puntos de distribución CRL, use un punto de distribución CRL al que puedan tener acceso los clientes de DirectAccess que estén conectados a la intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2 DNS para el servidor de ubicación de red  
Si hospeda el servidor de ubicación de red en el servidor de acceso remoto, debe agregar una entrada DNS para el sitio web del servidor de ubicación de red para cada punto de entrada de la implementación. Observe lo siguiente:  
  
-   El nombre de sujeto del primer certificado de servidor de ubicación de red en la implementación multisitio se utiliza como dirección URL del servidor de ubicación de red para todos los puntos de entrada, por lo tanto, el nombre de sujeto y la dirección URL del servidor de ubicación de red no pueden ser iguales que el nombre de equipo del primer servidor de acceso remoto en la implementación. Debe ser un FQDN dedicado para el servidor de ubicación de red.  
  
-   El servicio proporcionado por el tráfico del servidor de ubicación de red se equilibra entre los puntos de entrada mediante DNS y, por lo tanto, debe haber una entrada DNS con la misma dirección URL para cada punto de entrada, configurada con la dirección IP interna del punto de entrada.  
  
-   Todos los puntos de entrada deben configurarse con un certificado de servidor de ubicación de red con el mismo nombre de sujeto (que coincida con la dirección URL del servidor de ubicación de red).  
  
-   La infraestructura del servidor de ubicación de red (configuración de certificados y DNS) para un punto de entrada debe crearse antes de agregar el punto de entrada.  
  
## <a name="33-plan-the-ipsec-root-certificate-for-all-remote-access-servers"></a><a name="bkmk_3_3_IPsec"></a>3,3 planear el certificado raíz de IPsec para todos los servidores de acceso remoto  
Tenga en cuenta lo siguiente al planear la autenticación del cliente de IPsec en una implementación multisitio:  
  
1.  Si optó por usar el proxy Kerberos integrado para la autenticación de equipos al configurar el único servidor de acceso remoto, debe cambiar la configuración para usar certificados de equipo emitidos por una CA interna, ya que el proxy Kerberos no es compatible con un multisitio planta.  
  
2.  Si ha usado un certificado autofirmado, debe volver a configurar la implementación de un solo servidor para que use un certificado emitido por una CA interna.  
  
3.  Para que la autenticación de IPsec se realice correctamente durante la autenticación del cliente, todos los servidores de acceso remoto deben tener un certificado emitido por la entidad de certificación IPsec o la raíz intermedia, y con el OID de autenticación del cliente para el uso mejorado de claves.  
  
4.  El mismo certificado de raíz IPsec o intermedio debe estar instalado en todos los servidores de acceso remoto de la implementación multisitio.  
  
## <a name="34-plan-global-server-load-balancing"></a><a name="bkmk_3_4_GSLB"></a>3,4 planear el equilibrio de carga del servidor global  
En una implementación multisitio, también puede configurar un equilibrador de carga de servidor global. Un equilibrador de carga de servidor global puede ser útil para su organización si la implementación cubre una distribución geográfica grande, ya que puede distribuir la carga del tráfico entre los puntos de entrada.  El equilibrador de carga global del servidor se puede configurar para proporcionar a los clientes de DirectAccess la información de punto de entrada del punto de entrada más cercano. El proceso funciona de la siguiente manera:  
  
1.  Los equipos cliente que ejecutan Windows 10 o Windows 8 tienen una lista de direcciones IP del equilibrador de carga del servidor global, cada una asociada a un punto de entrada.  
  
2.  El equipo cliente de Windows 10 o Windows 8 intenta resolver el FQDN del equilibrador de carga del servidor global en el DNS público en una dirección IP. Si la dirección IP resuelta aparece como la dirección IP del equilibrador de carga del servidor global de un punto de entrada, el equipo cliente selecciona automáticamente ese punto de entrada y se conecta a su dirección URL de IP-HTTPS (dirección ConnectTo) o a la dirección IP del servidor Teredo. Tenga en cuenta que la dirección IP del equilibrador de carga del servidor global no tiene que ser idéntica a la dirección ConnectTo o a la dirección del servidor Teredo del punto de entrada, ya que los equipos cliente nunca intentan conectarse a la dirección IP del equilibrador de carga del servidor global.  
  
3.  Si el equipo cliente está detrás de un proxy web (y no puede usar la resolución DNS) o si el FQDN del equilibrador de carga del servidor global no se resuelve en ninguna dirección IP del equilibrador de carga del servidor global configurada, se seleccionará automáticamente un punto de entrada con un sondeo HTTPS para Direcciones URL IP-HTTPS de todos los puntos de entrada. El cliente se conectará al servidor que responda primero.  
  
Para obtener una lista de los dispositivos globales de equilibrio de carga del servidor que admiten el acceso remoto, vaya a la página buscar un partner en [Microsoft Server y en la plataforma en la nube](https://www.microsoft.com/server-cloud/).  
  
## <a name="35-plan-directaccess-client-entry-point-selection"></a><a name="bkmk_3_5_EP_Selection"></a>3,5 planear la selección del punto de entrada de cliente de DirectAccess  
Al configurar una implementación multisitio, de forma predeterminada, los equipos cliente de Windows 10 y Windows 8 se configuran con la información necesaria para conectarse a todos los puntos de entrada de la implementación y para conectarse automáticamente a un único punto de entrada en función de una selección. algoritmo. También puede configurar la implementación para permitir que los equipos cliente de Windows 10 y Windows 8 seleccionen manualmente el punto de entrada al que se conectarán. Si un equipo cliente de Windows 10 o Windows 8 está conectado actualmente al punto de entrada de Estados Unidos y la selección de punto de entrada automático está habilitada, si el punto de entrada de Estados Unidos deja de estar disponible, después de unos minutos el equipo cliente intentará conectarse. a través del punto de entrada de Europa. Se recomienda usar la selección automática de puntos de entrada; sin embargo, permitir la selección manual de puntos de entrada permite que los usuarios finales se conecten a un punto de entrada diferente en función de las condiciones de la red actual. Por ejemplo, si un equipo está conectado al punto de entrada de Estados Unidos y la conexión a la red interna se vuelve mucho más lenta de lo esperado. En esta situación, el usuario final puede seleccionar manualmente para conectarse al punto de entrada de Europa para mejorar la conexión a la red interna.  
  
> [!NOTE]  
> Después de que un usuario final Seleccione un punto de entrada manualmente, el equipo cliente no volverá a la selección automática de puntos de entrada. Es decir, si el punto de entrada seleccionado manualmente deja de estar disponible, el usuario final debe revertir a la selección de punto de entrada automático o seleccionar manualmente otro punto de entrada.  
  
 Los equipos cliente de Windows 7 se configuran con la información necesaria para conectarse a un único punto de entrada en la implementación multisitio. No pueden almacenar la información de varios puntos de entrada simultáneamente. Por ejemplo, se puede configurar un equipo cliente de Windows 7 para que se conecte al punto de entrada de Estados Unidos, pero no al punto de entrada de Europa. Si el punto de entrada de Estados Unidos es inaccesible, el equipo cliente de Windows 7 perderá la conectividad a la red interna hasta que el punto de entrada sea accesible. El usuario final no puede realizar ningún cambio para intentar conectarse al punto de entrada de Europa.  
  
## <a name="36-plan-prefixes-and-routing"></a><a name="bkmk_3_6_IPv6"></a>3,6 prefijos y enrutamiento del plan  
  
### <a name="internal-ipv6-prefix"></a>Prefijo IPv6 interno  
Durante la implementación del único servidor de acceso remoto planeado, los prefijos IPv6 de la red interna tienen en cuenta lo siguiente en una implementación multisitio:  
  
1.  Si incluyó todos los sitios de Active Directory al configurar la implementación de acceso remoto de un solo servidor, los prefijos IPv6 de la red interna ya estarán definidos en la consola de administración de acceso remoto.  
  
2.  Si crea sitios de Active Directory adicionales para la implementación multisitio, debe planear los nuevos prefijos IPv6 para los sitios adicionales y definirlos en el acceso remoto. Tenga en cuenta que los prefijos IPv6 solo se pueden configurar mediante la consola de administración de acceso remoto o los cmdlets de PowerShell si IPv6 está implementado en la red corporativa interna.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Prefijo IPv6 para equipos cliente de DirectAccess (prefijo IP-HTTPS)  
  
1.  Si IPv6 está implementado en la red corporativa interna, debe planear un prefijo IPv6 para asignarlo a los equipos cliente de DirectAccess en cualquier punto de entrada adicional de la implementación.  
  
2.  Asegúrese de que los prefijos IPv6 que se asignan a los equipos cliente de DirectAccess en cada punto de entrada son distintos y que no hay ninguna superposición en los prefijos IPv6.  
  
3.  Si IPv6 no está implementado en la red corporativa, se seleccionará automáticamente un prefijo IP-HTTPS para cada punto de entrada al agregar el punto de entrada.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Prefijo IPv6 para clientes VPN  
Si ha implementado VPN en el único servidor de acceso remoto, tenga en cuenta lo siguiente:  
  
1.  Solo es necesario agregar un prefijo de VPN IPv6 a un punto de entrada si desea permitir la conectividad IPv6 del cliente VPN con la red corporativa.  
  
2.  El prefijo de VPN solo se puede configurar en un punto de entrada mediante la consola de administración de acceso remoto o el cmdlet de PowerShell si IPv6 está implementado en la red corporativa interna y si la VPN está habilitada en el punto de entrada.  
  
3.  El prefijo VPN debe ser único en cada punto de entrada y no debe superponerse con otros prefijos VPN o IP-HTTPS.  
  
4.  Si IPv6 no está implementado en la red corporativa, no se asignará una dirección IPv6 a los clientes VPN que se conecten al punto de entrada.  
  
### <a name="routing"></a>Enrutamiento  
En una implementación multisitio, el enrutamiento simétrico se aplica mediante Teredo e IP-HTTPS. Cuando se implemente IPv6 en la red corporativa, tenga en cuenta lo siguiente:  
  
1. Los prefijos Teredo e IP-HTTPS de cada punto de entrada deben enrutarse a través de la red corporativa a su servidor de acceso remoto asociado.  
  
2. Las rutas deben configurarse en la infraestructura de enrutamiento de la red corporativa.  
  
3. En cada punto de entrada debe haber una o tres rutas en la red interna:  
  
   1. Prefijo IP-HTTPS: el administrador elige este prefijo en el Asistente para agregar un punto de entrada.  
  
   2. Prefijo IPv6 de VPN (opcional). Este prefijo se puede elegir después de habilitar la VPN para un punto de entrada  
  
   3. Prefijo Teredo (opcional). Este prefijo solo es relevante si el servidor de acceso remoto está configurado con dos direcciones IPv4 públicas consecutivas en el adaptador externo. El prefijo se basa en la primera dirección IPv4 pública del par de direcciones. Por ejemplo, si las direcciones externas son:  
  
      1. www\.xxx. yyy. zzz  
  
      2. www\.xxx. yyy. zzz + 1  
  
      Después, el prefijo Teredo para configurar es 2001:0: WWXX: YYZZ::/64, donde WWXX: YYZZ es la representación hexadecimal de la dirección IPv4 www\.xxx. yyy. zzz.  
  
      Tenga en cuenta que puede usar el siguiente script para calcular el prefijo Teredo:  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Todas las rutas anteriores deben enrutarse a la dirección IPv6 en el adaptador interno del servidor de acceso remoto (o a la dirección IP virtual interna (VIP) para un punto de entrada con equilibrio de carga).  
  
> [!NOTE]  
> Cuando se implementa IPv6 en la red corporativa y la administración del servidor de acceso remoto se realiza de forma remota a través de DirectAccess, se deben agregar rutas para los prefijos de Teredo e IP-HTTPS de todos los demás puntos de entrada a cada servidor de acceso remoto para que el tráfico se se reenvía a la red interna.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory prefijos IPv6 específicos del sitio  
Cuando un equipo cliente que ejecuta Windows 10 o Windows 8 está conectado a un punto de entrada, el equipo cliente se asocia inmediatamente al sitio de Active Directory del punto de entrada y se configura con prefijos IPv6 asociados al punto de entrada. La preferencia es que los equipos cliente se conecten a los recursos mediante estos prefijos IPv6, ya que se configuran dinámicamente en la tabla de directivas de prefijo IPv6 con mayor precedencia al conectarse a un punto de entrada.  
  
Si su organización usa una topología de Active Directory con prefijos IPv6 específicos del sitio (por ejemplo, un FQDN de recurso interno app.corp.com se hospeda en Norteamérica y en Europa con una dirección IP específica del sitio en cada ubicación), no lo configura. de forma predeterminada, el uso de la consola de acceso remoto y los prefijos IPv6 específicos del sitio no se configuran para cada punto de entrada. Si desea habilitar este escenario opcional, debe configurar cada punto de entrada con los prefijos IPv6 específicos que deberían ser preferidos por los equipos cliente que se conectan a un punto de entrada específico. Haga esto de la siguiente manera:  
  
1.  Para cada GPO usado para equipos cliente de Windows 10 o Windows 8, ejecute el cmdlet de PowerShell Set-DAEntryPointTableItem.  
  
2.  Establezca el parámetro EntryPointRange para el cmdlet con los prefijos IPv6 específicos del sitio. Por ejemplo, para agregar los prefijos específicos del sitio 2001: db8:1: 1::/64 y 2001: DB: 1:2::/64 a un punto de entrada denominado Europe, ejecute lo siguiente  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Al modificar el parámetro EntryPointRange, asegúrese de que no quita los prefijos de 128 bits existentes que pertenecen a los extremos del túnel IPsec y la dirección DNS64.  
  
## <a name="37-plan-the-transition-to-ipv6-when-multisite-remote-access-is-deployed"></a><a name="bkmk_3_7_TransitionIPv6"></a>3,7 planear la transición a IPv6 cuando se implementa el acceso remoto multisitio  
Muchas organizaciones usan el protocolo IPv4 en la red corporativa. Con el agotamiento de los prefijos IPv4 disponibles, muchas organizaciones están realizando la transición de solo IPv4 a redes solo IPv6.  
  
Es más probable que esta transición tenga lugar en dos fases:  
  
1.  Desde IPv4 solo a una red corporativa IPv6 + IPv4.  
  
2.  De IPv6 + IPv4 a una red corporativa solo IPv6.  
  
En cada parte, la transición se puede realizar en fases. En cada fase solo se puede cambiar una subred de la red a la nueva configuración de red. Por lo tanto, se requiere una implementación multisitio de DirectAccess para admitir una implementación híbrida donde, por ejemplo, algunos de los puntos de entrada pertenezcan a una subred solo IPv4 y otros pertenezcan a una subred IPv6 + IPv4. Además, los cambios de configuración durante los procesos de transición no deben interrumpir la conectividad de cliente a través de DirectAccess.  
  
### <a name="transition-from-an-ipv4-only-to-an-ipv6ipv4-corporate-network"></a><a name="TransitionIPv4toMixed"></a>Transición de una red corporativa IPv6 + IPv4 solo a IPv4  
Al agregar direcciones IPv6 a una red corporativa solo IPv4, puede que desee agregar una dirección IPv6 a un servidor de DirectAccess ya implementado. Además, puede que desee agregar un punto de entrada o un nodo a un clúster con equilibrio de carga con direcciones IPv4 e IPv6 a la implementación de DirectAccess.  
  
El acceso remoto permite agregar servidores con direcciones IPv4 e IPv6 a una implementación que se configuró originalmente solo con direcciones IPv4. Estos servidores se agregan como servidores de solo IPv4 y DirectAccess omite sus direcciones IPv6. por lo tanto, su organización no puede aprovechar las ventajas de la conectividad IPv6 nativa en estos nuevos servidores.  
  
Para transformar la implementación en una implementación de IPv6 + IPv4 y aprovechar las capacidades nativas de IPv6, debe volver a instalar DirectAccess. Para mantener la conectividad de cliente a través de la reinstalación, consulte transición de una implementación de IPv4 solo a una implementación de solo IPv6 con dos implementaciones de DirectAccess.  
  
> [!NOTE]  
> Al igual que con una red IPv4, en una red IPv4 + IPv6 mixta, la dirección del servidor DNS que se usa para resolver las solicitudes DNS del cliente debe configurarse con el DNS64 que se implementa en los propios servidores de acceso remoto y no con un DNS corporativo.  
  
### <a name="transition-from-an-ipv6ipv4-to-an-ipv6-only-corporate-network"></a><a name="TransitionMixedtoIPv6"></a>Transición de IPv6 + IPv4 a una red corporativa solo IPv6  
DirectAccess permite agregar únicamente puntos de entrada de IPv6 si el primer servidor de acceso remoto de la implementación tenía originalmente direcciones IPv4 e IPv6, o solo una dirección IPv6. Es decir, no se puede realizar la transición desde una red solo IPv4 a una red solo IPv6 en un solo paso sin volver a instalar DirectAccess. Para realizar la transición directamente desde una red solo IPv4 a una red solo IPv6, consulte transición de una implementación solo IPv4 a una implementación de solo IPv6 con dos implementaciones de DirectAccess.  
  
Después de haber completado la transición de una implementación solo IPv4 a una implementación de IPv6 + IPv4, puede realizar la transición a una red solo IPv6. Durante y después de la transición, tenga en cuenta lo siguiente:  
  
-   Si los servidores back-end de IPv4 solo permanecen en la red corporativa, no serán accesibles para los clientes que se conectan a través de los puntos de entrada solo IPv6.  
  
-   Al agregar puntos de entrada solo IPv6 a una implementación de IPv4 + IPv6, DNS64 y NAT64 no estarán habilitados en los nuevos servidores. Los clientes que se conectan a estos puntos de entrada se configurarán automáticamente para usar los servidores DNS corporativos.  
  
-   Si necesita eliminar direcciones IPv4 de un servidor implementado, debe quitar el servidor de la implementación de DirectAccess, quitar su dirección de red corporativa IPv4 y volver a agregarla a la implementación.  
  
Para admitir la conectividad de cliente a la red corporativa, debe asegurarse de que el servidor DNS corporativo puede resolver el servidor de ubicación de red en su dirección IPv6. También se puede establecer una dirección IPv4 adicional, pero no es necesario.  
  
### <a name="transition-from-an-ipv4-only-to-an-ipv6-only-deployment-using-dual-directaccess-deployments"></a><a name="DualDeployment"></a>Transición de solo IPv4 a una implementación de solo IPv6 mediante implementaciones de DirectAccess duales  
La transición de solo IPv4 a una red corporativa de solo IPv6 no se puede realizar sin volver a instalar la implementación de DirectAccess. Para mantener la conectividad de cliente durante la transición, puede usar otra implementación de DirectAccess. Se requiere una implementación dual cuando finaliza la primera fase de transición (la red solo IPv4 se actualiza a IPv4 + IPv6) y tiene previsto prepararse para una transición futura a una red corporativa solo de IPv6 y aprovechar las ventajas de conectividad IPv6 nativa. La implementación dual se describe en los siguientes pasos generales:  
  
1.  Instale una segunda implementación de DirectAccess. Puede instalar DirectAccess en nuevos servidores o quitar servidores de la primera implementación y usarlos para la segunda implementación.  
  
    > [!NOTE]  
    > Al instalar una implementación de DirectAccess adicional junto con una actual, asegúrese de que no haya dos puntos de entrada que compartan el mismo prefijo de cliente.  
    >   
    > Si instala DirectAccess mediante el Asistente para Introducción o con el cmdlet `Install-RemoteAccess`, el acceso remoto establece automáticamente el prefijo de cliente del primer punto de entrada de la implementación en un valor predeterminado de < subred IPv6\_prefijo >: 1000::/64. Si es necesario, debe cambiar el prefijo.  
  
2.  Quite los grupos de seguridad de cliente elegidos de la primera implementación.  
  
3.  Agregue los grupos de seguridad de cliente a la segunda implementación.  
  
    > [!IMPORTANT]  
    > Para mantener la conectividad de cliente a lo largo del proceso, debe agregar los grupos de seguridad a la segunda implementación inmediatamente después de quitarlos de la primera. Esto garantiza que los clientes no se actualizarán con dos GPO de DirectAccess o ninguno. Los clientes comenzarán a usar la segunda implementación una vez que recuperen y actualicen el GPO de cliente.  
  
4.  Opcional: Quite los puntos de entrada de DirectAccess de la primera implementación y agregue esos servidores como nuevos puntos de entrada en el segundo.  
  
Cuando haya completado la transición, puede desinstalar la primera implementación de DirectAccess. Al desinstalar, pueden producirse los siguientes problemas:  
  
-   Si la implementación se configuró para admitir solo clientes en equipos móviles, se eliminará el filtro WMI. Si los grupos de seguridad de cliente de la segunda implementación incluyen equipos de escritorio, el GPO de cliente de DirectAccess no filtrará los equipos de escritorio y puede causar problemas. Si se necesita un filtro de equipos móviles, vuelva a crearlo siguiendo las instrucciones de [creación de filtros WMI para el GPO](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Si ambas implementaciones se crearon originalmente en el mismo dominio de Active Directory, la entrada de sondeo de DNS que señala al localhost se eliminará y puede causar problemas de conectividad de cliente. Por ejemplo, los clientes pueden conectarse mediante IP-HTTPS en lugar de Teredo o cambiar entre puntos de entrada multisitio de DirectAccess. En este caso, debe agregar la siguiente entrada DNS al DNS corporativo:  
  
    -   Zona: nombre de dominio  
  
    -   Nombre: DirectAccess-corpConnectivityHost  
  
    -   Dirección IP::: 1  
  
    -   Tipo: AAAA  
  
  
  


