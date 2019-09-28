---
title: Paso 2 configurar el servidor de acceso remoto
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b4e3c2f4a27652e7b28b826981d192d6a4c6c107
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404553"
---
# <a name="step-2-configure-the-remote-access-server"></a>Paso 2 configurar el servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo configurar las opciones de cliente y servidor necesarias para la administración remota de clientes de DirectAccess. Antes de comenzar con los pasos de implementación, asegúrese de que ha completado los pasos de planeación descritos en [el paso 2 planear la implementación de acceso remoto](../plan/Step-2-Plan-the-Remote-Access-Deployment.md).  
  
|Tarea|Descripción|  
|----|--------|  
|Instalar el rol de acceso remoto|Instalar el rol de acceso remoto.|  
|Configurar el tipo de implementación|Configura el tipo de implementación como DirectAccess y VPN, solo DirectAccess o solo VPN.|  
|Configurar los clientes de DirectAccess|Configura el servidor de acceso remoto con los grupos de seguridad que contengan clientes de DirectAccess.|  
|Configurar el servidor de acceso remoto|Establezca la configuración del servidor de acceso remoto.|  
|Configurar los servidores de infraestructura|Configura los servidores de infraestructura que se usan en la organización.|  
|Configurar servidores de aplicaciones|Configure los servidores de aplicaciones para que requieran autenticación y cifrado.|  
|Configuración de resumen y GPO alternativos|Consulta el resumen de configuración de acceso remoto y, si fuera necesario, modifica el GPO.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>Instalar el rol de acceso remoto  
Debe instalar el rol de acceso remoto en un servidor de su organización que actuará como el servidor de acceso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Para instalar el rol de acceso remoto  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>Para instalar el rol de acceso remoto en los servidores de DirectAccess  
  
1.  En el servidor de DirectAccess, en la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el cuadro de diálogo **Seleccionar roles de servidor** , seleccione **acceso remoto**y, a continuación, haga clic en **siguiente**.  
  
4.  Haga clic en **siguiente** tres veces.  
  
5.  En el cuadro de diálogo **seleccionar servicios de rol** , seleccione **DirectAccess y VPN (RAS)** y, a continuación, haga clic en **Agregar características**.  
  
6.  Seleccione **enrutamiento**, **proxy de aplicación web**, haga clic en **Agregar características**y, a continuación, haga clic en **siguiente**.  
  
7. Haga clic en **Siguiente**y después en **Instalar**.  
  
8.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows powershell</em> de @no__t 0Windows PowerShell***  
  
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
> En esta guía se usa el método de implementación de solo DirectAccess en los procedimientos de ejemplo.  
  
#### <a name="to-configure-the-deployment-type"></a>Para configurar el tipo de implementación  
  
1.  En el servidor de acceso remoto, abre la consola de administración de acceso remoto: En la pantalla **Inicio** , escriba, escriba **consola de administración de acceso remoto**y, a continuación, presione Entrar. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En el panel central de la consola de administración de acceso remoto, haz clic en **Ejecutar el Asistente para la instalación de acceso remoto**.  
  
3.  En el cuadro de diálogo **Configurar acceso remoto** , seleccione DIRECTACCESS y VPN, solo DirectAccess o solo VPN.  
  
## <a name="BKMK_Clients"></a>Configurar clientes de DirectAccess  
Para que un equipo cliente se aprovisione para el uso de DirectAccess, este debe pertenecer al grupo de seguridad seleccionado. Una vez configurado DirectAccess, los equipos cliente del grupo de seguridad se aprovisionan para recibir los objetos de directiva de grupo de DirectAccess (GPO) para la administración remota.  
  
#### <a name="to-configure-directaccess-clients"></a>Cómo configurar los clientes de DirectAccess  
  
1.  En el área **Paso 1: Clientes remotos** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el Asistente para la instalación del cliente de DirectAccess, en la página **escenario de implementación** , haga clic en **implementar DirectAccess solo para la administración remota**y, a continuación, haga clic en **siguiente**.  
  
3.  En la página **Seleccionar grupos** , haz clic en **Agregar**.  
  
4.  En el cuadro de diálogo **seleccionar grupos** , seleccione los grupos de seguridad que contienen los equipos cliente de DirectAccess y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **Asistente para la conectividad de red**:  
  
    -   En la tabla, agregue los recursos que se usarán para determinar la conectividad a la red interna. Se creará automáticamente un sondeo web predeterminado si no se configuran otros recursos. Al configurar las ubicaciones de sondeo web para determinar la conectividad a la red de la empresa, asegúrese de que tiene configurado al menos un sondeo basado en HTTP. La configuración de un sondeo de ping no es suficiente y podría dar lugar a una determinación inexacta del estado de conectividad. Esto se debe a que el ping está exento de IPsec. Como resultado, ping no garantiza que se establezcan correctamente los túneles IPsec.  
  
    -   Agrega la dirección de correo del servicio de asistencia para permitir a los usuarios enviar información si tienen problemas de conectividad.  
  
    -   Especifica un nombre descriptivo para la conexión de DirectAccess.  
  
    -   Si fuera necesario, selecciona la casilla **Permitir que los clientes de DirectAccess usen la resolución local de nombres**.  
  
        > [!NOTE]  
        > Cuando se habilita la resolución local de nombres, los usuarios que ejecutan NCA pueden resolver nombres mediante el uso de servidores DNS que están configurados en el equipo cliente de DirectAccess.  
  
6.  Haga clic en **Finalizar**.  
  
## <a name="BKMK_Server"></a>Configurar el servidor de acceso remoto  
Para implementar el acceso remoto, debe configurar el servidor que actuará como el servidor de acceso remoto con lo siguiente:  
  
1.  Adaptadores de red correctos  
  
2.  Una dirección URL pública para el servidor de acceso remoto al que pueden conectarse los equipos cliente (la dirección ConnectTo)  
  
3.  Un certificado IP-HTTPS con un firmante que coincida con la dirección ConnectTo  
  
4.  Configuración de IPv6  
  
5.  Autenticación de equipos cliente  
  
#### <a name="to-configure-the-remote-access-server"></a>Para configurar el servidor de acceso remoto  
  
1.  En el área **Paso 2: Servidor de acceso remoto** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En la página **Topología de red** del Asistente para la instalación del servidor de acceso remoto, haz clic en la topología de implementación que usarás en la organización. En **Escriba el nombre público o dirección IPv4 que usan los clientes para conectarse al servidor de acceso remoto**, escribe el nombre público de la implementación (este nombre coincide con el nombre del firmante del certificado IP-HTTPS, por ejemplo, edge1.contoso.com) y haz clic en **Siguiente**.  
  
3.  En la página **adaptadores de red** , el asistente detecta automáticamente:  
  
    -   Adaptadores de red para las redes de la implementación. Si el asistente no detecta los adaptadores de red correctos, selecciona de forma manual los adaptadores correctos.  
  
    -   Certificado IP-HTTPS. Esto se basa en el nombre público de la implementación que estableció en el paso anterior del asistente. Si el asistente no detecta el certificado IP-HTTPS correcto, haga clic en **examinar** para seleccionar manualmente el certificado correcto.  
  
4.  Haz clic en **Siguiente**.  
  
5.  En la página **configuración de prefijo** (esta página solo está visible si se detecta IPv6 en la red interna), el asistente detecta automáticamente la configuración de IPv6 que se usa en la red interna. Si la implementación requiere prefijos adicionales, configura los prefijos IPv6 para la red interna (un prefijo IPv6 para asignarlo a los equipos cliente de DirectAccess y un prefijo IPv6 para asignarlo a los equipos cliente de VPN).  
  
6.  En la página **Autenticación**:  
  
    -   Para implementaciones multisitio con autenticación en dos fases es necesario usar la autenticación de certificados de equipo. Active la casilla **usar certificados de equipo** para usar autenticación de certificado de equipo y seleccione el certificado raíz de IPSec.  
  
    -   Para permitir que los equipos cliente que ejecutan Windows 7 se conecten a través de DirectAccess, active la casilla **Habilitar equipos cliente de Windows 7 para que se conecten a través de DirectAccess** . Además, también es necesario usar la autenticación de certificados de equipo en este tipo de implementación.  
  
7.  Haga clic en **Finalizar**.  
  
## <a name="BKMK_Infra"></a>Configurar los servidores de infraestructura  
Para configurar los servidores de infraestructura en una implementación de acceso remoto, debe configurar lo siguiente:  
  
-   Servidor de ubicación de red  
  
-   Configuración de DNS, incluida la lista de búsqueda de sufijos DNS  
  
-   Los servidores de administración que no se detectan automáticamente mediante el acceso remoto  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Para configurar los servidores de infraestructura  
  
1.  En el área **Paso 3: Servidores de infraestructura** del panel central de la consola de administración de acceso remoto, haz clic en **Configurar**.  
  
2.  En el Asistente para la instalación del servidor de infraestructura, en la página **Servidor de ubicación de red**, haz clic en la opción que se corresponda con la ubicación del servidor de ubicación de red de la implementación.  
  
    -   Si el servidor de ubicación de red se encuentra en un servidor Web remoto, escriba la dirección URL y, a continuación, haga clic en **validar** antes de continuar.  
  
    -   Si el servidor de ubicación de red se encuentra en el servidor de acceso remoto, haz clic en **Examinar** para buscar el certificado correspondiente y después haz clic en **Siguiente**.  
  
3.  En la página **DNS** , en la tabla, escriba sufijos de nombre adicionales que se aplicarán como exenciones de la tabla de directivas de resolución de nombres (NRPT). Selecciona una opción de resolución local de nombres y después haz clic en **Siguiente**.  
  
4.  En la página **lista de búsqueda de sufijos DNS** , el servidor de acceso remoto detecta automáticamente los sufijos de dominio en la implementación. Use los botones **Agregar** y **quitar** para crear la lista de sufijos de dominio que desea usar. Para agregar un nuevo sufijo de dominio, especifica el sufijo en el campo **Nuevo sufijo** y haz clic en **Agregar**. Haz clic en **Siguiente**.  
  
5.  En la página **Administración** , agregue los servidores de administración que no se detecten automáticamente y, a continuación, haga clic en **siguiente**. Acceso remoto agrega automáticamente los controladores de dominio y los servidores de System Center Configuration Manager.  
  
6.  Haga clic en **Finalizar**.  
  
## <a name="BKMK_App"></a>Configurar servidores de aplicaciones  
En una implementación de acceso remoto completa, la configuración de los servidores de aplicaciones es una tarea opcional. En este escenario para la administración remota de clientes de DirectAccess, los servidores de aplicaciones no se usan y este paso está atenuado para indicar que no está activo. Haga clic en **Finalizar** para aplicar la configuración.  
  
## <a name="BKMK_GPO"></a>Resumen de configuración y GPO alternativos  
Cuando completes la configuración de acceso remoto verás el cuadro de diálogo **Revisión de acceso remoto**. Podrás revisar todas las opciones que hayas seleccionado anteriormente, como por ejemplo:  
  
-   **Configuración de GPO**  
  
    Se enumeran el nombre del GPO del servidor de DirectAccess y el nombre del GPO del cliente. Puede hacer clic en el vínculo **cambiar** junto al encabezado **configuración de GPO** para modificar la configuración del GPO.  
  
-   **Clientes remotos**  
  
    Se muestra la configuración del cliente de DirectAccess, incluidos el grupo de seguridad, los comprobadores de conectividad y el nombre de conexión de DirectAccess.  
  
-   **Servidor de acceso remoto**  
  
    Se muestra la configuración de DirectAccess, incluidos el nombre público y la dirección, la configuración del adaptador de red y la información del certificado.  
  
-   **Servidores de infraestructura**  
  
    Esta lista contiene la URL del servidor de ubicación de red, los sufijos DNS usados por clientes de DirectAccess e información sobre el servidor de administración.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 3: Comprobar la implementación](Step-3-Verify-the-Deployment_2.md)  
  
  


