---
title: Implementación de Windows Admin Center con alta disponibilidad
description: Implementación de Windows Admin Center con alta disponibilidad (Proyecto Honolulu)
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.openlocfilehash: 132f566e8467179c1a58e3555d26ab834dae7129
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970862"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Implementación de Windows Admin Center con alta disponibilidad

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Puedes implementar Windows Admin Center en un clúster de conmutación por error para proporcionar alta disponibilidad para el servicio de puerta de enlace de Windows Admin Center. La solución proporcionada es una solución activa-pasiva, donde solo hay activa una instancia de Windows Admin Center. Si se produce un error en uno de los nodos del clúster, Windows Admin Center conmuta por error correctamente a otro nodo, lo que te permite seguir administrando los servidores de tu entorno sin problemas.

[Obtén información sobre otras opciones de implementación de Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Requisitos previos

- Un clúster de conmutación por error de 2 nodos o más en Windows Server 2016 o 2019. [Más información sobre la implementación de un clúster de conmutación por error](../../../failover-clustering/failover-clustering-overview.md).
- Un Volumen compartido de clúster (CSV) para que Windows Admin Center almacene los datos persistentes a los que pueden acceder todos los nodos del clúster. Serán suficientes 10 GB para el CSV.
- Script de implementación de alta disponibilidad desde el [archivo ZIP del script de alta disponibilidad de Windows Admin Center](https://aka.ms/WACHAScript). Descarga el archivo .zip que contiene el script en la máquina local y, luego, copia el script según sea necesario en función de las instrucciones siguientes.
- Se recomienda, aunque es opcional: un certificado firmado .pfx y contraseña. No es necesario que ya tengas instalado el certificado en los nodos del clúster: el script lo hará automáticamente. Si no proporcionas uno, el script de instalación generará un certificado autofirmado, que expira después de 60 días.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Instalación de Windows Admin Center en un clúster de conmutación por error

1. Copia el script ```Install-WindowsAdminCenterHA.ps1``` en un nodo del clúster. Descarga o copia el archivo .msi de Windows Admin Center en el mismo nodo.
2. Conéctate al nodo a través de RDP y ejecuta el script ```Install-WindowsAdminCenterHA.ps1``` desde ese nodo con los parámetros siguientes:
    - `-clusterStorage`: Ruta de acceso local del Volumen compartido de clúster para almacenar los datos de Windows Admin Center.
    - `-clientAccessPoint`: Elige un nombre que usarás para acceder a Windows Admin Center. Por ejemplo, si ejecutas el script con el parámetro `-clientAccessPoint contosoWindowsAdminCenter`, accederás al servicio de Windows Admin Center visitando `https://contosoWindowsAdminCenter.<domain>.com`.
    - `-staticAddress`: Opcional. Una o varias direcciones estáticas para el servicio genérico de clústeres.
    - `-msiPath`: Ruta de acceso del archivo .msi de Windows Admin Center.
    - `-certPath`: Opcional. Ruta de acceso de un archivo .pfx de certificados.
    - `-certPassword`: Opcional. Contraseña SecureString para el archivo .pfx de certificados que se proporciona en `-certPath`.
    - `-generateSslCert`: Opcional. Si no quieres proporcionar un certificado firmado, incluye esta marca de parámetro para generar un certificado autofirmado. Ten en cuenta que el certificado autofirmado expirará a los 60 días.
    - `-portNumber`: Opcional. Si no especificas un puerto, el servicio de puerta de enlace se implementa en el puerto 443 (HTTPS). Para usar otro puerto, especifica este parámetro. Tenga en cuenta que, si usa un puerto personalizado (uno que no sea el 443), accederá a Windows Admin Center desde https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> El script ```Install-WindowsAdminCenterHA.ps1``` admite los parámetros ```-WhatIf ``` y ```-Verbose```.

### <a name="examples"></a>Ejemplos

#### <a name="install-with-a-signed-certificate"></a>Instalación con un certificado firmado:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Instalación con un certificado autofirmado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Actualización de una instalación de alta disponibilidad existente

Use el mismo script ```Install-WindowsAdminCenterHA.ps1``` para actualizar la implementación de alta disponibilidad sin perder los datos de conexión.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Actualización a una nueva versión de Windows Admin Center

Cuando se publique una nueva versión de Windows Admin Center, simplemente vuelve a ejecutar el script ```Install-WindowsAdminCenterHA.ps1``` con el parámetro ```msiPath```:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Actualización del certificado que usa Windows Admin Center

Puedes actualizar el certificado que usa una implementación de alta disponibilidad de Windows Admin Center en cualquier momento; para ello, proporciona el archivo .pfx y la contraseña del nuevo certificado.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

También puedes actualizar el certificado al mismo tiempo que actualizas la plataforma de Windows Admin Center con un nuevo archivo .msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

## <a name="uninstall"></a>Desinstalar

Para desinstalar la implementación de alta disponibilidad de Windows Admin Center desde el clúster de conmutación por error, pasa el parámetro ```-Uninstall``` al script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Solucionar problemas

Los registros se guardan en la carpeta temporal del CSV (por ejemplo, C:\ClusterStorage\Volume1\temp).