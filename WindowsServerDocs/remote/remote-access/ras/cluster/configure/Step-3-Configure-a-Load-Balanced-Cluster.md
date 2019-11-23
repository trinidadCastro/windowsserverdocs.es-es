---
title: 'Paso 3: configurar un clúster con equilibrio de carga'
description: Este tema forma parte de la guía deploy Remote Access in a Cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fb7dca9a0f7875936cbb30cbc9c5e9e0a7473237
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404640"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Paso 3: configurar un clúster con equilibrio de carga

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de preparar los servidores para el clúster, configure el equilibrio de carga en el único servidor, configure los certificados necesarios e implemente el clúster.  
  
|Tarea|Descripción|  
|----|--------|  
|[3,1 configuración del prefijo IPv6](#BKMK_Prefix)|Si el entorno corporativo es IPv4 + IPv6, o solo IPv6, en el servidor de acceso remoto único, asegúrese de que el prefijo IPv6 asignado a los equipos cliente de DirectAccess es lo suficientemente grande como para abarcar todos los servidores del clúster.|  
|[3,2 habilitar el equilibrio de carga](#BKMK_NLB)|Habilite el equilibrio de carga en el único servidor de acceso remoto.|  
|[3,3 instalación del certificado IP-HTTPS](#BKMK_InstallIPHTTP)|Cada servidor del clúster requiere un certificado de servidor para autenticar la conexión IP-HTTPS.  Exporte el certificado IP-HTTPS desde el único servidor de acceso remoto e impleméntelo en cada servidor que vaya a agregar al clúster. Esto solo es necesario si se usan certificados no autofirmados.|  
|[3,4 instalar el certificado del servidor de ubicación de red](#BKMK_NLS)|Si el servidor único tiene el servidor de ubicación de red implementado localmente, deberá implementar el certificado del servidor de ubicación de red en cada servidor del clúster. Si el servidor de ubicación de red está hospedado en un servidor externo, no es necesario un certificado en cada servidor. Esto solo es necesario si se usan certificados no autofirmados.|  
|[3,5 agregar servidores al clúster](#BKMK_Add)|Agregue todos los servidores al clúster. El acceso remoto no debe estar configurado en los servidores que se van a agregar.|  
|[3,6 quitar un servidor del clúster](#BKMK_remove)|Instrucciones para quitar un servidor del clúster.|  
|[3,7 deshabilitar el equilibrio de carga](#BKBK_disable)|Instrucciones para deshabilitar el equilibrio de carga.|  
  
> [!NOTE]  
> La dirección IP seleccionada para la DIP no debe estar en uso en los adaptadores de red del primer servidor de acceso remoto del clúster. El inicio de la implementación de DirectAccess con VIP y DIP agregados al adaptador de red producirá un error.  
  
> [!NOTE]  
> Asegúrese de no usar una DIP que ya esté presente en otro equipo de la red.  
  
## <a name="BKMK_Prefix"></a>3,1 configuración del prefijo IPv6  
  
### <a name="configDA"></a>Para configurar el prefijo  
  
1.  En el servidor de acceso remoto, haga clic en **Inicio**y, a continuación, en **Administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**.  
  
3.  En el panel central de la consola, en el área **servidor de DirectAccess paso 2** , haga clic en **Editar**.  
  
4.  Haga clic en **configuración de prefijo**. En la página **configuración de prefijo** , en **prefijo IPv6 asignado a equipos cliente de DirectAccess**, escriba el prefijo IPv6 usado para equipos cliente de DirectAccess con una longitud de subred de 59, por ejemplo, **2001: db8:1: 1000::/59**. Si VPN también se habilitaron con IPv6, se mostraría un prefijo IPv6 y la longitud de la subred tendría que cambiarse a 59. Haz clic en **Siguiente**.  
  
5.  En el panel central de la consola, haga clic en **Finalizar**.  
  
6.  En el cuadro de diálogo **revisión de acceso remoto** , revise las opciones de configuración y, a continuación, haga clic en **aplicar**. En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
## <a name="BKMK_NLB"></a>3,2 habilitar el equilibrio de carga  
  
#### <a name="to-enable-load-balancing"></a>Para habilitar el equilibrio de carga  
  
1.  En el servidor de DirectAccess configurado, haga clic en **Inicio**y, a continuación, haga clic en **Administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En el panel izquierdo de la consola de administración de acceso remoto, haga clic en **configuración**y, a continuación, en el panel **tareas** , haga clic en **Habilitar equilibrio de carga**.  
  
3.  En el Asistente para habilitar equilibrio de carga, haga clic en **siguiente**.  
  
4.  En función de lo que haya elegido en los pasos de planeación:  
  
    1.  Windows NLB: en la página **método de equilibrio de carga** , haga clic en **usar equilibrio de carga de red (NLB) de Windows**y, a continuación, haga clic en **siguiente**.  
  
    2.  Equilibrador de carga externo: en la página **método de equilibrio de carga** , haga clic en **usar un equilibrador de carga externo**y, a continuación, haga clic en **siguiente**.  
  
5.  En una implementación de un único adaptador de red, en la página **direcciones IP dedicadas** , realice lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el cuadro **dirección IPv4** , escriba la nueva dirección IPv4 para este servidor de acceso remoto. la dirección IPv4 actual será la dirección IP virtual (VIP) del clúster de carga equilibrada. En el cuadro **máscara de subred** , escriba la máscara de subred.  
  
    2.  Si el entorno corporativo es IPv6 nativo, en el cuadro **dirección IPv6** , escriba la nueva dirección IPv6 para este servidor de acceso remoto. la dirección IPv6 actual será la VIP del clúster de carga equilibrada. En el cuadro **longitud de prefijo de subred** , escriba la longitud del prefijo de subred.  
  
6.  En una implementación de dos adaptadores de red, en la página **direcciones IP dedicadas externas** , realice lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el cuadro **dirección IPv4** , escriba la nueva dirección IPv4 externa para este servidor de acceso remoto. la dirección IPv4 actual será la dirección IP virtual (VIP) del clúster de equilibrio de carga. En el cuadro **máscara de subred** , escriba la máscara de subred.  
  
    2.  Si actualmente hay direcciones IPv6 nativas configuradas en el adaptador de red accesible desde Internet del servidor de acceso remoto, en el cuadro **dirección IPv6** , escriba la nueva dirección IPv6 externa para este servidor de acceso remoto. la dirección IPv6 actual será la VIP del clúster de equilibrio de carga. En el cuadro **longitud de prefijo de subred** , escriba la longitud del prefijo de subred.  
  
7.  En una implementación de dos adaptadores de red, en la página **direcciones IP dedicadas internas** , realice lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el cuadro **dirección IPv4** , escriba la nueva dirección IPv4 interna para este servidor de acceso remoto. la dirección IPv4 actual será la VIP del clúster de equilibrio de carga. En el cuadro **máscara de subred** , escriba la máscara de subred.  
  
    2.  Si el entorno corporativo es IPv6 nativo, en el cuadro **dirección IPv6** , escriba la nueva dirección IPv6 interna para este servidor de acceso remoto. la dirección IPv6 actual será la VIP del clúster de equilibrio de carga. En el cuadro **longitud de prefijo de subred** , escriba la longitud del prefijo de subred.  
  
8.  En la página **Resumen** , haga clic en **confirmar**.  
  
9. En el cuadro de diálogo **Habilitar equilibrio de carga** , haga clic en **cerrar**.  
  
10. En el Asistente para habilitar equilibrio de carga, haga clic en **cerrar**.  
  
    > [!NOTE]  
    > Si se está usando el equilibrio de carga externo, tenga en cuenta las direcciones IP virtuales y proporcione como en los equilibradores de carga externos.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Si decide usar Windows NLB en los pasos de planeación, ejecute lo siguiente:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Si decide usar un equilibrador de carga externo en los pasos de planeación: ejecute lo siguiente:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> Se recomienda no incluir los cambios en la configuración del equilibrador de carga con cambios en otras opciones, si utiliza GPO de almacenamiento provisional. Los cambios en la configuración del equilibrador de carga deben aplicarse primero y se deben realizar otros cambios en la configuración. Además, después de configurar el equilibrador de carga en un nuevo servidor de DirectAccess, deje tiempo para que los cambios de IP se apliquen y se repliquen en los servidores DNS de la empresa, antes de cambiar otras opciones de configuración de DirectAccess relacionadas con el nuevo clúster.  
  
## <a name="BKMK_InstallIPHTTP"></a>3,3 instalación del certificado IP-HTTPS  
Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.  
  
### <a name="IPHTTPSCert"></a>Para instalar el certificado IP-HTTPS  
  
1.  En el servidor de acceso remoto configurado, haga clic en **Inicio**, escriba **MMC** y presione Entrar. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el cuadro de diálogo **Agregar o quitar complementos** , haga clic en **certificados**, en **Agregar**, en **cuenta de equipo**, en **siguiente**, en **Finalizar**y, finalmente, en **Aceptar**.  
  
4.  En el panel izquierdo de la consola, vaya a **certificados (equipo local) \personal\certificados**. Haga clic con el botón derecho en el certificado IP-HTTPS, seleccione **todas las tareas** y haga clic en **exportar**.  
  
5.  En la página **Éste es el Asistente para exportación de certificados**, haga clic en **Siguiente**.  
  
6.  En la página **Exportar la clave privada** , haga clic en **Exportar la clave privada**y, a continuación, haga clic en **Siguiente**.  
  
7.  En la página **formato de archivo de exportación** , haga clic en **intercambio de Información Personal: PKCS #12 (. PFX)** y, a continuación, haga clic en **siguiente**.  
  
8.  En la página **seguridad** , active la casilla **contraseña** , escriba una contraseña en el cuadro **contraseña** y confirme la contraseña y, a continuación, haga clic en **siguiente**.  
  
9. En la página **archivo que se va a exportar** , escriba un nombre para el archivo de certificado y guárdelo en el escritorio y, a continuación, haga clic en **siguiente**.  
  
10. En la página **Finalización del Asistente para exportación de certificados**, haga clic en **Finalizar**.  
  
11. En el cuadro de diálogo **Asistente para exportación de certificados** , haga clic en **Aceptar**.  
  
12. Copie el certificado en todos los servidores que desee que sean miembros del clúster.  
  
13. En el nuevo servidor de DirectAccess, haga clic en **Inicio**, escriba **MMC** y presione Entrar. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
14. En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
15. En el cuadro de diálogo **Agregar o quitar complementos** , haga clic en **certificados**, en **Agregar**, en **cuenta de equipo**, en **siguiente**, en **Finalizar**y, finalmente, en **Aceptar**.  
  
16. En el panel izquierdo de la consola, vaya a **certificados (equipo local) \personal\certificados**. Haga clic con el botón secundario en el nodo **certificados** , seleccione **todas las tareas**y, a continuación, haga clic en **importar**.  
  
17. En la página **Éste es el Asistente para importación de certificados**, haga clic en **Siguiente**.  
  
18. En la página **archivo para importar** , haga clic en **examinar** para buscar el certificado. Seleccione el certificado y, a continuación, haga clic en **siguiente**.  
  
19. En la página **protección de clave privada** , en el cuadro **contraseña** , escriba la contraseña y, a continuación, haga clic en **siguiente**.  
  
20. En la página **Almacén de certificados**, haga clic en **Siguiente**.  
  
21. En la página **Finalización del Asistente para importación de certificados**, haga clic en **Finalizar**.  
  
22. En el cuadro de diálogo **Asistente para importación de certificados** , haga clic en **Aceptar**.  
  
23. Repita los pasos 13-22 en todos los servidores que desee que sean miembros del clúster.  
  
## <a name="BKMK_NLS"></a>3,4 instalar el certificado del servidor de ubicación de red  
Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar un certificado para la ubicación de red  
  
1.  En el servidor de acceso remoto, haga clic en **Inicio**, escriba **MMC**y, a continuación, presione Entrar. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  Haga clic en **Archivo** y, a continuación, haga clic en **Agregar o quitar complementos**.  
  
3.  Haga clic en **certificados**, **Agregar**, **cuenta de equipo**, **siguiente**, **equipo local**, **Finalizar**y, a continuación, haga clic en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En la página **solicitar certificados** , haga clic en la plantilla de certificado de servidor Web y, a continuación, haga clic en **se necesita más información para inscribirse en este certificado**.  
  
    Si no aparece la plantilla de certificado de servidor Web, asegúrese de que la cuenta de equipo del servidor de acceso remoto tiene permisos de inscripción para la plantilla de certificado de servidor Web. Para obtener más información, vea [configurar permisos en la plantilla de certificado de servidor Web](https://msdn.microsoft.com/library/ee649249.aspx).  
  
8.  En la pestaña **asunto** del cuadro de diálogo **propiedades de certificado** , en nombre de **sujeto**, en **tipo**, seleccione **nombre común**.  
  
9. En **valor**, escriba el nombre de dominio completo (FQDN) para el nombre de la intranet del sitio web del servidor de ubicación de red (por ejemplo, NLS.Corp.contoso.com) y, a continuación, haga clic en **Agregar**.  
  
10. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
11. En el panel de detalles del complemento Certificados, compruebe que se inscribió un nuevo certificado con el FQDN con el valor **Propósitos planteados** de **Autenticación del servidor**.  
  
12. Haga clic con el botón secundario en el certificado y, a continuación, haga clic en **Propiedades**.  
  
13. En **Nombre descriptivo**, escriba **Certificado de ubicación de red** y, a continuación, haga clic en **Aceptar**.  
  
    > [!TIP]  
    > Los pasos 12 y 13 son opcionales, pero facilitan la selección del certificado para la ubicación de red al configurar el acceso remoto.  
  
14. Repita este procedimiento en todos los servidores que desee que sean miembros del clúster.  
  
## <a name="BKMK_Add"></a>3,5 agregar servidores al clúster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Para agregar servidores al clúster  
  
1.  En el servidor de DirectAccess configurado, haga clic en **Inicio**y, a continuación, haga clic en **Administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. En el panel **tareas** , en **clúster de carga equilibrada**, haga clic en **Agregar o quitar servidores**.  
  
3.  En el cuadro de diálogo **Agregar o quitar servidores** , haga clic en **Agregar servidor**.  
  
4.  En el cuadro de diálogo **Agregar un servidor** , en la página **Seleccionar servidor** , escriba el nombre del servidor de acceso remoto adicional y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **adaptadores de red** , realice una de las acciones siguientes:  
  
    -   Si va a implementar una topología con dos adaptadores de red, en **adaptador externo**, seleccione el adaptador que está conectado a la red externa. En **adaptador interno**, seleccione el adaptador que está conectado a la red interna.  
  
    -   Si va a implementar una topología con un adaptador de red, en **adaptador de red**, seleccione el adaptador que está conectado a la red interna.  
  
6.  En la **Página adaptadores de red** , en **Seleccione el certificado usado para autenticar las conexiones IP-https**, haga clic en **examinar** para buscar y seleccionar el certificado IP-https y, a continuación, haga clic en **siguiente**.  
  
7.  En la página **servidor de ubicación de red** , haga clic en **examinar** para seleccionar el certificado para el sitio web del servidor de ubicación de red que se ejecuta en el servidor de acceso remoto y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > La página **servidor de ubicación de red** solo aparece cuando el sitio web del servidor de ubicación de red se ejecuta en el servidor de acceso remoto.  
  
    > [!NOTE]  
    > Si la VPN también se configuró en el servidor de acceso remoto, se le pedirá que agregue la información del grupo de direcciones IP de VPN en este momento.  
  
8.  En la página **Resumen** , haga clic en **Agregar**.  
  
9. En la página **Finalización**, haga clic en **Cerrar**.  
  
10. Repita este procedimiento para que todos los servidores de acceso remoto se agreguen al clúster.  
  
11. En el cuadro de diálogo **Agregar o quitar servidores** , haga clic en **confirmar**.  
  
12. En el cuadro de diálogo **Agregar y quitar servidores** , haga clic en **cerrar**.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Si no se ha habilitado VPN en un clúster con equilibrio de carga, no debe proporcionar ningún intervalo de direcciones VPN al agregar un nuevo servidor al clúster mediante los cmdlets de Windows PowerShell. Si lo ha hecho por error, quite el servidor del clúster y agréguelo de nuevo al clúster sin especificar los intervalos de direcciones de VPN.  
  
## <a name="BKMK_remove"></a>3,6 quitar un servidor del clúster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Para quitar un servidor del clúster  
  
1.  En el servidor de acceso remoto configurado, haga clic en **Inicio**y, a continuación, haga clic en **Administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. En el panel **tareas** , en **clúster de carga equilibrada**, haga clic en **Agregar o quitar servidores**.  
  
3.  En el cuadro de diálogo **Agregar o quitar servidores** , seleccione el servidor de acceso remoto que desea quitar y, a continuación, haga clic en **quitar servidor**.  
  
4.  En el cuadro de diálogo **quitar ADVERTENCIA de servidor** , asegúrese de elegir el servidor adecuado y, a continuación, haga clic en **Aceptar**.  
  
5.  Repita este procedimiento para que todos los servidores de acceso remoto se quiten del clúster.  
  
6.  En el cuadro de diálogo **Agregar o quitar servidores** , haga clic en **confirmar**.  
  
7.  En el cuadro de diálogo **Agregar y quitar servidores** , haga clic en **cerrar**.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="BKBK_disable"></a>3,7 deshabilitar el equilibrio de carga  
[Realice este paso con Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Para deshabilitar el equilibrio de carga  
  
1.  En el servidor de DirectAccess configurado, haga clic en **Inicio**y, a continuación, haga clic en **Administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. En el panel **tareas** , en **clúster de carga equilibrada**, haga clic en **deshabilitar equilibrio de carga**.  
  
3.  En el cuadro de diálogo **deshabilitar equilibrio de carga** , haga clic en **Aceptar**.  
  
4.  En el cuadro de diálogo **deshabilitar equilibrio de carga** , haga clic en **cerrar**.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
Al deshabilitar el equilibrio de carga, se quita la configuración de acceso remoto y la configuración de NLB (si se configura) de todos los servidores excepto del servidor desde el que se ejecuta. En este servidor de acceso remoto, se quitará la configuración de NLB (si se configuró), pero se conservará la configuración de acceso remoto.  
  
Al hacer clic en **quitar opciones de configuración** se quitará el acceso remoto y NLB (si está configurado) de todos los servidores de la implementación.  
  
> [!NOTE]  
> -   Si se desinstala el acceso remoto cuando se implementa el equilibrio de carga, todos los servidores quedan con DIP. Se quitan las VIP. Esto hace que todas las rutas de la red corporativa que están destinadas a las direcciones VIP generen un error. Esto también afecta a las entradas DNS que se resolvieron en las VIP, como el nombre de sujeto del certificado del servidor de ubicación de red. Para evitar este problema, deshabilite el equilibrio de carga, que deja las direcciones VIP en el último servidor de acceso remoto y, a continuación, desinstale el acceso remoto.  
> -   Después de usar el cmdlet **set-RemoteAccessLoadBalancer** para deshabilitar el equilibrio de carga, espere 2 minutos antes de ejecutar cualquier otro cmdlet. Esto también debe realizarse en cualquier script que ejecute otro cmdlet después del cmdlet **set-RemoteAccessLoadBalancer-Disable** .  
> -   Al deshabilitar el equilibrio de carga, se cambia la dirección IP virtual del clúster a una dirección IP dedicada. Como resultado, se producirá un error en cualquier operación que consulte el nombre del servidor hasta que expire la entrada DNS almacenada en caché en el servidor. Asegúrese de no ejecutar ningún cmdlet de acceso remoto de PowerShell después de deshabilitar el equilibrio de carga hasta que expire la memoria caché del servidor. Este problema es más común si intenta deshabilitar el equilibrio de carga en un equipo desde otra máquina que se encuentra en otro dominio. Esto también se produce si deshabilita el equilibrio de carga desde la consola de administración de acceso remoto y puede impedir que se cargue la configuración. La configuración se cargará después de que la memoria caché haya expirado o se haya vaciado.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 4: comprobar el clúster](Step-4-Verify-the-Cluster.md)  
  


