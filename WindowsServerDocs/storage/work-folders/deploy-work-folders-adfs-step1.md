---
description: 'Más información acerca de cómo implementar carpetas de trabajo con AD FS y el proxy de aplicación web: paso 1, configurar AD FS'
title: 'Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS'
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 10/18/2018
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 0cb9e88455a6c7c1b3d917a8ad839d7f7217a6a3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048583"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe el primer paso para implementar carpetas de trabajo con Servicios de federación de Active Directory (AD FS) (AD FS) y proxy de aplicación Web. Puede encontrar los demás pasos de este proceso en estos temas:

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: información general](deploy-work-folders-adfs-overview.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4: configurar el proxy de aplicación Web](deploy-work-folders-adfs-step4.md)

-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes](deploy-work-folders-adfs-step5.md)

> [!NOTE]
>   Las instrucciones que se describen en esta sección son para un entorno de Windows Server 2019 o Windows Server 2016. Si usa Windows Server 2012 R2, siga las instrucciones de [Windows server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn747208(v=ws.11)).

Para configurar AD FS para su uso con carpetas de trabajo, utilice los procedimientos siguientes.

## <a name="pre-installment-work"></a>\-Trabajo de preinstalación
Si tiene previsto convertir el entorno de prueba que está configurando con estas instrucciones en producción, hay dos cosas que podría querer hacer antes de empezar:

-   Configure un Active Directory cuenta de administrador de dominio que se usará para ejecutar el servicio AD FS.

-   Obtener un certificado de nombre alternativo del firmante (SAN) de Capa de sockets seguros (SSL) para la autenticación del servidor. En el ejemplo de prueba, utilizará un certificado autofirmado, pero para producción debe usar un certificado de confianza pública.

La obtención de estos elementos puede tardar algún tiempo, en función de las directivas de la empresa, por lo que puede ser beneficioso iniciar el proceso de solicitud de los elementos antes de empezar a crear el entorno de prueba.

Hay muchas entidades de certificación (CA) comerciales desde las que puede comprar el certificado. Puede encontrar una lista de las CA de confianza de Microsoft en el [artículo de KB 931125](https://support.microsoft.com/kb/931125). Otra alternativa es obtener un certificado de la CA de empresa de la empresa.

Para el entorno de prueba, utilizará un certificado autofirmado creado por uno de los scripts proporcionados.

> [!NOTE]
> AD FS no admite certificados CNG (Cryptography Next Generation), lo que significa que no se puede crear el certificado autofirmado mediante el cmdlet de Windows PowerShell New-SelfSignedCertificate. Sin embargo, puede usar el script de makecert.ps1 incluido en la entrada de blog [implementación de carpetas de trabajo con AD FS y proxy de aplicación web](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap/ba-p/425318) . Este script crea un \- certificado autofirmado que funciona con AD FS y solicita los nombres de San necesarios para crear el certificado.

Después, realice el trabajo de preinstalación adicional descrito en las secciones siguientes.

### <a name="create-an-ad-fs-self-signed-certificate"></a>Creación de un \- certificado autofirmado de AD FS
Para crear un certificado autofirmado AD FS, siga estos pasos:

1.  Descargue los scripts proporcionados en la entrada de blog [implementación de carpetas de trabajo con AD FS y proxy de aplicación web](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap/ba-p/425318) y, a continuación, copie el archivo makecert.ps1 en el equipo AD FS.

2.  Abra una ventana de Windows PowerShell con privilegios de administrador.

3.  Establezca la Directiva de ejecución en irrestricto:

    ```powershell
    Set-ExecutionPolicy –ExecutionPolicy Unrestricted
    ```

4.  Cambie al directorio donde copió el script.

5.  Ejecute el script Makecert:

    ```powershell
    .\makecert.ps1
    ```

6.  Cuando se le pida que cambie el certificado de asunto, escriba el nuevo valor para el asunto. En este ejemplo, el valor es **blueadfs.contoso.com**.

7.  Cuando se le pida que escriba los nombres de SAN, presione Y y, a continuación, escriba los nombres de SAN, de uno en uno.

    En este ejemplo, escriba **blueadfs.contoso.com** y presione entrar; a continuación, escriba **2016-ADFS.contoso.com** , presione entrar, escriba **enterpriseregistration.contoso.com** y presione Entrar.

    Una vez especificados todos los nombres de SAN, presione Entrar en una línea vacía.

8.  Cuando se le pida que instale los certificados en el almacén de entidades de certificación raíz de confianza, presione Y.

El certificado de AD FS debe ser un certificado SAN con los siguientes valores:

-   Nombre del servicio de AD FS. dominio

-   enterpriseregistration. dominio

-   Nombre del servidor de AD FS. dominio

En el ejemplo de prueba, los valores son:

-   **blueadfs.contoso.com**

-   **enterpriseregistration.contoso.com**

-   **2016-adfs.contoso.com**

La SAN enterpriseregistration es necesaria para Workplace Join.

### <a name="set-the-server-ip-address"></a>Establecer la dirección IP del servidor
Cambie la dirección IP del servidor a una dirección IP estática. En el ejemplo de prueba, use la clase IP A, que es 192.168.0.160/máscara de subred: 255.255.0.0/puerta de enlace predeterminada: 192.168.0.1/DNS preferido: 192.168.0.150 (la dirección IP del controlador de dominio \) .

## <a name="install-the-ad-fs-role-service"></a>Instalación del servicio de rol de AD FS
Para instalar AD FS, siga estos pasos:

1.  Inicie sesión en la máquina física o virtual en la que va a instalar AD FS, Abra **Administrador del servidor** e inicie el Asistente para agregar roles y características.

2.  En la página **roles de servidor** , seleccione el rol de **servicios de Federación de Active Directory (AD FS)** y, a continuación, haga clic en **siguiente**.

3.  En la página **servicios de Federación de Active Directory (AD FS) (AD FS)** , verá un mensaje que indica que el rol proxy de aplicación web no se puede instalar en el mismo equipo que AD FS. Haga clic en **Next**.

4.  Haga clic en **instalar** en la página confirmación.

Para realizar la instalación equivalente de AD FS a través de Windows PowerShell, use estos comandos:

```powershell
Add-WindowsFeature RSAT-AD-Tools
Add-WindowsFeature ADFS-Federation –IncludeManagementTools
```

## <a name="configure-ad-fs"></a>Configurar AD FS
A continuación, configure AD FS mediante Administrador del servidor o Windows PowerShell.

### <a name="configure-ad-fs-by-using-server-manager"></a>Configurar AD FS mediante Administrador del servidor
Para configurar AD FS mediante Administrador del servidor, siga estos pasos:

1.  Abra el Administrador del servidor.

2.  Haga clic en la marca **notificaciones** en la parte superior de la ventana de administrador del servidor y, a continuación, haga clic en **configurar el servicio de Federación en este servidor**.

3.  Se inicia el Asistente para configuración de Servicios de federación de Active Directory (AD FS). En la página **conectar a AD DS** , escriba la cuenta de administrador de dominio que desea usar como cuenta de AD FS y haga clic en **siguiente**.

4.  En la página **especificar propiedades del servicio** , escriba el nombre de sujeto del certificado SSL que se usará para la comunicación de AD FS. En el ejemplo de prueba, es **blueadfs.contoso.com**.

5.  Escriba el nombre del Servicio de federación. En el ejemplo de prueba, es **blueadfs.contoso.com**. Haga clic en **Next**.

    > [!NOTE]
    > El nombre del Servicio de federación no debe usar el nombre de un servidor existente en el entorno. Si usa el nombre de un servidor existente, se producirá un error en la instalación de AD FS y debe reiniciarse.

6.  En la página **especificar cuenta de servicio** , escriba el nombre que desea usar para la cuenta de servicio administrada. En el ejemplo de prueba, seleccione **crear una cuenta de servicio administrada de grupo** y, en **nombre de cuenta**, escriba **ADFSService**. Haga clic en **Next**.

7.  En la página **especificar base de datos de configuración** , seleccione **crear una base de datos en este servidor con Windows Internal Database** y haga clic en **siguiente**.

8.  La página **revisar opciones** muestra una visión general de las opciones que ha seleccionado. Haga clic en **Next**.

9. La página **comprobaciones de requisitos previos** indica si se han superado correctamente todas las comprobaciones de requisitos previos. Si no hay ningún problema, haga clic en **configurar**.

    > [!NOTE]
    > Si usó el nombre del servidor de AD FS o cualquier otro equipo existente para el nombre del Servicio de federación, se muestra un mensaje de error. Debe iniciar la instalación de y elegir un nombre distinto del nombre de un equipo existente.

10. Cuando la configuración se completa correctamente, la página **resultados** confirma que AD FS se configuró correctamente.

### <a name="configure-ad-fs-by-using-powershell"></a>Configuración de AD FS mediante PowerShell
Para realizar la configuración equivalente de AD FS a través de Windows PowerShell, use los siguientes comandos.

Para instalar AD FS:

```powershell
Add-WindowsFeature RSAT-AD-Tools
Add-WindowsFeature ADFS-Federation -IncludeManagementTools
```

Para crear la cuenta de servicio administrada:

```powershell
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com
```

Después de configurar AD FS, debe configurar una granja de AD FS con la cuenta de servicio administrada que creó en el paso anterior y el certificado que creó en los pasos previos a la configuración.

Para configurar una granja de AD FS:

```powershell
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1 
$thumbprint = $cert.Thumbprint
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop
```

Paso siguiente: [implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md)

## <a name="see-also"></a>Consulte también
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)

