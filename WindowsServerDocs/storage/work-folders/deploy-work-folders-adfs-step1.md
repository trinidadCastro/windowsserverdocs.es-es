---
title: "Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 1; configurar AD FS"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 4a8a044ad6a8ec5275f5be4b949a2ab58d16da61
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: Paso 1; configurar AD FS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el primer paso para implementar Carpetas de trabajo con los Servicios de federación de Active Directory (AD FS) y el Proxy de aplicación web. Puedes encontrar el resto de pasos de este proceso en estos temas:  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web (WAP): información general](deploy-work-folders-adfs-overview.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 2; trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 3; configurar Carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 4; configurar el Proxy de aplicación web](deploy-work-folders-adfs-step4.md)  
  
-   [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: Paso 5; configurar clientes](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Las instrucciones incluidas en esta sección toman como referencia un entorno de Server 2016. Si estás usando Windows Server 2012 R2, lee el artículo en el que se detallan las [instrucciones de Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Para configurar AD FS y poder usarlo con Carpetas de trabajo, emplea los siguientes procedimientos.  
  
## <a name="pre-installment-work"></a>Trabajo previo a la instalación  
Si quieres convertir el entorno de prueba que vas a configurar mediante estas instrucciones en un entorno de producción, hay dos cosas que debes hacer antes de empezar:  
  
-   Configura una cuenta de administrador de dominio de Active Directory y úsala para ejecutar el servicio de AD FS.
  
-   Obtén un certificado del nombre alternativo del firmante (SAN) y una capa de sockets seguros (SSL) para realizar la autenticación de servidor. En el ejemplo de prueba deberás usar un certificado autofirmado, pero recuerda que para la producción debes usar un certificado de confianza pública.  
  
Dependiendo de las directivas de tu empresa, es posible que te cueste un poco obtener estos elementos, por lo que te recomendamos que comiences con el proceso de solicitud de esos elementos antes de empezar a crear el entorno de prueba.  
  
Existen muchas entidades de certificación comercial (CAs) en las cuales puedes comprar el certificado. Encontrarás una lista de las entidades de certificación de confianza de Microsoft en el [artículo de Knowledge Base 931125](http://support.microsoft.com/kb/931125). Como alternativa, puedes obtener un certificado de la misma entidad de certificación de tu empresa.  
  
En cuanto al entorno de prueba, deberás usar un certificado autofirmado que haya creado uno de los scripts proporcionados.  
  
> [!NOTE]  
> AD FS no admite certificados Cryptography Next Generation (CNG), lo que significa que no se puede crear un certificado firmado mediante el cmdlet de Windows PowerShell New-SelfSignedCertificate. Sin embargo, puedes usar el script makecert.ps1 que se incluye en la entrada de blog [Deploying Work Folders with AD FS and Web Application Proxy](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap) (Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web [WAP]). Este script crea un certificado autofirmado que funciona con AD FS y que solicita los nombres SAN que serán necesarios para crear el certificado.  
  
A continuación, debes realizar el trabajo previo a la instalación tal como se describe en las siguientes secciones.  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>Crear un certificado autofirmado de AD FS  
Para crear un certificado autofirmado de AD FS, sigue estos pasos:  
  
1.  Descarga los scripts que se proporcionan en el artículo de blog [Implementar Carpetas de trabajo con AD FS y Proxy de aplicación web (WAP)](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap) y copia el archivo makecert.ps1 en el equipo de AD FS.  
  
2.  Abre una ventana de Windows PowerShell con privilegios de administrador.  
  
3.  Establece la directiva de ejecución en la opción "sin restricciones":  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1 C:\temp\scripts> Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  Accede al directorio donde has copiado el script.  
  
5.  Ejecuta el script makecert:  
  
    ```powershell  
    PS C:\temp\scripts> .\makecert.ps1  
    ```  
  
6.  Cuando tengas que cambiar el certificado de firmante, escribe el nuevo valor del mismo. En este ejemplo, el valor es **blueadfs.contoso.com**.  
  
7.  Cuando debas escribir los nombres SAN, presiona Y y, a continuación, escribe los nombres SAN, uno por uno.  
  
    En este ejemplo debes escribir **blueadfs.contoso.com** y presionar Entrar; a continuación, escribe **2016-adfs.contoso.com** y presiona Entrar y, por último, escribe **enterpriseregistration.contoso.com** y presiona Entrar.  
  
    Cuando hayas introducido todos los nombres SAN, presiona Entrar en una línea vacía.  
  
8.  En el momento en que tengas que instalar los certificados en el almacén de la entidad de certificación raíz de confianza, presiona Y.  
  
El certificado de AD FS debe ser un certificado SAN que tenga los siguientes valores:  

-   nombre de servicio de AD FS.dominio

-   enterpriseregistration.dominio

-   nombre de servidor de AD FS.dominio

En el ejemplo de prueba, los valores son:  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
El valor SAN enterpriseregistration es necesario para Workplace Join.  
  
### <a name="set-the-server-ip-address"></a>Establecer la dirección IP del servidor  
Cambia la dirección IP del servidor a una dirección IP estática. En el ejemplo prueba, usa la clase IP A, que es 192.168.0.160 / máscara de subred: 255.255.0.0 / puerta de enlace predeterminada: 192.168.0.1 / DNS preferido: 192.168.0.150 (es la dirección IP del controlador de dominio\).  
  
## <a name="install-the-ad-fs-role-service"></a>Instalar el servicio de rol de AD FS  
Sigue estos pasos para instalar AD FS:  
  
1.  Inicia sesión en la máquina virtual o física en la que vayas a instalar AD FS y abre el **Administrador del servidor** para iniciar el asistente para agregar roles y características.  
  
2.  En la página **Roles de servidor**, selecciona el rol **Servicios de federación de Active Directory** y, a continuación, haz clic en **Siguiente**.  
  
3.  En la página **Servicios de federación de Active Directory (AD FS)**, verás un mensaje que indica que no se puede instalar el rol del Proxy de aplicación web en el mismo equipo que AD FS. Haz clic en **Siguiente**.  
  
4.  En la página de confirmación, haz clic en **Instalar**.  
  
Para realizar la misma instalación de AD FS mediante Windows PowerShell, usa estos comandos:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature AD FS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>Configurar AD FS  
A continuación, configura AD FS mediante el Administrador del servidor o Windows PowerShell.  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>Configurar AD FS mediante el Administrador del servidor  
Para configurar AD FS mediante el Administrador del servidor, sigue estos pasos:  
  
1.  Abre el Administrador del servidor.  
  
2.  En la parte superior de la ventana del Administrador del servidor, haz clic en la marca **Notificaciones** y, a continuación, en **Configurar el Servicio de federación en el servidor**.  
  
3.  Se iniciará el Asistente para la configuración de Servicios de federación de Active Directory. En la página **Conectar AD DS**, escribe la cuenta de administrador de dominio que quieres usar como cuenta de AD FS y haz clic en **Siguiente**.  
  
4.  En la página **Especificar propiedades del servicio**, escribe el nombre del firmante del certificado SSL que se usará para la comunicación de AD FS. En este ejemplo de prueba, el valor es **blueadfs.contoso.com**.  
  
5.  Escribe el nombre del Servicio de federación. En este ejemplo de prueba, el valor es **blueadfs.contoso.com**. Haz clic en **Siguiente**.  
  
    > [!NOTE]  
    > Recuerda que el nombre del Servicio de federación no debe usar el nombre de un servidor que ya exista en el entorno. Si usas el nombre de un servidor existente, se producirá un error en la instalación de AD FS y tendrás que comenzar de nuevo.  
  
6.  En la página **Especificar cuenta de servicio**, escribe el nombre que quieres usar para la cuenta de servicio administrada. Para el ejemplo de prueba, selecciona **Crear una cuenta de servicio administrada de grupo**y en **Nombre de cuenta**, escribe **ADFSService**. Haz clic en **Siguiente**.  
  
7.  En la página **Especificar base de datos de configuración**, selecciona **Crear una base de datos en este servidor mediante Windows Internal Database** y haz clic en **Siguiente**.  
  
8.  La página **Opciones de revisión** te muestra una descripción general de las opciones que hayas seleccionado. Haz clic en **Siguiente**.  
  
9. La página **Comprobaciones de requisitos previos** te indica si todas las comprobaciones de requisitos previos realizadas fueron satisfactorias. Si no hay ningún problema, haz clic en **Configurar**.  
  
    > [!NOTE]  
    > Si usaste el nombre del servidor de AD FS o de cualquier otra máquina existente en el nombre del servicio de federación, se mostrará un mensaje de error. Debes comenzar la instalación de nuevo y elegir un nombre que no coincida con el nombre de la máquina.  
  
10. Una vez completada la configuración, la página **Resultados** confirmará que AD FS se configuró correctamente.  
  
### <a name="configure-ad-fs-by-using-powershell"></a>Configurar AD FS mediante PowerShell  
Para realizar la misma configuración de AD FS mediante Windows PowerShell, usa los siguientes comandos.  
  
Para instalar AD FS:  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
Para crear una nueva cuenta de servicio administrada:  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
Después de configurar AD FS, debes configurar una granja de servidores de AD FS mediante la cuenta de servicio administrada que creaste en el paso anterior y el certificado que creaste en los pasos de preconfiguración.  
  
Para configurar una granja de servidores de AD FS:  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
Siguiente paso: [Implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web: paso 2; trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md)  
  
## <a name="see-also"></a>Consulta también  
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)  
  

