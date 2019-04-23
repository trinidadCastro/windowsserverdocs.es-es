---
title: 'Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 2; trabajo posterior a la configuración de AD FS'
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 87fdcf06c601d3362488eaf6a83e4f88ad191305
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828236"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 2, trabajo posterior a la configuración de AD FS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el segundo paso para implementar Carpetas de trabajo con los Servicios de federación de Active Directory (AD FS) y el Proxy de aplicación web. Puedes encontrar el resto de pasos de este proceso en estos temas:  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 1: configuración de AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 4: configurar el Proxy de aplicación Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: El paso 5, configure los clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Las instrucciones incluidas en esta sección toman como referencia un entorno de Server 2016. Si estás usando Windows Server 2012 R2, lee el artículo en el que se detallan las [instrucciones para Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

En el primer paso instalaste y configuraste AD FS. Ahora debes realizar los pasos posteriores a la configuración de AD FS.  
  
## <a name="configure-dns-entries"></a>Configurar las entradas DNS  
Debes crear dos entradas DNS de AD FS. Estas son las mismas entradas que se usaron en los pasos de preinstalación cuando creaste el certificado de nombre alternativo del firmante (SAN).  
  
Las entradas DNS están en el siguiente formulario:  
  
-   nombre de servicio de AD FS.dominio  
  
-   enterpriseregistration.dominio  
  
-   nombre del servidor de AD FS.dominio   (la entrada DNS ya debería estar indicada. Por ejemplo, 2016-ADFS.contoso.com)
  
En el ejemplo de prueba, los valores son:  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>Crear los registros A y CNAME de AD FS  
Para crear los registros A y CNAME de AD FS, sigue estos pasos:  
  
1.  Desde el controlador de dominio, abre el administrador de DNS.  
  
2.  Expande la carpeta Zonas de búsqueda directa, haz clic con el botón derecho en el dominio y selecciona **Nuevo host (A)**.  
  
3.  Se abrirá la ventana **Nuevo host**. En el campo **Nombre**, escribe el alias del nombre de servicio de AD FS. En el ejemplo de prueba, este nombre es **blueadfs**.  
  
    Recuerda que el alias debe ser igual al del firmante del certificado que se usó en AD FS. Por ejemplo, si el firmante es adfs.contoso.com, el alias que tienes que escribir aquí debe ser **adfs**.  
  
    > [!IMPORTANT]  
    > Cuando configures AD FS mediante la interfaz de usuario (UI) de Windows Server en lugar de Windows PowerShell, debes crear un registro A en lugar de un registro CNAME para AD FS. Esto es debido a que el nombre de entidad de seguridad de servicio (SPN) que se creó mediante la interfaz de usuario, solo contiene el alias que se usó para configurar el servicio de AD FS a modo de host.  
    >   
4.  En **Dirección IP**, escribe la dirección IP del servidor de AD FS. En el ejemplo de prueba, esta es **192.168.0.160**. Haga clic en **Agregar host**.  
  
5.  En la carpeta de zonas de búsqueda directa, haz clic de nuevo en el dominio y selecciona **Nuevo alias (CNAME)**.  
  
6.  En la ventana **Nuevo registro de recursos**, agrega el alias **enterpriseregistration** y escribe el nombre de dominio completo (FQDN) del servidor de AD FS. Este alias se usa para unir el dispositivo y debe llamarse **enterpriseregistration**.
  
7.  Haga clic en **Aceptar**.  
  
Para realizar estos mismos pasos mediante Windows PowerShell, usa el siguiente comando. El comando debe ejecutarse en el controlador de dominio.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com   
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>Configurar la relación de confianza para el usuario autenticado de AD FS en Carpetas de trabajo  
Puedes instalar y configurar la relación de confianza para usuario autenticado en Carpetas de trabajo, incluso si todavía no has instalado Carpetas de trabajo. Recuerda que debes configurar la relación de confianza para usuario autenticado para que Carpetas de trabajo pueda usar AD FS. Como ya estás configurando AD FS, ahora es buen momento para realizar este paso.  
  
Para configurar la relación de confianza para usuario autenticado:  
  
1.  Abre el **Administrador del servidor**, en el menú **Herramientas**, selecciona **Administración de AD FS**.  
  
2.  En el panel derecho, en la sección **Acciones**, haz clic en **Agregar relación de confianza para usuario autenticado**.  
  
3.  En la página de **Bienvenida**, selecciona **Compatible con notificaciones** y haz clic en **Inicio**.  
  
4.  En la página **Seleccionar origen de datos**, selecciona **Escribir manualmente los datos acerca del usuario de confianza** y, a continuación, haz clic en **Siguiente**.  
  
5.  En el campo **Nombre para mostrar**, escribe **WorkFolders** y, a continuación, haz clic en **Siguiente**.  
  
6.  En la página **Configurar certificado**, haz clic en **Siguiente**. Los certificados de cifrado de tokens son opcionales y no son necesarios para la realizar la configuración de prueba.  
  
7.  En la página **Configurar URL**, haz clic en **Siguiente**.  
  
8. En el **configurar identificadores** página, agregue el siguiente identificador: **https://windows-server-work-folders/V1**. Este identificador es un valor codificado de forma rígida que usa Carpetas de trabajo y que el servicio de Carpetas de trabajo envía cuando se está comunicando con AD FS. Haz clic en **Siguiente**.  
  
9. En la página Elegir la directiva de control de acceso, selecciona **Permitir todos los usuarios** y, a continuación, haz clic en **Siguiente**.  
  
10. En la página **Listo para agregar confianza**, haz clic en **Siguiente**.  
  
11. Una vez finalizada la configuración, la última página del asistente indica que la configuración se realizó correctamente. Selecciona la casilla para editar las reglas de notificaciones y haz clic en **Cerrar**.  
  
12. En el complemento de AD FS, selecciona la relación de confianza para usuario autenticado en **WorkFolders** y haz clic en **Editar directiva de emisión de notificaciones** que se encuentra en el apartado Acciones.

13. Si abrirá la ventana **Editar directiva de emisión de notificaciones de WorkFolders**. Haz clic en **Agregar regla**.  
  
14. En la lista desplegable **Plantilla de regla de notificación**, selecciona **Enviar atributos LDAP como notificaciones** y haz clic en **Siguiente**.  
  
15. En la página **Configurar regla de notificación**, en el campo **Nombre de regla de notificación**, escribe **WorkFolders**.  
  
16. En la lista desplegable **Almacén de atributos**, selecciona **Active Directory**.  
  
17. En la tabla de asignaciones, escribe estos valores:  
  
    -   Entidad de seguridad-nombre de usuario: UPN  
  
    -   Nombre para mostrar: Nombre  
  
    -   Apellido: Apellido  
  
    -   Dado nombre: Nombre propio  
  
18. Haga clic en **Finalizar**. Verás que la regla WorkFolders aparece en la pestaña Reglas de transformación de emisión; haz clic en **Aceptar**.  
  
### <a name="set-relying-part-trust-options"></a>Establecer las opciones de la relación de confianza para usuario autenticado  
Después configurar la relación de confianza para usuario autenticado en AD FS, debes finalizar la configuración ejecutando cinco comandos en Windows PowerShell. Estos comandos establecen las opciones necesarias para que Carpetas de trabajo se comunique correctamente con AD FS, y no se pueden establecer a través de la interfaz de usuario. Estas opciones son:  
  
-   Habilitar el uso de estándares de JSON Web Token (JWTs)  
  
-   Deshabilitar las notificaciones cifradas  
  
-   Habilitar actualizaciones\-automáticas  
  
-   Establecer la emisión del token de actualización de Oauth a todos los dispositivos.  

-   Conceder a los clientes acceso a la relación de confianza para usuario autenticado

Para establecer estas opciones, usa los siguientes comandos:  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://windows-server-work-folders/V1" -AllowAllRegisteredClients -ScopeNames openid,profile  
```  
  
## <a name="enable-workplace-join"></a>Habilitar Workplace Join  
Puedes habilitar Workplace Join de manera opcional; recuerda que puede serte de utilidad si quieres permitir que los usuarios usen sus dispositivos personales para acceder a los recursos de la empresa.  
  
Para habilitar el registro del dispositivos de Workplace Join, debes ejecutar los siguientes comandos de Windows PowerShell; estos configurarán el registro de dispositivos y establecerán la directiva de autenticación global:  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>Exportar el certificado de AD FS  
A continuación, exporta el certificado auto\-firmado de AD FS para que puedas instalarlo en las siguientes máquinas del entorno de prueba:  
  
-   El servidor que se usó en Carpetas de trabajo  
  
-   El servidor que se usó en el Proxy de aplicación web  
  
-   El cliente de Windows unido a un dominio  
  
-   El cliente de Windows que no está unido a un dominio  
  
Para exportar el certificado, sigue los siguientes pasos:  
  
1.  Haga clic en **Inicio** y, a continuación, en **Ejecutar**.  
  
2.  Escribe **MMC**.  
  
3.  En el menú **Archivo**, haga clic en **Agregar o quitar complemento**.  
  
4.  En la lista **Complementos disponibles**, selecciona **Certificados** y, a continuación, haz clic en **Agregar**. Se iniciará el asistente del complemento de certificados.  
  
5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  Selecciona **Equipo local: (el equipo en el que se ejecuta la consola)** y haz clic en **Finalizar**.  
  
7.  Haga clic en **Aceptar**.  
  
8.  Expande la carpeta **Raíz de consola\Certificados\(equipo local) \Personal\Certificados**.  
  
9.  Haz clic con el botón derecho en el **Certificado de AD FS**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Exportar...**.  
  
10. Se abre el Asistente para exportación de certificados. Selecciona **Sí, exportar la clave privada**.  
  
11. En la página **Exportar formato de archivo**, deja seleccionadas las opciones predeterminadas y haz clic en **Siguiente**.  
  
12. Crea una contraseña para el certificado. Esta es la contraseña que usarás cuando importes el certificado a otros dispositivos. Haz clic en **Siguiente**.  
  
13. Escribe una ubicación y un nombre para el certificado y, a continuación, haz clic en **Finalizar**.  
  
La manera de instalar el certificado se explica más adelante en el procedimiento de implementación.  
  
## <a name="manage-the-private-key-setting"></a>Administrar la configuración de la clave privada  
Debes asignar el permiso de cuenta de servicio de AD FS para acceder a la clave privada del nuevo certificado. Asimismo, deberás conceder este permiso de nuevo al reemplazar el certificado de comunicación después de que expire. Para conceder el permiso, sigue estos pasos:  
  
1.  Haga clic en **Inicio** y, a continuación, en **Ejecutar**.  
  
2.  Escribe **MMC**.  
  
3.  En el menú **Archivo**, haga clic en **Agregar o quitar complemento**.  
  
4.  En la lista **Complementos disponibles**, selecciona **Certificados** y, a continuación, haz clic en **Agregar**. Se iniciará el asistente del complemento de certificados.  
  
5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  Selecciona **Equipo local: (el equipo en el que se ejecuta la consola)** y haz clic en **Finalizar**.  
  
7.  Haga clic en **Aceptar**.  
  
8.  Expande la carpeta **Raíz de consola\Certificados\(equipo local) \Personal\Certificados**.  
  
9.  Haz clic con el botón derecho en el **Certificado de AD FS**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Administrar claves privadas**.  
  
10. En la ventana **Permisos**, haz clic en **Agregar**.  
  
11. En la ventana **Tipos de objeto**, selecciona **Cuentas de servicio** y, a continuación, haz clic en **Aceptar**.  
  
12. Escribe el nombre de la cuenta que ejecuta AD FS. En el ejemplo de prueba, este nombre es ADFSService. Haga clic en **Aceptar**.  
  
13. En la ventana **Permisos**, concédele a la cuenta, como mínimo, permiso de lectura y haz clic en **Aceptar**.  
  
Si no tiene la opción para administrar las claves privadas, necesita ejecutar el comando siguiente: `certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>Comprobar que AD FS funciona correctamente  
Para comprobar que AD FS esté funcionando, abra una ventana del explorador y vaya a https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml 
  
En la ventana del explorador verás los metadatos del servidor de federación sin formato. Si puedes ver los datos sin errores de SSL ni advertencias, eso quiere decir que el servidor de federación está operativo.  
  
Paso siguiente: [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
## <a name="see-also"></a>Vea también  
[Introducción a las carpetas de trabajo](Work-Folders-Overview.md)  
  

