---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Deploy Access-Denied Assistance (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: afc05f395753e5c5614e92d109d71e05980d5d92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407179"
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Deploy Access-Denied Assistance (Demonstration Steps)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica cómo configurar la asistencia para acceso denegado y cómo comprobar que funciona correctamente.  
  
**En este documento**  
  
-   [Paso 1: configurar la asistencia para acceso denegado](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Paso 2: configuración de las notificaciones de correo electrónico](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Paso 3: comprobar que la asistencia para acceso denegado esté configurada correctamente](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Paso 1: configurar la asistencia para acceso denegado  
Puedes configurar la asistencia para acceso denegado en un dominio mediante la Directiva de grupo, o bien puedes configurar la asistencia individualmente en cada servidor de archivos mediante la consola del Administrador de recursos del servidor de archivos. Asimismo, puedes cambiar el mensaje de acceso denegado para una carpeta compartida específica en un servidor de archivos.  
  
Puedes configurar la asistencia para acceso denegado para el domino mediante la Directiva de grupo de la siguiente manera:  
  
[Realice este paso con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Cómo configurar la asistencia para acceso denegado mediante la Directiva de grupo  
  
1.  Abra Administración de directivas de grupo. En Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Administración de directivas de grupo**.  
  
2.  Haz clic con el botón secundario en la Directiva de grupo adecuada y, después, haz clic en **Editar**.  
  
3.  Haz clic en **Configuración del equipo**, en **Directivas**, en **Plantillas administrativas**, en **Sistema** y en **Asistencia para acceso denegado**.  
  
4.  Haz clic con el botón secundario en **Personalizar el mensaje de error de acceso denegado**y, después, haz clic en **Editar**.  
  
5.  Seleccione la opción **Habilitado**.  
  
6.  Configure las siguientes opciones:  
  
    1.  En el cuadro **Mostrar el siguiente mensaje a usuarios a los que se deniegue el acceso** , escribe el mensaje que verán los usuarios cuando no se les permita el acceso a un archivo o carpeta.  
  
        Puedes agregar macros al mensaje para insertar texto personalizado. Las macros incluyen:  
  
        -   **[Ruta de acceso del archivo original]** La ruta de acceso del archivo original a la que accedió el usuario.  
  
        -   **[Carpeta de la ruta de acceso del archivo original]** La carpeta principal de la ruta de acceso del archivo original a la que accedió el usuario.  
  
        -   **[Correo electrónico de admin.]** La lista de destinatarios de correos electrónicos del administrador.  
  
        -   **[Correo electrónico del propietario de los datos]** La lista de destinatarios de correos electrónicos del propietario de los datos.  
  
    2.  Selecciona el cuadro de texto **Permitir que los usuarios soliciten asistencia** .  
  
    3.  Deja los demás ajustes de la configuración predeterminada.  
  
![guías de soluciones](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName AllowEmailRequests -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName GenerateLog -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeDeviceClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeUserClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutAdminOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutDataOwnerOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName ErrorMessage -Type MultiString -value "Type the text that the user will see in the error message dialog box."  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName Enabled -Type DWORD -value 1 
  
```  
  
También se puede configurar la asistencia para acceso denegado individualmente en cada servidor de archivos mediante la consola del Administrador de recursos del servidor de archivos.  
  
[Realice este paso con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Cómo configurar la asistencia para acceso denegado mediante el Administrador de recursos del servidor de archivos  
  
1.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
2.  Haz clic con el botón secundario en **Administrador de recursos del servidor de archivos (local)** y, después, haz clic en **Configurar opciones**.  
  
3.  Haz clic en la pestaña **Asistencia para acceso denegado** .  
  
4.  Selecciona el cuadro de texto **Habilitar asistencia para acceso denegado** .  
  
5.  En el cuadro **Mostrar el siguiente mensaje a usuarios a los que se deniegue el acceso a una carpeta o archivo**, escribe el mensaje que verán los usuarios cuando no se les permita el acceso a un archivo o carpeta.  
  
    Puedes agregar macros al mensaje para insertar texto personalizado. Las macros incluyen:  
  
    -   **[Ruta de acceso del archivo original]** La ruta de acceso del archivo original a la que accedió el usuario.  
  
    -   **[Carpeta de la ruta de acceso del archivo original]** La carpeta principal de la ruta de acceso del archivo original a la que accedió el usuario.  
  
    -   **[Correo electrónico de admin.]** La lista de destinatarios de correos electrónicos del administrador.  
  
    -   **[Correo electrónico del propietario de los datos]** La lista de destinatarios de correos electrónicos del propietario de los datos.  
  
6.  Haz clic en **Configurar solicitudes de correo electrónico**, selecciona el cuadro de diálogo **Permitir que los usuarios soliciten asistencia** y haz clic en **Aceptar**.  
  
7.  Haz clic en **Vista previa** si deseas comprobar cómo verá el usuario el mensaje de error.  
  
8.  Haz clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Después de configurar la asistencia para acceso denegado, debes habilitarla para todos los tipos de archivos mediante la Directiva de grupo.  
  
[Realice este paso con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Cómo configurar la asistencia para acceso denegado para todos los tipos de archivos mediante la Directiva de grupo  
  
1.  Abra Administración de directivas de grupo. En Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Administración de directivas de grupo**.  
  
2.  Haz clic con el botón secundario en la Directiva de grupo adecuada y, después, haz clic en **Editar**.  
  
3.  Haz clic en **Configuración del equipo**, en **Directivas**, en **Plantillas administrativas**, en **Sistema** y en **Asistencia para acceso denegado**.  
  
4.  Haz clic con el botón secundario en **Habilitar la asistencia para acceso denegado en cliente para todos los tipos de archivos**y, después, haz clic en **Editar**.  
  
5.  Haga clic en **Habilitada**y, a continuación, haga clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
También puede especificarse un mensaje de acceso denegado diferente para cada carpeta compartida en un servidor de archivos mediante la consola del Administrador de recursos del servidor de archivos.  
  
[Realice este paso con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Cómo especificar un mensaje de acceso denegado diferente para cada carpeta compartida mediante el Administrador de recursos del servidor de archivos  
  
1.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
2.  Expande **Administrador de recursos del servidor de archivos (local)** y haz clic en **Administración de clasificaciones**.  
  
3.  Haz clic con el botón secundario en **Propiedades de clasificación** y, después, haz clic en **Establecer propiedades de administración de carpetas**.  
  
4.  En el cuadro **Propiedad**, haz clic en **Mensaje de asistencia para acceso denegado** y, después, haz clic en **Agregar**.  
  
5.  Haz clic en **Examinar** y elige la carpeta que debe tener el mensaje personalizado para el acceso denegado.  
  
6.  En el cuadro **Valor**, escribe el mensaje que verán los usuarios cuando no puedan acceder a un recurso dentro de esa carpeta.  
  
    Puedes agregar macros al mensaje para insertar texto personalizado. Las macros incluyen:  
  
    -   **[Ruta de acceso del archivo original]** La ruta de acceso del archivo original a la que accedió el usuario.  
  
    -   **[Carpeta de la ruta de acceso del archivo original]** La carpeta principal de la ruta de acceso del archivo original a la que accedió el usuario.  
  
    -   **[Correo electrónico de admin.]** La lista de destinatarios de correos electrónicos del administrador.  
  
    -   **[Correo electrónico del propietario de los datos]** La lista de destinatarios de correos electrónicos del propietario de los datos.  
  
7.  Haga clic en **Aceptar** y, a continuación, en **Cerrar**.  
  
![guías de soluciones](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Paso 2: configuración de las notificaciones de correo electrónico  
Debes configurar las notificaciones de correo electrónico en cada servidor de archivos que enviará los mensajes de asistencia para acceso denegado.  
  
[Realice este paso con Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
2.  Haz clic con el botón secundario en **Administrador de recursos del servidor de archivos (local)** y, después, haz clic en **Configurar opciones**.  
  
3.  Haz clic en la pestaña **Notificaciones de correo electrónico**.  
  
4.  Configura los siguientes ajustes:  
  
    -   En el cuadro **Nombre del servidor SMTP o dirección IP** , escribe el nombre de la dirección IP del servidor SMTP de tu organización.  
  
    -   En los cuadros **destinatarios de administrador predeterminados** y **dirección de correo electrónico predeterminada "de"** , escriba la dirección de correo electrónico del administrador del servidor de archivos.  
  
5.  Haz clic en **Enviar correo electrónico de prueba** para asegurarte de que las notificaciones de correo electrónico están configuradas correctamente.  
  
6.  Haz clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Paso 3: comprobar que la asistencia para acceso denegado esté configurada correctamente  
Puede comprobar que la asistencia para acceso denegado esté configurada correctamente si un usuario que ejecuta Windows 8 intenta tener acceso a un recurso compartido o a un archivo de ese recurso compartido al que no tienen acceso. Cuando aparezca el mensaje de acceso denegado, el usuario debería ver el botón **Solicitar asistencia** . Después de hacer clic en el botón Solicitar asistencia, el usuario podrá especificar la razón del acceso y enviar un correo electrónico al propietario de la carpeta o al administrador del servidor de archivos. El propietario de la carpeta o el administrador del servidor de archivos podrán informarte de que han recibido el correo electrónico y de que contiene los detalles adecuados.  
  
> [!IMPORTANT]  
> Si desea comprobar la asistencia para acceso denegado con un usuario que ejecuta Windows Server 2012, debe instalar la experiencia de escritorio antes de conectarse al recurso compartido de archivos.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Escenario: asistencia para acceso denegado](Scenario--Access-Denied-Assistance.md)  
  
-   [Planear la asistencia para acceso denegado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Access Control dinámico: información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

