---
description: 'Más información acerca de cómo implementar carpetas de trabajo con AD FS y el proxy de aplicación web: paso 3, configuración de carpetas de trabajo'
title: 'Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo'
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 5a43b104-4d02-4d73-a385-da1cfb67e341
ms.openlocfilehash: 2c85e0fc34fcbd735a79c8c768dbee70ff5e2e8d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049163"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-3-set-up-work-folders"></a>Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el tercer paso en la implementación de carpetas de trabajo con Servicios de federación de Active Directory (AD FS) (AD FS) y proxy de aplicación Web. Puede encontrar los demás pasos de este proceso en estos temas:

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: información general](deploy-work-folders-adfs-overview.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS](deploy-work-folders-adfs-step1.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4: configurar el proxy de aplicación Web](deploy-work-folders-adfs-step4.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes](deploy-work-folders-adfs-step5.md)

> [!NOTE]
>   Las instrucciones que se describen en esta sección son para un entorno de Windows Server 2019 o Windows Server 2016. Si usa Windows Server 2012 R2, siga las instrucciones de [Windows server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn747208(v=ws.11)).

Para configurar carpetas de trabajo, utilice los procedimientos siguientes.

## <a name="pre-installment-work"></a>\-Trabajo de preinstalación
Para instalar carpetas de trabajo, debe tener un servidor que esté unido al dominio y que ejecute Windows Server 2016. El servidor debe tener una configuración de red válida.

En el ejemplo de prueba, una el equipo que ejecutará las carpetas de trabajo en el dominio contoso y configurará la interfaz de red como se describe en las secciones siguientes.

### <a name="set-the-server-ip-address"></a>Establecer la dirección IP del servidor
Cambie la dirección IP del servidor a una dirección IP estática. En el ejemplo de prueba, use la clase IP A, que es 192.168.0.170/máscara de subred: 255.255.0.0/puerta de enlace predeterminada: 192.168.0.1/DNS preferido: 192.168.0.150 (la dirección IP del controlador de dominio).

### <a name="create-the-cname-record-for-work-folders"></a>Crear el registro CNAME para carpetas de trabajo
Para crear el registro CNAME para carpetas de trabajo, siga estos pasos:

1.  En el controlador de dominio, abra el **Administrador de DNS**.

2.  Expanda la carpeta zonas de búsqueda directa, haga clic con el botón secundario en el dominio y haga clic en **nuevo alias (CNAME)**.

3.  En la ventana **nuevo registro de recursos** , en el campo **nombre de alias** , escriba el alias de carpetas de trabajo. En el ejemplo de prueba, es **workfolders**.

4.  En el campo **nombre de dominio completo** , el valor debe ser **workfolders.contoso.com**.

5.  En el campo **nombre de dominio completo para host de destino** , escriba el FQDN del servidor de carpetas de trabajo. En el ejemplo de prueba, es **2016-WF.contoso.com**.

6.  Haga clic en **OK**.

Para realizar los pasos equivalentes a través de Windows PowerShell, use el siguiente comando. El comando debe ejecutarse en el controlador de dominio.

```powershell
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name workfolders -CName  -HostNameAlias 2016-wf.contoso.com
```

### <a name="install-the-ad-fs-certificate"></a>Instalación del certificado AD FS
Instale el certificado de AD FS que se creó durante la instalación de AD FS en el almacén de certificados del equipo local, siguiendo estos pasos:

1.  Haga clic en **Inicio** y, a continuación, haga clic en **Ejecutar**.

2.  Escriba **MMC**.

3.  En el menú **Archivo** , haga clic en **Agregar o quitar complemento**.

4.  En la lista **complementos disponibles** , seleccione **certificados** y, a continuación, haga clic en **Agregar**. Se inicia el Asistente para complementos de certificados.

5.  Seleccione **Cuenta de equipo** y, a continuación, haga clic en **Siguiente**.

6.  Seleccione **equipo local: (el equipo en el que se está ejecutando esta consola)** y, a continuación, haga clic en **Finalizar**.

7.  Haga clic en **OK**.

8.  Expanda la **consola de carpeta Root\Certificates \( equipo local) \personal\certificados**.

9. Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas** y haga clic en **importar**.

10. Vaya a la carpeta que contiene el certificado AD FS y siga las instrucciones del Asistente para importar el archivo y colocarlo en el almacén de certificados.

11. Expanda la **consola de carpeta Root\Certificates \( equipo local) \Trusted raíz confianza\certificados**.

12. Haga clic con el botón secundario en **certificados**, seleccione **todas las tareas** y haga clic en **importar**.

13. Vaya a la carpeta que contiene el certificado de AD FS y siga las instrucciones del Asistente para importar el archivo y colocarlo en el almacén de entidades de certificación raíz de confianza.

### <a name="create-the-work-folders-self-signed-certificate"></a>Crear el certificado autofirmado de carpetas de trabajo
Para crear el certificado autofirmado de carpetas de trabajo, siga estos pasos:

1.  Descargue los scripts proporcionados en la entrada de blog [implementación de carpetas de trabajo con AD FS y proxy de aplicación web](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap/ba-p/425318) y, a continuación, copie el archivo makecert.ps1 en el equipo carpetas de trabajo.

2.  Abra una ventana de Windows PowerShell con privilegios de administrador.

3.  Establezca la Directiva de ejecución en irrestricto:

    ```powershell
    PS C:\temp\scripts> Set-ExecutionPolicy -ExecutionPolicy Unrestricted
    ```

4.  Cambie al directorio donde copió el script.

5.  Ejecute el script makeCert:

    ```powershell
    PS C:\temp\scripts> .\makecert.ps1
    ```

6.  Cuando se le pida que cambie el certificado de asunto, escriba el nuevo valor para el asunto. En este ejemplo, el valor es **workfolders.contoso.com**.

7.  Cuando se le pida que escriba los nombres de nombre alternativo del firmante (SAN), presione Y y, a continuación, escriba los nombres de SAN, de uno en uno.

    En este ejemplo, escriba **workfolders.contoso.com** y presione Entrar. A continuación, escriba **2016-WF.contoso.com** y presione Entrar.

    Una vez especificados todos los nombres de SAN, presione Entrar en una línea vacía.

8.  Cuando se le pida que instale los certificados en el almacén de entidades de certificación raíz de confianza, presione Y.

El certificado de carpetas de trabajo debe ser un certificado SAN con los siguientes valores:

-   **workfolders**. **dominio** de

-   **nombre del equipo**. **dominio** de

En el ejemplo de prueba, los valores son:

-   **workfolders.contoso.com**

-   **2016-WF.contoso.com**

## <a name="install-work-folders"></a>Instalar carpetas de trabajo
Para instalar el rol carpetas de trabajo, siga estos pasos:

1.  Abra **Administrador del servidor**, haga clic en **Agregar roles y características** y, a continuación, haga clic en **siguiente**.

2.  En la página **tipo de instalación** , seleccione Instalación basada en **características o en roles** y haga clic en **siguiente**.

3.  En la página **selección de servidor** , seleccione el servidor actual y haga clic en **siguiente**.

4.  En la **Página roles de servidor** , expanda servicios de **archivos y almacenamiento**, expanda **servicios de archivos e iSCSI** y, a continuación, seleccione **carpetas de trabajo**.

5.  En la página **Asistente para agregar roles y características** , haga clic en **Agregar características** y, a continuación, en **siguiente**.

6.  En la página **características** , haga clic en **siguiente**.

7.  En la página **Confirmación**, haz clic en **Instalar**.

## <a name="configure-work-folders"></a>Configurar las carpetas de trabajo
Para configurar carpetas de trabajo, siga estos pasos:

1.  Abra el **Administrador del servidor**.

2.  Seleccione **servicios de archivos y almacenamiento** y, a continuación, seleccione **carpetas de trabajo**.

3.  En la página **carpetas de trabajo** , inicie el **Asistente para nuevo recurso compartido de sincronización** y haga clic en **siguiente**.

4.  En la página **servidor y ruta de acceso** , seleccione el servidor en el que se creará el recurso compartido de sincronización, escriba una ruta de acceso local donde se almacenarán los datos de carpetas de trabajo y haga clic en **siguiente**.

    Si la ruta de acceso no existe, se le pedirá que la cree. Haga clic en **OK**.

5.  En la página **estructura de carpetas de usuario** , seleccione alias de **usuario** y, a continuación, haga clic en **siguiente**.

6.  En la página **nombre del recurso compartido de sincronización** , escriba el nombre del recurso compartido de sincronización. En el ejemplo de prueba, es **WorkFolders**. Haga clic en **Next**.

7.  En la página **acceso de sincronización** , agregue los usuarios o grupos que tendrán acceso al nuevo recurso compartido de sincronización. En el ejemplo de prueba, conceda acceso a todos los usuarios del dominio. Haga clic en **Next**.

8.  En la página **directivas de seguridad de equipos** , seleccione **cifrar carpetas de trabajo** y **bloquear página automáticamente y requerir una contraseña**. Haga clic en **Next**.

9. En la página **confirmación** , haga clic en **crear** para finalizar el proceso de configuración.

## <a name="work-folders-post-configuration-work"></a>Trabajo posterior a la configuración de carpetas de trabajo
Para finalizar la configuración de carpetas de trabajo, siga estos pasos adicionales:

-   Enlazar el certificado de carpetas de trabajo al puerto SSL

-   Configurar carpetas de trabajo para usar la autenticación AD FS

-   Exportar el certificado de carpetas de trabajo (si usa un certificado autofirmado)

### <a name="bind-the-certificate"></a>Enlace del certificado
Carpetas de trabajo solo se comunica a través de SSL y debe tener el certificado autofirmado que ha creado anteriormente (o que la entidad de certificación ha emitido) enlazado al puerto.

Hay dos métodos que puede usar para enlazar el certificado al puerto a través de Windows PowerShell: cmdlets de IIS y netsh.

#### <a name="bind-the-certificate-by-using-netsh"></a>Enlazar el certificado mediante netsh
Para usar la utilidad de scripting de la línea de comandos Netsh en Windows PowerShell, debe canalizar el comando a netsh. El siguiente script de ejemplo busca el certificado con el asunto **workfolders.contoso.com** y lo enlaza al puerto 443 mediante netsh:

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

#### <a name="bind-the-certificate-by-using-iis-cmdlets"></a>Enlazar el certificado mediante cmdlets de IIS
También puede enlazar el certificado al puerto mediante cmdlets de administración de IIS, que están disponibles si instaló las herramientas y los scripts de administración de IIS.

> [!NOTE]
> La instalación de las herramientas de administración de IIS no habilita la versión completa de Internet Information Services (IIS) en el equipo carpetas de trabajo; solo habilita los cmdlets de administración. Hay algunas ventajas posibles para esta configuración. Por ejemplo, si está buscando cmdlets para proporcionar la funcionalidad que obtiene de Netsh. Cuando el certificado se enlaza al puerto a través del cmdlet New-WebBinding, el enlace no depende de IIS de ningún modo. Después de realizar el enlace, puede incluso quitar la característica web-MGMT-Console y el certificado se seguirá enlazando al puerto. Puede comprobar el enlace a través de Netsh escribiendo **netsh http show sslcert**.

En el ejemplo siguiente se usa el cmdlet New-WebBinding para buscar el certificado con el asunto **workfolders.contoso.com** y enlazarlo al puerto 443:

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
Para configurar carpetas de trabajo para utilizar AD FS para la autenticación, siga estos pasos:

1.  Abra el **Administrador del servidor**.

2.  Haga clic en **servidores** y, a continuación, seleccione el servidor de carpetas de trabajo en la lista.

3.  Haga clic con el botón secundario en el nombre del servidor y haga clic en **configuración de carpetas de trabajo**.

4.  En la ventana Configuración de la **carpeta de trabajo** , seleccione **servicios de Federación de Active Directory (AD FS)** y escriba la dirección URL del servicio de Federación. Haga clic en **Aplicar**.

    En el ejemplo de prueba, la dirección URL es **https://blueadfs.contoso.com** .

El cmdlet para realizar la misma tarea a través de Windows PowerShell es:

```powershell
Set-SyncServerSetting -ADFSUrl "https://blueadfs.contoso.com"
```

Si está configurando AD FS con certificados autofirmados, es posible que reciba un mensaje de error que indica que la dirección URL de Servicio de federación es incorrecta, no se puede tener acceso a ella o no se ha configurado una relación de confianza para usuario autenticado.

Este error también puede producirse si el certificado de AD FS no se ha instalado en el servidor de carpetas de trabajo o si el CNAME de AD FS no se ha configurado correctamente. Debe corregir estos problemas antes de continuar.

### <a name="export-the-work-folders-certificate"></a>Exportar el certificado de carpetas de trabajo
El certificado autofirmado de carpetas de trabajo debe exportarse para poder instalarlo posteriormente en las siguientes máquinas del entorno de prueba:

-   El servidor que se usa para el proxy de aplicación Web

-   Cliente de Windows unido a un dominio

-   Cliente de Windows no unido a un dominio

Para exportar el certificado, siga los mismos pasos que usó para exportar el certificado de AD FS anterior, como se describe en [implementación de carpetas de trabajo con AD FS y proxy de aplicación web: paso 2, AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md), exporte el certificado de AD FS.

Paso siguiente: [implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4, configurar el proxy de aplicación web](deploy-work-folders-adfs-step4.md)

## <a name="see-also"></a>Consulte también
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)

