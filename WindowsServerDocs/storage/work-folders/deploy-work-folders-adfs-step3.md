---
title: 'Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 3; configurar Carpetas de trabajo'
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: d6b21579fb1dedc777733317e7222debd8d944a1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812668"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 3, configurar carpetas de trabajo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el tercer paso para implementar Carpetas de trabajo con los Servicios de federación de Active Directory (AD FS) y el Proxy de aplicación web. Puedes encontrar el resto de pasos de este proceso en estos temas:  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 1: configuración de AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 2, trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 4: configurar el Proxy de aplicación Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: El paso 5, configure los clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Las instrucciones descritas en esta sección son para un entorno de Windows Server 2019 o Windows Server 2016. Si estás usando Windows Server 2012 R2, lee el artículo en el que se detallan las [instrucciones para Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Para configurar Carpetas de trabajo, usa los siguientes procedimientos.  
  
## <a name="pre-installment-work"></a>Trabajo previo de\-instalación  
Para instalar Carpetas de trabajo, debes tener un servidor unido al dominio y que ejecute Windows Server 2016. El servidor debe tener una configuración de red válida.  
  
En el ejemplo de prueba, debes unir el equipo que ejecute Carpetas de trabajo al dominio de Contoso y configurar la interfaz de red tal como se describe en las siguientes secciones. 

### <a name="set-the-server-ip-address"></a>Establecer la dirección IP del servidor  
Cambia la dirección IP del servidor a una dirección IP estática. Por ejemplo, prueba, utilice la clase IP A, que es 192.168.0.170 / máscara de subred: 255.255.0.0 / puerta de enlace predeterminada: 192.168.0.1 / preferido DNS: 192.168.0.150 (la dirección IP del controlador de dominio). 
  
### <a name="create-the-cname-record-for-work-folders"></a>Crear el registro CNAME de Carpetas de trabajo  
Para crear el registro CNAME de Carpetas de trabajo, sigue estos pasos:  
  
1.  Desde el controlador de dominio, abre el **administrador de DNS**.  
  
2.  Expande la carpeta Zonas de búsqueda directa, haz clic con el botón derecho en el dominio y haz clic en **Nuevo alias (CNAME)** .  
  
3.  En la ventana **Nuevo registro de recursos** que se encuentra en el campo **Nombre de alias**, escribe el alias de Carpetas de trabajo. En el ejemplo de prueba, este nombre es **workfolders**.  
  
4.  En el campo **Nombre de dominio completo**, el valor debe ser **workfolders.contoso.com**.  
  
5.  En el campo **Nombre de dominio completo del host de destino**, escribe el nombre de dominio completo (FQDN) del servidor de Carpetas de trabajo. En el ejemplo de prueba, este nombre es **2016-WF.contoso.com**.  
  
6.  Haga clic en **Aceptar**.  
  
Para realizar estos mismos pasos mediante Windows PowerShell, usa el siguiente comando. El comando debe ejecutarse en el controlador de dominio.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com   
```  
  
### <a name="install-the-ad-fs-certificate"></a>Instalar el certificado de AD FS  
Para instalar el certificado de AD FS que se creó durante la instalación de AD FS en el almacén de certificados del equipo local, sigue estos pasos:  
  
1.  Haga clic en **Inicio** y, a continuación, en **Ejecutar**.  
  
2.  Escribe **MMC**.  
  
3.  En el menú **Archivo**, haga clic en **Agregar o quitar complemento**.  
  
4.  En la lista **Complementos disponibles**, selecciona **Certificados** y, a continuación, haz clic en **Agregar**. Se iniciará el asistente del complemento de certificados.  
  
5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.  
  
6.  Selecciona **Equipo local: (el equipo en el que se ejecuta la consola)** y haz clic en **Finalizar**.  
  
7.  Haga clic en **Aceptar**.  
  
8.  Expande la carpeta **Raíz de consola\Certificados\(equipo local) \Personal\Certificados**.  
  
9. Haz clic con el botón derecho en **Certificados**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Importar**.  
  
10. Busca la carpeta que contiene el certificado de AD FS y sigue las instrucciones del asistente para importar el archivo; a continuación, colócalo en el almacén de certificados.

11. Expande la carpeta **Raíz de consola\Certificados\(equipo local) \Entidades de certificación raíz de confianza\Certificados**.  
  
12. Haz clic con el botón derecho en **Certificados**, haz clic en **Todas las tareas** y, a continuación, haz clic en **Importar**.  
  
13. Busca la carpeta que contiene el certificado de AD FS y sigue las instrucciones del asistente para importar el archivo; a continuación, colócalo en el almacén Entidades de certificación raíz de confianza.  
  
### <a name="create-the-work-folders-self-signed-certificate"></a>Crear el certificado autofirmado de Carpetas de trabajo  
Para crear el certificado autofirmado de Carpetas de trabajo, sigue estos pasos:  
  
1.  Descarga los scripts que se proporcionan en el artículo de blog [Implementar Carpetas de trabajo con AD FS y Proxy de aplicación web (WAP)](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap) y copia el archivo makecert.ps1 en el equipo de Carpetas de trabajo.  
  
2.  Abre una ventana de Windows PowerShell con privilegios de administrador.  
  
3.  Establece la directiva de ejecución en la opción "sin restricciones":  
  
    ```powershell  
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted   
    ```  
  
4.  Accede al directorio donde has copiado el script.  
  
5.  Ejecuta el script makeCert:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  Cuando tengas que cambiar el certificado de firmante, escribe el nuevo valor del mismo. En este ejemplo, el valor es **workfolders.contoso.com**.  
  
7.  Cuando debas escribir los nombres alternativos del firmante (SAN), presiona Y y, a continuación, escribe los nombres SAN, uno por uno.  
  
    Para este ejemplo, escribe **workfolders.contoso.com**, y presiona Entrar. A continuación, escribe **2016-WF.contoso.com** y presiona Entrar.  
  
    Cuando hayas introducido todos los nombres SAN, presiona Entrar en una línea vacía.  
  
8.  En el momento en que tengas que instalar los certificados en el almacén de la entidad de certificación raíz de confianza, presiona Y.  
  
El certificado de Carpetas de trabajo debe ser un certificado SAN que tenga los siguientes valores:  
  
-   **workfolders**.**dominio**  
  
-   **nombre de la máquina**.**dominio**  
  
En el ejemplo de prueba, los valores son:  
  
-   **workfolders.contoso.com**  
  
-   **2016-WF.contoso.com**  
  
## <a name="install-work-folders"></a>Instalar Carpetas de trabajo  
Para instalar el rol de Carpetas de trabajo, sigue estos pasos:  
  
1.  Abre el **Administrador del servidor** y, a continuación, haz clic en **Agregar roles y características** y en **Siguiente**.  
  
2.  En la página **Tipo de instalación**, selecciona **Instalación basada en características o en roles** y, a continuación, haz clic en **Siguiente**.  
  
3.  En la página **Selección de servidores**, selecciona el servidor actual y haz clic en **Siguiente**.  
  
4.  En la página **Roles de servidor**, expande la opción **Servicios de archivos y almacenamiento**, expande **Servicios de iSCSI y archivo** y posteriormente selecciona **Carpetas de trabajo**.  
  
5.  En la página **Asistente para agregar roles y características**, haz clic en **Agregar características**y, a continuación, en **Siguiente**.  
  
6.  En la página **Características**, haz clic en **Siguiente**.  
  
7.  En la página **Confirmación**, haz clic en **Instalar**.  
  
## <a name="configure-work-folders"></a>Configurar las carpetas de trabajo  
Para configurar Carpetas de trabajo, sigue estos pasos:  
  
1.  Abre el **Administrador del servidor**.  
  
2.  Selecciona **Servicios de archivos y almacenamiento** y, a continuación, selecciona **Carpetas de trabajo**.  
  
3.  En la página **Carpetas de trabajo**, inicia el **Asistente para crear recursos compartidos de sincronización** y haz clic en **Siguiente**.  
  
4.  En la página **Ruta de acceso y servidor**, selecciona el servidor donde se crearán los recursos compartidos de sincronización, escribe la ruta de acceso local donde se almacenarán los datos de Carpetas de trabajo y haz clic en **Siguiente**.  
  
    Si no existe la ruta de acceso, deberás crear una. Haga clic en **Aceptar**.  
  
5.  En la página **Estructura de carpetas de usuario**, selecciona **Alias del usuario** y, a continuación, haz clic en **Siguiente**.  
  
6.  En la página **Nombre del recurso compartido de sincronización**, escribe el nombre del recurso compartido de sincronización. En el ejemplo de prueba, este nombre es **WorkFolders**. Haz clic en **Siguiente**.  
  
7.  En la página **Sincronizar acceso**, agrega los usuarios o grupos que tendrán acceso al nuevo recurso de sincronización. En el ejemplo de prueba, se concede acceso a todos los usuarios del dominio. Haz clic en **Siguiente**.  
  
8.  En la página **Directivas de seguridad del equipo**, selecciona **Cifrar Carpetas de trabajo** y **Bloquear automáticamente la página y solicitar contraseña**. Haz clic en **Siguiente**.  
  
9. En la página **Confirmación**, haz clic en **Crear** para completar el proceso de configuración.  
  
## <a name="work-folders-post-configuration-work"></a>Trabajo posterior a la configuración de Carpetas de trabajo  
Para finalizar la configuración de Carpetas de trabajo, sigue estos pasos adicionales:  
  
-   Enlaza el certificado de Carpetas de trabajo al puerto SSL  
  
-   Configura Carpetas de trabajo para poder usar la autenticación de AD FS  
  
-   Exporta el certificado de Carpetas de trabajo (si estás usando un certificado autofirmado)  
  
### <a name="bind-the-certificate"></a>Enlazar el certificado  
Carpetas de trabajo se comunica solo a través de SSL y debe tener el certificado autofirmado que creaste anteriormente (o el certificado que emitió la entidad de certificación) enlazado al puerto.  
  
Hay dos métodos que puede usar para enlazar el certificado al puerto a través de Windows PowerShell: Cmdlets IIS y netsh.  
  
#### <a name="bind-the-certificate-by-using-netsh"></a>Enlazar el certificado mediante netsh  
Para usar la utilidad de scripting de línea de comandos netsh en Windows PowerShell, debes canalizar el comando netsh. El siguiente script de ejemplo busca el certificado con el firmante **workfolders.contoso.com** y lo enlaza al puerto 443 mediante netsh:  
  
```powershell  
$subject = "workfolders.contoso.com"   
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
$Command = "http add sslcert ipport=0.0.0.0:443 certhash=$thumbprint appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY"  
$Command | netsh  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>Enlazar el certificado mediante los cmdlets de IIS  
Asimismo, puedes enlazar el certificado al puerto mediante los cmdlets de administración IIS, los cuales puedes tener disponibles si se instalan los scripts y herramientas de administración IIS.  
  
> [!NOTE]  
> Recuerda que instalar las herramientas de administración IIS no habilita la versión completa de Internet Information Services (IIS) en el equipo de Carpetas de trabajo; esto solo lo tienen permitido los cmdlets de administración. En cambio, esta configuración produce ciertos beneficios. Por ejemplo, si estás buscando cmdlets para proporcionar la funcionalidad que obtienes de netsh. Cuando el certificado se enlaza al puerto mediante el cmdlet New-WebBinding, el enlace no depende de IIS en modo alguno. Después de realizar el enlace, incluso puedes quitar la función Web-Mgmt-Console y el certificado seguirá enlazado al puerto. Puedes comprobar el enlace a través de netsh escribiendo **netsh http show sslcert**.  
  
En el siguiente ejemplo se usa el cmdlet New-WebBinding para encontrar el certificado con el firmante **workfolders.contoso.com** y enlazarlo a puerto 443:  
  
```powershell  
$subject = "workfolders.contoso.com"  
Try  
{  
#In case there are multiple certificates with the same subject, get the latest version   
$cert =Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match $subject } | sort $_.NotAfter -Descending | select -first 1   
$thumbprint = $cert.Thumbprint  
New-WebBinding -Name "Default Web Site" -IP * -Port 443 -Protocol https  
#The default IIS website name must be used for the binding. Because Work Folders uses Hostable Web Core and its own configuration file, its website name, 'ECSsite', will not work with the cmdlet. The workaround is to use the default IIS website name, even though IIS is not enabled, because the NewWebBinding cmdlet looks for a site in the default IIS configuration file.   
Push-Location IIS:\SslBindings  
Get-Item cert:\LocalMachine\MY\$thumbprint | new-item *!443  
Pop-Location  
}  
Catch  
{  
"     Error: unable to locate certificate for $($subject)"  
Exit  
}   
```  
  
### <a name="set-up-ad-fs-authentication"></a>Configurar la autenticación de AD FS  
Para configurar Carpetas de trabajo y así poder usar AD FS para la autenticación, sigue estos pasos:  
  
1.  Abre el **Administrador del servidor**.  
  
2.  Haz clic en **Servidores** y, a continuación, selecciona el servidor de Carpetas de trabajo de la lista.  
  
3.  Haz clic con el botón derecho en el nombre del servidor y haz clic en **Configuración de Carpetas de trabajo**.  
  
4.  En la ventana **Configuración de Carpetas de trabajo**, selecciona **Servicios de federación de Active Directory** y escribe la dirección URL del Servicio de federación. Haga clic en **Aplicar**.  
  
    En el ejemplo de prueba, la dirección URL es **https://blueadfs.contoso.com** .  
  
El cmdlet para lograr la misma tarea mediante Windows PowerShell es:  
  
```powershell  
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"   
```  
  
Si estás configurando AD FS con certificados autofirmados, puedes recibir un mensaje de error que indique que la dirección URL del Servicio de federación es incorrecta, inaccesible o que no se ha configurado una relación de confianza de usuario autenticado.  
  
Este error también puede ocurrir si el certificado de AD FS no se instaló en el servidor de Carpetas de trabajo o si el registro CNAME de AD FS no se ha configurado correctamente. Antes de continuar, asegúrate de resolver estos errores.  
  
### <a name="export-the-work-folders-certificate"></a>Exportar el certificado de Carpetas de trabajo  
Debes exportar el certificado autofirmado de Carpetas de trabajo, para que después puedas instalarlo en los siguientes equipos del entorno de prueba:  
  
-   El servidor que se usó en el Proxy de aplicación web  
  
-   El cliente de Windows unido a un dominio  
  
-   El cliente de Windows que no está unido a un dominio  
  
Para exportar el certificado, siga los mismos pasos utilizados para exportar el certificado de AD FS en versiones anteriores, como se describe en [implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 2, trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md), exporte el certificado de AD FS.  
  
Paso siguiente: [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 4: configurar el Proxy de aplicación Web](deploy-work-folders-adfs-step4.md)  
  
## <a name="see-also"></a>Vea también  
[Introducción a las carpetas de trabajo](Work-Folders-Overview.md)  
  

