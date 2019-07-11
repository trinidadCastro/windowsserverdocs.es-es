---
title: Paso 3 Plan la implementación multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 973ef70614f056adac1463918cc425d82b21ac62
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792312"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Paso 3 Plan la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura de multisitio, planear los requisitos de certificado adicionales, cómo los equipos cliente seleccionan puntos de entrada y las direcciones IPv6 asignadas en la implementación.  

Las secciones siguientes proporcionan información detallada.
  
## <a name="bkmk_3_1_IPHTTPS"></a>Certificados IP-HTTPS planear 3.1  
Al configurar los puntos de entrada, configure cada punto de entrada con una dirección de ConnectTo específica. El certificado IP-HTTPS para cada punto de entrada debe coincidir con la dirección ConnectTo. Al obtener el certificado, tenga en cuenta lo siguiente:  
  
-   Este tipo de certificados no se puede usar en una implementación multisitio.  
  
-   Se recomienda usar una entidad de certificación pública para que haya CRL disponibles.  
  
-   En el campo Asunto, especifique la dirección IPv4 del adaptador externo del servidor de acceso remoto (si se ha especificado la dirección ConnectTo como una dirección IP y no es un nombre DNS) o el FQDN de la dirección URL IP-HTTPS.  
  
-   El nombre común del certificado debe coincidir con el nombre del sitio Web IP-HTTPS. También se admite el uso de una dirección URL de carácter comodín que coincide con el nombre DNS de ConnectTo.  
  
-   Certificados IP-HTTPS pueden usar caracteres comodín en el nombre del sujeto. Puede utilizarse el mismo certificado comodín para todos los puntos de entrada.  
  
-   En el campo Uso mejorado de claves, use el identificador de objeto (OID) Autenticación de servidor.  
  
-   Si son compatibles con equipos cliente que ejecutan Windows 7 en la implementación multisitio, en el campo puntos de distribución CRL, especifique un punto de distribución de CRL que sea accesible para los clientes de DirectAccess que están conectados a Internet. Esto no es necesario para los clientes que ejecutan Windows 8 (de forma predeterminada que la comprobación de revocación CRL está deshabilitada para IP-HTTPS en estos clientes).  
  
-   El certificado IP-HTTPS debe tener una clave privada.  
  
-   El certificado IP-HTTPS debe importarse directamente en el almacén personal del equipo y no del usuario.  
  
## <a name="bkmk_3_2_NLS"></a>3.2 Planear el servidor de ubicación de red  
El sitio Web servidor de ubicación de red puede hospedarse en el servidor de acceso remoto o en otro servidor en su organización. Si hospeda el servidor de ubicación de red en el servidor de acceso remoto, el sitio Web se crea automáticamente al implementar acceso remoto. Si hospeda el servidor de ubicación de red en otro servidor que ejecuta un sistema operativo de Windows en su organización, debe asegurarse de que está instalado Internet Information Services (IIS) para crear el sitio Web.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 requisitos de certificados para el servidor de ubicación de red  
Asegúrese de que el sitio Web servidor de ubicación de red cumple los siguientes requisitos de implementación de certificado:  
  
-   Requiere un certificado de servidor HTTPS.  
  
-   Si el servidor de ubicación de red se encuentra en el servidor de acceso remoto, y ha seleccionado para usar un certificado autofirmado cuando implementó el servidor de acceso remoto único, debe volver a configurar la implementación de servidor único para usar un certificado emitido por una CA interna.  
  
-   Los equipos cliente de DirectAccess deben confiar en la entidad de certificación que emitió el certificado de servidor para el sitio web del servidor de ubicación de red.  
  
-   Los equipos cliente de DirectAccess en la red interna deben ser capaces de resolver el nombre del sitio Web del servidor de ubicación de red.  
  
-   El sitio Web servidor de ubicación de red debe tener alta disponibilidad a los equipos de la red interna.  
  
-   El servidor de ubicación de red no debe ser accesible para los equipos cliente de DirectAccess en Internet.  
  
-   El certificado de servidor se debe proteger frente a una lista de revocación de certificados (CRL).  
  
-   No se admiten certificados comodín cuando se hospeda el servidor de ubicación de red en el servidor de acceso remoto.  
  
Al obtener el certificado del sitio Web que se usará para el servidor de ubicación de red, tenga en cuenta lo siguiente:  
  
1.  En el campo Asunto, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red. Tenga en cuenta que no debe especificar una dirección IP si el servidor de ubicación de red está hospedado en el servidor de acceso remoto. Esto es porque el servidor de ubicación de red debe usar el mismo nombre de sujeto para todos los puntos de entrada y no todos los puntos de entrada tienen la misma dirección IP.  
  
2.  Para el campo Uso mejorado de clave, usa el OID Autenticación de servidor.  
  
3.  Para el campo puntos de distribución CRL, use un punto de distribución de CRL que sea accesible para los clientes de DirectAccess que están conectados a la intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2DNS para el servidor de ubicación de red  
Si hospeda el servidor de ubicación de red en el servidor de acceso remoto, debe agregar una entrada DNS para el sitio Web servidor de ubicación de red para cada punto de entrada en la implementación. Tenga en cuenta lo siguiente:  
  
-   El nombre del sujeto de la primer certificado de servidor de ubicación de red en la implementación multisitio se usa como dirección URL del servidor de ubicación de red para todos los puntos de entrada, por lo tanto, el nombre del sujeto y la URL del servidor de ubicación de red no pueden ser el mismo que el nombre del equipo de la primer servidor de acceso remoto en la implementación. Debe ser un FQDN para el servidor de ubicación de red dedicado.  
  
-   El servicio proporcionado por el tráfico del servidor de ubicación de red se equilibra entre los puntos de entrada mediante DNS y, por lo tanto, debe haber una entrada DNS con la misma dirección URL para cada punto de entrada, configurada con la dirección IP interna del punto de entrada.  
  
-   Todos los puntos de entrada deben configurarse con un certificado de servidor de ubicación de red con el mismo nombre de sujeto (que coincide con la URL del servidor de ubicación de red).  
  
-   La infraestructura de servidor de ubicación de red (configuración de DNS y el certificado) para un punto de entrada debe crearse antes de agregar el punto de entrada.  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 planear el certificado raíz IPsec para todos los servidores de acceso remoto  
Al planear la autenticación de cliente de IPsec en una implementación multisitio, tenga en cuenta lo siguiente:  
  
1.  Si optó por usar al proxy Kerberos integrado para la autenticación de equipo cuando se configura el servidor de acceso remoto único, debe cambiar la configuración para usar certificados de equipo, emitidos por una CA interna, puesto que no se admite el proxy Kerberos para una funcionalidad de multisitio despliegue.  
  
2.  Si usa un certificado autofirmado, debe volver a configurar la implementación de servidor único para usar un certificado emitido por una CA interna.  
  
3.  Para que la autenticación de IPsec se realice correctamente durante la autenticación de cliente, todos los servidores de acceso remoto deben tener un certificado emitido por la raíz de IPsec o una entidad de certificación intermedia y con el OID autenticación de cliente para el uso mejorado de clave.  
  
4.  La misma raíz de IPsec o un certificado intermedio debe instalarse en todos los servidores de acceso remoto en la implementación multisitio.  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 planear el equilibrio de carga de servidor global  
En una implementación multisitio, además puede configurar un equilibrador de carga global del servidor. Un equilibrador de carga global del servidor puede ser útil para su organización si la implementación abarca una gran distribución geográfica porque puede distribuir la carga de tráfico entre los puntos de entrada.  El equilibrador de carga global del servidor puede configurarse para proporcionar a los clientes de DirectAccess con la información de punto de entrada del punto de entrada más cercano. El proceso funciona del siguiente modo:  
  
1.  Equipos cliente que ejecutan Windows 10 o Windows 8 tienen una lista de direcciones IP de equilibrador de carga global del servidor, cada uno asociado con un punto de entrada.  
  
2.  El equipo cliente de Windows 10 o Windows 8 intenta resolver el FQDN del equilibrador de carga global del servidor en el DNS público a una dirección IP. Si la dirección IP resuelta aparece como la dirección IP de equilibrador de carga global del servidor de un punto de entrada, el equipo cliente automáticamente selecciona dicho punto de entrada y se conecta a su dirección URL de IP-HTTPS (dirección ConnectTo), así como su dirección IP del servidor Teredo. Tenga en cuenta que la dirección IP del equilibrador de carga global del servidor no debe ser idéntica a la dirección ConnectTo o a la dirección del servidor Teredo de punto de entrada, ya que los equipos cliente intentan conectarse a la dirección IP del equilibrador de carga global del servidor.  
  
3.  Si el equipo cliente está detrás de un proxy web (y no puede utilizar la resolución DNS), o si el FQDN del equilibrador de carga global del servidor no se resuelve en alguno configurado la dirección IP de equilibrador de carga global del servidor y, después, un punto de entrada se seleccionará automáticamente con un sondeo HTTPS las direcciones URL de IP-HTTPS de todos los puntos de entrada. El cliente se conectará al servidor que responda primero.  
  
Para obtener una lista de dispositivos que admiten el acceso remoto de equilibrio de carga global del servidor, vaya a la búsqueda de una página de socios en [Microsoft Server y plataforma de nube](https://www.microsoft.com/server-cloud/).  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 planear la selección de punto de entrada de cliente de DirectAccess  
Al configurar una implementación multisitio, de forma predeterminada, Windows 10 y se configuran los equipos cliente de Windows 8 con la información necesaria para conectarse a todos los puntos de entrada en la implementación y conectarse automáticamente a un único punto de entrada basado en una selección algoritmo. También puede configurar la implementación para permitir que Windows 10 y seleccione los equipos cliente de Windows 8 para seleccionar manualmente la entrada que se conectará. Si un equipo cliente de Windows 10 o Windows 8 está conectado actualmente al punto de entrada de Estados Unidos y selección del punto de entrada automática está habilitada, si el punto de entrada de Estados Unidos se convierte en inalcanzable, después de unos minutos el equipo cliente intentará conectarse a través del punto de entrada de Europa. Se recomienda utilizar la selección del punto de inserción automática; Sin embargo, lo que permite el punto de entrada manual de selección de los usuarios finales pueden conectarse a un punto de entrada diferentes según las condiciones de red actual. Por ejemplo, si un equipo está conectado a la conexión a la red interna y el punto de entrada de Estados Unidos se vuelve mucho más lento de lo esperado. En esta situación, el usuario final puede seleccionar manualmente para conectarse al punto de entrada de Europa para mejorar la conexión a la red interna.  
  
> [!NOTE]  
> Después de que un usuario final selecciona manualmente un punto de entrada, el equipo cliente no volverá a la selección de punto de inserción automática. Es decir, si el punto de entrada seleccionados manualmente se convierte en inalcanzable, el usuario final debe revertir a la selección de punto de entrada automática, o seleccione otro punto de entrada de manualmente.  
  
 Los equipos cliente de Windows 7 se configuran con la información necesaria para conectarse a un único punto de entrada en la implementación multisitio. No se almacenan la información de varios puntos de entrada al mismo tiempo. Por ejemplo, un equipo cliente de Windows 7 puede configurarse para conectar con el punto de entrada de Estados Unidos, pero no el punto de entrada de Europa. Si el punto de entrada de Estados Unidos es inaccesible, el equipo cliente Windows 7 pierde la conectividad a la red interna hasta que se puede alcanzar el punto de entrada. El usuario final no puede realizar los cambios para intentar conectarse al punto de entrada de Europa.  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 planear los prefijos y enrutamiento  
  
### <a name="internal-ipv6-prefix"></a>Prefijo IPv6 interno  
Durante la implementación del acceso remoto único servidor planeado los prefijos de IPv6 de la red interna, tenga en cuenta lo siguiente en una implementación multisitio:  
  
1.  Si incluye todos los sitios de Active Directory cuando se ha configurado el servidor único de implementación de acceso remoto, los prefijos de IPv6 de la red interna ya se definirá en la consola de administración de acceso remoto.  
  
2.  Si crea sitios de Active Directory adicionales para la implementación multisitio, debe planear nuevos prefijos IPv6 para los sitios adicionales y definirlos en el acceso remoto. Tenga en cuenta que los prefijos IPv6 solo se pueden configurar mediante la consola de administración de acceso remoto o los cmdlets de PowerShell si IPv6 está implementado en la red corporativa interna.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Prefijo IPv6 de los equipos cliente de DirectAccess (prefijo IP-HTTPS)  
  
1.  Si IPv6 está implementado en la red corporativa interna, debe planear un prefijo IPv6 para asignar a los equipos cliente de DirectAccess en los puntos de entrada adicionales en la implementación.  
  
2.  Asegúrese de que los prefijos IPv6 para asignar a los equipos cliente de DirectAccess en cada punto de entrada son distintos y que no hay ninguna superposición en los prefijos IPv6.  
  
3.  Si no se implementa IPv6 en la red corporativa, un prefijo IP-HTTPS para cada punto de entrada se seleccionará automáticamente al agregar el punto de entrada.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Prefijo IPv6 para clientes de VPN  
Si implementa VPN en el servidor de acceso remoto único, tenga en cuenta lo siguiente:  
  
1.  Adición de una VPN de IPv6 prefijo para un punto de entrada es solo es necesario si desea permitir que el cliente VPN conectividad IPv6 a la red corporativa.  
  
2.  El prefijo VPN solo puede configurarse en un punto de entrada mediante la consola de administración de acceso remoto o el cmdlet de PowerShell si IPv6 está implementado en la red corporativa interna, y si VPN está habilitada en el punto de entrada.  
  
3.  El prefijo VPN debe ser único en cada punto de entrada y no debe superponerse con otros prefijos IP-HTTPS o VPN.  
  
4.  Si no se implementa IPv6 en la red corporativa, los clientes VPN para conectarse al punto de entrada no se asignará una dirección IPv6.  
  
### <a name="routing"></a>Enrutamiento  
En una implementación multisitio enrutamiento simétrico se aplica mediante Teredo e IP-HTTPS. Cuando se implementa IPv6 en la red corporativa, tenga en cuenta lo siguiente:  
  
1. Los prefijos IP-HTTPS y Teredo de cada punto de entrada deben enrutarse a través de la red corporativa con su servidor de acceso remoto asociado.  
  
2. Las rutas deben configurarse en la infraestructura de enrutamiento de la red corporativa.  
  
3. Para cada punto de entrada debe haber uno y tres rutas en la red interna:  
  
   1. Prefijo este prefijo de IP-HTTPS se elige el administrador en el agregar un asistente de punto de entrada.  
  
   2. Prefijo IPv6 de VPN (opcional). Se puede elegir este prefijo después de habilitar un punto de entrada VPN  
  
   3. Prefijo de Teredo (opcional). Este prefijo solo es pertinente si el servidor de acceso remoto está configurado con dos direcciones de IPv4 públicas consecutivas en el adaptador externo. El prefijo se basa en la primera dirección IPv4 pública del par de direcciones. Por ejemplo, si las direcciones externas son:  
  
      1. www\.xxx.yyy.zzz  
  
      2. www\.xxx.yyy.zzz+1  
  
      A continuación, configure el prefijo de Teredo es 2001:0:WWXX:YYZZ:: / 64, donde WWXX: YYZZ es la representación hexadecimal de la dirección IPv4, World Wide Web\.xxx.yyy.zzz.  
  
      Tenga en cuenta que puede usar el siguiente script para calcular el prefijo de Teredo:  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Todas las rutas anteriores deben enrutarse a la dirección IPv6 en el adaptador interno del servidor de acceso remoto (o a la dirección IP (VIP) virtual interna para una carga equilibrada punto de entrada).  
  
> [!NOTE]  
> Cuando se implementa IPv6 en la red corporativa y la administración del servidor de acceso remoto se realizó de forma remota a través de DirectAccess, las rutas para el Teredo y prefijos IP-HTTPS de todos los demás puntos de entrada deben agregarse a cada servidor de acceso remoto para que sea el tráfico reenviar a la red interna.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Prefijos IPv6 específica del sitio de Active Directory  
Cuando un equipo cliente que ejecutan Windows 10 o Windows 8 está conectado a un punto de entrada, el equipo cliente está inmediatamente asociado con el sitio de Active Directory del punto de entrada y se configura con prefijos IPv6 asociados al punto de entrada. La preferencia es para que equipos cliente para conectarse a los recursos mediante estos prefijos IPv6, ya que se configuran dinámicamente en la tabla de directivas de prefijo de IPv6 con mayor prioridad al conectarse a un punto de entrada.  
  
Si su organización usa una topología de Active Directory con prefijos específicos del sitio de IPv6 (por ejemplo un app.corp.com FQDN de recursos internos se hospeda en América del Norte y Europa con una dirección IP específica del sitio en cada ubicación) no está configurado de forma predeterminada mediante la consola de acceso remoto y los prefijos IPv6 específicos del sitio no está configurado para cada punto de entrada. Si desea habilitar este escenario opcional, deberá configurar cada punto de entrada con los prefijos IPv6 específicos que deberían ser preferidos por equipos cliente se conectan a un punto de entrada específica. Hacerlo como sigue:  
  
1.  Para cada GPO se usa para los equipos de cliente de Windows 8 o Windows 10, ejecute el cmdlet de PowerShell Set-DAEntryPointTableItem  
  
2.  Establezca el parámetro EntryPointRange para el cmdlet con los prefijos IPv6 específica del sitio. Por ejemplo agregar la información específica de prefijos 2001:db8:1:1:: / 64 y 2001:db:1:2:: / 64 a un punto de entrada llamado Europe, ejecute lo siguiente  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Cuando se modifica el parámetro EntryPointRange, asegúrese de que no se quitan los prefijos de 128 bits existentes que pertenecen a los extremos del túnel IPsec y la dirección DNS64.  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 planear la transición a IPv6 cuando acceso remoto multisitio está implementado  
Muchas organizaciones usan el protocolo IPv4 en la red corporativa. Con el agotamiento de los prefijos IPv4 disponibles, muchas organizaciones realizan la transición de IPv4 únicamente a las redes de solo IPv6.  
  
Esta transición es más probable que llevará a cabo en dos fases:  
  
1.  Desde un solo IPv4 a una red corporativa de IPv6 + IPv4.  
  
2.  De IPv6 + IPv4 a una red corporativa solo IPv6.  
  
En cada parte, la transición puede realizarse en fases. En cada fase solo una subred de la red puede cambiarse a la nueva configuración de red. Por lo tanto, una implementación multisitio de DirectAccess se requiere para admitir una implementación híbrida donde, por ejemplo, algunos de los puntos de entrada pertenecen a una subred solo IPv4 y otros pertenecen a una subred IPv6 + IPv4. Además, los cambios de configuración durante los procesos de transición no deben interrumpir la conectividad de cliente a través de DirectAccess.  
  
### <a name="TransitionIPv4toMixed"></a>La transición desde un solo IPv4 a una red corporativa de IPv6 + IPv4  
Al agregar direcciones IPv6 a una red corporativa solo IPv4, desea agregar una dirección IPv6 a un servidor de DirectAccess ya implementada. Además, es posible que desee agregar un punto de entrada o un nodo a un clúster con equilibrio de carga con direcciones IPv4 e IPv6 para la implementación de DirectAccess.  
  
Acceso remoto le permite agregar servidores con direcciones IPv4 e IPv6 en una implementación que se configuró originalmente con solo direcciones IPv4. Estos servidores se agregan como servidores de sólo IPv4 y sus direcciones IPv6 son ignoradas por DirectAccess; por lo tanto, su organización no puede aprovechar las ventajas de la conectividad IPv6 nativa en estos servidores nuevos.  
  
Para transformar la implementación a una implementación de IPv6 + IPv4 y aprovechar las capacidades nativas de IPv6, debe volver a instalar DirectAccess. Para mantener la conectividad de cliente a lo largo de la reinstalación consulte transición desde un solo IPv4 para una implementación de IPv6 únicamente con las implementaciones de DirectAccess duales.  
  
> [!NOTE]  
> Al igual que con una red de sólo IPv4, en una red IPv4 + IPv6 mixta, la dirección del servidor DNS que se utiliza para resolver las solicitudes de cliente DNS debe configurarse con DNS64 que se implementa en los servidores de acceso remoto a sí mismos y no con un DNS corporativo.  
  
### <a name="TransitionMixedtoIPv6"></a>La transición de IPv6 + IPv4 a una red corporativa solo IPv6  
DirectAccess le permite agregar puntos de entrada de sólo IPv6 sólo si el primer servidor de acceso remoto en la implementación tenía originalmente tanto IPv4 y de las direcciones IPv6 o solo una dirección IPv6. Es decir, no se puede pasar de una red de sólo IPv4 a una red IPv6 únicamente en un solo paso sin volver a instalar DirectAccess. Para realizar la transición directamente desde una red de sólo IPv4 a una red IPv6 únicamente, consulte transición desde un solo IPv4 para una implementación de IPv6 únicamente con las implementaciones de DirectAccess duales.  
  
Después de haber completado la transición de una implementación de sólo IPv4 a una implementación de IPv6 + IPv4, puede realizar la transición a una red IPv6 únicamente. Durante y después de la transición, tenga en cuenta lo siguiente:  
  
-   Si todos los servidores back-end de sólo IPv4 permanecen en la red corporativa, no estarán accesibles para los clientes que se conectan a través de los puntos de entrada solo para IPv6.  
  
-   Al agregar puntos de entrada de IPv6 únicamente a una implementación de IPv6 + IPv4, DNS64 y NAT64 no se habilitará en los nuevos servidores. Los clientes que se conectan a estos puntos de entrada se configurarán automáticamente para usar los servidores DNS corporativos.  
  
-   Si tiene que eliminar las direcciones IPv4 de un servidor implementado, debe quitar el servidor de la implementación de DirectAccess, quitar su dirección de red corporativa de IPv4 y agregarlo de nuevo a la implementación.  
  
Para admitir la conectividad de cliente a la red corporativa, debe asegurarse de que el servidor de ubicación de red se puede resolver mediante DNS corporativo para su dirección IPv6. También se puede establecer una dirección IPv4 adicional, pero no es necesaria.  
  
### <a name="DualDeployment"></a>La transición desde un solo IPv4 a una implementación de IPv6 únicamente con las implementaciones de DirectAccess duales  
No se puede realizar la transición de un solo IPv4 a una red corporativa solo IPv6 sin volver a instalar la implementación de DirectAccess. Para mantener la conectividad de cliente durante la transición, puede usar otra implementación de DirectAccess. Implementación dual es necesaria cuando la primera fase de transición finaliza (red solo para IPv4, actualizado a IPv4 + IPv6) y va a prepararse para una transición futuras a una solo IPv6 red corporativa OAL aprovechar las ventajas de las ventajas de conectividad IPv6 nativas. La implementación doble se describe en los siguientes pasos generales:  
  
1.  Instalar una segunda implementación de DirectAccess. Puede instalar DirectAccess en los servidores nuevos, o quitar servidores de la primera implementación y usarlos para la segunda implementación.  
  
    > [!NOTE]  
    > Al instalar una implementación de DirectAccess adicional junto con la actual, asegúrese de que no hay puntos de dos entradas comparten el mismo prefijo de cliente.  
    >   
    > Si instala mediante el Asistente para introducción de DirectAccess o con el cmdlet `Install-RemoteAccess`, acceso remoto establece automáticamente el prefijo de cliente del primer punto de entrada en la implementación en un valor predeterminado de < subred IPv6\_prefijo >: 1000:: / 64. Si es necesario, debe cambiar el prefijo.  
  
2.  Quitar los grupos de seguridad de cliente elegido de la primera implementación.  
  
3.  Agregue los grupos de seguridad de cliente para la segunda implementación.  
  
    > [!IMPORTANT]  
    > Para mantener la conectividad de cliente en todo el proceso, debe agregar los grupos de seguridad para la segunda implementación inmediatamente después de eliminarlos de la primera. Esto garantiza que los clientes no se actualizará con dos o ninguno de los GPO de DirectAccess. Los clientes iniciará con la segunda implementación una vez que se recupere y actualice su GPO de cliente.  
  
4.  Opcional: Quitar los puntos de entrada de DirectAccess de la primera implementación y agregue esos servidores como nuevos puntos de entrada en el segundo.  
  
Cuando haya completado la transición, puede desinstalar la primera implementación de DirectAccess. Al desinstalar, pueden producirse los siguientes problemas:  
  
-   Si la implementación se ha configurado para admitir a sólo clientes en equipos móviles, se eliminará el filtro WMI. Si los grupos de seguridad de cliente de la segunda implementación incluyen los equipos de escritorio, el GPO de cliente de DirectAccess no filtrará los equipos de escritorio y puede causar problemas en ellos. Si se necesita un filtro de los equipos móviles, volver a crearlo siguiendo las instrucciones de [crear filtros WMI para el GPO](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Si ambas implementaciones se crearon originalmente en el mismo dominio de Active Directory, el DNS de sondeo de entrada que apunte a localhost se eliminará y puede provocar problemas de conectividad de cliente. Por ejemplo, los clientes pueden conectarse mediante IP-HTTPS en lugar de Teredo, o cambiar entre los puntos de entrada multisitio de DirectAccess. En este caso, debe agregar la siguiente entrada DNS al DNS corporativo:  
  
    -   Zona: nombre de dominio de  
  
    -   Nombre: directaccess-corpConnectivityHost  
  
    -   Dirección IP::: 1  
  
    -   Escriba:  AAAA  
  
  
  


