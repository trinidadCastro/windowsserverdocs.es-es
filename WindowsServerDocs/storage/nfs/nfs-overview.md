---
title: Introducción al sistema de archivos de red
description: Explica qué es Network File System.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876306"
---
# <a name="network-file-system-overview"></a>Introducción al sistema de archivos de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describe el servicio de rol de sistema de archivos de red y las características incluidas con el rol de servidor Servicios de archivos y almacenamiento en Windows Server. Network File System (NFS) proporciona un archivo de solución para las empresas que tienen entornos heterogéneos que incluyen Windows y equipos que no son Windows de uso compartido.

## <a name="feature-description"></a>Descripción de la característica

Mediante el protocolo NFS, puede transferir archivos entre equipos que ejecutan Windows y otros sistemas operativos que no sean Windows, como Linux o UNIX.

NFS en Windows Server incluye el servidor para NFS y cliente para NFS. Un equipo que ejecuta Windows Server puede usar servidor para NFS para que actúe como un servidor de archivos NFS para otros equipos cliente que no sean Windows. Cliente para NFS permite a un equipo basado en Windows que ejecutan Windows Server para obtener acceso a archivos almacenados en un servidor NFS que no son de Windows.

## <a name="windows-and-windows-server-versions"></a>Versiones de Windows y Windows Server

Windows admiten varias versiones del cliente para NFS y del servidor, dependiendo de la familia y versión del sistema operativo. 

| Sistemas operativos | Versiones de servidor NFS |Versiones de cliente NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Aplicaciones prácticas

Estas son algunas maneras de usar NFS:

- Usar un servidor de archivos NFS de Windows para proporcionar acceso de protocolo múltiple para el mismo recurso compartido de archivos a través de los protocolos SMB y NFS desde clientes multiplataformas.
- Implementar un servidor de archivos NFS de Windows en un entorno de sistema operativo de que no sean Windows fundamentalmente para proporcionar a los clientes que no sean Windows equipos tengan acceso a recursos compartidos de archivos NFS.
- Migrar aplicaciones de un sistema operativo a otro mediante el almacenamiento de los datos en recursos compartidos de archivos accesibles a través de los protocolos SMB y NFS.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

Funciones nuevas y modificadas en Network File System incluyen compatibilidad con la versión 4.1 de NFS e implementación mejoradas y facilidad de uso. Para obtener información sobre la funcionalidad que es nuevos o han cambiado en Windows Server 2012, revise la siguiente tabla:

|Característica/funcionalidad|Nueva o actualizada|Descripción|
|---|---|---|
|[Versión 4.1 de NFS](#nfs-version-4.1)|Nuevo|Mayor seguridad, rendimiento y la interoperabilidad en comparación con NFS versión 3.|
|[Infraestructura NFS](#nfs-infrastructure)|Actualizado|Mejora la implementación y la facilidad de uso y aumenta la seguridad.|
|[Disponibilidad continua de la versión 3 de NFS](#nfs-version-3-continuous-availability)|Actualizado|Mejora la disponibilidad continua en los clientes NFS versión 3.|
|[Mejoras de implementación y facilidad de uso](#deployment-and-manageability-improvements)|Actualizado|Permite implementar y administrar NFS con nuevos cmdlets de Windows PowerShell y un nuevo proveedor de WMI fácilmente.|

## <a name="nfs-version-41"></a>Versión 4.1 de NFS

Versión 4.1 de NFS implementa todos los aspectos necesarios, además de algunos de los aspectos opcionales, de [5661 RFC](https://tools.ietf.org/html/rfc5661):

- **Sistema de archivos de pseudo**, un sistema de archivos que separa el espacio de nombres lógico y físico y es compatible con NFS versión 3 y NFS versión 2. Se proporciona un alias para el sistema de archivo exportado, que forma parte del sistema de archivos pseudo.
- **Compuesta RPC** combinar operaciones pertinentes y reducir el intercambio de mensajes.
- **Enlace troncal de sesiones y sesión** permite solo una semántica y permite la disponibilidad continua y un mejor rendimiento al uso de varias redes entre los clientes de NFS 4.1 y el servidor NFS.

## <a name="nfs-infrastructure"></a>Infraestructura NFS

Mejoras en la infraestructura general de NFS en Windows Server 2012 se detallan a continuación:

- El **llamada a procedimiento remoto (RPC) / representación de datos externos (XDR)** infraestructura de transporte, con la tecnología del protocolo de red WinSock, está disponible para ambos servidor para NFS y cliente para NFS. Esto reemplaza la interfaz de dispositivo de transporte (TDI), ofrece una mejor compatibilidad y proporciona una mejor escalabilidad y escalado de lado de recepción (RSS).
- El **puerto RPC multiplexor** característica es compatible con firewall (menos puertos para administrar) y simplifica la implementación de NFS.
- **Ajuste automático de las memorias caché y los grupos de subprocesos** son recursos de capacidades de administración de la nueva infraestructura RPC/XDR que son dinámicas, optimización automáticamente las memorias caché y según la carga de trabajo de grupos de subprocesos. Esto quita completamente las conjeturas implicadas al optimizar los parámetros, proporcionando un rendimiento óptimo, tan pronto como se implementa NFS.
- **Nuevas opciones Kerberos de autenticación y la implementación de privacidad** con la adición de compatibilidad de privacidad (Krb5p) de Kerberos junto con los krb5 existentes y las opciones de autenticación krb5i.
- **Módulo de PowerShell de Windows de asignación de identidad** cmdlets que sea más fácil administrar la asignación de identidades, configurar Active Directory Lightweight Directory Services (AD LDS) y configurar archivos sin formato y passwd UNIX y Linux.
- **Punto de montaje** le permite acceder a los volúmenes montados en un recurso compartido NFS con la versión 4.1 de NFS.
- El **puerto multiplexación** característica es compatible con el puerto RPC multiplexor (puerto 2049), que es compatible con firewall y simplifica la implementación de NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilidad continua de la versión 3 de NFS

Los clientes NFS versión 3 pueden tener rápida y transparente conmutaciones por error planeadas más disponibilidad y tiempo de inactividad reducido. El proceso de conmutación por error es más rápido para los clientes NFS versión 3 porque:

- La infraestructura de agrupación en clústeres permite ahora un recurso por nombre de red en lugar de un recurso por recurso compartido, lo que mejora significativamente el tiempo de conmutación por error de los recursos.
- Las rutas de acceso de conmutación por error dentro de un servidor NFS se optimizan para mejorar el rendimiento.
- Ya no es necesario el registro de carácter comodín en un servidor NFS y las conmutaciones por error son más optimizados.
- Después de un proceso de conmutación por error se envían las notificaciones del Monitor de estado (NSM) de red y los clientes ya no necesitan esperar tiempos de espera TCP para volver a conectar con el error a través del servidor.

Observe que Servidor para NFS admite la conmutación por error transparente, pero solo cuando se inicia de manera manual (por lo general, durante las labores de mantenimiento planeado). Si se produce una conmutación por error no planeada, los clientes NFS pierden sus conexiones. Servidor para NFS también no tiene ninguna integración con el filtro de clave de reanudación. Esto significa que si una aplicación local o una sesión SMB intenta tener acceso al mismo archivo al que tiene acceso un cliente NFS inmediatamente después de una conmutación por error planeada, el cliente NFS podría perder sus conexiones (se produciría un error en la conmutación por error transparente).

## <a name="deployment-and-manageability-improvements"></a>Mejoras de implementación y facilidad de uso

Implementación y administración de NFS se ha mejorado de las maneras siguientes:

- Más de cuarenta nuevos cmdlets de PowerShell de Windows facilitan la configuración y administración de recursos compartidos de archivos NFS. Para obtener más información, consulte [Cmdlets de NFS en Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Asignación de identidad se ha mejorado con un almacén de asignación de archivo sin formato local y nuevos cmdlets de Windows PowerShell para configurar la asignación de identidades.
- La interfaz gráfica de usuario de administrador del servidor es más fácil de usar.
- El nuevo proveedor de la versión 2 de WMI está disponible para facilitar la administración.
- El puerto multiplexor (puerto RPC 2049) es compatible con firewall y simplifica la implementación de NFS.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor

En las versiones más recientes o de administrador del servidor - [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -usar Agregar Roles y características Asistente para agregar el servicio servidor para NFS rol (en el archivo e iSCSI rol de servicios). Para obtener información general sobre la instalación de características, vea el tema sobre la [instalación o desinstalación de roles, servicios de rol o características](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Herramientas NFS de servidor para incluir los servicios para el complemento MMC de sistema de archivos de red para administrar el servidor para NFS y cliente para los componentes NFS. Mediante el complemento, puede administrar el servidor de componentes NFS instalados en el equipo. Servidor para NFS también contiene varias herramientas de línea de comandos de administración de Windows:

- **Montar** monta un recurso de compartido NFS remoto (también conocido como una exportación) localmente y lo asigna a una letra de unidad local en el equipo cliente de Windows.
- **Nfsadmin** administra la configuración del servidor para NFS y cliente para los componentes NFS.
- **Nfsshare** configura las opciones de recurso compartido NFS para carpetas compartidas mediante servidor para NFS.
- **Nfsstat** muestra o restablece las estadísticas de llamadas recibidas por el servidor para NFS.
- **Showmount** muestra los sistemas de archivos montado exportados por servidor para NFS.
- **Umount** quita las unidades de montaje NFS.

NFS en Windows Server 2012 incorpora el módulo de NFS para Windows PowerShell con varios cmdlets nuevos específicamente para NFS. Estos cmdlets ofrecen una manera fácil de automatizar las tareas de administración de NFS. Para obtener más información, consulte [cmdlets de NFS en Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Información adicional

En la tabla siguiente proporciona recursos adicionales para evaluar NFS.

|Tipo de contenido|Referencias|
|---|---|
|Implementación|[Implementar el sistema de archivos de red](deploy-nfs.md)|
|Operaciones|[Cmdlets de NFS en Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologías relacionadas|[Almacenamiento en Windows Server](../storage.md)|