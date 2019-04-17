---
title: Implementar Windows Admin Center con alta disponibilidad
description: Implementar Windows Admin Center con alta disponibilidad (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 802a7bd537cab22893abb7e6657c4be90346ef88
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "9025037"
---
# Implementar Windows Admin Center con alta disponibilidad

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Puedes implementar Windows Admin Center en un clúster de conmutación por error para ofrecer una alta disponibilidad para el servicio de puerta de enlace de Windows Admin Center. La solución que proporcionan es una solución de activo / pasivo, donde solo una instancia de Windows Admin Center está activa. Si se produce un error en uno de los nodos del clúster, Windows Admin Center correctamente conmutar por error a otro nodo, lo que te permite continuar con la administración de servidores en su entorno sin problemas. 

[Obtén información sobre otras opciones de implementación de Windows Admin Center.](../plan/installation-options.md)

## Requisitos previos

- Un clúster de conmutación por error de 2 o más nodos en Windows Server 2016 o 2019. [Más información sobre cómo implementar un clúster de conmutación por error](../../../failover-clustering/failover-clustering-overview.md).
- Un clúster de volumen compartido (CSV para Windows Admin Center almacenar datos persistentes que pueden tener acceso a todos los nodos del clúster). 10 GB será suficiente para tu CSV.
- Script de implementación de alta disponibilidad del [archivo zip de alta disponibilidad Script de Windows Admin Center](https://aka.ms/WACHAScript). Descargar el archivo .zip que contiene la secuencia de comandos en el equipo local y, a continuación, copia el script, según sea necesario según las instrucciones siguientes.
- Recomendado, pero opcional: una contraseña de certificado .pfx &. No tienes que ya se ha instalado el certificado en los nodos del clúster, para el script lo hará. Si no proporcionas uno, el script de instalación genera un certificado autofirmado que expire después de 60 días.

## Instalar Windows Admin Center en un clúster de conmutación por error

1. Copia el ```Install-WindowsAdminCenterHA.ps1``` script a un nodo en el clúster. Descargar o copiar el archivo .msi de Windows Admin Center en el mismo nodo.
2. Conectar con el nodo a través de RDP y ejecuta el ```Install-WindowsAdminCenterHA.ps1``` script desde ese nodo con los siguientes parámetros:
    - `-clusterStorage`: la ruta de acceso local del volumen compartido de clúster para almacenar los datos de Windows Admin Center.
    - `-clientAccessPoint`: elige un nombre que usarás para acceder al centro de administración de Windows. Por ejemplo, si ejecuta el script con el parámetro `-clientAccessPoint contosoWindowsAdminCenter`, tendrá acceso a los servicios de Windows Admin Center visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Opcional. Una o varias direcciones estáticas para el servicio de clúster genérico. 
    - `-msiPath`: La ruta de acceso del archivo .msi de Windows Admin Center.
    - `-certPath`: Opcional. La ruta de acceso de un archivo de certificado pfx.
    - `-certPassword`: Opcional. Una contraseña SecureString para el .pfx certificado proporcionado en `-certPath`
    - `-generateSslCert`: Opcional. Si no quieres proporcionar un certificado firmado, incluir esta marca de parámetro para generar un certificado autofirmado. Ten en cuenta que el certificado autofirmado expirará en 60 días.
    - `-portNumber`: Opcional. Si no se especifica un puerto, el servicio de puerta de enlace se implementa en el puerto 443 (HTTPS). Para usar un puerto diferente se especifica en este parámetro. Ten en cuenta que si usas un puerto personalizado (nada además 443), tendrá acceso a la Windows Admin Center yendo a https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> El ```Install-WindowsAdminCenterHA.ps1``` admite la secuencia de comandos ```-WhatIf ``` y ```-Verbose``` parámetros

### Ejemplos

#### Instalar con un certificado de firma:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### Instalar con un certificado autofirmado:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## Actualizar una instalación existente de alta disponibilidad

Usar el mismo ```Install-WindowsAdminCenterHA.ps1``` script para actualizar la implementación de alta disponibilidad, sin pérdida de datos de la conexión.

### Actualizar a una nueva versión de Windows Admin Center

Cuando se publique una nueva versión de Windows Admin Center, solo tienes que ejecutar el ```Install-WindowsAdminCenterHA.ps1``` nuevo script con solo la ```msiPath``` parámetro:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### Actualizar el certificado usado por Windows Admin Center

Puedes actualizar el certificado utilizado por una implementación de alta disponibilidad de Windows Admin Center en cualquier momento mediante .pfx archivo el nuevo certificado y y la contraseña.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

También puede actualizar el certificado al mismo tiempo que actualice la plataforma de Windows Admin Center con un nuevo archivo MSI.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## Desinstalar

Para desinstalar la implementación de alta disponibilidad de Windows Admin Center desde el clúster de conmutación por error, pasar el ```-Uninstall``` parámetro a la ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## Solución de problemas

Los registros se guardan en la carpeta temporal de la CSV (por ejemplo, C:\ClusterStorage\Volume1\temp).