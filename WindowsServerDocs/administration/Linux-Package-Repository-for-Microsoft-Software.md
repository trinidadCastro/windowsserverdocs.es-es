---
title: Repositorio de software de Linux para productos de Microsoft
description: En este documento se describe cómo usar e instalar paquetes de software de Linux para productos de Microsoft.
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: victorcheng7
ms.author: vichen
ms.date: 08/14/2020
ms.openlocfilehash: 28ce502a78c58eda74d5b412fe4e4d0d3279d442
ms.sourcegitcommit: dac52260fdcc3721daf7e32cd45760a0ced96de7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91663686"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Repositorio de software de Linux para productos de Microsoft

## <a name="overview"></a>Información general

Microsoft crea y admite una gran variedad de productos de software para los sistemas Linux y los pone a disposición de los repositorios de paquetes de YUM estándar y APT. En este documento se describe cómo configurar el repositorio en el sistema Linux, de modo que pueda instalar o actualizar el software de Linux de Microsoft con las herramientas de administración de paquetes estándar de la distribución.

El repositorio de software de Linux de Microsoft consta de varios subrepositorios:

 - Prod: el subrepositorio de producción se designa para los paquetes destinados a su uso en producción. Microsoft ofrece soporte técnico comercial a estos paquetes según los términos del contrato de soporte técnico o programa aplicable que tenga con Microsoft.

 - MSSQL-Server: estos repositorios contienen paquetes para Microsoft SQL Server en Linux; vea también: [SQL Server en Linux](https://www.microsoft.com/sql-server/sql-server-vnext-including-Linux).

> [!NOTE]
> Los paquetes de los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en los paquetes. Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="configuring-the-repositories"></a>Configuración de los repositorios

Los repositorios se pueden configurar automáticamente mediante la instalación del paquete de Linux que se aplica a la versión y distribución de Linux. El paquete instalará la configuración del repositorio junto con la clave pública GPG usada por herramientas como apt/yum/zypper para validar los paquetes firmados y/o los metadatos del repositorio.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL y variantes)

 - Enterprise Linux 6 (EL6)<p>`sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm`

 - Enterprise Linux 7 (EL7)<p>`sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm`

 - Enterprise Linux 8 (EL8)<p>`sudo rpm -Uvh https://packages.microsoft.com/config/rhel/8/packages-microsoft-prod.rpm`

### <a name="suse"></a>SUSE

 - SUSE Linux Enterprise Server 12<p>`sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm`

 - SUSE Linux Enterprise Server 15<p>`sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm`

### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 16,04 (Xenial)<p>`curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -`<p>`sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod`<p>`sudo apt-get update`

 - Ubuntu 18,04 (Bionic)<p>`curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -`<p>`sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod`<p>`sudo apt-get update`

 - Ubuntu 20,04 (focal)<p>`curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -`<p>`sudo apt-add-repository https://packages.microsoft.com/ubuntu/20.04/prod`<p>`sudo apt-get update`

## <a name="manual-configuration"></a>Configuración manual

Los archivos de configuración del repositorio están disponibles en [packages.Microsoft.com/config](https://packages.microsoft.com/config/). El nombre y la ubicación de estos archivos se pueden encontrar mediante la siguiente Convención de nomenclatura de URI:

`https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)`

### <a name="package-and-repository-signing-key"></a>Clave de firma de paquete y repositorio

- La clave pública GPG de Microsoft puede descargarse aquí: [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
- IDENTIFICADOR de clave pública: Microsoft (firma de lanzamiento) <gpgsecurity@microsoft.com>
- Huella digital de clave pública: `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>Ejemplos

 - RHEL/CentOS 7

```
# Install repository configuration
curl -sSL https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft-prod.repo

# Install Microsoft's GPG public key
curl -sSL https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
sudo rpm --import ./microsoft.asc
```

 - Ubuntu 20.04

```
# Install repository configuration
curl -sSL https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/microsoft-prod.list

# Install Microsoft GPG public key
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Update package index files
sudo apt-get update
```
