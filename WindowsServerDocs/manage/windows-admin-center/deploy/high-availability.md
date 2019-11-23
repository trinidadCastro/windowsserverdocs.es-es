---
title: Implementación del centro de administración de Windows con alta disponibilidad
description: Implementación del centro de administración de Windows con alta disponibilidad (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406948"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Implementación del centro de administración de Windows con alta disponibilidad

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Puede implementar el centro de administración de Windows en un clúster de conmutación por error para proporcionar alta disponibilidad para el servicio de puerta de enlace del centro de administración de Windows. La solución proporcionada es una solución activa-pasiva, donde solo hay una instancia del centro de administración de Windows activa. Si se produce un error en uno de los nodos del clúster, el centro de administración de Windows conmuta por error correctamente a otro nodo, lo que le permite seguir administrando los servidores de su entorno sin problemas. 

[Obtenga información acerca de otras opciones de implementación del centro de administración de Windows.](../plan/installation-options.md)

## <a name="prerequisites"></a>Requisitos previos

- Un clúster de conmutación por error de 2 o más nodos en Windows Server 2016 o 2019. [Más información sobre la implementación de un clúster de conmutación por error](../../../failover-clustering/failover-clustering-overview.md).
- Un volumen compartido de clúster (CSV) para que el centro de administración de Windows almacene los datos persistentes a los que pueden tener acceso todos los nodos del clúster. 10 GB serán suficientes para su CSV.
- Script de implementación de alta disponibilidad del [archivo zip del script de alta](https://aka.ms/WACHAScript)disponibilidad del centro de administración de Windows. Descargue el archivo. zip que contiene el script en la máquina local y, a continuación, copie el script según sea necesario en función de las instrucciones que se indican a continuación.
- Se recomienda, pero opcional: un certificado firmado. pfx & contraseña. No es necesario que ya tenga instalado el certificado en los nodos del clúster: el script lo hará automáticamente. Si no proporciona uno, el script de instalación genera un certificado autofirmado, que expira después de 60 días.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Instalar el centro de administración de Windows en un clúster de conmutación por error

1. Copie el script de ```Install-WindowsAdminCenterHA.ps1``` en un nodo del clúster. Descargue o copie el archivo. msi del centro de administración de Windows en el mismo nodo.
2. Conéctese al nodo a través de RDP y ejecute el script de ```Install-WindowsAdminCenterHA.ps1``` desde ese nodo con los parámetros siguientes:
    - `-clusterStorage`: ruta de acceso local del Volumen compartido de clúster para almacenar los datos del centro de administración de Windows.
    - `-clientAccessPoint`: elija un nombre que usará para tener acceso al centro de administración de Windows. Por ejemplo, si ejecuta el script con el parámetro `-clientAccessPoint contosoWindowsAdminCenter`, tendrá acceso al servicio centro de administración de Windows visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: opcional. Una o varias direcciones estáticas para el servicio genérico de clúster. 
    - `-msiPath`: la ruta de acceso del archivo. msi del centro de administración de Windows.
    - `-certPath`: opcional. La ruta de acceso de un archivo. pfx de certificado.
    - `-certPassword`: opcional. Una contraseña SecureString para el archivo Certificate. pfx proporcionado en `-certPath`
    - `-generateSslCert`: opcional. Si no desea proporcionar un certificado firmado, incluya esta marca de parámetro para generar un certificado autofirmado. Tenga en cuenta que el certificado autofirmado expirará en 60 días.
    - `-portNumber`: opcional. Si no especifica un puerto, el servicio de puerta de enlace se implementa en el puerto 443 (HTTPS). Para usar un puerto diferente, especifique en este parámetro. Tenga en cuenta que si usa un puerto personalizado (algo más que 443), tendrá acceso al centro de administración de Windows yendo a https://\<clientAccessPoint\>:\<\>de puertos.

> [!NOTE]
> El script de ```Install-WindowsAdminCenterHA.ps1``` admite los parámetros ```-WhatIf ``` y ```-Verbose```

### <a name="examples"></a>Ejemplos

#### <a name="install-with-a-signed-certificate"></a>Instalar con un certificado firmado:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Instalación con un certificado autofirmado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Actualizar una instalación de alta disponibilidad existente

Use el mismo script ```Install-WindowsAdminCenterHA.ps1``` para actualizar la implementación de alta disponibilidad sin perder los datos de conexión.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Actualizar a una nueva versión del centro de administración de Windows

Cuando se lance una nueva versión del centro de administración de Windows, simplemente vuelva a ejecutar el script ```Install-WindowsAdminCenterHA.ps1``` con el parámetro ```msiPath```:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Actualizar el certificado usado por el centro de administración de Windows

Puede actualizar el certificado usado por una implementación de alta disponibilidad del centro de administración de Windows en cualquier momento. para ello, proporcione el archivo. pfx y la contraseña del nuevo certificado.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

También puede actualizar el certificado al mismo tiempo que actualiza la plataforma del centro de administración de Windows con un nuevo archivo. msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Desinstalar

Para desinstalar la implementación de alta disponibilidad del centro de administración de Windows desde el clúster de conmutación por error, pase el parámetro ```-Uninstall``` al script de ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Solución de problemas

Los registros se guardan en la carpeta temporal del CSV (por ejemplo, C:\ClusterStorage\Volume1\temp).