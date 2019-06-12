---
title: Paso 2 configurar el servidor de acceso remoto
description: En este tema forma parte de la Guía de los clientes de DirectAccess de administrar remotamente en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85bc7385fc3413bedb539cdbe9982b33dc653d6f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446892"
---
# <a name="step-2-configure-the-remote-access-server"></a>Paso 2 configurar el servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo configurar los ajustes de servidor y cliente que son necesarios para la administración remota de los clientes de DirectAccess. Antes de comenzar los pasos de implementación, asegúrese de que ha completado los pasos de planificación que se describen en [paso 2 Planear la implementación de acceso remoto](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|Tarea|Descripción|  
|----|--------|  
|Instalar el rol de acceso remoto|Instalar el rol de acceso remoto.|  
|Configurar el tipo de implementación|Configura el tipo de implementación como DirectAccess y VPN, solo DirectAccess o solo VPN.|  
|Configurar los clientes de DirectAccess|Configura el servidor de acceso remoto con los grupos de seguridad que contengan clientes de DirectAccess.|  
|Configurar el servidor de acceso remoto|Configure el servidor de acceso remoto.|  
|Configurar los servidores de infraestructura|Configura los servidores de infraestructura que se usan en la organización.|  
|Configurar servidores de aplicaciones|Configurar los servidores de aplicaciones para que requiera autenticación y cifrado.|  
|Configuración de resumen y GPO alternativos|Consulta el resumen de configuración de acceso remoto y, si fuera necesario, modifica el GPO.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Instalar el rol de acceso remoto  
Debe instalar el rol de acceso remoto en un servidor en su organización, que actuará como servidor de acceso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar el rol de acceso remoto  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Para instalar el rol de acceso remoto en servidores de DirectAccess  
  
1.  En el servidor de DirectAccess, en la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el **seleccionar Roles de servidor** cuadro de diálogo, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente**.  
  
4.  Haga clic en **siguiente** tres veces.  
  
5.  En el **seleccionar servicios de rol** cuadro de diálogo, seleccione **DirectAccess y VPN (RAS)** y, a continuación, haga clic en **agregar características**.  
  
6.  Seleccione **enrutamiento**, seleccione **Proxy de aplicación Web**, haga clic en **agregar características**y, a continuación, haga clic en **siguiente**.  
  
7. Haga clic en **Siguiente**y después en **Instalar**.  
  
8.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>Configurar el tipo de implementación  
Hay tres opciones que puede usar para implementar el acceso remoto desde la consola de administración de acceso remoto:  
  
-   DirectAccess y VPN  
  
-   Solo DirectAccess  
  
-   Solo VPN  
  
> [!NOTE]  
> Esta guía utiliza el método solo DirectAccess de implementación en los procedimientos de ejemplo.  
  
#### <a name="to-configure-the-deployment-type"></a>Para configurar el tipo de implementación  
  
1.  En el servidor de acceso remoto, abre la consola de administración de acceso remoto: En el **iniciar** pantalla, tipo, tipo **consola de administración de acceso remoto**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En el panel central de la consola de administración de acceso remoto, haz clic en **Ejecutar el Asistente para la instalación de acceso remoto**.  
  
3.  En el **configuración del acceso remoto** cuadro de diálogo, seleccione DirectAccess y VPN, solo DirectAccess o VPN solo.  
  
## <a name="BKMK_Clients"></a>Configurar a los clientes de DirectAccess  
Para que un equipo cliente se aprovisione para el uso de DirectAccess, este debe pertenecer al grupo de seguridad seleccionado. Después de configurar DirectAccess, los equipos cliente en el grupo de seguridad se aprovisionan para recibir los objetos de directiva de grupo (GPO) de DirectAccess para la administración remota.  
  
#### <a name="to-configure-directaccess-clients"></a>Cómo configurar los clientes de DirectAccess  
  
1.  En el área **Paso 1: Clientes remotos** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el Asistente para la instalación del cliente de DirectAccess, en el **escenario de implementación** página, haga clic en **implementar DirectAccess para la administración remota solo**y, a continuación, haga clic en **siguiente**.  
  
3.  En la página **Seleccionar grupos** , haz clic en **Agregar**.  
  
4.  En el **seleccionar grupos** cuadro de diálogo, seleccione los grupos de seguridad que contienen los equipos cliente de DirectAccess y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **Asistente para la conectividad de red**:  
  
    -   En la tabla, agregue los recursos que se usará para determinar la conectividad a la red interna. Se creará automáticamente un sondeo web predeterminado si no se configuran otros recursos. Al configurar las ubicaciones de sondeo web para determinar la conectividad a la red empresarial, asegúrese de que tiene al menos un sondeo basado en HTTP configurado. Configurar únicamente un sondeo de ping no es suficiente y podría provocar una no se detectara correctamente del estado de conectividad. Esto es porque ping está exento de IPsec. Como resultado, ping no garantiza que los túneles IPsec se establezcan correctamente.  
  
    -   Agrega la dirección de correo del servicio de asistencia para permitir a los usuarios enviar información si tienen problemas de conectividad.  
  
    -   Especifica un nombre descriptivo para la conexión de DirectAccess.  
  
    -   Si fuera necesario, selecciona la casilla **Permitir que los clientes de DirectAccess usen la resolución local de nombres**.  
  
        > [!NOTE]  
        > Cuando se habilita la resolución de nombres local, los usuarios que ejecutan el NCA pueden resolver nombres mediante el uso de los servidores DNS que están configurados en el equipo cliente de DirectAccess.  
  
6.  Haga clic en **Finalizar**.  
  
## <a name="BKMK_Server"></a>Configurar el servidor de acceso remoto  
Para implementar el acceso remoto, deberá configurar el servidor que actuará como servidor de acceso remoto con lo siguiente:  
  
1.  Adaptadores de red correctos  
  
2.  Una dirección URL pública para el servidor de acceso remoto al cliente que pueden conectar los equipos (la dirección ConnectTo)  
  
3.  Un certificado IP-HTTPS con un asunto que coincide con la dirección ConnectTo  
  
4.  Configuración de IPv6  
  
5.  Autenticación del equipo cliente  
  
#### <a name="to-configure-the-remote-access-server"></a>Para configurar el servidor de acceso remoto  
  
1.  En el área **Paso 2: Servidor de acceso remoto** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En la página **Topología de red** del Asistente para la instalación del servidor de acceso remoto, haz clic en la topología de implementación que usarás en la organización. En **Escriba el nombre público o dirección IPv4 que usan los clientes para conectarse al servidor de acceso remoto**, escribe el nombre público de la implementación (este nombre coincide con el nombre del firmante del certificado IP-HTTPS, por ejemplo, edge1.contoso.com) y haz clic en **Siguiente**.  
  
3.  En el **adaptadores de red** página, el asistente detecta automáticamente:  
  
    -   Adaptadores de red para las redes en la implementación. Si el asistente no detecta los adaptadores de red correctos, selecciona de forma manual los adaptadores correctos.  
  
    -   Certificado IP-HTTPS. Esto se basa en el nombre público para la implementación que estableció durante el paso anterior del asistente. Si el asistente no detecta el certificado IP-HTTPS correcto, haga clic en **examinar** para seleccionar manualmente el certificado correcto.  
  
4.  Haz clic en **Siguiente**.  
  
5.  En el **configuración de prefijo** página (esta página solo está visible si IPv6 no se detecta en la red interna), el asistente detecta automáticamente la configuración de IPv6 que se usa en la red interna. Si la implementación requiere prefijos adicionales, configura los prefijos IPv6 para la red interna (un prefijo IPv6 para asignarlo a los equipos cliente de DirectAccess y un prefijo IPv6 para asignarlo a los equipos cliente de VPN).  
  
6.  En la página **Autenticación**:  
  
    -   Para implementaciones multisitio con autenticación en dos fases es necesario usar la autenticación de certificados de equipo. Seleccione el **usar certificados de equipo** casilla de verificación para usar la autenticación de certificado de equipo y seleccione el certificado raíz IPsec.  
  
    -   Para habilitar equipos cliente que ejecutan Windows 7 se conecten mediante DirectAccess, seleccione el **los equipos cliente de habilitar Windows 7 se conecten mediante DirectAccess** casilla de verificación. Además, también es necesario usar la autenticación de certificados de equipo en este tipo de implementación.  
  
7.  Haga clic en **Finalizar**.  
  
## <a name="BKMK_Infra"></a>Configurar los servidores de infraestructura  
Para configurar los servidores de infraestructura en una implementación de acceso remoto, debe configurar lo siguiente:  
  
-   Servidor de ubicación de red  
  
-   Lista de búsqueda de sufijo de configuración de DNS, incluidos los de DNS  
  
-   Todos los servidores de administración que no se detectan automáticamente mediante acceso remoto  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Para configurar los servidores de infraestructura  
  
1.  En el área **Paso 3: Servidores de infraestructura** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el Asistente para la instalación del servidor de infraestructura, en la página **Servidor de ubicación de red**, haz clic en la opción que se corresponda con la ubicación del servidor de ubicación de red de la implementación.  
  
    -   Si el servidor de ubicación de red está en un servidor web remoto, escriba la dirección URL y, a continuación, haga clic en **validar** antes de continuar.  
  
    -   Si el servidor de ubicación de red se encuentra en el servidor de acceso remoto, haz clic en **Examinar** para buscar el certificado correspondiente y después haz clic en **Siguiente**.  
  
3.  En el **DNS** página, en la tabla, escriba los sufijos de nombre adicionales que se aplicarán como exenciones de la tabla de directivas de resolución de nombres (NRPT). Selecciona una opción de resolución local de nombres y después haz clic en **Siguiente**.  
  
4.  En el **lista de búsqueda de sufijos DNS** página, el servidor de acceso remoto detecta automáticamente los sufijos de dominio en la implementación. Utilice la **agregar** y **quitar** botones para crear la lista de sufijos de dominio que desea usar. Para agregar un nuevo sufijo de dominio, especifica el sufijo en el campo **Nuevo sufijo** y haz clic en **Agregar**. Haz clic en **Siguiente**.  
  
5.  En el **administración** página, agregue los servidores de administración que no se detectan automáticamente y, a continuación, haga clic en **siguiente**. Acceso remoto agrega automáticamente los controladores de dominio y los servidores de System Center Configuration Manager.  
  
6.  Haga clic en **Finalizar**.  
  
## <a name="BKMK_App"></a>Configurar servidores de aplicaciones  
En una implementación completa de acceso remoto, la configuración de servidores de aplicaciones es una tarea opcional. En este escenario para la administración remota de los clientes de DirectAccess, servidores de aplicaciones no se utilizan y este paso está atenuado para indicar que no está activo. Haga clic en **finalizar** para aplicar la configuración.  
  
## <a name="BKMK_GPO"></a>Configuración resumen y GPO alternativos  
Cuando completes la configuración de acceso remoto verás el cuadro de diálogo **Revisión de acceso remoto**. Podrás revisar todas las opciones que hayas seleccionado anteriormente, como por ejemplo:  
  
-   **Configuración de GPO**  
  
    Se muestran el nombre de GPO de servidor de DirectAccess y el nombre de GPO de cliente. Puede hacer clic en el **cambio** vínculo junto a la **configuración del GPO** encabezado para modificar la configuración del GPO.  
  
-   **Clientes remotos**  
  
    Se muestra la configuración de cliente de DirectAccess, incluido el grupo de seguridad, los comprobadores de conectividad y nombre de la conexión de DirectAccess.  
  
-   **Servidor de acceso remoto**  
  
    Se muestra la configuración de DirectAccess, incluido el nombre público y la dirección, la configuración del adaptador de red y la información del certificado.  
  
-   **Servidores de infraestructura**  
  
    Esta lista contiene la URL del servidor de ubicación de red, los sufijos DNS usados por clientes de DirectAccess e información sobre el servidor de administración.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 3: Comprobar la implementación](Step-3-Verify-the-Deployment_2.md)  
  
  


