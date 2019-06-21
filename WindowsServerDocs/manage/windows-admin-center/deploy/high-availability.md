---
title: Implementar Windows Admin Center con alta disponibilidad
description: Implementar Windows Admin Center con alta disponibilidad (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ad8e2a8eade1ea9d3faaba8f387b1f489854e589
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280631"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Implementar Windows Admin Center con alta disponibilidad

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Puede implementar Windows Admin Center en un clúster de conmutación por error para proporcionar alta disponibilidad para el servicio de puerta de enlace de Windows Admin Center. La solución proporcionada es una solución activo / pasivo, donde solo una instancia de Windows Admin Center está activa. Si se produce un error en uno de los nodos del clúster, Windows Admin Center correctamente conmuta por error a otro nodo, lo que le permite seguir administrando los servidores en su entorno sin problemas. 

[Obtenga información sobre otras opciones de implementación de Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Requisitos previos

- Un clúster de conmutación por error de 2 o más nodos en Windows Server 2016 o de 2019. [Más información sobre cómo implementar un clúster de conmutación por error](../../../failover-clustering/failover-clustering-overview.md).
- Un clúster compartido (CSV) de volumen para Windows Admin Center almacenar datos persistentes que pueden tener acceso a todos los nodos del clúster. 10 GB será suficiente para el archivo CSV.
- Script de implementación de alta disponibilidad [archivo zip de alta disponibilidad de Script de Windows Admin Center](https://aka.ms/WACHAScript). Descargue el archivo .zip que contiene el script en el equipo local y, a continuación, copie el script según sea necesario según las instrucciones siguientes.
- Opcional pero recomendable: un certificado firmado .pfx & contraseña. No es necesario ya se ha instalado el certificado en los nodos del clúster: el script hará eso por usted. Si no especifica ninguno, el script de instalación genera un certificado autofirmado, que expira después de 60 días.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Instalar Windows Admin Center en un clúster de conmutación por error

1. Copia el ```Install-WindowsAdminCenterHA.ps1``` script a un nodo del clúster. Descargue o copie el archivo .msi de Windows Admin Center al mismo nodo.
2. Conéctese al nodo a través de RDP y ejecute el ```Install-WindowsAdminCenterHA.ps1``` secuencia de comandos de ese nodo con los siguientes parámetros:
    - `-clusterStorage`: la ruta de acceso local del volumen compartido en clúster para almacenar los datos de Windows Admin Center.
    - `-clientAccessPoint`: elija un nombre que va a utilizar para tener acceso a Windows Admin Center. Por ejemplo, si ejecuta la secuencia de comandos con el parámetro `-clientAccessPoint contosoWindowsAdminCenter`, tendrá acceso al servicio de Windows Admin Center, visite `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Opcional. Uno o más direcciones estáticas para el servicio de clúster genérico. 
    - `-msiPath`: La ruta de acceso del archivo .msi de Windows Admin Center.
    - `-certPath`: Opcional. La ruta de acceso para un archivo .pfx del certificado.
    - `-certPassword`: Opcional. Contraseña para el archivo .pfx de certificado proporcionado en SecureString `-certPath`
    - `-generateSslCert`: Opcional. Si no desea proporcionar un certificado autofirmado, incluya este marcador de parámetro para generar un certificado autofirmado. Tenga en cuenta que el certificado autofirmado expirará en 60 días.
    - `-portNumber`: Opcional. Si no especifica un puerto, el servicio de puerta de enlace se implementa en el puerto 443 (HTTPS). Para utilizar un puerto diferente se especifique en este parámetro. Tenga en cuenta que si usa un puerto personalizado (nada además de 443), tendrá acceso a la de Windows Admin Center, vaya a https://\<clientAccessPoint\>:\<puerto\>.

> [!NOTE]
> El ```Install-WindowsAdminCenterHA.ps1``` script admite ```-WhatIf ``` y ```-Verbose``` parámetros

### <a name="examples"></a>Ejemplos

#### <a name="install-with-a-signed-certificate"></a>Instalar con un certificado de firma:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Instalar con un certificado autofirmado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Actualizar una instalación existente de alta disponibilidad

Usar el mismo ```Install-WindowsAdminCenterHA.ps1``` script para actualizar la implementación de alta disponibilidad, sin perder los datos de conexión.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Actualizar a una nueva versión de Windows Admin Center

Cuando se lanza una nueva versión de Windows Admin Center, simplemente ejecute el ```Install-WindowsAdminCenterHA.ps1``` script de nuevo con solo el ```msiPath``` parámetro:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Actualizar el certificado utilizado por Windows Admin Center

Puede actualizar el certificado utilizado por una implementación de alta disponibilidad de Windows Admin Center en cualquier momento, ya que proporciona el archivo .pfx del certificado nuevo y la contraseña.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

También puede actualizar el certificado a la vez que actualice la plataforma Windows Admin Center con un nuevo archivo .msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Desinstalar

Para desinstalar la implementación de alta disponibilidad de Windows Admin Center desde el clúster de conmutación por error, pasar la ```-Uninstall``` parámetro para el ```Install-WindowsAdminCenterHA.ps1``` secuencia de comandos.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Solución de problemas

Los registros se guardan en la carpeta temporal del archivo CSV (por ejemplo, C:\ClusterStorage\Volume1\temp).