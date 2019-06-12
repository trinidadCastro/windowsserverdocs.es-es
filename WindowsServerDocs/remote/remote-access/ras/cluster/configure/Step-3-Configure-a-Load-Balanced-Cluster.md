---
title: Paso 3 configurar un clúster con equilibrio de carga
description: En este tema forma parte de la Guía de implementación de acceso remoto en un clúster en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f835e27a80e661ff1f066af4779bd7c033cddc99
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446624"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Paso 3 configurar un clúster con equilibrio de carga

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de preparar servidores para el clúster, configurar el equilibrio de carga en el servidor único, configurar los certificados necesarios e implementar el clúster.  
  
|Tarea|Descripción|  
|----|--------|  
|[3.1 configure el prefijo de IPv6](#BKMK_Prefix)|Si el entorno corporativo es IPv4 + IPv6, o solo para IPv6, a continuación, en el servidor de acceso remoto único, asegúrese de que el prefijo IPv6 asignado a los equipos cliente de DirectAccess es lo suficientemente grande como para cubrir todos los servidores del clúster.|  
|[3.2 Habilitar equilibrio de carga](#BKMK_NLB)|Habilitar el equilibrio de carga en el servidor de acceso remoto único.|  
|[3.3 instalar el certificado IP-HTTPS](#BKMK_InstallIPHTTP)|Cada servidor del clúster requiere un certificado de servidor para autenticar la conexión IP-HTTPS.  Exportar el certificado IP-HTTPS desde el servidor de acceso remoto único e implementarlo en cada servidor que se va a agregar al clúster. Esto es necesario sólo si se usan certificados no autofirmados.|  
|[3.4 instalar el certificado de servidor de ubicación de red](#BKMK_NLS)|Si el servidor solo tiene el servidor de ubicación de red implementado localmente, deberá implementar el certificado de servidor de ubicación de red en cada servidor del clúster. Si el servidor de ubicación de red está hospedado en un servidor externo, no se requiere un certificado en cada servidor. Esto es necesario sólo si se usan certificados no autofirmados.|  
|[3.5 agregar servidores al clúster](#BKMK_Add)|Agregue todos los servidores en el clúster. Acceso remoto no debe configurarse en los servidores que se va a agregar.|  
|[3.6 quitar un servidor del clúster](#BKMK_remove)|Instrucciones para quitar un servidor del clúster.|  
|[3.7 deshabilitar Equilibrio de carga](#BKBK_disable)|Instrucciones para deshabilitar el equilibrio de carga.|  
  
> [!NOTE]  
> No debe ser la dirección IP que está seleccionada para la DIP en uso en los adaptadores de red del primer servidor de acceso remoto en el clúster. A partir de la implementación de DirectAccess con la VIP y DIP agregado para el adaptador de red se producirá un error.  
  
> [!NOTE]  
> Asegúrese de que no se debe utilizar una DIP que ya está presente en otro equipo de la red.  
  
## <a name="BKMK_Prefix"></a>3.1 configure el prefijo de IPv6  
  
### <a name="configDA"></a>Para configurar el prefijo  
  
1.  En el servidor de acceso remoto, haga clic en **iniciar**y, a continuación, haga clic en **administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**.  
  
3.  En el panel central de la consola, en el **paso 2: servidor de DirectAccess** área, haga clic en **editar**.  
  
4.  Haga clic en **configuración de prefijo**. En el **configuración de prefijo** página **prefijo IPv6 asignado a los equipos cliente de DirectAccess**, escriba el prefijo IPv6 que se usa para los equipos cliente de DirectAccess con una longitud de la subred de 59, por ejemplo, **2001:db8:1:1000:: / 59**. Si VPN también se habilita con IPv6, a continuación, se mostraría un prefijo IPv6 y la longitud de la subred deberá cambiarse a 59. Haz clic en **Siguiente**.  
  
5.  En el panel central de la consola, haga clic en **finalizar**.  
  
6.  En el **revisión de acceso remoto** cuadro de diálogo, revisar la configuración predeterminada y, a continuación, haga clic en **aplicar**. En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.  
  
## <a name="BKMK_NLB"></a>3.2 Habilitar equilibrio de carga  
  
#### <a name="to-enable-load-balancing"></a>Para habilitar el equilibrio de carga  
  
1.  En el servidor de DirectAccess configurado, haga clic en **iniciar**y, a continuación, haga clic en **administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, en el panel izquierdo, haga clic en **configuración**y, a continuación, en el **tareas** panel, haga clic en **habilitar Equilibrio de carga**.  
  
3.  Habilitar Asistente de equilibrio de carga, haga clic en **siguiente**.  
  
4.  Dependiendo de lo que eligió en pasos de planeación:  
  
    1.  Windows NLB: En el **el método de equilibrio de carga** página, haga clic en **equilibrio de carga de red (NLB) Use Windows**y, a continuación, haga clic en **siguiente**.  
  
    2.  Equilibrador de carga externo: En el **método de equilibrio de carga** página, haga clic en **uso de un equilibrador de carga externo**y, a continuación, haga clic en **siguiente**.  
  
5.  En una implementación de adaptador de red único, en el **direcciones IP dedicadas** página, haga lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el **dirección IPv4** cuadro, escriba la nueva dirección IPv4 para este servidor de acceso remoto; la actual IPv4 dirección será la dirección IP virtual (VIP) del clúster con equilibrio de carga. En el **máscara de subred** , escriba la máscara de subred.  
  
    2.  Si el entorno corporativo es IPv6 nativo, a continuación, en el **dirección IPv6** cuadro, escriba la nueva dirección IPv6 para este servidor de acceso remoto; la actual IPv6 dirección será la dirección VIP del clúster con equilibrio de carga. En el **longitud del prefijo de subred** , escriba la longitud del prefijo de subred.  
  
6.  En un dos implementación del adaptador de red en el **de direcciones IP externas dedicado** página, haga lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el **dirección IPv4** cuadro, escriba la nueva dirección IPv4 externa para este servidor de acceso remoto; la actual IPv4 dirección será la dirección IP virtual (VIP) de equilibrio de carga del clúster. En el **máscara de subred** , escriba la máscara de subred.  
  
    2.  Si hay direcciones de IPv6 nativas actualmente configuradas en el adaptador de red a través de internet del servidor de acceso remoto, en el **dirección IPv6** , escriba la nueva dirección IPv6 externa para el acceso remoto IPv6 actual; de servidor dirección será la dirección VIP del clúster de equilibrio de carga. En el **longitud del prefijo de subred** , escriba la longitud del prefijo de subred.  
  
7.  En un dos implementación del adaptador de red en el **las direcciones IP internas dedicado** página, haga lo siguiente y, a continuación, haga clic en **siguiente**:  
  
    1.  En el **dirección IPv4** cuadro, escriba la nueva dirección IPv4 interna de este servidor de acceso remoto; la actual IPv4 dirección será la dirección VIP del equilibrio de carga del clúster. En el **máscara de subred** , escriba la máscara de subred.  
  
    2.  Si el entorno corporativo es IPv6 nativo, a continuación, en el **dirección IPv6** cuadro, escriba la nueva dirección IPv6 interna de este servidor de acceso remoto; la actual IPv6 dirección será la dirección VIP del equilibrio de carga del clúster. En el **longitud del prefijo de subred** , escriba la longitud del prefijo de subred.  
  
8.  En el **resumen** página, haga clic en **confirmar**.  
  
9. En el **habilitar Equilibrio de carga** cuadro de diálogo, haga clic en **cerrar**.  
  
10. Habilitar Asistente de equilibrio de carga, haga clic en **cerrar**.  
  
    > [!NOTE]  
    > Si se está usando el equilibrio de carga externo, tenga en cuenta las direcciones IP virtuales y proporcione lo que en los equilibradores de carga externo.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Si decide usar NLB de Windows en los pasos de planificación, a continuación, ejecute lo siguiente:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Si se decide usar un equilibrador de carga externo en los pasos de planificación: a continuación, ejecute lo siguiente:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> Se recomienda no incluir cambios en la configuración del equilibrador de carga con los cambios realizados en cualquier otra configuración, si usas GPO de almacenamiento provisional. En primer lugar se deben aplicar los cambios realizados en la configuración del equilibrador de carga y, a continuación, se deben realizar otros cambios de configuración. Además, después de configurar el equilibrador de carga en un nuevo servidor de DirectAccess, espere algún tiempo para que los cambios IP se aplica y se replican entre los servidores DNS de la empresa, antes de cambiar otras opciones de DirectAccess relacionados al nuevo clúster.  
  
## <a name="BKMK_InstallIPHTTP"></a>3.3 instalar el certificado IP-HTTPS  
Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.  
  
### <a name="IPHTTPSCert"></a>Para instalar el certificado IP-HTTPS  
  
1.  En el servidor de acceso remoto configurado, haga clic en **iniciar**, tipo **mmc** y presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
3.  En el **agregar o quitar complementos** cuadro de diálogo, haga clic en **certificados**, haga clic en **agregar**, haga clic en **cuenta de equipo**, haga clic en  **Siguiente**, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**.  
  
4.  En el panel izquierdo de la consola, vaya a **\Personal\Certificates certificados (equipo Local)** . Haga clic en el certificado IP-HTTPS, seleccione **todas las tareas** y haga clic en **exportar**.  
  
5.  En la página **Éste es el Asistente para exportación de certificados**, haga clic en **Siguiente**.  
  
6.  En la página **Exportar la clave privada** , haga clic en **Exportar la clave privada**y, a continuación, haga clic en **Siguiente**.  
  
7.  En el **Exportar formato de archivo** página, haga clic en **intercambio de información Personal: PKCS #12 (. PFX)** y, a continuación, haga clic en **siguiente**.  
  
8.  En el **seguridad** página, seleccione el **contraseña** casilla de verificación, escriba una contraseña en el **contraseña** cuadro y confirme la contraseña y, a continuación, haga clic en **siguiente**.  
  
9. En el **archivo para exportar** página, escriba un nombre para el archivo de certificado y guárdelo en el escritorio y, a continuación, haga clic en **siguiente**.  
  
10. En la página **Finalización del Asistente para exportación de certificados**, haga clic en **Finalizar**.  
  
11. En el **Asistente para exportar certificados** cuadro de diálogo, haga clic en **Aceptar**.  
  
12. Copie el certificado en todos los servidores que desea que los miembros del clúster.  
  
13. En el nuevo servidor de DirectAccess, haga clic en **iniciar**, tipo **mmc** y presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
14. En la consola MMC, en el menú **Archivo**, haz clic en **Agregar o quitar complemento**.  
  
15. En el **agregar o quitar complementos** cuadro de diálogo, haga clic en **certificados**, haga clic en **agregar**, haga clic en **cuenta de equipo**, haga clic en  **Siguiente**, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**.  
  
16. En el panel izquierdo de la consola, vaya a **\Personal\Certificates certificados (equipo Local)** . Haga clic en el **certificados** nodo, seleccione **todas las tareas**y, a continuación, haga clic en **importación**.  
  
17. En la página **Éste es el Asistente para importación de certificados**, haga clic en **Siguiente**.  
  
18. En el **archivo para importar** página, haga clic en **examinar** para encontrar el certificado. Seleccione el certificado y, a continuación, haga clic en **siguiente**.  
  
19. En el **protección de clave privada** página, en el **contraseña** , escriba la contraseña y, a continuación, haga clic en **siguiente**.  
  
20. En la página **Almacén de certificados**, haga clic en **Siguiente**.  
  
21. En la página **Finalización del Asistente para importación de certificados**, haga clic en **Finalizar**.  
  
22. En el **Asistente para importar certificados** cuadro de diálogo, haga clic en **Aceptar**.  
  
23. Repita los pasos 13 a 22 en todos los servidores que desea que los miembros del clúster.  
  
## <a name="BKMK_NLS"></a>3.4 instalar el certificado de servidor de ubicación de red  
Para completar este procedimiento, se requiere como mínimo la pertenencia al grupo local **Administradores** o equivalente.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar un certificado para la ubicación de red  
  
1.  En el servidor de acceso remoto, haga clic en **iniciar**, tipo **mmc**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  Haga clic en **Archivo** y, a continuación, haga clic en **Agregar o quitar complementos**.  
  
3.  Haga clic en **certificados**, haga clic en **agregar**, haga clic en **cuenta de equipo**, haga clic en **siguiente**, haga clic en **ordenador**, haga clic en **finalizar**y, a continuación, haga clic en **Aceptar**.  
  
4.  En el árbol de consola del complemento Certificados, abra **Certificados (equipo local)\Personal\Certificados**.  
  
5.  Haga clic con el botón secundario en **Certificados**, elija **Todas las tareas** y, a continuación, haga clic en **Solicitar un nuevo certificado**.  
  
6.  Haga clic en **Siguiente** dos veces.  
  
7.  En el **solicitar certificados** página, haga clic en la plantilla de certificado de servidor Web y, a continuación, haga clic en **se necesita más información para inscribir este certificado**.  
  
    Si la plantilla de certificado de servidor Web no aparece, asegúrese de que la cuenta de equipo del servidor de acceso remoto con permisos de inscripción para la plantilla de certificado de servidor Web. Para obtener más información, consulte [configurar permisos en la plantilla de certificado de servidor Web](https://msdn.microsoft.com/library/ee649249.aspx).  
  
8.  En el **asunto** pestaña de la **las propiedades del certificado** cuadro de diálogo **nombre de sujeto**, para **tipo**, seleccione **comunes nombre**.  
  
9. En **valor**, escriba el nombre de dominio completo (FQDN) para el nombre de la intranet del sitio Web servidor de ubicación de red (por ejemplo, nls.corp.contoso.com) y, a continuación, haga clic en **agregar**.  
  
10. Haga clic en **Aceptar**, haga clic en **Inscribir** y, a continuación, haga clic en **Finalizar**.  
  
11. En el panel de detalles del complemento Certificados, compruebe que se inscribió un nuevo certificado con el FQDN con el valor **Propósitos planteados** de **Autenticación del servidor**.  
  
12. Haga clic con el botón secundario en el certificado y, a continuación, haga clic en **Propiedades**.  
  
13. En **Nombre descriptivo**, escriba **Certificado de ubicación de red** y, a continuación, haga clic en **Aceptar**.  
  
    > [!TIP]  
    > Los pasos 12 y 13 son opcionales, pero que sea más fácil para seleccionar el certificado para la ubicación de red al configurar el acceso remoto.  
  
14. Repita este procedimiento en todos los servidores que desea que los miembros del clúster.  
  
## <a name="BKMK_Add"></a>3.5 agregar servidores al clúster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Para agregar servidores al clúster  
  
1.  En el servidor de DirectAccess configurado, haga clic en **iniciar**y, a continuación, haga clic en **administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. En el **tareas** panel, en **clúster con equilibrio de carga**, haga clic en **agregar o quitar servidores**.  
  
3.  En el **agregar o quitar servidores** cuadro de diálogo, haga clic en **Agregar servidor**.  
  
4.  En el **agregar un servidor** cuadro de diálogo el **Seleccionar servidor** página, escriba el nombre del servidor de acceso remoto adicional y, a continuación, haga clic en **siguiente**.  
  
5.  En el **adaptadores de red** , realice una de los siguientes:  
  
    -   Si va a implementar una topología con dos adaptadores de red en **adaptador externo**, seleccione el adaptador que está conectado a la red externa. En **adaptador interno**, seleccione el adaptador que está conectado a la red interna.  
  
    -   Si va a implementar una topología con un adaptador de red, en **adaptador de red**, seleccione el adaptador que está conectado a la red interna.  
  
6.  En el **adaptadores de red** página **seleccione el certificado utilizado para autenticar conexiones IP-HTTPS**, haga clic en **examinar** para buscar y seleccionar el certificado IP-HTTPS, y, a continuación, haga clic en **siguiente**.  
  
7.  En el **Network Location Server** página, haga clic en **examinar** para seleccionar el certificado para el sitio Web servidor de ubicación de red que se ejecutan en el servidor de acceso remoto y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > El **Network Location Server** página aparece sólo cuando se ejecuta el sitio Web servidor de ubicación de red en el servidor de acceso remoto.  
  
    > [!NOTE]  
    > Si VPN también se configuraron en el servidor de acceso remoto, a continuación, pedirá para agregar la información de grupo de direcciones de IP de VPN en este momento.  
  
8.  En el **resumen** página, haga clic en **agregar**.  
  
9. En la página **Finalización**, haga clic en **Cerrar**.  
  
10. Repita este procedimiento para todos los servidores de acceso remoto a agregarse al clúster.  
  
11. En el **agregar o quitar servidores** cuadro de diálogo, haga clic en **confirmar**.  
  
12. En el **agregar y quitar servidores** cuadro de diálogo, haga clic en **cerrar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Si no se habilitó la VPN en un clúster con equilibrio de carga, no debe proporcionar los intervalos de direcciones VPN cuando se agrega un nuevo servidor al clúster mediante los cmdlets de Windows PowerShell. Si lo ha hecho por error, quite el servidor del clúster y agregarlo de nuevo al clúster sin especificar los intervalos de direcciones VPN.  
  
## <a name="BKMK_remove"></a>3.6 quitar un servidor del clúster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Para quitar un servidor del clúster  
  
1.  En el servidor de acceso remoto configurado, haga clic en **iniciar**y, a continuación, haga clic en **administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. En el **tareas** panel, en **clúster con equilibrio de carga**, haga clic en **agregar o quitar servidores**.  
  
3.  En el **agregar o quitar servidores** cuadro de diálogo, seleccione el servidor de acceso remoto que desea quitar y, a continuación, haga clic en **quitar servidor**.  
  
4.  En el **quitar servidor advertencia** cuadro de diálogo, asegúrese de que elige el servidor adecuado y, a continuación, haga clic en **Aceptar**.  
  
5.  Repita este procedimiento para todos los servidores de acceso remoto debe quitarse del clúster.  
  
6.  En el **agregar o quitar servidores** cuadro de diálogo, haga clic en **confirmar**.  
  
7.  En el **agregar y quitar servidores** cuadro de diálogo, haga clic en **cerrar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="BKBK_disable"></a>3.7 deshabilitar Equilibrio de carga  
[Realice este paso mediante Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Para deshabilitar el equilibrio de carga  
  
1.  En el servidor de DirectAccess configurado, haga clic en **iniciar**y, a continuación, haga clic en **administración de acceso remoto**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, haga clic en **Configuración**. En el **tareas** panel, en **clúster con equilibrio de carga**, haga clic en **deshabilitar Equilibrio de carga**.  
  
3.  En el **deshabilitar Equilibrio de carga** cuadro de diálogo, haga clic en **Aceptar**.  
  
4.  En el **deshabilitar Equilibrio de carga** cuadro de diálogo, haga clic en **cerrar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
Deshabilitar Equilibrio de carga se quitará la configuración de acceso remoto y la configuración de NLB (si está configurado) de todos los servidores excepto el servidor desde el que se está ejecutando. En este servidor de acceso remoto, se quitará la configuración de NLB (si se configuró) pero seguirá siendo la configuración de acceso remoto.  
  
Al hacer clic en **quitar opciones de configuración** quitará el acceso remoto y NLB (si está configurado) de todos los servidores en la implementación.  
  
> [!NOTE]  
> -   Si se desinstala el acceso remoto cuando se implementa el equilibrio de carga, se mantienen todos los servidores con el DIP. Se quitan las direcciones VIP. Esto hace que todas las rutas de la red corporativa que tienen como destino las direcciones VIP para producir un error. Esto afecta también a las entradas DNS que se resolvieron para las VIP, por ejemplo, el nombre de sujeto del certificado del servidor de ubicación de red. Para evitar este problema, deshabilite el equilibrio de carga, lo que deja las VIP en el último servidor de acceso remoto y, a continuación, desinstalar acceso remoto.  
> -   Después de usar el **conjunto RemoteAccessLoadBalancer** cmdlet para deshabilitar el equilibrio de carga, espere dos minutos antes de ejecutar cualquier otro cmdlet. También debe realizarse en los scripts que se ejecutan otro cmdlet después de la **conjunto RemoteAccessLoadBalancer-deshabilitar** cmdlet.  
> -   Deshabilitar los cambios de equilibrio de carga en la dirección IP virtual del clúster en una dirección IP dedicada. Como resultado, cualquier operación que consulta el nombre del servidor se producirá un error hasta que expire la entrada almacenada en caché de DNS en el servidor. Asegúrese de que no ejecute los cmdlets de PowerShell de acceso remoto después de deshabilitar el equilibrio de carga hasta que la memoria caché en el servidor ha expirado. Este problema es más habitual si se intenta deshabilitar el equilibrio de carga en una máquina desde otra máquina que está en otro dominio. Esto también ocurre si deshabilita el equilibrio de carga de la consola de administración de acceso remoto y puede impedir que la configuración de la carga. La carga de la configuración después de la memoria caché ha expirado o se ha vaciado.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 4: Comprobando el clúster](Step-4-Verify-the-Cluster.md)  
  


