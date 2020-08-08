---
title: Novedades de IPAM
description: En este tema se describen las funciones de administración de direcciones IP (IPAM) nuevas o modificadas en Windows Server 2016.
manager: brianlic
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2d30db0228177b80050eaae3e56919245b750a0c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997335"
---
# <a name="whats-new-in-ipam"></a>Novedades de IPAM

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen las funciones de administración de direcciones IP (IPAM) nuevas o modificadas en Windows Server 2016.

IPAM proporciona capacidades de supervisión y administración muy personalizables para la infraestructura de DNS y la dirección IP en una red empresarial o proveedor de servicios en la nube (CSP). Puede supervisar, auditar y administrar servidores que ejecutan el protocolo de configuración dinámica de host (DHCP) y el sistema de nombres de dominio (DNS) mediante IPAM.

## <a name="updates-in-ipam-server"></a><a name="BKMK_IPAM2012R2"></a>Actualizaciones en el servidor IPAM
A continuación se indican las características nuevas y mejoradas de IPAM en Windows Server 2016.

|Característica/función|Nueva o mejorada|Descripción|
|--------------------------|-------------------|---------------|
|[Administración de direcciones IP mejorada](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Se ha mejorado|Las funcionalidades de IPAM se han mejorado en escenarios como el control de subredes IPv4/32 y IPv6/128 y la búsqueda de subredes de direcciones IP gratuitas e intervalos en un bloque de direcciones IP.|
|[Administración de servicios DNS mejorada](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nuevo|IPAM admite registros de recursos DNS, reenviadores condicionales y administración de zonas DNS para los servidores DNS Active Directory Unidos a un dominio y los basados en archivos.|
|[Administración de DNS, DHCP y dirección IP (DDI) integrada](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Se ha mejorado|Se habilitan varias experiencias nuevas y las operaciones de administración del ciclo de vida integradas, como la visualización de todos los registros de recursos DNS que pertenecen a una dirección IP, el inventario automatizado de direcciones IP basado en registros de recursos DNS y la administración del ciclo de vida de las direcciones IP para las operaciones DNS y DHCP.|
|[Compatibilidad con varios Active Directory bosques](#bkmk_ad)|Nuevo|Puede usar IPAM para administrar los servidores DNS y DHCP de varios bosques de Active Directory cuando hay una relación de confianza bidireccional entre el bosque en el que está instalado IPAM y cada uno de los bosques remotos.|
|[Purgar datos de uso](#bkmk_purge)|Nuevo|Ahora puede reducir el tamaño de la base de datos de IPAM purgando los datos de uso de direcciones IP anteriores a la fecha que especifique.|
|[Compatibilidad de Windows PowerShell con el Access Control basado en roles](#bkmk_ps)|Nuevo|Puede usar Windows PowerShell para establecer ámbitos de acceso en objetos IPAM.|

### <a name="enhanced-ip-address-management"></a><a name="EIP"></a>Administración de direcciones IP mejorada
Las siguientes características mejoran las capacidades de administración de direcciones de IPAM.
>[!NOTE]
>Para ver la referencia de comandos de Windows PowerShell para IPAM, consulte [cmdlets de servidor de administración de direcciones IP (IPAM) en Windows PowerShell](/powershell/module/ipamserver/).

#### <a name="support-for-31-32-and-128-subnets"></a>Compatibilidad con subredes/31,/32 y/128
IPAM en Windows Server 2016 admite ahora las subredes/31,/32 y/128. Por ejemplo, puede ser necesaria una subred de dos direcciones (/31 IPv4) para un vínculo punto a punto entre los conmutadores. Además, algunos conmutadores pueden requerir direcciones de bucle invertido único (/32 para IPv4,/128 para IPv6).

#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Busque subredes gratuitas con Find-IpamFreeSubnet**

Este comando devuelve las subredes que están disponibles para la asignación, dado un bloque IP, la longitud del prefijo y el número de subredes solicitadas.

Si el número de subredes disponibles es menor que el número de subredes solicitadas, se devuelven las subredes disponibles con una advertencia que indica que el número disponible es menor que el número solicitado.

>[!NOTE]
>Esta función no asigna realmente las subredes, solo informa de su disponibilidad. Sin embargo, la salida del cmdlet se puede canalizar al comando **Add-IpamSubnet** para crear la subred.

Para obtener más información, consulte [Find-IpamFreeSubnet](/powershell/module/ipamserver/Find-IpamFreeSubnet).

#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Busque intervalos de direcciones gratis con Find-IpamFreeRange**

Este nuevo comando devuelve los intervalos de direcciones IP disponibles dada una subred IP, el número de direcciones que se necesitan en el intervalo y el número de intervalos solicitados.

El comando busca una serie continua de direcciones IP sin asignar que coincidan con el número de direcciones solicitadas. El proceso se repite hasta que se encuentre el número solicitado de intervalos o hasta que no haya más intervalos de direcciones disponibles.

> [!NOTE]
> En realidad, esta función no asigna los intervalos, sino que solo informa de su disponibilidad. Sin embargo, la salida del cmdlet se puede canalizar al comando **Add-IpamRange** para crear el intervalo.

Para obtener más información, consulte [Find-IpamFreeRange](/powershell/module/ipamserver/Find-IpamFreeRange).

### <a name="enhanced-dns-service-management"></a><a name="EDNS"></a>Administración de servicios DNS mejorada
IPAM en Windows Server 2016 admite ahora la detección de servidores DNS Unidos a un dominio basados en archivos en un bosque Active Directory en el que se ejecuta IPAM.

Además, se han agregado las siguientes funciones de DNS:

-   La colección de registros de recursos y zonas DNS (aparte de las relativas a DNSSEC) de los servidores DNS que ejecutan Windows Server 2008 o posterior.

-   Configure (crear, modificar y eliminar) propiedades y operaciones en todos los tipos de registros de recursos (distintos de los que pertenecen a DNSSEC).

-   Configurar (crear, modificar, eliminar) propiedades y operaciones en todos los tipos de zonas DNS, incluidas las zonas secundarias principal y de código auxiliar.

-   Tareas desencadenadas en zonas secundarias y de rutas internas, independientemente de si son zonas de búsqueda directa o inversa. Por ejemplo, tareas como la **transferencia desde la página maestra** o la transferencia de una **nueva copia de la zona desde la maestra**.

-   Control de acceso basado en roles para la configuración de DNS compatible (registros DNS y zonas DNS).

-   Colección y configuración de reenviadores condicionales (crear, eliminar, editar).

### <a name="integrated-dns-dhcp-and-ip-address-ddi-management"></a><a name="DDI"></a>Administración de DNS, DHCP y dirección IP (DDI) integrada
Al ver una dirección IP en el inventario de direcciones IP, tiene la opción en la vista de detalles para ver todos los registros de recursos DNS asociados con la dirección IP.

Como parte de la recopilación de registros de recursos DNS, IPAM recopila los registros PTR para las zonas de búsqueda inversa de DNS. Para todas las zonas de búsqueda inversa que están asignadas a cualquier intervalo de direcciones IP, IPAM crea los registros de direcciones IP para todos los registros PTR pertenecientes a esa zona en el intervalo de direcciones IP asignadas correspondiente. Si la dirección IP ya existe, el registro PTR simplemente está asociado a esa dirección IP. Las direcciones IP no se crean automáticamente si la zona de búsqueda inversa no se asigna a ningún intervalo de direcciones IP.

Cuando se crea un registro PTR en una zona de búsqueda inversa a través de IPAM, el inventario de direcciones IP se actualiza de la misma manera que se ha descrito anteriormente. Durante la recopilación posterior, como la dirección IP ya existe en el sistema, el registro PTR se asignará simplemente con esa dirección IP.

### <a name="multiple-active-directory-forest-support"></a><a name="bkmk_ad"></a>Compatibilidad con varios Active Directory bosques
En Windows Server 2012 R2, IPAM podía detectar y administrar servidores DNS y DHCP que pertenecen al mismo bosque de Active Directory que el servidor IPAM. Ahora puede administrar los servidores DNS y DHCP que pertenecen a un bosque de AD diferente cuando tiene una relación de confianza bidireccional con el bosque en el que está instalado el servidor IPAM. Puede ir al cuadro de diálogo **configurar detección de servidores** y agregar dominios desde los otros bosques de confianza que desea administrar. Una vez detectados los servidores, la experiencia de administración es la misma que para los servidores que pertenecen al mismo bosque en el que está instalado IPAM.

Para obtener más información, consulte [Administración de recursos en varios bosques de Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)

### <a name="purge-utilization-data"></a><a name="bkmk_purge"></a>Purgar datos de uso
Purgar los datos de uso permite reducir el tamaño de la base de datos de IPAM mediante la eliminación de los datos antiguos de uso de direcciones IP. Para realizar la eliminación de datos, especifique una fecha y IPAM eliminará todas las entradas de la base de datos que sean anteriores o iguales a la fecha proporcionada.

Para obtener más información, consulte [purgar datos de uso](../../technologies/ipam/Purge-Utilization-Data.md).

### <a name="windows-powershell-support-for-role-based-access-control"></a><a name="bkmk_ps"></a>Compatibilidad de Windows PowerShell con el Access Control basado en roles
Ahora puede usar Windows PowerShell para configurar el Access Control basado en roles. Puede usar comandos de Windows PowerShell para recuperar objetos DNS y DHCP en IPAM y cambiar sus ámbitos de acceso. Por este motivo, puede escribir scripts de Windows PowerShell para asignar ámbitos de acceso a los siguientes objetos.

-   Espacio de direcciones IP

-   Bloque de direcciones IP

-   Subredes de direcciones IP

-   Intervalos de direcciones IP

-   Servidores DNS

-   Zonas DNS

-   Reenviadores condicionales de DNS

-   Registros de recursos DNS

-   Servidores DHCP

-   Superámbitos DHCP

-   Ámbitos DHCP

Para obtener más información, vea [administrar roles basados en Access Control con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) y [cmdlets del servidor de administración de direcciones IP (IPAM) en Windows PowerShell](/powershell/module/ipamserver/).