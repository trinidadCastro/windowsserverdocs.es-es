---
title: 'Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración'
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 06/06/2019
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 42e7477c168b0b7c1230fd37b5fb26d77590ef1c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965777"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el segundo paso en la implementación de carpetas de trabajo con Servicios de federación de Active Directory (AD FS) (AD FS) y el proxy de aplicación Web. Puede encontrar los demás pasos de este proceso en estos temas:  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4: configurar el proxy de aplicación Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
> Las instrucciones que se describen en esta sección son para un entorno de Windows Server 2019 o Windows Server 2016. Si usa Windows Server 2012 R2, siga las instrucciones de [Windows server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn747208(v=ws.11)).

En el paso 1, instaló y configuró AD FS. Ahora, debe realizar los siguientes pasos posteriores a la configuración para AD FS.  
  
## <a name="configure-dns-entries"></a>Configurar entradas DNS

Debe crear dos entradas DNS para AD FS. Se trata de las mismas dos entradas que se usaron en los pasos previos a la instalación al crear el certificado de nombre alternativo del firmante (SAN).  
  
Las entradas DNS tienen el formato siguiente:  
  
-   Nombre del servicio de AD FS. dominio  
  
-   enterpriseregistration. dominio  
  
-   AD FS nombre del servidor. dominio (la entrada DNS debe existir ya. p. ej., 2016-ADFS.contoso.com)
  
En el ejemplo de prueba, los valores son:  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>Cree los registros A y CNAME para AD FS

Para crear registros de y CNAME para AD FS, siga estos pasos:  
  
1.  En el controlador de dominio, abra el administrador de DNS.  
  
2.  Expanda la carpeta zonas de búsqueda directa, haga clic con el botón derecho en el dominio y seleccione **host nuevo (A)**.  
  
3.  Se abre la ventana **nuevo host** . En el campo **nombre** , escriba el alias del nombre del servicio AD FS. En el ejemplo de prueba, es **blueadfs**.  
  
    El alias debe ser el mismo que el sujeto del certificado que se usó para AD FS. Por ejemplo, si el asunto fuera adfs.contoso.com, el alias especificado aquí sería **ADFS**.  
  
    > [!IMPORTANT]  
    > Al configurar AD FS mediante la interfaz de usuario (UI) de Windows Server en lugar de Windows PowerShell, debe crear un registro A en lugar de un registro CNAME para AD FS. La razón es que el nombre de entidad de seguridad de servicio (SPN) que se crea a través de la interfaz de usuario solo contiene el alias que se usa para configurar el servicio de AD FS como host.  

4.  En **dirección IP**, escriba la dirección IP del servidor de AD FS. En el ejemplo de prueba, es **192.168.0.160**. Haz clic en **Agregar host**.  
  
5.  En la carpeta zonas de búsqueda directa, haga clic con el botón derecho en el dominio de nuevo y seleccione **nuevo alias (CNAME)**.  
  
6.  En la ventana **nuevo registro de recursos** , agregue el nombre de alias **enterpriseregistration** y escriba el FQDN del servidor de AD FS. Este alias se usa para la Unión de dispositivos y se debe llamar **enterpriseregistration**.
  
7.  Haga clic en **OK**.  
  
Para realizar los pasos equivalentes a través de Windows PowerShell, use el siguiente comando. El comando debe ejecutarse en el controlador de dominio.  
  
```Powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>Configurar la AD FS relación de confianza para usuario autenticado para carpetas de trabajo

Puede instalar y configurar la relación de confianza para usuario autenticado para carpetas de trabajo, aunque aún no se hayan configurado las carpetas de trabajo. La relación de confianza para usuario autenticado debe estar configurada para permitir que las carpetas de trabajo usen AD FS. Dado que se encuentra en el proceso de configuración de AD FS, ahora es un buen momento para realizar este paso.  
  
Para configurar la relación de confianza para usuario autenticado:  
  
1.  Abra **Administrador del servidor**, en el menú **herramientas** , seleccione **Administración de AD FS**.  
  
2.  En el panel derecho, en **acciones**, haga clic en **Agregar relación de confianza para usuario autenticado**.  
  
3.  En la página de **bienvenida** , seleccione **reconocimiento de notificaciones** y haga clic en **iniciar**.  
  
4.  En la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre el usuario de confianza**y, a continuación, haga clic en **siguiente**.  
  
5.  En el campo **nombre para mostrar** , escriba **WorkFolders**y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **configurar certificado** , haga clic en **siguiente**. Los certificados de cifrado de tokens son opcionales y no son necesarios para la configuración de prueba.  
  
7.  En la página **configurar dirección URL** , haga clic en **siguiente**.  
  
8. En la página **configurar identificadores** , agregue el siguiente identificador: `https://windows-server-work-folders/V1` . Este identificador es un valor codificado de forma rígida que usan las carpetas de trabajo y lo envía el servicio carpetas de trabajo cuando se comunica con AD FS. Haga clic en **Next**.  
  
9. En la página elegir Directiva de Access Control, seleccione **permitir todos**y, a continuación, haga clic en **siguiente**.  
  
10. En la página **Listo para agregar confianza**, haz clic en **Siguiente**.  
  
11. Una vez finalizada la configuración, la última página del asistente indica que la configuración se realizó correctamente. Active la casilla para editar las reglas de notificaciones y haga clic en **cerrar**.  
  
12. En el complemento AD FS, seleccione la relación de confianza para usuario autenticado de **WorkFolders** y haga clic en **Editar Directiva de emisión de notificaciones** en acciones.

13. Se abre la ventana **Editar Directiva de emisión de notificaciones para WorkFolders** . Haga clic en **Agregar regla**.  
  
14. En la lista desplegable **plantilla de regla de notificación** , seleccione **Enviar atributos LDAP como notificaciones**y haga clic en **siguiente**.  
  
15. En la página **configurar regla de notificaciones** , en el campo **nombre de regla de notificaciones** , escriba **WorkFolders**.  
  
16. En la lista desplegable **almacén de atributos** , seleccione **Active Directory**.  
  
17. En la tabla de asignación, escriba estos valores:  
  
    -   Nombre principal de usuario: UPN  
  
    -   Nombre para mostrar: nombre  
  
    -   Apellidos: apellidos  
  
    -   Nombre dado: nombre especificado  
  
18. Haga clic en **Finalizar** Verá la regla WorkFolders en la pestaña reglas de transformación de emisión y haga clic en **Aceptar**.  
  
### <a name="set-relying-part-trust-options"></a>Establecer opciones de confianza de elementos de confianza

Una vez configurada la relación de confianza para usuario autenticado para AD FS, debe finalizar la configuración ejecutando cinco comandos en Windows PowerShell. Estos comandos establecen las opciones necesarias para que las carpetas de trabajo se comuniquen correctamente con AD FS y no se pueden establecer a través de la interfaz de usuario. Estas opciones son:  
  
-   Habilitación del uso de tokens web JSON (JWT)  
  
-   Deshabilitar notificaciones cifradas  
  
-   Habilitar la \- actualización automática  
  
-   Establezca la emisión de tokens de actualización de OAuth en todos los dispositivos.  

-   Conceder a los clientes acceso a la relación de confianza para usuario autenticado

Para establecer estas opciones, use los comandos siguientes:  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://windows-server-work-folders/V1" -AllowAllRegisteredClients -ScopeNames openid,profile  
```  
  
## <a name="enable-workplace-join"></a>Habilitar Workplace Join

Habilitar Workplace Join es opcional, pero puede ser útil si desea que los usuarios puedan usar sus dispositivos personales para acceder a los recursos del área de trabajo.  
  
Para habilitar el registro de dispositivos para Workplace Join, debe ejecutar los siguientes comandos de Windows PowerShell, que configurarán el registro del dispositivo y establecerán la Directiva de autenticación global:  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>Exportar el certificado de AD FS

A continuación, exporte el \- certificado de AD FS autofirmado para que se pueda instalar en las siguientes máquinas del entorno de prueba:  
  
-   El servidor que se usa para carpetas de trabajo  
  
-   El servidor que se usa para el proxy de aplicación Web  
  
-   Cliente de Windows unido a un dominio  
  
-   Cliente de Windows no unido a un dominio  
  
Para exportar el certificado, siga estos pasos:  
  
1.  Haga clic en **Inicio**y, a continuación, haga clic en **Ejecutar**.  
  
2.  Escriba **MMC**.  
  
3.  En el menú **Archivo** , haga clic en **Agregar o quitar complemento**.  
  
4.  En la lista **complementos disponibles** , seleccione **certificados**y, a continuación, haga clic en **Agregar**. Se inicia el Asistente para complementos de certificados.  
  
5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  Seleccione **equipo local: (el equipo en el que se está ejecutando esta consola)** y, a continuación, haga clic en **Finalizar**.  
  
7.  Haga clic en **OK**.  
  
8.  Expanda la **consola de carpeta Root\Certificates \( equipo local) \personal\certificados**.  
  
9.  Haga clic con el botón secundario en el **certificado AD FS**, haga clic en **todas las tareas**y, a continuación, haga clic en **exportar..**..  
  
10. Se abre el Asistente para exportar certificados. Selecciona **Sí, exportar la clave privada**.  
  
11. En la página **formato de archivo de exportación** , deje seleccionadas las opciones predeterminadas y haga clic en **siguiente**.  
  
12. Cree una contraseña para el certificado. Se trata de la contraseña que utilizará más adelante cuando importe el certificado en otros dispositivos. Haga clic en **Next**.  
  
13. Escriba una ubicación y un nombre para el certificado y, a continuación, haga clic en **Finalizar**.  
  
La instalación del certificado se trata más adelante en el procedimiento de implementación.  
  
## <a name="manage-the-private-key-setting"></a>Administrar la configuración de la clave privada

Debe conceder permiso a la cuenta de servicio de AD FS para tener acceso a la clave privada del nuevo certificado. Tendrá que conceder este permiso de nuevo cuando reemplace el certificado de comunicación después de que expire. Para conceder permiso, siga estos pasos:  
  
1.  Haga clic en **Inicio**y, a continuación, haga clic en **Ejecutar**.  
  
2.  Escriba **MMC**.  
  
3.  En el menú **Archivo** , haga clic en **Agregar o quitar complemento**.  
  
4.  En la lista **complementos disponibles** , seleccione **certificados**y, a continuación, haga clic en **Agregar**. Se inicia el Asistente para complementos de certificados.  
  
5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  Seleccione **equipo local: (el equipo en el que se está ejecutando esta consola)** y, a continuación, haga clic en **Finalizar**.  
  
7.  Haga clic en **OK**.  
  
8.  Expanda la **consola de carpeta Root\Certificates \( equipo local) \personal\certificados**.  
  
9.  Haga clic con el botón secundario en el **certificado AD FS**, haga clic en **todas las tareas**y, a continuación, haga clic en **administrar claves privadas**.  
  
10. En la ventana **permisos** , haga clic en **Agregar**.  
  
11. En la ventana **tipos de objeto** , seleccione cuentas de **servicio**y, a continuación, haga clic en **Aceptar**.  
  
12. Escriba el nombre de la cuenta que ejecuta AD FS. En el ejemplo de prueba, es ADFSService. Haga clic en **OK**.  
  
13. En la ventana **permisos** , asigne a la cuenta como mínimo permisos de lectura y haga clic en **Aceptar**.  
  
Si no tiene la opción de administrar claves privadas, puede que tenga que ejecutar el siguiente comando:`certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>Comprobar que AD FS está operativo

Para comprobar que AD FS está operativo, abra una ventana del explorador y vaya a `https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` , cambiando la dirección URL para que coincida con su entorno.
  
La ventana del explorador mostrará los metadatos del servidor de Federación sin ningún formato. Si puede ver los datos sin errores ni advertencias de SSL, el servidor de Federación está operativo.  
  
Paso siguiente: [implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
## <a name="see-also"></a>Consulte también  
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)
