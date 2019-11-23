---
title: Paso 1 configurar la infraestructura de DirectAccess básica
description: Este tema forma parte de la guía de implementación de un único servidor de DirectAccess con el Asistente para Introducción para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba4de2a4-f237-4b14-a8a7-0b06bfcd89ad
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2cd84949dddf75730aca6302f1244f784b5933d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388567"
---
# <a name="step-1-configure-the-basic-directaccess-infrastructure"></a>Paso 1 configurar la infraestructura de DirectAccess básica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo configurar la infraestructura necesaria para una implementación básica de DirectAccess con un único servidor de DirectAccess en un entorno mixto de IPv4 e IPv6. Antes de comenzar con los pasos de implementación, asegúrese de que ha completado los pasos de planeación descritos en [planear una implementación básica de DirectAccess](../../../remote-access/directaccess/single-server-wizard/Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Tarea|Descripción|  
|----|--------|  
|Configurar los valores de red del servidor|Configura las opciones de red del servidor en el servidor de DirectAccess.|  
|Configurar el enrutamiento en la red corporativa|Configura el enrutamiento en la red corporativa para asegurarte de que el tráfico se enruta correctamente.|  
|Configurar los firewalls|Configure los firewalls adicionales, si es necesario.|  
|Configurar el servidor DNS|Configurar los ajustes de DNS para el servidor de DirectAccess.|  
|Configurar Active Directory|Une los equipos cliente y el servidor de DirectAccess al dominio de Active Directory.|  
|Configurar GPO|Configura los GPO para la implementación, si es necesario.|  
|Configurar grupos de seguridad|Configura los grupos de seguridad que contendrán equipos cliente de DirectAccess, y cualquier otro grupo de seguridad necesario en la implementación.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="ConfigNetworkSettings"></a>Configurar las opciones de red del servidor  
Los siguientes valores de la interfaz de red son necesarios para una implementación de un solo servidor en un entorno con IPv4 e IPv6. Todas las direcciones IP se configuran mediante **Cambiar configuración del adaptador** en el **Centro de redes y recursos compartidos de Windows**.  
  
-   Topología perimetral  
  
    -   Una dirección IPv4 o IPv6 estática pública con acceso a Internet.  
  
        > [!NOTE]  
        > Se requieren dos direcciones IPv4 públicas consecutivas para Teredo. Si no usas Teredo, puedes configurar una sola dirección IPv4 estática y pública.  
  
    -   Una sola dirección IPv4 o IPv6 estática interna.  
  
-   Detrás de un dispositivo NAT (dos adaptadores de red)  
  
    -   Una sola dirección IPv4 o IPv6 estática interna con acceso a la red.  
  
    -   Una sola dirección IPv4 o IPv6 estática con acceso a la red perimetral.  
  
-   Detrás de un dispositivo NAT (un adaptador de red)  
  
    -   Una sola dirección IPv4 o IPv6 estática.  
  
> [!NOTE]  
> Si el servidor de DirectAccess tiene dos o más adaptadores de red (uno clasificado en el perfil de dominio y otro en un perfil público/privado), pero desea usar una sola topología NIC, las recomendaciones son:  
>   
> 1.  Compruebe que el segundo NIC y los NIC adicionales también estén clasificados en el perfil de dominio.  
> 2.  Si el segundo NIC no puede configurarse para el perfil de dominio por alguna razón, el ámbito de la directiva IPsec de DirectAccess debe ampliarse manualmente a todos los perfiles mediante los siguientes comandos de Windows PowerShell:  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
>   
>     Los nombres de las directivas de IPsec son DirectAccess-DaServerToInfra y DirectAccess-DaServerToCorp.  
  
## <a name="ConfigRouting"></a>Configurar el enrutamiento en la red corporativa  
Configura el enrutamiento en la red corporativa de la siguiente manera:  
  
-   Si se implementa IPv6 nativa en la organización, agrega una ruta para que los enrutadores de la red interna enruten el tráfico IPv6 a través del servidor de acceso remoto.  
  
-   Configura manualmente las rutas de IPv4 e IPv6 de la organización en los servidores de acceso remoto. Agrega una ruta publicada para que todo el tráfico con un prefijo IPv6 (/48) de organización se reenvíe a la red interna. Además, para el tráfico IPv4, agrega rutas explícitas para que el tráfico IPv4 se reenvíe a la red interna.  
  
## <a name="ConfigFirewalls"></a>Configuración de firewalls  
Si usa firewalls adicionales en la implementación, aplique las siguientes excepciones del firewall con conexión a Internet para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentra en Internet IPv4:  
  
-   tráfico 6to4: protocolo IP 41 entrante y saliente.  
  
-   IP-HTTPS-puerto de destino del Protocolo de control de transmisión (TCP) 443 y puerto de origen TCP 443 de salida. Si el servidor de acceso remoto tiene un único adaptador de red y el servidor de ubicación de red se encuentra en el servidor de acceso remoto, el puerto TCP 62000 también es obligatorio.  
  
    > [!NOTE]  
    > Esta exención debe configurarse en el servidor de acceso remoto. El resto de excepciones deben configurarse en el firewall perimetral.  
  
> [!NOTE]  
> Para el tráfico Teredo y 6to4, estas excepciones se deben aplicar para las dos direcciones IPv4 públicas consecutivas con conexión a Internet del servidor de acceso remoto. Para IP-HTTPS, las excepciones solo se deben aplicar a la dirección en la que se resuelve el nombre externo del servidor.  
  
Si usa firewalls adicionales, aplique las siguientes excepciones del firewall con conexión a Internet para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentra en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
Si usa firewalls adicionales, aplique las siguientes excepciones de firewall de la red interna para el tráfico de acceso remoto:  
  
-   ISATAP-protocolo 41 entrante y saliente  
  
-   TCP/UDP para todo el tráfico IPv4/IPv6  
  
## <a name="ConfigDNS"></a>Configurar el servidor DNS  
Debes configurar manualmente una entrada DNS para el sitio web del servidor de ubicación de red para la red interna de tu implementación.  
  
### <a name="NLS_DNS"></a>Para crear el servidor de ubicación de red y los registros DNS de sondeo de NCSI  
  
1.  En el servidor DNS de la red interna, ejecute **DNSMgmt. msc** y, a continuación, presione Entrar.  
  
2.  En el panel izquierdo de la consola del **Administrador del DNS**, expande la zona de búsqueda directa de tu dominio. Haz clic con el botón secundario en el dominio y haz clic en **Host nuevo (A o AAAA)** .  
  
3.  En el cuadro de diálogo **Host nuevo**, en el cuadro **Nombre (si se deja en blanco, se usa el nombre del dominio primario)** , escribe el nombre DNS del sitio web del servidor de ubicación de red (es el nombre que los clientes de DirectAccess usan para conectarse al servidor de ubicación de red). En el cuadro **Dirección IP**, escribe la dirección IPv4 del servidor de ubicación de red y haz clic en **Agregar host**. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
4.  En el cuadro de diálogo **Host nuevo**, en el cuadro **Nombre (si se deja en blanco, se usa el nombre del dominio primario)** , escribe el nombre DNS del sondeo web (el nombre del sondeo web predeterminado es directaccess-webprobehost). En el cuadro **Dirección IP**, escribe la dirección IPv4 del sondeo web y haz clic en **Agregar host**. Repite este proceso para directaccess-corpconnectivityhost y todos los comprobadores de conectividad creados manualmente. En el cuadro de diálogo **DNS**, haz clic en **Aceptar**.  
  
5.  Haz clic en **Listo**.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
También debes configurar entradas DNS para los siguiente:  
  
-   **El servidor IP-https** : los clientes de DirectAccess deben ser capaces de resolver el nombre DNS del servidor de acceso remoto desde Internet.  
  
-   **Comprobación de revocación de CRL** : DirectAccess usa la comprobación de revocación de certificados para la conexión IP-https entre los clientes de DirectAccess y el servidor de acceso remoto, y para la conexión basada en https entre el cliente de DirectAccess y el servidor de ubicación de red. En ambos casos, los clientes de DirectAccess deben ser capaces de resolver y acceder a la ubicación del punto de distribución de CRL.  
  
## <a name="ConfigAD"></a>Configurar Active Directory  
El servidor de acceso remoto y todos los equipos cliente de DirectAccess deben estar unidos a un dominio de Active Directory. Los equipos cliente de DirectAccess deben pertenecer a uno de los siguientes tipos de dominio:  
  
-   Dominios que pertenecen al mismo bosque que el servidor de acceso remoto.  
  
-   Dominios que pertenecen a bosques con confianza bidireccional con el bosque del servidor de acceso remoto.  
  
-   Dominios con confianza de dominio bidireccional con el dominio del servidor de acceso remoto.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Para unir el servidor de acceso remoto a un dominio  
  
1.  En el Administrador del servidor, haga clic en **Servidor local**. En el panel de detalles, haga clic en el vínculo que aparece junto al **Nombre de equipo**.  
  
2.  En el cuadro de diálogo **propiedades del sistema** , haga clic en la pestaña **nombre de equipo** . En la pestaña **nombre de equipo** , haga clic en **cambiar**.  
  
3.  En **Nombre de equipo**, escriba el nombre del equipo si también está cambiando el nombre del equipo al unir el servidor al dominio. En **Miembro de**, haga clic en **Dominio** y, a continuación, escriba el nombre del dominio al que desea unir el servidor, por ejemplo corp.contoso.com, y haga clic en **Aceptar**.  
  
4.  Cuando se le solicite un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con derechos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
5.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio, haga clic en **Aceptar**.  
  
6.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cerrar**.  
  
8.  Cuando se le pida que reinicie el equipo, haga clic en **Reiniciar ahora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Cómo unir equipos cliente al dominio  
  
1.  Ejecute **Explorer. exe**.  
  
2.  Haz clic con el botón secundario en el icono del equipo y, a continuación, haz clic en **Propiedades**.  
  
3.  En la página **Sistema**, haz clic en **Configuración avanzada del sistema**.  
  
4.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre de equipo** haz clic en **Cambiar**.  
  
5.  En **Nombre de equipo**, escribe el nombre del equipo si también estás cambiando el nombre del equipo al unir el servidor al dominio. En **Miembro de**, haga clic en **Dominio** y, a continuación, escriba el nombre del dominio al que desea unir el servidor, por ejemplo corp.contoso.com, y haga clic en **Aceptar**.  
  
6.  Cuando se le solicite un nombre de usuario y una contraseña, escriba el nombre de usuario y la contraseña de un usuario con derechos para unir equipos al dominio y, a continuación, haga clic en **Aceptar**.  
  
7.  Cuando vea un cuadro de diálogo en el que se le da la bienvenida al dominio, haga clic en **Aceptar**.  
  
8.  Cuando se le indique que debe reiniciar el equipo, haga clic en **Aceptar**.  
  
9. En el cuadro de diálogo **Propiedades del sistema**, haga clic en Cerrar. Haz clic en **Reiniciar ahora** cuando se te solicite.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Tenga en cuenta que debe proporcionar credenciales de dominio después de introducir el comando Add-Computer a continuación.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>Configurar GPO  
Para implementar el acceso remoto, se requiere un mínimo de dos objetos de directiva de Grupo: un objeto de directiva de grupo contiene la configuración para el servidor de acceso remoto y otra contiene la configuración de los equipos cliente de DirectAccess. Al configurar el acceso remoto, el asistente crea automáticamente el objeto de directiva de grupo requerido. Sin embargo, si su organización exige una Convención de nomenclatura, o si no tiene los permisos necesarios para crear o editar objetos de directiva de grupo, deben crearse antes de configurar el acceso remoto.  
  
Para crear un objeto de directiva de grupo, vea [crear y editar un objeto de directiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> El administrador puede vincular manualmente el objeto de directiva de grupo de DirectAccess a una unidad organizativa siguiendo estos pasos:  
>   
> 1.  Antes de configurar DirectAccess, vincula los GPO creados a las unidades organizativas respectivas.  
> 2.  Configura DirectAccess, especificando un grupo de seguridad para los equipos cliente.  
> 3.  El administrador podría tener permisos para vincular los objetos de directiva de grupo al dominio. En cualquier caso, los objetos de directiva de grupo se configurarán automáticamente. Si los GPO ya están vinculados a una unidad organizativa, los vínculos no se eliminarán y los GPO no se vincularán al dominio. Para un GPO de servidor, la unidad organizativa debe contener el objeto de equipo del servidor, o el GPO se vinculará a la raíz del dominio.  
> 4.  Si la unidad organizativa no se vinculó antes de ejecutar el asistente de DirectAccess, una vez finalizada la configuración, el administrador puede vincular los objetos de directiva de grupo de DirectAccess a las unidades organizativas necesarias. El vínculo al dominio puede eliminarse. [Aquí](https://technet.microsoft.com/library/cc732979.aspx) encontrará los pasos para vincular un objeto de directiva de grupo a una unidad organizativa.  
  
> [!NOTE]  
> Si un objeto de directiva de grupo se creó manualmente, es posible que durante la configuración de DirectAccess el objeto de directiva de grupo no esté disponible. Es posible que el objeto de directiva de grupo no se haya replicado en el controlador de dominio más cercano al equipo de administración. En este caso, el administrador puede esperar a que la replicación finalice, o bien forzarla.  
  
## <a name="ConfigSGs"></a>Configurar grupos de seguridad  
La configuración de DirectAccess contenida en los objetos de directiva de grupo del equipo cliente solo se aplica a los equipos que son miembros de los grupos de seguridad que se especifican al configurar el acceso remoto.  
  
### <a name="Sec_Group"></a>Para crear un grupo de seguridad para clientes de DirectAccess  
  
1.  Ejecute **DSA. msc**. En la consola **Usuarios y equipos de Active Directory**, en el panel izquierdo, expande el dominio que contendrá el grupo de seguridad, haz clic con el botón secundario en **Usuarios**, elige **Nuevo** y haz clic en **Grupo**.  
  
2.  En el cuadro de diálogo **Nuevo objeto: Grupo**, en **Nombre de grupo**, escribe el nombre de grupo de seguridad.  
  
3.  En **Ámbito de grupo** haz clic en **Global**, en **Tipo de grupo** haz clic en **Seguridad** y, a continuación, haz clic en **Aceptar**.  
  
4.  Haz doble clic en el grupo de seguridad de equipos cliente de DirectAccess y, en el cuadro de diálogo de propiedades, haz clic en la pestaña **Miembros**.  
  
5.  En la pestaña **Miembros** , haga clic en **Agregar**.  
  
6.  En el cuadro de diálogo **Seleccionar Usuarios, Contactos, Equipos o Cuentas de servicio**, selecciona los equipos cliente que deseas habilitar para DirectAccess y haz clic en **Aceptar**.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**comandos equivalentes** de Windows PowerShell Windows PowerShell  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_Links"></a>Paso siguiente  
  
-   [Paso 2: configurar el servidor de DirectAccess básico](da-basic-configure-s2-server.md)  
  


