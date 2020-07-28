---
title: Introducción a Network File System
description: Explica qué es Network File System.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: aff9fbdfa6dc97cb644e207efdae9c44533c320b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181751"
---
# <a name="network-file-system-overview"></a>Introducción a Network File System

>Se aplica a: Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema se describen el servicio de rol sistema de archivos de red y las características incluidas con el rol de servidor servicios de archivos y almacenamiento en Windows Server. Network File System (NFS) proporciona una solución de uso compartido de archivos para empresas que tienen entornos heterogéneos que incluyen equipos Windows y otros que no son de Windows.

## <a name="feature-description"></a>Descripción de la característica

Mediante el protocolo NFS, puede transferir archivos entre equipos que ejecutan Windows y otros sistemas operativos que no son de Windows, como Linux o UNIX.

NFS en Windows Server incluye servidor para NFS y cliente para NFS. Un equipo que ejecuta Windows Server puede usar servidor para NFS para actuar como un servidor de archivos NFS para otros equipos cliente que no son de Windows. Cliente para NFS permite a un equipo basado en Windows que ejecuta Windows Server tener acceso a archivos almacenados en un servidor que no es de Windows NFS.

## <a name="windows-and-windows-server-versions"></a>Versiones de Windows y Windows Server

Windows admite varias versiones del cliente y el servidor NFS, dependiendo de la versión del sistema operativo y de la familia.

| Sistemas operativos | Versiones de servidor NFS |Versiones de cliente NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv 4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Aplicaciones prácticas

Estas son algunas formas de usar NFS:

- Use un servidor de archivos NFS de Windows para proporcionar acceso multiprotocolo al mismo recurso compartido de archivos a través de los protocolos SMB y NFS de los clientes de varias plataformas.
- Implemente un servidor de archivos NFS de Windows en un entorno de sistema operativo que no sea Windows, principalmente, para proporcionar a los equipos cliente que no son de Windows acceso a recursos compartidos de archivos NFS.
- Migre las aplicaciones de un sistema operativo a otro mediante el almacenamiento de los datos en recursos compartidos de archivos accesibles a través de los protocolos SMB y NFS.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

La funcionalidad nueva y modificada en Network File System incluye compatibilidad con la versión 4,1 de NFS y una mejor implementación y facilidad de administración. Para obtener información acerca de las funciones nuevas o modificadas en Windows Server 2012, revise la siguiente tabla:

|Característica/funcionalidad|Nueva o actualizada|Descripción|
|---|---|---|
|[Versión 4,1 de NFS](#nfs-version-41)|Nuevo|Aumento de la seguridad, el rendimiento y la interoperabilidad en comparación con la versión 3 de NFS.|
|[Infraestructura de NFS](#nfs-infrastructure)|Actualizado|Mejora la implementación y la capacidad de administración, y aumenta la seguridad.|
|[Disponibilidad continua de NFS versión 3](#nfs-version-3-continuous-availability)|Actualizado|Mejora la disponibilidad continua en los clientes de NFS versión 3.|
|[Mejoras en la implementación y la facilidad de administración](#deployment-and-manageability-improvements)|Actualizado|Permite implementar y administrar fácilmente NFS con nuevos cmdlets de Windows PowerShell y un nuevo proveedor de WMI.|

## <a name="nfs-version-41"></a>Versión 4,1 de NFS

La versión 4,1 de NFS implementa todos los aspectos necesarios, además de algunos de los aspectos opcionales, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- El **sistema**de archivos es un sistema de archivos que separa el espacio de nombres físico y lógico y es compatible con NFS versión 3 y NFS versión 2. Se proporciona un alias para el sistema de archivos exportado, que forma parte del sistema de pseudo archivo.
- Las **RPC compuestas** combinan las operaciones pertinentes y reducen el chat.
- Las **sesiones y la Troncalización de sesión** permiten solo una semántica y permiten una disponibilidad continua y un mejor rendimiento, al mismo tiempo que se usan varias redes entre clientes NFS 4,1 y el servidor NFS.

## <a name="nfs-infrastructure"></a>Infraestructura de NFS

A continuación se detallan las mejoras en la infraestructura de NFS general en Windows Server 2012:

- La infraestructura de transporte de **representación de datos (/external) de la llamada a procedimiento remoto (RPC)** , basada en el protocolo de red Winsock, está disponible para servidor para NFS y cliente para NFS. Esto reemplaza la interfaz de dispositivo de transporte (TDI), ofrece una mejor compatibilidad y proporciona una mejor escalabilidad y escalado en lado de recepción (RSS).
- La característica de **multiplexor de Puerto RPC** es compatible con firewall (menos puertos para administrar) y simplifica la implementación de NFS.
- Las **memorias caché y los grupos de subprocesos optimizados automáticamente** son funciones de administración de recursos de la nueva infraestructura de RPC/XDR que son dinámicas, ajustando automáticamente las cachés y los grupos de subprocesos basados en la carga de trabajo. Esto elimina completamente las conjeturas que conlleva la optimización de los parámetros, lo que proporciona un rendimiento óptimo en cuanto se implementa NFS.
- **Nuevas opciones de autenticación y implementación de privacidad de Kerberos** con la adición de compatibilidad con la privacidad de Kerberos (Krb5p) junto con las opciones de autenticación de krb5 y krb5i existentes.
- Los cmdlets del **módulo de Windows PowerShell de asignación de identidad** facilitan la administración de la asignación de identidades, la configuración de Active Directory Lightweight Directory Services (AD LDS) y la configuración de archivos sin formato y de la passwd de UNIX y Linux.
- El **punto de montaje de volumen** permite acceder a los volúmenes montados en un recurso compartido de NFS con la versión 4,1 de NFS.
- La característica de **multiplexación de puertos** es compatible con el multiplexor de puertos RPC (puerto 2049), que es fácil de usar para el firewall y simplifica la implementación de NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilidad continua de NFS versión 3

Los clientes de NFS versión 3 pueden tener conmutaciones por error planeadas rápidas y transparentes con más disponibilidad y menor tiempo de inactividad. El proceso de conmutación por error es más rápido para los clientes de NFS versión 3 porque:

- La infraestructura de agrupación en clústeres ahora permite un recurso por cada nombre de red en lugar de un recurso por recurso compartido, lo que mejora significativamente el tiempo de conmutación por error de los recursos.
- Las rutas de acceso de conmutación por error dentro de un servidor NFS se optimizan para mejorar el rendimiento.
- Ya no es necesario el registro de caracteres comodín en un servidor NFS y las conmutaciones por error están más ajustadas.
- Las notificaciones de Monitor de estado de red (NSM) se envían después de un proceso de conmutación por error, y los clientes ya no necesitan esperar a que los tiempos de espera de TCP vuelvan a conectarse al servidor que ha conmutado por error.

Observe que Servidor para NFS admite la conmutación por error transparente, pero solo cuando se inicia de manera manual (por lo general, durante las labores de mantenimiento planeado). Si se produce una conmutación por error no planeada, los clientes NFS pierden sus conexiones. Servidor para NFS tampoco tiene ninguna integración con el filtro de clave de reanudación. Esto significa que si una aplicación local o una sesión SMB intenta tener acceso al mismo archivo al que tiene acceso un cliente NFS inmediatamente después de una conmutación por error planeada, el cliente NFS podría perder sus conexiones (se produciría un error en la conmutación por error transparente).

## <a name="deployment-and-manageability-improvements"></a>Mejoras en la implementación y la facilidad de administración

La implementación y administración de NFS se ha mejorado de las siguientes maneras:

- Más de 40 nuevos cmdlets de Windows PowerShell facilitan la configuración y administración de recursos compartidos de archivos NFS. Para obtener más información, consulte [cmdlets de NFS en Windows PowerShell](/powershell/module/nfs/?view=win10-ps).
- La asignación de identidades se ha mejorado con un almacén de asignación de archivos sin formato local y nuevos cmdlets de Windows PowerShell para configurar la asignación de identidades.
- La interfaz gráfica de usuario Administrador del servidor es más fácil de usar.
- El nuevo proveedor de WMI versión 2 está disponible para facilitar la administración.
- El multiplexor de Puerto RPC (puerto 2049) es compatible con firewall y simplifica la implementación de NFS.

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor

En Administrador del servidor-o en el [centro de administración de Windows](../../manage/windows-admin-center/overview.md) más reciente: Use el Asistente para agregar roles y características para agregar el servicio de rol servidor para NFS (en el rol servicios de archivos e iSCSI). Para obtener información general sobre la instalación de características, vea el tema sobre la [instalación o desinstalación de roles, servicios de rol o características](</previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Las herramientas de servidor para NFS incluyen el complemento Servicios para Network File System MMC para administrar los componentes de servidor para NFS y cliente para NFS. Con el complemento, puede administrar los componentes de servidor para NFS instalados en el equipo. Servidor para NFS también contiene varias herramientas de administración de línea de comandos de Windows:

- **Montar** monta un recurso compartido de NFS remoto (también conocido como exportación) localmente y lo asigna a una letra de unidad local en el equipo cliente de Windows.
- **Nfsadmin** administra los valores de configuración de los componentes de servidor para NFS y cliente para NFS.
- **Nfsshare** configura las opciones del recurso compartido NFS para las carpetas que se comparten con servidor para NFS.
- **Nfsstat** muestra o restablece las estadísticas de llamadas recibidas por servidor para NFS.
- **Showmount** muestra los sistemas de archivos montados exportados por servidor para NFS.
- **Umount** quita las unidades montadas por NFS.

NFS en Windows Server 2012 presenta el módulo NFS para Windows PowerShell con varios cmdlets nuevos específicamente para NFS. Estos cmdlets proporcionan una manera sencilla de automatizar las tareas de administración de NFS. Para obtener más información, consulte [cmdlets de NFS en Windows PowerShell](/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Información adicional

En la tabla siguiente se proporcionan recursos adicionales para evaluar NFS.

|Tipo de contenido|Referencias|
|---|---|
|Implementación|[Implementar el sistema de archivos de red](deploy-nfs.md)|
|Operaciones|[Cmdlets de NFS en Windows PowerShell](/powershell/module/nfs/?view=win10-ps)|
|Tecnologías relacionadas|[Almacenamiento en Windows Server](../storage.yml)|
