---
title: Implementación del acceso remoto en un clúster
description: Este tema forma parte de la guía deploy Remote Access in a Cluster in Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e4108eee0c62ae4d4db31560b31a6f90751c6b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404648"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Implementación del acceso remoto en un clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 y Windows Server 2012 combinan DirectAccess y el servicio de acceso remoto \(RAS\) VPN en un solo rol de acceso remoto. Puede implementar el acceso remoto en varios escenarios empresariales. Esta información general proporciona una introducción al escenario empresarial para implementar varios servidores de acceso remoto en una carga de clúster equilibrada con equilibrio de carga de red de Windows \(NLB\) o con un equilibrador de carga externo \(el\)de ELB, como F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Descripción del escenario  
La implementación de un clúster reúne varios servidores de acceso remoto en una sola unidad, que luego actúa como un único punto de contacto para equipos cliente remotos que se conectan a través de DirectAccess o VPN a la red corporativa interna con la dirección IP virtual externa \(VIP\) dirección del clúster de acceso remoto.  Se equilibra la carga del tráfico al clúster mediante Windows NLB o con un equilibrador de carga externo \(como F5 Big\-IP\).  

## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  

-   Equilibrio de carga predeterminado mediante Windows NLB.  

-   Se admiten los equilibradores de carga externos.  

-   El modo de unidifusión es el modo predeterminado y recomendado para NLB.  

-   No se admite la opción de cambiar directivas fuera de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.  

-   Cuando se usa NLB o un equilibrador de carga externo, el prefijo IPHTTPS no se puede cambiar a algo que no sea \/59.  

-   Los nodos de carga equilibrada deben estar en la misma subred IPv4.  

-   En las implementaciones de ELB, si se necesita la administración de salida, los clientes de DirectAccess no pueden usar&nbsp;Teredo. Solo se puede usar IPHTTPS para end\-a fin de\-la comunicación final.  

-   Asegúrese de que todas las revisiones de NLB\/ELB están instaladas.  

-   ISATAP no es compatible con la red corporativa. Si utilizas ISATAP, debes eliminarlo y usar IPv6 nativo.  

## <a name="in-this-scenario"></a>En este escenario  
El escenario de implementación del clúster incluye varios pasos:  

1.  [Implementar un servidor VPN de AlwaysOn con opciones avanzadas](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Debe implementarse un solo servidor de acceso remoto con configuración avanzada antes de configurar una implementación de clúster.  

2.  [Planear una implementación de clúster de acceso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Para crear un clúster a partir de una implementación de un solo servidor, es necesario realizar una serie de pasos adicionales, incluida la preparación de los certificados para la implementación del clúster.  

3.  [Configurar un clúster de acceso remoto](configure/Configure-a-Remote-Access-Cluster.md). Consta de varios pasos de configuración, como preparar el servidor único para Windows NLB o el equilibrador de carga externo, preparar servidores adicionales para unirse al clúster y habilitar el equilibrio de carga.  

## <a name="BKMK_APP"></a>Aplicaciones prácticas  
La recopilación de varios servidores en un clúster de servidores ofrece lo siguiente:  

-   Escalabilidad. Un solo servidor de acceso remoto ofrece un nivel limitado de confiabilidad del servidor y un rendimiento escalable. Al agrupar los recursos de dos o más servidores en un único clúster, se aumenta la capacidad para la cantidad de usuarios y el rendimiento.  

-   Alta disponibilidad. Un clúster proporciona alta disponibilidad para siempre\-en el acceso. Si se produce un error en un servidor del clúster, los usuarios remotos pueden seguir teniendo acceso a la red corporativa a través de un servidor diferente en el clúster. Todos los servidores del clúster tienen el mismo conjunto de direcciones IP virtuales \(VIP\), a la vez que mantienen una única dirección IP dedicada para cada servidor.  

-   Facilite la\-de la administración de\-. Un clúster permite la administración de varios servidores como una sola entidad. La configuración compartida se puede establecer fácilmente en el servidor del clúster. La configuración de acceso remoto se puede administrar desde cualquiera de los servidores del clúster o de forma remota mediante Herramientas de administración remota del servidor \(RSAT\). Además, se puede supervisar todo el clúster desde una única consola de administración de acceso remoto.  

## <a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la siguiente tabla, se muestran los roles y características requeridos para el escenario:  

|Característica de\/de roles|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala mediante la consola del Administrador del servidor. Engloba tanto DirectAccess, que antes era una característica de Windows Server 2008 R2, como los servicios de enrutamiento y acceso remoto \(RRAS\), que antes era un servicio de rol en la Directiva de red y los servicios de acceso \(NPAS\) rol de servidor. El rol de acceso remoto consta de dos componentes:<br /><br />-Always On servicios de enrutamiento y acceso remoto \(RRAS\) VPN: DirectAccess y VPN se administran juntos en la consola de administración de acceso remoto.<br />-Enrutamiento RRAS: las características de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />Las dependencias son las siguientes:<br /><br />-Internet Information Services \(servidor Web\) IIS: esta característica es necesaria para configurar el servidor de ubicación de red y el sondeo Web predeterminado.<br />-Windows Internal Database: se usa para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecute el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />-Consola de administración de directivas de grupo<br />-Kit de administración del administrador de conexiones RAS \(CMAK\)<br />-Windows PowerShell 3,0<br />-Infraestructura y herramientas de administración de gráficos|  
|Equilibrio de carga de red|Esta característica ofrece equilibrio de carga en un clúster con Windows NLB.|  

## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  

-   Al menos dos equipos que cumplan los requisitos de hardware para Windows Server 2012.  

-   En el escenario de Load Balancer externo, se requiere hardware dedicado \(es decir, F5 BigIP\).  

-   Para probar el escenario, debe tener al menos un equipo que ejecute Windows 10 configurado como Always On cliente VPN.   

## <a name="BKMK_SOFT"></a>Requisitos de software  
Hay varios requisitos para este escenario:  

-   Requisitos de software para la implementación de un solo servidor. Para obtener más información, vea [implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un solo acceso remoto).  

-   Además de los requisitos de software para un solo servidor, hay varios requisitos específicos de\-de clúster:  

    -   En cada servidor de clúster, el nombre de sujeto del certificado HTTPS\-IP debe coincidir con la dirección ConnectTo. Una implementación de clúster admite una combinación de certificados comodín y no\-comodín en servidores de clúster.  

    -   Si el servidor de ubicación de red está instalado en el servidor de acceso remoto, el certificado del servidor de ubicación de red debe tener el mismo nombre de firmante en cada servidor de clúster. Además, el nombre del certificado del servidor de ubicación de red no debe tener el mismo nombre que cualquier otro servidor en la implementación de DirectAccess.  

    -   Los certificados de servidor de ubicación de red y HTTPS de IP\-se deben emitir con el mismo método con el que se emitió el certificado para el servidor único. Por ejemplo, si el servidor único usa una entidad de certificación pública \(CA\), todos los servidores del clúster deben tener un certificado emitido por una CA pública. O bien, si el servidor único usa un certificado firmado\-para IP\-HTTPS, todos los servidores del clúster deben hacerlo de la misma manera.  

    -   El prefijo IPv6 asignado a equipos clientes de DirectAccess en clústeres de servidor debe ser de 59 bits. Si VPN está habilitado, el prefijo VPN también debe ser de 59 bits.  

## <a name="KnownIssues"></a>Problemas conocidos  
Los problemas que se mencionan a continuación son problemas conocidos de la configuración de un escenario de clúster:  

-   Después de configurar DirectAccess en una implementación de IPv4\-solo con un adaptador de red único y después de que el DNS64 predeterminado \(la dirección IPv6 que contiene ": 3333::"\) se configura automáticamente en el adaptador de red, al intentar habilitar el equilibrio de\-de carga a través de la consola de administración de acceso remoto, se solicita al usuario que proporcione una DIP de IPv6. Si se proporciona una DIP de IPv6, después de hacer clic en **Confirmar** , se produce el siguiente error de configuración: el parámetro es incorrecto.  

    Para resolver este problema:  

    1.  Descargue la copia de seguridad y restaure los scripts desde [Back up and Restore Remote Access Configuration](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Realice una copia de seguridad de los GPO de acceso remoto mediante la copia de seguridad de script descargado\-RemoteAccess. ps1  

    3.  Intente habilitar el equilibrio de carga hasta el paso en el que se produce el error. En el cuadro de diálogo habilitar equilibrio de carga, expanda el área de detalles, haga clic con el botón secundario\-en el área de detalles y, a continuación, haga clic en **copiar script**.  

    4.  Abra el Bloc de notas y pegue el contenido del Portapapeles. Por ejemplo:  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    5.  Cierre los cuadros de diálogo de acceso remoto abiertos y la Consola de administración de acceso remoto.  

    6.  Edite el texto pegado y quite las direcciones IPv6. Por ejemplo:  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    7.  En una ventana de PowerShell con privilegios elevados, ejecute el comando del paso anterior.  

    8.  Si se produce un error en el cmdlet mientras se está ejecutando \(no debido a valores de entrada incorrectos\), ejecute el comando restore\-RemoteAccess. PS1 y siga las instrucciones para asegurarse de que se mantiene la integridad de la configuración original.  

    9. Ahora puede volver a abrir la Consola de administración de acceso remoto.  
