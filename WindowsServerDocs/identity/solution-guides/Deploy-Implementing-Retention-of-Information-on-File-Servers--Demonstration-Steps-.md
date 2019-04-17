---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: "Implementar la implementación de retención de la información en los servidores de archivos (pasos de demostración)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Implementar la implementación de retención de la información en los servidores de archivos (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes establecer períodos de retención de carpetas y colocar archivos legalmente mediante la infraestructura de clasificación de archivo y el Administrador de recursos del servidor de archivos.  
  
**En este documento**  
  
-   [Requisitos previos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Paso 1: Crear recurso de definiciones de propiedad](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Paso 2: Configurar notificaciones](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [Paso 3: Crear una tarea de administración de archivos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Paso 4: Clasificar un archivo manualmente](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de muestra que puedes usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta [uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Requisitos previos  
Los pasos de este tema suponen que tiene un servidor SMTP configurado para notificaciones de expiración de archivo.  
  
## <a name="BKMK_Step1"></a>Paso 1: Crear recurso de definiciones de propiedad  
En este paso, permitimos que las propiedades del recurso período de retención y detectabilidad para que la infraestructura de clasificación de archivo pueda usar estas propiedades de recurso para etiquetar los archivos que se analizan en una carpeta compartida de red.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Crear recurso de definiciones de propiedad  
  
1.  En el controlador de dominio, inicie sesión en el servidor como miembro del grupo Administradores de dominio.  
  
2.  Abre el centro de administración de Active Directory. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **centro de administración de Active Directory**.  
  
3.  Expande **Control de acceso dinámico**y, a continuación, haz clic en **propiedades de recurso**.  
  
4.  Haz clic en **período de retención**y, a continuación, haz clic en **habilitar**.  
  
5.  Haz clic en **detectabilidad**y, a continuación, haz clic en **habilitar**.  
  
![guías de solución](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Paso 2: Configurar notificaciones  
En este paso, usamos la consola del Administrador de recursos del servidor de archivos para configurar el servidor SMTP, la dirección de correo electrónico de administrador predeterminada y la dirección de correo electrónico predeterminada que los informes se envían desde.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Para configurar las notificaciones  
  
1.  Iniciar sesión como miembro del grupo de seguridad de los administradores el servidor de archivos.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **actualización FsrmClassificationPropertyDefinition**, y, a continuación, presione ENTRAR. Esto sincronizará las definiciones de propiedad que se crean en el controlador de dominio al servidor de archivos.  
  
3.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
4.  Haz clic en **Administrador de recursos del servidor de archivos (locales)**y, a continuación, haz clic en **configurar opciones**.  
  
5.  En la **notificaciones por correo electrónico** pestaña, configura lo siguiente:  
  
    -   En la **SMTP servidor nombre o dirección IP**, escriba el nombre del servidor SMTP de la red.  
  
    -   En la **Administradores receptores predeterminados** cuadro, escribe la dirección de correo electrónico del administrador que debe acceder a la notificación.  
  
    -   En la **predeterminado "Desde" dirección de correo electrónico**, escriba la dirección de correo electrónico que debe usarse para enviar las notificaciones.  
  
6.  Haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Paso 3: Crear una tarea de administración de archivos  
En este paso, usamos la consola del Administrador de recursos del servidor de archivos para crear una tarea de administración de archivos que se ejecutan en el último día del mes y expiran ningún archivo con los siguientes criterios:  
  
-   El archivo no está clasificado como está en espera legal.  
  
-   El archivo se clasifica como si tuvieran un período de retención a largo plazo.  
  
-   El archivo no se modificó en los últimos 10 años.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Para crear una tarea de administración de archivos  
  
1.  Iniciar sesión como miembro del grupo de seguridad de los administradores el servidor de archivos.  
  
2.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
3.  Haz clic en **tareas de administración de archivos**y, a continuación, haz clic en **crear la tarea de administración de archivos**.  
  
4.  En la **General** pestaña la **nombre de la tarea**, escriba un nombre para la tarea de administración de archivos, como tarea de retención.  
  
5.  En la **ámbito**, haga clic **agregar**y elige las carpetas que deberían estar incluidas en esta regla, como documentos D:\Finance.  
  
6.  En la **acción** pestaña la **tipo** cuadro, haz clic en **expiración del archivo**. En la **directorio de expiración**, escriba una ruta de acceso a una carpeta en el servidor de archivos local que se moverán los archivos que ha expirado. Esta carpeta debe tener una lista de control de acceso que concede acceso de solo los administradores de servidor de archivos.  
  
7.  En la **notificación**, haga clic **agregar**.  
  
    -   Selecciona el **enviar correo electrónico a los administradores de los siguientes** casilla de verificación.  
  
    -   Selecciona el **envía un correo electrónico a los usuarios con los archivos afectados** y, a continuación, haz clic en **Aceptar**.  
  
8.  En la **condición**, haga clic **agregar**y agrega las siguientes propiedades:  
  
    -   En la **propiedad** la lista, haz clic en **detectabilidad**. En la **operador** la lista, haz clic en **no es igual**. En la **valor** la lista, haz clic en **mantenga**.  
  
    -   En la **propiedad** la lista, haz clic en **período de retención**. En la **operador** la lista, haz clic en **igual**. En la **valor** la lista, haz clic en **a largo plazo**.  
  
9. En la **condición**, selecciona el **días desde la última modificación del archivo** y, a continuación, Establece el valor en **3650**.  
  
10. En la **programación** pestaña, haz clic en el **mensual** opción y, a continuación, selecciona el **última** casilla de verificación.  
  
11. Haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
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
En este paso, clasificamos manualmente un archivo para estar en espera legal. La carpeta primaria de este archivo se clasifica con un período de retención a largo plazo.  
  
#### <a name="to-manually-classify-a-file"></a>Para clasificar manualmente un archivo  
  
1.  Iniciar sesión como miembro del grupo de seguridad de los administradores el servidor de archivos.  
  
2.  Navega a la carpeta que se ha configurado en el ámbito de la tarea de administración de archivo creado en el paso 3.  
  
3.  Haz clic en la carpeta y, a continuación, haz clic en **propiedades**.  
  
4.  En la **clasificación**, haga clic **período de retención**, haz clic en **a largo plazo**y, a continuación, haz clic en **Aceptar**.  
  
5.  Haz clic en un archivo dentro de esa carpeta y, a continuación, haz clic en **propiedades**.  
  
6.  En la **clasificación**, haga clic **detectabilidad**, haz clic en **mantenga**, haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
7.  En el servidor de archivos, ejecuta la tarea de administración de archivos mediante la consola del Administrador de recursos del servidor de archivos. Una vez completada la tarea de administración de archivos, comprueba la carpeta y asegurarse de que el archivo no se movió al directorio de expiración.  
  
8.  Haz clic en el mismo archivo dentro de esa carpeta y, a continuación, haz clic en **propiedades**.  
  
9. En la **clasificación**, haga clic **detectabilidad**, haz clic en **no es aplicable**, haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
10. En el servidor de archivos, vuelve a ejecutar la tarea de administración de archivos mediante la consola del Administrador de recursos del servidor de archivos. Una vez completada la tarea de administración de archivos, comprueba la carpeta y asegurarse de que ese archivo se ha movido al directorio de expiración.  
  
## <a name="BKMK_Links"></a>Consulta también  
  
-   [Escenario: Implementar la retención de información en servidores de archivos](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Planear la retención de información en los servidores de archivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

