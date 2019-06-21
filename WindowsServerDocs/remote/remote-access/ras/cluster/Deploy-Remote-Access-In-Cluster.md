---
title: Implementación del acceso remoto en un clúster
description: En este tema forma parte de la Guía de implementación de acceso remoto en un clúster en Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 853788f20c452391c802f0681fa23978b4892c6a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281228"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Implementación del acceso remoto en un clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 y Windows Server 2012 combinan DirectAccess y el servicio de acceso remoto \(RAS\) VPN en un solo rol de acceso remoto. Puede implementar el acceso remoto en un número de escenarios empresariales. Esta información general proporciona una introducción al escenario empresarial para implementar varios servidores de acceso remoto en una carga de clúster equilibrada con Windows Network Load Balancing \(NLB\) o con un equilibrador de carga externo \(ELB \), por ejemplo, F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Descripción del escenario  
Implementación de un clúster reúne varios servidores de acceso remoto en una sola unidad, lo cual luego actúa como un único punto de contacto para equipos cliente remotos que conectan a través de DirectAccess o VPN a la red corporativa interna con la dirección IP virtual externa \(VIP\) dirección del clúster de acceso remoto.  El tráfico al clúster de carga equilibrada mediante NLB de Windows o equilibrador de carga con una referencia externa \(como F5 Big\-IP\).  

## <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  

-   Equilibrio de carga predeterminado mediante Windows NLB.  

-   Se admiten los equilibradores de carga externos.  

-   El modo de unidifusión es el modo predeterminado y recomendado para NLB.  

-   No se admite la opción de cambiar directivas fuera de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.  

-   Cuando se usa NLB o un equilibrador de carga externo, el prefijo IP-HTTPS no se puede cambiar en cualquier valor distinto de \/59.  

-   Los nodos de carga equilibrada deben estar en la misma subred IPv4.  

-   En las implementaciones de ELB, si administrar se necesita, a continuación, los clientes de DirectAccess no pueden usar&nbsp;Teredo. IPHTTPS solo puede usarse para end\-a\-fin a la comunicación.  

-   Asegúrese de NLB conocido todos los\/ELB revisiones se instalan.  

-   ISATAP no es compatible con la red corporativa. Si utilizas ISATAP, debes eliminarlo y usar IPv6 nativo.  

## <a name="in-this-scenario"></a>En este escenario  
El escenario de implementación del clúster incluye varios pasos:  

1.  [Implementar un servidor VPN con opciones avanzadas de AlwaysOn](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Un único servidor de acceso remoto con configuración avanzada debe implementarse antes de configurar una implementación de clúster.  

2.  [Planear una implementación de clúster de acceso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Para crear un clúster desde una implementación de servidor único de una serie de pasos son necesarios, como preparar los certificados para la implementación del clúster.  

3.  [Configurar un clúster de acceso remoto](configure/Configure-a-Remote-Access-Cluster.md). Esto se compone de una serie de pasos de configuración, incluida la preparación del servidor solo para Windows NLB o el equilibrador de carga externo, preparar servidores adicionales a unirse al clúster y habilitar el equilibrio de carga.  

## <a name="BKMK_APP"></a>Aplicaciones prácticas  
La recopilación de varios servidores en un clúster de servidores ofrece lo siguiente:  

-   Escalabilidad. Un único servidor de acceso remoto proporciona un nivel limitado de confiabilidad del servidor y un rendimiento escalable. Al agrupar los recursos de dos o más servidores en un único clúster, se aumenta la capacidad para la cantidad de usuarios y el rendimiento.  

-   Alta disponibilidad. Un clúster proporciona alta disponibilidad para siempre\-en access. Si se produce un error en un servidor del clúster, los usuarios remotos pueden seguir teniendo acceso a la red corporativa a través de un servidor diferente en el clúster. Todos los servidores del clúster tienen el mismo conjunto de IP virtual del clúster \(VIP\) direcciones, manteniendo un único, las direcciones IP para cada servidor dedicado.  

-   Facilidad\-de\-administración. Un clúster permite la administración de varios servidores como una sola entidad. La configuración compartida se puede establecer fácilmente en el servidor del clúster. Configuración de acceso remoto se puede administrar desde cualquiera de los servidores en clúster o de forma remota mediante las herramientas de administración remota del servidor \(RSAT\). Además, se puede supervisar todo el clúster desde una única consola de administración de acceso remoto.  

## <a name="BKMK_NEW"></a>Roles y características que se incluyen en este escenario  
En la siguiente tabla, se muestran los roles y características requeridos para el escenario:  

|Rol\/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala mediante la consola del Administrador del servidor. Incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2 como enrutamiento y servicios de acceso remoto \(RRAS\), que antes eran un servicio de rol en la directiva de red y servicios de acceso \(NPAS\) rol de servidor. El rol de acceso remoto consta de dos componentes:<br /><br />-Siempre en VPN de enrutamiento y servicios de acceso remoto \(RRAS\) VPN de DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br />-Características de enrutamiento de enrutamiento de RRAS RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />Las dependencias son las siguientes:<br /><br />-Internet Information Services \(IIS\) Web Server: esta característica es necesaria para configurar el sondeo de web de servidor y el valor predeterminado de ubicación de red.<br />-Windows Database-Used interno para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecuta el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />: Consola de administración de directivas de grupo<br />-Kit de administración de administrador de conexiones de RAS \(CMAK\)<br />-Windows PowerShell 3.0<br />-Infraestructura y herramientas de administración gráfico|  
|Equilibrio de carga de red|Esta característica ofrece equilibrio de carga en un clúster con Windows NLB.|  

## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  

-   Al menos dos equipos que cumplan los requisitos de hardware para Windows Server 2012.  

-   Para el escenario del equilibrador de carga externo, es necesario hardware dedicado \(; es decir, F5 BigIP\).  

-   Para probar el escenario, debe tener al menos un equipo que ejecuta Windows 10, configurado como un cliente de VPN de Always On.   

## <a name="BKMK_SOFT"></a>Requisitos de software  
Hay varios requisitos para este escenario:  

-   Requisitos de software para la implementación de un solo servidor. Para obtener más información, consulte [implementar un único servidor de DirectAccess con configuración avanzada](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un único acceso remoto).  

-   Además de los requisitos de software para un único servidor hay una serie de clúster\-requisitos específicos:  

    -   En cada servidor la dirección IP de clúster\-certificado HTTPS nombre de sujeto debe coincidir con la dirección ConnectTo. Una combinación de caracteres comodín y no admite la implementación de un clúster\-certificados con caracteres comodín en los servidores del clúster.  

    -   Si el servidor de ubicación de red está instalado en el servidor de acceso remoto, el certificado del servidor de ubicación de red debe tener el mismo nombre de firmante en cada servidor de clúster. Además, el nombre del certificado del servidor de ubicación de red no debe tener el mismo nombre que cualquier otro servidor en la implementación de DirectAccess.  

    -   IP\-se deben emitir los certificados de servidor de ubicación de red y HTTPS con el mismo método con el que se emitió el certificado en un único servidor. Por ejemplo, si el servidor único usa una entidad de certificación pública \(CA\) , a continuación, todos los servidores del clúster deben tener un certificado emitido por una CA pública. O si el servidor único usa un autoservicio\-certificado autofirmado para IP\-HTTPS, a continuación, todos los servidores del clúster deben hacer lo mismo.  

    -   El prefijo IPv6 asignado a equipos clientes de DirectAccess en clústeres de servidor debe ser de 59 bits. Si VPN está habilitado, el prefijo VPN también debe ser de 59 bits.  

## <a name="KnownIssues"></a>Problemas conocidos  
Los problemas que se mencionan a continuación son problemas conocidos de la configuración de un escenario de clúster:  

-   Después de configurar DirectAccess en IPv4\-solamente la implementación con un único adaptador de red y después el valor predeterminado de DNS64 \(la dirección IPv6 que contiene ": 3333::"\) se configura automáticamente en el adaptador de red al intentar habilitar carga\-equilibrio a través de la consola de administración de acceso remoto hace que un símbolo del sistema para el usuario que proporcione una DIP de IPv6. Si se proporciona una DIP de IPv6, se produce el siguiente error de configuración después de hacer clic en **Confirmar**: El parámetro es incorrecto.  

    Para resolver este problema:  

    1.  Descargue la copia de seguridad y restaure los scripts desde [Back up and Restore Remote Access Configuration](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Copia de seguridad de secuencias de comandos de copia de seguridad de los GPO de acceso remoto con los datos descargados\-remoteaccess.ps1.  

    3.  Intente habilitar el equilibrio de carga hasta el paso en el que se produce el error. En el cuadro de diálogo Habilitar equilibrio de carga, expanda el área de detalles, derecha\-haga clic en el área de detalles y, a continuación, haga clic en **copiar Script**.  

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

    8.  Si el cmdlet produce un error mientras se está ejecutando \(no debido a valores de entrada incorrectos\), ejecute el comando Restore\-RemoteAccess.ps1 y siga las instrucciones para asegurarse de que se mantiene la integridad de la configuración original .  

    9. Ahora puede volver a abrir la Consola de administración de acceso remoto.  
