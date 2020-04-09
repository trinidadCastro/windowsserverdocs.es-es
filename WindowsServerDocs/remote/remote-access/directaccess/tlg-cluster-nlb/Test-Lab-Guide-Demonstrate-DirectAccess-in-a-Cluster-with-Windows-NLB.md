---
title: 'Guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB'
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc9f619835262a47894f23c5c04e4c89d4463d7a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819068"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>Guía del laboratorio de prueba: mostrar DirectAccess en un clúster con Windows NLB

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El acceso remoto es un rol de servidor de los sistemas operativos Windows Server 2016, Windows Server 2012 R2 andWindows Server 2012 que permite a los usuarios remotos acceder de forma segura a los recursos de red internos mediante DirectAccess o RRAS VPN. Esta guía contiene instrucciones paso a paso que amplían la [guía del laboratorio de prueba: mostrar la instalación de un solo servidor DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para mostrar la configuración de clúster y el equilibrio de carga de red de DirectAccess.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía contiene instrucciones para configurar y mostrar el acceso remoto usando seis servidores y dos equipos cliente. El laboratorio de prueba de Acceso remoto finalizado con NLB simula una intranet, la conexión a Internet y una red doméstica, y demuestra la funcionalidad de Acceso remoto en distintos escenarios de conexión a Internet.  
  
> [!IMPORTANT]  
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.  
  
## <a name="known-issues"></a><a name="KnownIssues"></a>Problemas conocidos  
Los problemas que se mencionan a continuación son problemas conocidos de la configuración de un escenario de clúster:  
  
-   Después de configurar DirectAccess en una implementación de solo IPv4 con un solo adaptador de red y después de que el valor predeterminado de DNS64 (la dirección IPv6 que contiene ":3333::") se configure automáticamente en el adaptador de red, al intentar habilitar el equilibrio de carga a través de la Consola de administración de acceso remoto se muestra un mensaje al usuario que le indica que proporcione una DIP de IPv6. Si se proporciona una DIP de IPv6, después de hacer clic en **Confirmar** , se produce el siguiente error de configuración: el parámetro es incorrecto.  
  
    Para solucionar este problema:  
  
    1.  Descargue la copia de seguridad y restaure los scripts desde [Back up and Restore Remote Access Configuration](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  
  
    2.  Realice una copia de seguridad de los GPO de acceso remoto mediante el script descargado Backup-RemoteAccess.ps1.  
  
    3.  Intente habilitar el equilibrio de carga hasta el paso en el que se produce el error. En el cuadro de diálogo Habilitar equilibrio de carga, expanda el área de detalles, haga clic con el botón derecho en esta área y, a continuación, haga clic en **Copiar script**.  
  
    4.  Abra el Bloc de notas y pegue el contenido del Portapapeles. Por ejemplo:  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    5.  Cierre los cuadros de diálogo de acceso remoto abiertos y la Consola de administración de acceso remoto.  
  
    6.  Edite el texto pegado y quite las direcciones IPv6. Por ejemplo:  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    7.  En una ventana de PowerShell con privilegios elevados, ejecute el comando del paso anterior.  
  
    8.  Si se produce un error en el cmdlet mientras se está ejecutando (no debido a valores de entrada incorrectos), ejecute el comando Restore-RemoteAccess.ps1 y siga las instrucciones para asegurarse de que se mantiene la integridad de la configuración original.  
  
    9. Ahora puede volver a abrir la Consola de administración de acceso remoto.  
  


