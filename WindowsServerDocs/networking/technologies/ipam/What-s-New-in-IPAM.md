---
title: Novedades de IPAM
description: Este tema describe la funcionalidad de administración de direcciones IP (IPAM) nuevas o modificadas en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04838cba63805d20ba31629ed9c8e95290046320
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624679"
---
# <a name="whats-new-in-ipam"></a>Novedades de IPAM

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe la funcionalidad de administración de direcciones IP (IPAM) nuevas o modificadas en Windows Server 2016.  
  
IPAM proporciona funcionalidades de supervisión y administrativas altamente personalizables para la dirección IP y la infraestructura de DNS en una red empresarial o proveedor de servicios en la nube (CSP). Puede supervisar, auditar y administrar servidores que ejecutan el protocolo de configuración dinámica de Host (DHCP) y sistema de nombres de dominio (DNS) mediante el uso de IPAM.  
  
## <a name="BKMK_IPAM2012R2"></a>Actualizaciones en el servidor IPAM  
Siguiente es las características nuevas y mejoradas de IPAM en Windows Server 2016.  
  
|Característica/función|Nueva o mejorada|Descripción|  
|--------------------------|-------------------|---------------|  
|[Administración mejorada de direcciones IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Mejorada|Se han mejorado las capacidades IPAM para escenarios como control subredes /32 IPv4 y IPv6 /128 y buscar libres subredes de direcciones IP e intervalos en un bloque de direcciones IP.|  
|[Administración del servicio DNS mejorada](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nuevo|IPAM admite el registro de recursos DNS, el reenviador condicional y la administración de zonas DNS para los servidores DNS unido al dominio, integrado en Active Directory y basado en archivos.|  
|[Administración (DDI) de direcciones integrado IP, DHCP y DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Mejorada|Varias nuevas experiencias y están habilitadas las operaciones de administración integrada del ciclo de vida, como la visualización de todos los recursos DNS los registros que pertenecen a una dirección IP dirección, inventario automatizada de direcciones IP en función de los registros de recursos DNS y la administración del ciclo de vida de direcciones IP para las operaciones de DNS y DHCP.|  
|[Compatibilidad con múltiples bosques de Active Directory](#bkmk_ad)|Nuevo|Puede usar IPAM para administrar los servidores DNS y DHCP de varios bosques de Active Directory cuando hay una relación de confianza bidireccional entre el bosque donde se instala IPAM y cada uno de los bosques remotos.|  
|[Purgar datos de uso](#bkmk_purge)|Nuevo|Ahora podrá reducir el tamaño de la base de datos IPAM mediante la purga de los datos de uso de direcciones IP que es anteriores a una fecha que especifique.|  
|[Soporte técnico de Windows PowerShell para Control de acceso basado en roles](#bkmk_ps)|Nuevo|Puede usar Windows PowerShell para establecer ámbitos de acceso en objetos IPAM.|  
  
### <a name="EIP"></a>Administración mejorada de direcciones IP  
Las siguientes características mejoran las capacidades de administración de direcciones IPAM.  
>[!NOTE]
>Para la referencia de comandos de IPAM de Windows PowerShell, consulte [Cmdlets de servidor de administración de direcciones IP (IPAM) en Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Compatibilidad con /31, /32 y /128 subredes  
IPAM en Windows Server 2016 ahora admite /31, /32 y /128 subredes. Por ejemplo, una subred de direcciones de dos (o IPv4 31) pueden ser necesarios para un vínculo punto a punto entre los conmutadores. Además, algunos conmutadores pueden requerir direcciones de bucle invertido solo (/ 32 para IPv4, / 128 para IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Buscar subredes gratuitas con búsqueda IpamFreeSubnet**  
  
Este comando devuelve las subredes que están disponibles para la asignación, dado un bloque IP, la longitud del prefijo y el número de subredes solicitados.   
  
Si el número de subredes disponibles es menor que el número de subredes solicitados, se devuelven las subredes disponibles con una advertencia que indica que el número disponible es inferior al número solicitado.  
  
>[!NOTE]
>Esta función no asignar realmente las subredes, solo notifica su disponibilidad. Sin embargo, se puede canalizar la salida del cmdlet para el **agregar IpamSubnet** comando para crear la subred.  
  
Para obtener más información, consulte [buscar IpamFreeSubnet](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Buscar los intervalos de direcciones libres con Buscar IpamFreeRange**  
  
Este nuevo comando devuelve disponibles intervalos de direcciones IP dada una subred IP, el número de direcciones que son necesarios en el intervalo y el número de intervalos que se solicita.   
  
El comando busca una serie continua de direcciones IP sin asignar que coinciden con el número de direcciones solicitados. El proceso se repite hasta que se encuentra el número solicitado de intervalos, o hasta que no están disponibles hay intervalos de direcciones disponibles.  
  
> [!NOTE]
> Esta función no asignar realmente los intervalos, solo notifica su disponibilidad. Sin embargo, se puede canalizar la salida del cmdlet para el **Add-IpamRange** comando para crear el intervalo.  
  
Para obtener más información, consulte [buscar IpamFreeRange](https://docs.microsoft.com/en-us/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="EDNS"></a>Administración del servicio DNS mejorada  
IPAM en Windows Server 2016 admite ahora la detección de servidores DNS basados en archivos, unido al dominio en un bosque de Active Directory en el que se está ejecutando IPAM.  
  
Además, se han agregado las siguientes funciones DNS:  
  
-   Recursos y zonas DNS registra la colección (distintos de los relacionados con DNSSEC) desde los servidores DNS que ejecutan Windows Server 2008 o posterior.  
  
-   Configurar (crear, modificar y eliminar) las propiedades y operaciones en todos los tipos de registros de recursos (distintos de los relacionados con DNSSEC).  
  
-   Configurar (crear, modificar, eliminar) las propiedades y operaciones en todos los tipos de zonas DNS incluidos principal secundario y zonas de rutas internas).  
  
-   Desencadena las tareas en la base de datos secundaria y zonas de rutas internas, sin importar si están al día o zonas de búsqueda inversa. Por ejemplo, tareas como **transferir del maestro de** o **transferir una copia nueva de la zona del patrón**.  
  
-   Control de acceso basado en roles para la configuración de DNS compatible (registros DNS y zonas DNS).  
  
-   Colección de los reenviadores condicionales y de configuración (crear, eliminar y editar).  
  
### <a name="DDI"></a>Administración (DDI) de direcciones integrado IP, DHCP y DNS  
Cuando ve una dirección IP en el inventario de direcciones IP, tiene la opción en la vista de detalles para ver todos los registros de recursos DNS asociados con la dirección IP.  
  
Como parte de la colección de registros de recursos DNS, IPAM recopila los registros PTR para las zonas de búsqueda inversa de DNS. Para todas las zonas de búsqueda inversa que se asignan a cualquier intervalo de direcciones IP, IPAM crea los registros de direcciones IP para todos los registros PTR que pertenecen a esa zona en el intervalo de direcciones IP asignada correspondiente. Si ya existe la dirección IP, el registro PTR es simplemente asociado a esa dirección IP. Las direcciones IP no se crean automáticamente si la zona de búsqueda inversa no está asignada a cualquier intervalo de direcciones IP.  
  
Cuando se crea un registro PTR en una zona de búsqueda inversa mediante IPAM, se actualiza el inventario de direcciones IP de la misma manera como se describió anteriormente. Durante la recopilación posteriores, puesto que la dirección IP ya existirá en el sistema, el registro PTR simplemente asignará con esa dirección IP.  
  
### <a name="bkmk_ad"></a>Compatibilidad con múltiples bosques de Active Directory  
En Windows Server 2012 R2, IPAM fue capaz de detectar y administrar servidores DNS y DHCP que pertenecen al mismo bosque de Active Directory que el servidor IPAM. Ahora puede administrar servidores DNS y DHCP que pertenecen a un bosque de AD diferente cuando tiene una relación de confianza bidireccional con el bosque donde está instalado el servidor IPAM. Puede ir a la **Configurar detección de servidor** diálogo cuadro y agregar bosques que desea administrar de confianza de dominios del otro. Después de detectan los servidores de la experiencia de administración es la misma que para los servidores que pertenecen al mismo bosque donde se instala IPAM.  
  
Para obtener más información, consulte [administrar recursos en varios bosques de Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Purgar datos de uso  
Purgar los datos de uso permite reducir el tamaño de la base de datos IPAM mediante la eliminación de datos de uso de direcciones IP antiguos. Para realizar una eliminación de datos, especifica una fecha y IPAM elimina todas las entradas de base de datos que sean más antiguas que o igual a la fecha que proporcione.   
  
Para obtener más información, consulte [purgar datos de uso](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Soporte técnico de Windows PowerShell para Control de acceso basado en roles  
Ahora puede usar Windows PowerShell para configurar el Control de acceso basado en roles. Puede usar los comandos de Windows PowerShell para recuperar objetos DNS y DHCP en IPAM y cambiar sus ámbitos de acceso. Por este motivo, puede escribir scripts de Windows PowerShell para asignar ámbitos de acceso a los objetos siguientes.  
  
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
  
Para obtener más información, consulte [administrar Role Based Access Control con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) y [Cmdlets de servidor de administración de direcciones IP (IPAM) en Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/ipamserver/).  

