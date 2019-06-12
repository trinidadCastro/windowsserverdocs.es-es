---
title: Paso 2 configurar el servidor de DirectAccess básica
description: Este tema forma parte de la Guía de implementar un único servidor de DirectAccess mediante el Asistente de iniciado para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9983fb475143109d191f3b6d69afef48d109472a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446952"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>Paso 2 configurar el servidor de DirectAccess básica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo configurar los ajustes de servidor y cliente necesarios para una implementación básica de DirectAccess. Antes de comenzar los pasos de implementación, asegúrese de que ha completado los pasos de planificación descritos en [planear una implementación básica de DirectAccess](Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Tarea|Descripción|  
|----|--------|  
|Instalar el rol de acceso remoto|Instalar el rol de acceso remoto.|  
|Configurar DirectAccess con el Asistente para introducción|El nuevo Asistente para introducción presenta una experiencia de configuración considerablemente simplificada. El asistente enmascara la complejidad de DirectAccess, y permite una instalación automatizada en unos pocos pasos sencillos. El asistente ofrece al administrador una experiencia sin problemas al configurar el proxy Kerberos automáticamente y evitar así la necesidad de una implementación PKI interna.|  
|Actualizar clientes con la configuración de DirectAccess|Para recibir la configuración de DirectAccess, los clientes deben actualizar la directiva de grupo mientras están conectados a la intranet.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Instalar el rol de acceso remoto  
Para implementar acceso remoto debes instalar el rol de acceso remoto en un servidor de la organización que actúe como el servidor de acceso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar el rol de acceso remoto  
  
1.  En el servidor de acceso remoto, en la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el cuadro de diálogo **Seleccionar roles de servidor** , seleccione **Acceso remoto**y, a continuación, haga clic en **Siguiente**.  
  
4.  En el cuadro de diálogo **Seleccionar características**, haga clic en **Siguiente**.  
  
5.  Haga clic en **siguiente**y, a continuación, en el **seleccionar servicios de rol** cuadro de diálogo, haga clic en el **DirectAccess y VPN (RAS)** casilla de verificación.  
  
6.  Haga clic en **agregar características**, haga clic en **siguiente**y, a continuación, haga clic en **instalar**.  
  
7.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
![Windows PowerShell](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de instalar el rol de acceso remoto: 

1. Abra PowerShell como administrador.

2. Instale la característica de acceso remoto:

   ```  
   Install-WindowsFeature RemoteAccess   
   ```  

3. Reinicie el equipo:

   ```
   Restart-Computer
   ```
   
4. Instale PowerShell de acceso remoto:

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```



  
## <a name="configure-directaccess-with-the-getting-started-wizard"></a>Configurar DirectAccess con el Asistente para introducción  
  
#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>Para configurar DirectAccess con el Asistente para introducción  
  
1.  En Administrador del servidor haga clic en **Herramientas** y, a continuación, haga clic en **Administración de acceso remoto**.  
  
2.  En la consola de administración de acceso remoto, seleccione el servicio de rol para configurar en el panel de navegación izquierdo y, a continuación, haga clic en **ejecutar el Asistente para introducción**.  
  
3.  Haga clic en **Implementar solo DirectAccess**.  
  
4.  Seleccione la topología de la configuración de red y escriba el nombre público al cual se conectarán los clientes de acceso remoto. Haz clic en **Siguiente**.  
  
    > [!NOTE]  
    > De forma predeterminada, el Asistente para introducción implementa DirectAccess en todos los equipos portátiles del dominio al aplicar un filtro WMI a la configuración GPO de cliente.  
  
5.  Haga clic en **Finalizar**.  
  
6.  Dado que no se usa ninguna PKI en esta implementación, si no se encuentran los certificados, el asistente aprovisionará automáticamente certificados autofirmados para IP-HTTPS y el servidor de ubicación de red, y habilitará automáticamente el proxy Kerberos. El asistente también habilitará NAT64 y DNS64 para la traducción de protocolo en el entorno de solo IPv4. Una vez que el asistente haya terminado de aplicar la configuración correctamente, haga clic en **Cerrar**.  
  
7.  En el árbol de la consola de administración de acceso remoto, haga clic en **Estado de las operaciones**. Espere hasta que el estado de todos los monitores se muestren como "En funcionamiento". En el panel de tareas debajo de Supervisión, haga clic en **Actualizar** periódicamente para actualizar la pantalla.  
  
## <a name="update-clients-with-the-directaccess-configuration"></a>Actualizar clientes con la configuración de DirectAccess  
  
#### <a name="to-update-directaccess-clients"></a>Para actualizar los clientes de DirectAccess  
  
1.  Abra PowerShell como administrador.  
  
2.  En la ventana de PowerShell, escriba **gpupdate** y, a continuación, presione **ENTRAR**.  
  
3.  Espere a que termine correctamente la actualización de la directiva de equipo.  
  
4.  Escriba **Get-DnsClientNrptPolicy** y presione **ENTRAR**  
  
    Se muestran las entradas de la tabla de directivas de resolución de nombres (NRPT) para DirectAccess. Tenga en cuenta que se muestra la exención del servidor NLS. El Asistente para introducción creó esta entrada DNS automáticamente para el servidor de DirectAccess, y aprovisionó un certificado autofirmado asociado de modo que el servidor de DirectAccess puede funcionar como el servidor de ubicación de red.  
  
5.  Escriba **Get-NCSIPolicyConfiguration** y presione **ENTRAR**. Se muestra la configuración del indicador del estado de conectividad de red implementada por el asistente. Tenga en cuenta el valor de DomainLocationDeterminationURL. Cada vez que la dirección URL del servidor de ubicación de red sea accesible, el cliente determinará que está dentro de la red corporativa, y no se aplicará la configuración NRPT.  
  
6.  Escriba **Get-DAConnectionStatus** y presione **ENTRAR**. Debido a que el cliente puede acceder a la dirección URL del servidor de ubicación de red, el estado aparecerá como **ConectadoLocalmente**.  
  
## <a name="BKMK_Links"></a>Paso anterior  
  
-   [Paso 1: Configurar la infraestructura de DirectAccess](Step-1-Configure-the-DirectAccess-Infrastructure.md)  
  
## <a name="next-step"></a>Paso siguiente  
  
-   [Paso 3 comprobar las implementaciones de DirectAccess básica](da-basic-configure-s3-verify.md)  
  


