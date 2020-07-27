---
title: Cargar una imagen especializada de Windows Server 2008/2008 R2 en Azure
description: Windows Server 2008 y 2008 R2 están acercándose al final de servicio. Aprende a subir y moverte a Azure, alojando Windows Server en la nube.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/11/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: f8f8cc63740c6823a01e0c86b387d57b54a06419
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960297"
---
# <a name="upload-a-windows-server-20082008-r2-specialized-image-to-azure"></a>Cargar una imagen especializada de Windows Server 2008/2008 R2 en Azure 

![Banner presentando un tema de imagen de WS08](media/WS08-image-banner-large.png)

Ahora puedes ejecutar máquinas VM de Windows Server 2008/2008 R2 en la nube con Azure. 

## <a name="prep-the-windows-server-20082008-r2-specialized-image"></a>Preparar la imagen especializada de Windows Server 2008/2008 R2
Antes de cargar imágenes, realiza los cambios siguientes:

- Descarga e instala Windows Server 2008 Service Pack 2 (SP2) si todavía no lo tienes instalado en tu imagen.

- Configuración de Escritorio remoto (RDP).
  1. Ve a **Panel de control** > **Configuración del sistema**.   
  2. Selecciona **Configuración remota** en la barra de menús de la izquierda.

     ![Captura de pantalla de la configuración del sistema, destacando "Configuración remota".](media/1a_remote_settings.png)

  3. Selecciona la pestaña **Remoto** en Propiedades del sistema.   

     ![Captura de pantalla de la pestaña Remoto en las propiedades del sistema.](media/2c_sysprops.png)

  4. Selecciona Permitir las conexiones desde equipos que ejecutan cualquier versión de Escritorio remoto (menos seguro).   
  5. Haga clic en **Aplicar** y en **Aceptar**.
- Configurar Firewall de Windows.   
   1. En el símbolo del sistema en modo Administrador, introduce "**wf.msc**" para ir a la configuración de Firewall de Windows y de seguridad avanzada.   
   2. Ordena los resultados por **Puertos** selecciona **puerto 3389**.   
     ![Captura de pantalla de reglas de entrada de la configuración del Firewall de Windows.](media/3b_inboundrules.png)   
   3. Habilitar Escritorio remoto (TCP-IN) para los perfiles: **Dominio**, **Privado** y **Público** (como se muestra arriba).

- Guarda toda la configuración y cierra la imagen.   
- Si estás usando Hyper-V, asegúrate que el AVHD secundario esté combinado en el VHD para cambios persistentes.

Un error actual conocido provoca que la contraseña de administrador en la imagen cargada expire en 24 horas. Para evitar este problema, sigue estos pasos: 

1. Ve a **Inicio** > **Ejecutar**
2. Escribe **lusrmgr.msc**
3. Selecciona **Usuarios** en Usuarios y grupos locales
4. Haz clic con el botón derecho en **Administrador** y selecciona **Propiedades**.
5. Selecciona **la contraseña nunca expira** y después **Aceptar**
![Captura de pantalla de propiedades de administrador.](media/6_adminprops.png)

## <a name="uploading-the-image-vhd"></a>Carga del VHD de la imagen
Puedes usar el script posterior para cargar el VHD. Para poder hacerlo, necesitarás el archivo de configuración de publicación de tu cuenta de Azure. Obtén tu [archivo de configuración de Azure](https://azure.microsoft.com/downloads/).

Este es el script:

```powershell
Get-AzurePublishSettingsFile 

Login-AzureRmAccount
 
      # Import publishsettings
      Import-AzurePublishSettingsFile -PublishSettingsFile <LocationOfPublishingFile>
      $subscriptionId = 'xxxx-xxxx-xxxx-xxxx-xxxxx'
 
      # Set NodeFlight subscription as default subscription
      Select-AzureRmSubscription -SubscriptionId $subscriptionId
      Set-AzureRmContext -SubscriptionId $subscriptionId
      $rgName = "<resourcegroupname>"
    
      $urlOfUploadedImageVhd = "<BlobUrl>/<NameForVHD>.vhd"
      Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "<FilePath>"  
```
## <a name="deploy-the-image-in-azure"></a>Implementa la imagen en Azure
En esta sección, implementarás la imagen VHD in Azure. 

> [!IMPORTANT]
> No uses imágenes de usuario predefinidas en Azure.

1.    Crear un nuevo [grupo de recursos](/rest/api/resources/resourcegroups/createorupdate). 
2.    Crea un nuevo [blob de almacenamiento](/rest/api/storageservices/put-blob) dentro del grupo de recursos.
3.    Crea un [contenedor](/rest/api/storageservices/create-container) dentro del blob de almacenamiento.
4.    Copia el URL del blob de almacenamiento desde las propiedades.
5.    Usa el script facilitado anteriormente para cargar tu imagen al nuevo blob de almacenamiento.
6.    Crea un [disco](/azure/virtual-machines/windows/prepare-for-upload-vhd-image) para el VHD.   
     a.    Ve a Discos y haz clic en **Agregar**.  
     b.    Introduce un nombre para el disco. Selecciona la suscripción que quieres usar, configura la región y elige el tipo de cuenta.   
     c. En Tipo de fuente, selecciona Almacenamiento. Explora la ubicación del VHD del blob creado usando el script.  
     d. Selecciona el sistema operativo de tipo Windows y Tamaño (predeterminado: 1023).   
     e. Haga clic en **Crear**.   

7.    Ve al disco creado y haz clic en **Crear VM**.   
     a.    Nombra la VM.   
     b.    Selecciona el grupo existente que creaste en el paso 5, al cargar el disco.   
     c.    Elige un tamaño y un plan de SKU para la VM.   
     d.    Selecciona una interfaz de red en la página de configuración. Asegúrate de que la interfaz de red tenga especificada la regla siguiente:
 
        PUERTO: 3389 Protocolo: TCP Acción: Permitir Prioridad: 1000 Nombre: "Regla-RDP".   
     e.    Haga clic en **Crear**.
