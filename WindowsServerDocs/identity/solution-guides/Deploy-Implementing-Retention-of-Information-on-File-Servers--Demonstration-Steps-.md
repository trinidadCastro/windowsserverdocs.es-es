---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Deploy Implementing Retention of Information on File Servers (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870806"
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Deploy Implementing Retention of Information on File Servers (Demonstration Steps)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes establecer períodos de retención para carpetas e imponer una retención legal a un archivo mediante la Infraestructura de clasificación de archivos y el Administrador de recursos del servidor de archivos.  
  
**En este documento**  
  
-   [Requisitos previos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Paso 1: Crear definiciones de propiedad de recurso](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Paso 2: Configurar notificaciones](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [Paso 3: Crear una tarea de administración de archivos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Paso 4: Clasificar un archivo manualmente](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Requisitos previos  
Los pasos de este tema dan por supuesto que tienes un servidor SMTP configurado para notificaciones de expiración de archivos.  
  
## <a name="BKMK_Step1"></a>Paso 1: crear definiciones de propiedad de recurso  
En este paso, se habilitan las propiedades de recurso Período de retención y Detectabilidad, de modo que la Infraestructura de clasificación de archivos utilice estas propiedades de recurso para etiquetar los archivos que se analizan en una carpeta compartida en red.  
  
[Realice este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Para crear definiciones de propiedad de recurso  
  
1.  En el controlador de dominio, inicia sesión en el servidor como miembro del grupo de seguridad Admins. del dominio.  
  
2.  Abre el Centro de administración de Active Directory. En Administrador del servidor, haz clic en **Herramientas** y, después, en **Centro de administración de Active Directory**.  
  
3.  Expande **Control de acceso dinámico** y, luego, haz clic en **Propiedades de recurso**.  
  
4.  Haz clic con el botón secundario en **Período de retención** y, después, haz clic en **Habilitar**.  
  
5.  Haz clic con el botón secundario en **Detectabilidad**y, después, haz clic en **Habilitar**.  
  
![guías de soluciones](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Paso 2: Configurar notificaciones  
En este paso, se utiliza la consola del Administrador de recursos del servidor de archivos para configurar el servidor SMTP, la dirección de correo electrónico del administrador predeterminado y la dirección de correo electrónico predeterminada desde la que se envían los informes.  
  
[Realice este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Cómo configurar notificaciones  
  
1.  Inicia sesión en el servidor de archivos como miembro del grupo de seguridad Administradores.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **Update-FsrmClassificationPropertyDefinition** y presiona ENTRAR. Esto hará que las definiciones de propiedades creadas en el controlador de dominio se sincronicen en el servidor de archivos.  
  
3.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
4.  Haz clic con el botón secundario en **Administrador de recursos del servidor de archivos (local)** y, después, haz clic en **Configurar opciones**.  
  
5.  En la pestaña **Notificaciones de correo electrónico**, configura lo siguiente:  
  
    -   En el cuadro **Nombre del servidor SMTP o dirección IP** , escribe el nombre del servidor SMTP de la red.  
  
    -   En el cuadro **Administradores receptores predeterminados** , escribe la dirección de correo electrónico del administrador que debe recibir la notificación.  
  
    -   En el **predeterminado "De" dirección de correo electrónico** , escriba la dirección de correo electrónico que debe usarse para enviar las notificaciones.  
  
6.  Haga clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Paso 3: Crear una tarea de administración de archivos  
En este paso, se utiliza la consola del Administrador de recursos del servidor de archivos para crear una tarea de administración de archivos que se ejecutará el último día del mes y que hará que expiren los archivos que cumplan los siguientes criterios:  
  
-   El archivo no está clasificado con el estado de retención legal.  
  
-   El archivo está clasificado con un período de retención a largo plazo.  
  
-   El archivo no se ha modificado en los últimos 10 años.  
  
[Realice este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Para crear una tarea de administración de archivos  
  
1.  Inicia sesión en el servidor de archivos como miembro del grupo de seguridad Administradores.  
  
2.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
3.  Haga clic con el botón secundario en **Tarea de administración de archivos**y, a continuación, en **Crear tarea de administración de archivos**.  
  
4.  En la pestaña **General**, en el cuadro **Nombre de tarea**, escribe un nombre para la tarea de administración de archivos, como Tarea de retención.  
  
5.  En la pestaña **Ámbito**, haz clic en **Agregar** y elige las carpetas que deben incluirse en esta regla (por ejemplo, D:\Finance Documents).  
  
6.  En la pestaña **Acción**, en el cuadro **Tipo**, haz clic en **Expiración de archivos**. En el cuadro **Directorio de expiración** , escribe una ruta de acceso a una carpeta del servidor de archivos local adonde se moverán los archivos expirados. Esta carpeta debe tener una lista de control de acceso que solo conceda acceso a los administradores del servidor de archivos.  
  
7.  En la pestaña **Notificación**, haz clic en **Agregar**.  
  
    -   Activa la casilla **Enviar correo electrónico a los siguientes administradores**.  
  
    -   Activa la casilla **Enviar un mensaje de correo electrónico a los usuarios con archivos afectados** y haz clic en **Aceptar**.  
  
8.  En la pestaña **Condición**, haz clic en **Agregar** y agrega las siguientes propiedades:  
  
    -   En la lista **Propiedad** , haz clic en **Detectabilidad**. En la lista **Operador** , haz clic en **Distinto de**. En la lista **Valor**, haz clic en **Retener**.  
  
    -   En la lista **Propiedad**, haz clic en **Período de retención**. En la lista **Operador**, haz clic en **Igual**. En la lista **Valor** , haz clic en **Largo plazo**.  
  
9. En la pestaña **Condición** , activa la casilla **Días desde la última modificación en el archivo** y establece el valor en **3650**.  
  
10. En la pestaña **Programar** , haz clic en la opción **Mensualmente** y activa la casilla **Último** .  
  
11. Haga clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
$fmjexpiration = New-FSRMFmjAction -Type 'Expiration' -ExpirationFolder folder  
$fmjNotificationAction = New-FsrmFmjNotificationAction -Type Email -MailTo "[FileOwner],[AdminEmail]"  
$fmjNotification = New-FsrmFMJNotification -Days 10 -Action @($fmjNotificationAction)  
$fmjCondition1 = New-FSRMFmjCondition -Property 'Discoverability_MS' -Condition 'NotEqual' -Value "Hold" 
$fmjCondition2 = New-FSRMFmjCondition -Property 'RetentionPeriod_MS' -Condition 'Equal' -Value "Long-term"  
$fmjCondition3 = New-FSRMFmjCondition -Property 'File.DateLastAccessed' -Condition 'Equal' -Value 3650  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Monthly @(-1)    
$fmj1=New-FSRMFileManagementJob -Name "Retention Task" -Namespace @('D:\Finance Documents') -Action $fmjexpiration -Schedule $schedule -Notification @($fmjNotification) -Condition @( $fmjCondition1, $fmjCondition2, $fmjCondition3)  
```  
  
## <a name="BKMK_Step4"></a>Paso 4: Clasificar un archivo manualmente  
En este paso, se clasifica manualmente un archivo con el estado de retención legal. La carpeta principal de este archivo se clasificará con un período de retención a largo plazo.  
  
#### <a name="to-manually-classify-a-file"></a>Cómo clasificar manualmente un archivo  
  
1.  Inicia sesión en el servidor de archivos como miembro del grupo de seguridad Administradores.  
  
2.  Navega a la carpeta que se configuró en el ámbito de la tarea de administración de archivos creada en el Paso 3.  
  
3.  Haga clic con el botón secundario en la carpeta y, a continuación, haga clic en **Propiedades**.  
  
4.  En la pestaña **Clasificación** , haz clic en **Período de retención**, en **Largo plazo**y en **Aceptar**.  
  
5.  Haz clic con el botón secundario en un archivo de esa carpeta y, después, haz clic en **Propiedades**.  
  
6.  En la pestaña **Clasificación** , haz clic en **Detectabilidad**, en **Retener**, en **Aplicar**y en **Aceptar**.  
  
7.  En el servidor de archivos, ejecuta la tarea de administración de archivos mediante la consola del Administrador de recursos del servidor de archivos. Cuando la tarea de administración de archivos se haya completado, comprueba la carpeta y asegúrate de que el archivo no se ha movido al directorio de expiración.  
  
8.  Haz clic con el botón secundario en el mismo archivo de esa carpeta y, después, haz clic en **Propiedades**.  
  
9. En la pestaña **Clasificación** , haz clic en **Detectabilidad**, en **No aplicable**, en **Aplicar**y en **Aceptar**.  
  
10. En el servidor de archivos, vuelve a ejecutar la tarea de administración de archivos mediante la consola del Administrador de recursos del servidor de archivos. Cuando la tarea de administración de archivos se haya completado, comprueba la carpeta y asegúrate de que el archivo se ha movido al directorio de expiración.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Escenario: Implementar la retención de información en servidores de archivos](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Planificar la retención de información en servidores de archivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

