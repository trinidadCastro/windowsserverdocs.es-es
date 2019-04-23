---
title: Repositorio de Software de Linux para productos de Microsoft
description: Este documento describe cómo utilizar e instalar los paquetes de software de Linux para productos de Microsoft.
ms.custom: na
ms.prod: windows-server-threshold
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: dbdbd0f436645f7e19c07e4f3278c5073636a547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831866"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Repositorio de Software de Linux para productos de Microsoft

## <a name="overview"></a>Información general
Microsoft genera y admite una variedad de productos de software para los sistemas Linux y hace que estén disponibles a través de los repositorios de paquetes APT y YUM estándares. Este documento describe cómo configurar el repositorio en el sistema Linux, por lo que se puede, a continuación, instalación o actualización del software de Linux de Microsoft mediante las herramientas de administración de paquete estándar de su distribución.

Repositorio de Software de Microsoft Linux consta de varios repositorios subcarpetas:

 - producción: la producción subrepositorio designado para los paquetes destinados para su uso en producción. Estos paquetes comercialmente son compatibles con Microsoft bajo los términos del contrato de soporte técnico aplicable o programa que tenga con Microsoft.

 - MSSQL-server - estos repositorios contienen paquetes para Microsoft SQL Server en Linux: Véase también: [SQL Server en Linux](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux).

>[!Note]
Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentra en los paquetes. Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye su aceptación de estos términos. Si no está de acuerdo con los términos de licencia, no use el paquete.


## <a name="configuring-the-repositories"></a>Configuración de los repositorios
Repositorios pueden configurarse automáticamente al instalar el paquete de Linux que se aplica a la distribución de Linux y la versión. El paquete instalará la configuración del repositorio junto con la clave pública de GPG usada herramientas como apt y yum/zypper para validar los paquetes firmados o repositorio de metadatos.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL y sus variantes)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 (furgoneta)

        wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.04 (Xenial)

        wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.10 (Yakkety)

        wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update


### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configuración manual
Están disponibles en los archivos de configuración del repositorio [packages.microsoft.com/config](https://packages.microsoft.com/config/). El nombre y la ubicación de estos archivos pueden encontrarse con la siguiente convención de nomenclatura de URI:

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Clave de firma de repositorio y paquete**

 - Clave pública de GPG de Microsoft puede descargarse aquí: [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - Id. de clave pública: Microsoft (firma de lanzamiento) <gpgsecurity@microsoft.com>
 - Huella digital de clave pública: `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>Ejemplos:

 - RHEL/CentOS 7

        # Install repository configuration
        curl https://packages.microsoft.com/config/rhel/7/prod.repo > ./microsoft-prod.repo
        sudo cp ./microsoft-prod.repo /etc/yum.repos.d/

        # Install Microsoft's GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
        sudo rpm --import ./microsoft.asc

 - Ubuntu 16.04

        # Install repository configuration
        curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
        sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

        # Install Microsoft GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
        sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/



