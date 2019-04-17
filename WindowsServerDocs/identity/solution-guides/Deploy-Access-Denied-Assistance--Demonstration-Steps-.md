---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: "Implementar la asistencia de acceso denegado (pasos de demostración)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5201441ba884fe4658b917919e60c7d20530341b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Implementar la asistencia de acceso denegado (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica cómo configurar acceso denegado asistencia y comprobar que funciona correctamente.  
  
**En este documento**  
  
-   [Paso 1: Configurar acceso denegado asistencia](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Paso 2: Configurar la configuración de notificaciones de correo electrónico](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Paso 3: Comprobar que asistencia acceso denegado está correctamente configurado](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de muestra que puedes usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta [uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Paso 1: Configurar acceso denegado asistencia  
Puede configurar acceso denegado asistencia dentro de un dominio mediante la directiva de grupo, o puedes configurar la asistencia individualmente en cada servidor de archivos mediante la consola del Administrador de recursos del servidor de archivos. También puedes cambiar el mensaje de acceso denegado para una carpeta compartida específica en un servidor de archivos.  
  
Puedes configurar acceso denegado asistencia para el dominio mediante la directiva de grupo como sigue:  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Para configurar acceso denegado asistencia mediante la directiva de grupo  
  
1.  Administración de directivas de grupo abierto. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Group Policy Management**.  
  
2.  Haz clic en la directiva de grupo correspondiente y, a continuación, haz clic en **editar**.  
  
3.  Haz clic en **configuración del equipo**, haz clic en **directivas**, haz clic en **plantillas administrativas**, haz clic en **sistema**y, a continuación, haz clic en **Access-Denied asistencia**.  
  
4.  Haz clic en **Personalizar mensaje para los errores de acceso denegado**y, a continuación, haz clic en **editar**.  
  
5.  Selecciona el **habilitado** opción.  
  
6.  Configurar las opciones siguientes:  
  
    1.  En la **mostrará el mensaje siguiente a los usuarios que se deniegan el acceso** cuadro, escribe un mensaje que verán los usuarios cuando se les deniega el acceso a un archivo o carpeta.  
  
        Puedes agregar macros hasta el mensaje que se insertará texto personalizado. Las macros incluyen:  
  
        -   **[Ruta del archivo original] **La ruta de acceso del archivo original que se tuvo acceso por parte del usuario.  
  
        -   **[Carpeta de ruta de acceso de archivo original] **La carpeta primaria de la ruta de acceso del archivo original que se tuvo acceso por parte del usuario.  
  
        -   **[Correo electrónico de administrador] **La lista de destinatarios del correo electrónico de administrador.  
  
        -   **[Datos propietario, correo electrónico] **La lista de destinatarios del correo electrónico de propietario de datos.  
  
    2.  Selecciona el **permitir que los usuarios para solicitar asistencia** casilla de verificación.  
  
    3.  Deja los valores predeterminados restantes.  
  
![guías de solución](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
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
  
Como alternativa, puedes configurar acceso denegado asistencia individualmente en cada servidor de archivos mediante la consola del Administrador de recursos del servidor de archivos.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Para configurar acceso denegado asistencia mediante el Administrador de recursos del servidor de archivos  
  
1.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
2.  Haz clic en **Administrador de recursos del servidor de archivos (locales)**y, a continuación, haz clic en **configurar opciones**.  
  
3.  Haz clic en el **Access-Denied asistencia** pestaña.  
  
4.  Selecciona el **habilitar acceso denegado asistencia** casilla de verificación.  
  
5.  En la **mostrará el mensaje siguiente a los usuarios que se deniegan el acceso a un archivo o carpeta** cuadro, escribe un mensaje que verán los usuarios cuando se les deniega el acceso a un archivo o carpeta.  
  
    Puedes agregar macros hasta el mensaje que se insertará texto personalizado. Las macros incluyen:  
  
    -   **[Ruta del archivo original] **La ruta de acceso del archivo original que se tuvo acceso por parte del usuario.  
  
    -   **[Carpeta de ruta de acceso de archivo original] **La carpeta primaria de la ruta de acceso del archivo original que se tuvo acceso por parte del usuario.  
  
    -   **[Correo electrónico de administrador] **La lista de destinatarios del correo electrónico de administrador.  
  
    -   **[Datos propietario, correo electrónico] **La lista de destinatarios del correo electrónico de propietario de datos.  
  
6.  Haz clic en **solicitudes de correo electrónico configurar**, selecciona el **permitir que los usuarios para solicitar asistencia** y, a continuación, haz clic en **Aceptar**.  
  
7.  Haz clic en **Preview** si quieres ver el aspecto que tendrá el mensaje de error al usuario.  
  
8.  Haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Después de configurar la Ayuda de acceso denegado, debes habilitar para todos los tipos de archivo mediante la directiva de grupo.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Configurar acceso denegado asistencia para todos los tipos de archivo mediante la directiva de grupo  
  
1.  Administración de directivas de grupo abierto. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Group Policy Management**.  
  
2.  Haz clic en la directiva de grupo correspondiente y, a continuación, haz clic en **editar**.  
  
3.  Haz clic en **configuración del equipo**, haz clic en **directivas**, haz clic en **plantillas administrativas**, haz clic en **sistema**y, a continuación, haz clic en **Access-Denied asistencia**.  
  
4.  Haz clic en **habilitar asistencia deniega el acceso de cliente para todos los tipos de archivo**y, a continuación, haz clic en **editar**.  
  
5.  Haz clic en **habilitado**y, a continuación, haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
También puedes especificar un mensaje de acceso denegado independiente para cada carpeta compartida en un servidor de archivos mediante la consola del Administrador de recursos del servidor de archivos.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Para especificar un mensaje de acceso denegado independiente para una carpeta compartida mediante el Administrador de recursos del servidor de archivos  
  
1.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
2.  Expande **Administrador de recursos del servidor de archivos (locales)**y, a continuación, haz clic en **administración de clasificaciones**.  
  
3.  Haz clic en **propiedades de la clasificación**y, a continuación, haz clic en **establecer propiedades de administración de la carpeta**.  
  
4.  En la **propiedad** cuadro, haz clic en **Access-Denied asistencia mensaje**y, a continuación, haz clic en **agregar**.  
  
5.  Haz clic en **examinar**y, a continuación, elige la carpeta que debería tener el mensaje personalizado de acceso denegado.  
  
6.  En la **valor** cuadro, escribe el mensaje que se debe presentar a los usuarios cuando pueden acceder a un recurso de esa carpeta.  
  
    Puedes agregar macros hasta el mensaje que se insertará texto personalizado. Las macros incluyen:  
  
    -   **[Ruta del archivo original] **La ruta de acceso del archivo original que se tuvo acceso por parte del usuario.  
  
    -   **[Carpeta de ruta de acceso de archivo original] **La carpeta primaria de la ruta de acceso del archivo original que se tuvo acceso por parte del usuario.  
  
    -   **[Correo electrónico de administrador] **La lista de destinatarios del correo electrónico de administrador.  
  
    -   **[Datos propietario, correo electrónico] **La lista de destinatarios del correo electrónico de propietario de datos.  
  
7.  Haz clic en **Aceptar**y, a continuación, haz clic en **cerrar**.  
  
![guías de solución](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Paso 2: Configurar la configuración de notificaciones de correo electrónico  
Debes configurar la configuración de notificaciones de correo electrónico en cada servidor de archivos que se envía los mensajes de acceso denegado asistencia.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
2.  Haz clic en **Administrador de recursos del servidor de archivos (locales)**y, a continuación, haz clic en **configurar opciones**.  
  
3.  Haz clic en el **notificaciones por correo electrónico** pestaña.  
  
4.  Configura las opciones siguientes:  
  
    -   En la **SMTP servidor nombre o dirección IP**, escriba el nombre de la dirección IP del servidor SMTP en la organización.  
  
    -   En la **Administradores receptores predeterminados** y **predeterminado 'De' dirección de correo electrónico** cuadros, escribe la dirección de correo electrónico del administrador del servidor de archivos.  
  
5.  Haz clic en **enviar correo electrónico de prueba** para garantizar que las notificaciones de correo electrónico están configuradas correctamente.  
  
6.  Haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Paso 3: Comprobar que asistencia acceso denegado está correctamente configurado  
Para comprobar que la Ayuda de acceso denegado está configurada correctamente al disponer de un usuario que ejecuta Windows 8 intenta obtener acceso a un recurso compartido o un archivo en que comparten que no tienen acceso a. Cuando aparezca el mensaje de acceso denegado, el usuario debe ver un **solicitar asistencia** botón. Después de hacer clic en el botón de la solicitud de asistencia, el usuario puede especificar un motivo de acceso y, a continuación, enviar un correo electrónico al propietario de la carpeta o el administrador del servidor de archivos. El propietario de la carpeta o el administrador del servidor de archivos puede comprobar que el correo electrónico llegado y contiene los detalles correspondientes.  
  
> [!IMPORTANT]  
> Si quieres comprobar la asistencia de acceso denegado al disponer de un usuario que ejecuta Windows Server 2012, debes instalar la experiencia de escritorio antes de conectar el recurso compartido de archivos.  
  
## <a name="BKMK_Links"></a>Consulta también  
  
-   [Escenario: Acceso denegado asistencia](Scenario--Access-Denied-Assistance.md)  
  
-   [Plan para obtener ayuda de acceso denegado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

