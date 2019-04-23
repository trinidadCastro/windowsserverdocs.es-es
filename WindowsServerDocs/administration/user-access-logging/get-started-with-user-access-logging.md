---
title: Empezar a trabajar con usuario registro de acceso
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8656bf278519b48f8d26008fd98e46428106e511
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861506"
---
# <a name="get-started-with-user-access-logging"></a>Empezar a trabajar con usuario registro de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Registro de acceso de usuarios (UAL) es la característica de Windows Server que agrega datos de uso del cliente por rol y productos en un servidor local. Ayuda a los administradores de Windows server a cuantificar las solicitudes de los equipos cliente de roles y servicios en un servidor local.  
  
UAL se instala y habilita de forma predeterminada y recopila los datos de prácticamente en tiempo real. No se necesita ninguna configuración de administrador, aunque UAL se puede habilitar o deshabilitar. Para obtener más información, consulte [Manage User Access Logging](Manage-User-Access-Logging.md). El servicio de registro de acceso de usuarios agrega datos de uso de cliente por roles y productos en archivos de base de datos local.  Los administradores de TI pueden usar Windows Management Instrumentation (WMI) o cmdlets de Windows PowerShell para recuperar cantidades e instancias por rol de servidor (o producto de software), por usuario, por dispositivo, por el servidor local y por fecha.  
  
> [!NOTE]  
> UAL admite el [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000).  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
UAL agrega usuarios y dispositivos eventos solicitud cliente único que se registran en una base de datos local. Después, estos registros se ponen a disposición de los usuarios (a través de una consulta realizada por un administrador del servidor) para recuperar cantidades e instancias por rol de servidor, por usuario, por dispositivo, por el servidor local y por fecha.  Además, se ha ampliado UAL para permitir que los desarrolladores de software ajeno a Microsoft instrumentar sus eventos UAL se puede agregar Windows Server.  
  
UAL puede realizar las siguientes tareas:  
  
-   Cuantificar las solicitudes de usuario de clientes para servidores virtuales o físicos locales.  
  
-   Cuantificar las solicitudes de usuario de clientes para productos de software instalados en un servidor físico local o virtual.  
  
-   Recuperar datos en un servidor local que ejecuta Hyper-V para identificar periodos de alta y baja demanda en un equipo virtual Hyper-V.  
  
-   Recuperar datos de UAL desde varios servidores remotos.  
  
Además, los desarrolladores de software pueden instrumentar eventos UAL que se pueden agregar y recuperar usando las interfaces WMI y Windows PowerShell.  
  
UAL admite los siguientes roles de servidor y servicios:  
  
-   Servicios de certificados de Active Directory (AD CS)  
  
-   Active Directory Rights Management Services (AD RMS)  
  
-   BranchCache  
  
-   Sistema de nombres de dominio (DNS)  
  
    > [!NOTE]  
    > UAL recopila datos DNS cada 24 horas y hay un cmdlet de UAL independiente para este escenario.  
  
-   Protocolo de configuración dinámica de host (DHCP)  
  
-   Servidor de fax  
  
-   Servicios de archivos  
  
-   Servidor de Protocolo de transferencia de archivos (FTP)  
  
-   Hyper-V  
  
    > [!NOTE]  
    > UAL recopila datos de Hyper-V cada 24 horas y hay un cmdlet de UAL independiente para este escenario.  
  
-   Servidor web (IIS)  
  
    > [!WARNING]  
    > Para usar UAL con IIS, debe utilizar iisual.exe. Para obtener más información, consulte [Análisis de datos de uso del cliente con el Registro de acceso de usuarios de IIS](http://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging).  
  
-   Servicios de la cola de mensajes Microsoft Message Queue (MSMQ)  
  
-   Servicios de acceso y directivas de redes  
  
-   Servicios de impresión y documentos  
  
-   Servicio de enrutamiento y acceso remoto (RRAS)  
  
-   Servicios de implementación de Windows (WDS)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> El uso de UAL no es aconsejable en servidores que se conectan directamente a Internet (como los servidores web de un espacio de direcciones accesible desde Internet) o en escenarios donde la función primordial del servidor es mantener un rendimiento extremadamente alto (como en entornos de carga de trabajo HPC). UAL es destina principalmente a pequeño, mediano y escenarios de intranet empresarial donde se espera un gran volumen, pero no tan alto como las implementaciones que sirven de volumen de tráfico a través de Internet de forma periódica.  
  
## <a name="BKMK_NEW"></a>Funcionalidad importante  
En la tabla siguiente se describen las funciones clave de UAL y sus valores posibles.  
  
|Funcionalidad|Valor|  
|-----------------|---------|  
|Recopilar y agregar los datos de eventos de solicitud de cliente prácticamente en tiempo real.|Se pueden guardar hasta tres años de datos. **Importante:** Los administradores necesitan exigir la compatibilidad de los datos recopilados y los periodos de retención de datos con la directiva de privacidad de la organización y la normativa local.|  
|Consulte UAL mediante interfaces WMI o Windows PowerShell para recuperar datos de solicitud de cliente en un servidor local o remoto.|UAL permite una sola vista de datos de uso continuo. Los administradores de servidor y de organización pueden recuperar estos datos y coordinarse con los administradores empresariales para optimizar el uso de sus licencias por volumen de software.|  
|Opción habilitada de forma predeterminada.|Los administradores de servidor no necesitan configurar ni instalar en modo alguno para que toda la funcionalidad básica esté disponible y en funcionamiento.|  
  
## <a name="data-logged-with-ual"></a>Datos registrados con UAL  
Los siguientes datos relacionados con el usuario se registran con UAL.  
  
|Datos|Descripción|  
|--------|---------------|  
|**UserName**|Nombre de usuario en el cliente que acompaña las entradas de UAL desde los roles y productos instalados, si procede.|  
|**ActivityCount**|Número de veces que un usuario determinado ha tenido acceso a un rol o servicio.|  
|**FirstSeen**|Fecha y hora de la primera vez que un usuario tuvo acceso a un rol o servicio.|  
|**LastSeen**|Fecha y hora de la última vez que un usuario tuvo acceso a un rol o servicio.|  
|**ProductName**|Nombre del producto principal de software, como puede ser Windows, que proporciona datos de UAL.|  
|**RoleGUID**|GUID asignado o registrado por UAL que representa el rol de servidor o el producto instalado.|  
|**RoleName**|Nombre del rol, componente o subproducto que está proporcionando los datos de UAL. Esto también está asociado con un ProductName y un RoleGUID.|  
|**TenantIdentifier**|GUID único para un cliente inquilino de un rol instalado o para un producto que acompaña a los datos de UAL, si procede.|  
  
Los siguientes datos relacionados con el dispositivo se registran con UAL.  
  
|Datos|Descripción|  
|--------|---------------|  
|**IPAddress**|Dirección IP de un dispositivo de cliente que se usó para acceder a un rol o servicio.|  
|**ActivityCount**|Número de veces que un dispositivo determinado ha tenido acceso a un rol o servicio.|  
|**FirstSeen**|Fecha y hora de la primera vez que se usó una dirección IP para tener acceso a un rol o un servicio.|  
|**LastSeen**|Fecha y hora de la última vez que se usó una dirección IP para tener acceso a un rol o un servicio.|  
|**ProductName**|Nombre del producto principal de software, como puede ser Windows, que proporciona datos de UAL.|  
|**RoleGUID**|GUID asignado o registrado por UAL que representa el rol de servidor o el producto instalado.|  
|**RoleName**|Nombre del rol, componente o subproducto que está proporcionando los datos de UAL. Esto también está asociado con un ProductName y un RoleGUID.|  
|**TenantIdentifier**|GUID único para un cliente inquilino de un rol instalado o para un producto que acompaña a los datos de UAL, si procede.|  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
UAL se puede usar en cualquier equipo que ejecute versiones de Windows Server posteriores a Windows Server 2012.  
  
## <a name="see-also"></a>Vea también  
[Registro de acceso de usuarios](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) en MSDN.  
[Usuario de administrar registro de acceso](Manage-User-Access-Logging.md)  
  

