---
title: Novedades de IPAM
description: En este tema se describe la funcionalidad de administración de direcciones IP (IPAM) que es nuevo o cambiado en Windows Server 2016.
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
ms.openlocfilehash: b90cd1ab223e38cbf5933b58a594b32d5e3d4858
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ipam"></a>Novedades de IPAM

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema se describe la funcionalidad de administración de direcciones IP (IPAM) que es nuevo o cambiado en Windows Server 2016.  
  
IPAM proporciona capacidades de administración y supervisión altamente personalizables para la dirección IP y la infraestructura DNS de una red de empresa o proveedor de servicio de nube (CSP). Puedes supervisar, auditoría y administrar los servidores que ejecutan el protocolo de configuración dinámica de Host (DHCP) y sistema de nombres de dominio (DNS) mediante el uso de IPAM.  
  
## <a name="BKMK_IPAM2012R2"></a>Actualizaciones en el servidor IPAM  
A continuación es las características nuevas y mejoradas de IPAM en Windows Server 2016.  
  
|Características y funcionalidades|Nuevas o mejoradas|Descripción|  
|--------------------------|-------------------|---------------|  
|[Administración de direcciones IP mejorada](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Mejorado|Se han mejorado las funcionalidades IPAM para escenarios como control de subredes /32 IPv4 y IPv6 /128 y encontrar gratuitas subredes con direcciones IP y los intervalos en un bloque de direcciones IP.|  
|[Administración de servicios DNS mejorada](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nuevo|IPAM admite el registro de recursos DNS, reenviador condicional y la administración de la zona DNS para los servidores DNS Unidos a un dominio, integrada en Active Directory y respaldados por el archivo.|  
|[Integrado DNS, DHCP y la dirección IP administración (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Mejorado|Varias nuevas experiencias y administración del ciclo de vida integrado se habilitan las operaciones, como la visualización de todos los registros de recursos DNS que pertenecen a una dirección IP, automatizar el inventario de direcciones IP basadas en registros de recursos DNS y administración del ciclo de vida de direcciones IP para las operaciones de DNS y DHCP.|  
|[Compatibilidad con múltiples de bosque de Active Directory](#bkmk_ad)|Nuevo|Puedes usar IPAM para administrar los servidores DNS y DHCP de varios bosques de Active Directory cuando hay una relación de confianza bidireccional entre el bosque donde está instalado IPAM y cada uno de los bosques remotos.|  
|[Depurar los datos de uso](#bkmk_purge)|Nuevo|Ahora puedes reducir el tamaño de la base de datos IPAM purga los datos de uso de direcciones IP es anteriores a una fecha que especifiques.|  
|[Soporte técnico de Windows PowerShell para el Control de acceso en función de rol](#bkmk_ps)|Nuevo|Puedes usar Windows PowerShell para establecer los ámbitos de acceso de los objetos IPAM.|  
  
### <a name="EIP"></a>Administración de direcciones IP mejorada  
Las siguientes características mejoran las funcionalidades de administración de IPAM dirección.  
>[!NOTE]
>Para la referencia de comandos de IPAM Windows PowerShell, consulta [Cmdlets del servidor de administración de direcciones IP (IPAM) en Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Compatibilidad con /31, /32 y /128 subredes  
IPAM en Windows Server 2016 ahora admite /31, /32 y /128 subredes. Por ejemplo, una subred dos direcciones (o 31 IPv4) pueden ser necesarias para un vínculo de punto a punto entre los cambios. Además, algunos conmutadores pueden exigir direcciones de bucle invertido solo (/ 32 para IPv4, /128 para IPv6).  
  
#### **<a name="find-free-subnets-with-find-ipamfreesubnet"></a>Encontrar gratuitas subredes con Buscar IpamFreeSubnet**  
  
Este comando devuelve subredes que están disponibles para la asignación, dado un bloque IP, la longitud del prefijo y número de subredes solicitados.   
  
Si el número de subredes disponibles es menor que el número de subredes solicitados, se devuelven las subredes disponibles con una advertencia que indica que el número disponible es menor que el número solicitado.  
  
>[!NOTE]
>Esta función no asignar realmente las subredes, únicamente notifica su disponibilidad. Sin embargo, se puede canalizar la salida del cmdlet para el **agregar IpamSubnet** comando para crear la subred.  
  
Para obtener más información, consulta [buscar IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx).  
  
#### **<a name="find-free-address-ranges-with-find-ipamfreerange"></a>Busca los intervalos de direcciones gratuita con Buscar IpamFreeRange**  
  
Este nuevo comando devuelve disponible intervalos de direcciones IP dada una subred IP, el número de direcciones que son necesarios en el intervalo y el número de intervalos solicitado.   
  
El comando se busca una serie continua de sin asignar direcciones IP que coincidan con el número de direcciones solicitados. El proceso se repite hasta que se encuentra el número de intervalos solicitado, o hasta que hay disponibles no más intervalos de direcciones disponibles.  
  
> [!NOTE]
> Esta función no asignar realmente los intervalos, únicamente notifica su disponibilidad. Sin embargo, se puede canalizar la salida del cmdlet para el **agregar IpamRange** comando para crear el intervalo.  
  
Para obtener más información, consulta [buscar IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx).  
  
### <a name="EDNS"></a>Administración de servicios DNS mejorada  
IPAM en Windows Server 2016 ahora admite la detección de servidores DNS basados en archivos, Unidos a un dominio en un bosque de Active Directory en el que se ejecuta IPAM.  
  
Además, se han agregado las siguientes funciones DNS:  
  
-   Zonas DNS y recursos los registros de colección (que no sean aquellos que pertenecen al DNSSEC) desde servidores DNS que ejecuten Windows Server 2008 o posterior.  
  
-   Configurar (crear, modificar y eliminar) propiedades y operaciones en todos los tipos de registros de recursos (excepto aquellos que pertenecen al DNSSEC).  
  
-   Configurar (crear, modificar, eliminar) propiedades y operaciones en todos los tipos de zonas DNS como principal secundario y zonas de código auxiliar).  
  
-   Desencadenar tareas en secundario y zonas de código auxiliar, con independencia si están hacia delante o zonas de búsqueda inversa. Por ejemplo, como tareas **transferir desde el principal** o **transferir la nueva copia de la zona de maestro**.  
  
-   Control de acceso basado en funciones para la configuración de DNS compatible (registros DNS y las zonas DNS).  
  
-   Configuración y la colección de servidores de reenvío condicional (crear, eliminar, modificar).  
  
### <a name="DDI"></a>Integrado DNS, DHCP y la dirección IP administración (DDI)  
Cuando veas una dirección IP en el inventario de la dirección IP, tienes la opción en la vista de detalles para ver todos los registros de recursos DNS asociados con la dirección IP.  
  
Como parte colección de registros de recursos DNS, IPAM recopila los registros PTR para las zonas de búsqueda inversa de DNS. Para todas las zonas búsqueda inversa que se asignan a cualquier intervalo de direcciones IP, IPAM crea los registros de la dirección IP todos los registros de puntero que pertenecen a esa zona en el intervalo de direcciones IP asignada correspondiente. Si ya existe la dirección IP, el registro PTR es simplemente asociado a esa dirección IP. Las direcciones IP no se crean automáticamente si la zona de búsqueda inversa no está asignada a un intervalo de direcciones IP.  
  
Cuando se crea un registro PTR en una zona de búsqueda inversa a través de IPAM, se actualiza el inventario de la dirección IP de la misma manera como se describió anteriormente. Durante la colección posterior, dado que la dirección IP ya existe en el sistema, el registro PTR simplemente se asignará a esa dirección IP.  
  
### <a name="bkmk_ad"></a>Compatibilidad con múltiples de bosque de Active Directory  
En Windows Server 2012 R2, podía IPAM detectar y administrar los servidores DNS y DHCP que pertenecen al mismo bosque de Active Directory como el servidor IPAM. Ahora puedes administrar los servidores DNS y DHCP que pertenecen a un bosque de AD diferente cuando tenga una relación de confianza bidireccional con el bosque donde está instalado el servidor IPAM. Puedes ir a la **configurar la detección de servidor** diálogo cuadro y agregar dominios de la otra confianza bosques que quieras administrar. Después de que se detectan los servidores, la experiencia de administración es el mismo que los servidores que pertenecen al mismo bosque donde está instalado IPAM.  
  
Para obtener más información, consulta [administrar recursos en varios bosques de Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Depurar los datos de uso  
Datos de uso de depuración te permite reducir el tamaño de la base de datos IPAM mediante la eliminación de datos de uso de direcciones IP antiguos. Para realizar la eliminación de datos, especifican una fecha y IPAM elimina todas las entradas de base de datos más antiguos que o igual a la fecha que proporciones.   
  
Para obtener más información, consulta [purgar datos de uso](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Soporte técnico de Windows PowerShell para el Control de acceso en función de rol  
Ahora puedes usar Windows PowerShell para configurar el Control de acceso en función de funciones. Puedes usar comandos de Windows PowerShell para recuperar objetos DNS y DHCP de IPAM y cambiar sus ámbitos de acceso. Por este motivo, puedes escribir scripts de Windows PowerShell para asignar los ámbitos de acceso a los siguientes objetos.  
  
-   Espacio de direcciones IP  
  
-   Bloque de direcciones IP  
  
-   Subredes con direcciones IP  
  
-   Intervalos de direcciones IP  
  
-   Servidores DNS  
  
-   Zonas DNS  
  
-   Servidores de reenvío condicionales DNS  
  
-   Registros de recursos DNS  
  
-   Servidores DHCP  
  
-   Superámbitos DHCP  
  
-   Ámbitos DHCP  
  
Para obtener más información, consulta [administrar rol de Control de acceso con Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) y [Cmdlets del servidor de administración de direcciones IP (IPAM) en Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  

