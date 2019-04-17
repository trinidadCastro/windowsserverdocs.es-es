---
title: Introducción al sistema de archivos de red
description: Explica lo que es el sistema de archivos de red.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: bad0692ffd0773f61ac62bd140a421cb02c68acf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099144"
---
# Introducción al sistema de archivos de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe el servicio de rol de sistema de archivos de red y las características que se incluyen con el rol de servidor de servicios de archivos y almacenamiento en Windows Server. Network File System (NFS) proporciona un archivo de solución para las empresas que tienen entornos heterogéneos que incluya Windows y equipos que no son de Windows de uso compartido.

## Descripción de la característica

Mediante el protocolo NFS, puede transferir archivos entre equipos que ejecutan Windows y otros sistemas operativos que no son de Windows, como Linux o UNIX.

NFS en Windows Server incluye servidor para NFS y cliente para NFS. Un equipo que ejecuta Windows Server puede utilizar el servidor para NFS para que actúe como un servidor de archivos NFS para otros equipos cliente que no son de Windows. Cliente para NFS permite a un equipo basado en Windows que se ejecuta Windows Server para acceder a los archivos almacenados en un servidor NFS que no son de Windows.

## Versiones de Windows y Windows Server

Windows admite varias versiones del cliente para NFS y del servidor, según la versión del sistema operativo y familiares. 

| Sistemas operativos | Versiones de servidor NFS |Versiones de cliente NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFS v2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFS v2, NFSv3 | NFS v2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFS v2, NFSv3, NFSv4.1  | NFS v2, NFSv3 |

## Aplicaciones prácticas

Estas son algunas formas de usar NFS:

- Usar un servidor de archivos NFS de Windows para proporcionar acceso de múltiples protocolos para el mismo recurso compartido de archivos a través de protocolos SMB y NFS desde clientes multiplataformas.
- Implementar un servidor de archivos NFS de Windows en un entorno de sistema operativo que no sea Windows principalmente para proporcionar equipos tengan acceso a recursos compartidos de archivos NFS de cliente que no son de Windows.
- Migrar aplicaciones desde un sistema operativo a otro al almacenar los datos de recursos compartidos de archivos accesibles a través de protocolos de SMB y NFS.

## Funcionalidad nueva y modificada

Funcionalidades nuevas y modificadas en el sistema de archivos de red incluyen compatibilidad con la versión 4.1 NFS e implementación mejorado y facilidad de uso. Para obtener información sobre la funcionalidad de nuevos o cambiados en Windows Server 2012, revise la siguiente tabla:

|Característica/funcionalidad|Nueva o actualizada|Descripción|
|---|---|---|
|[NFS versión 4.1](#nfs-version-4.1)|Nuevo|Una mayor seguridad, rendimiento e interoperabilidad en comparación con NFS versión 3.|
|[Infraestructura NFS](#nfs-infrastructure)|Actualizado|Mejora la implementación y la facilidad de uso y aumenta la seguridad.|
|[Disponibilidad continua de NFS versión 3](#nfs-version-3-continuous-availability)|Actualizado|Mejora la disponibilidad continua de los clientes NFS versión 3.|
|[Mejoras de implementación y la facilidad de uso](#deployment-and-manageability-improvements)|Actualizado|Te permite implementar y administrar NFS con nuevos cmdlets de Windows PowerShell y un nuevo proveedor WMI.|

## NFS versión 4.1

NFS versión 4.1 implementa todos los aspectos necesarios, además de algunos de los aspectos opcionales, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Sistema de archivos de seguimiento**, un sistema de archivos que separa el espacio de nombres física y lógica y es compatible con NFS versión 3 y NFS versión 2. Se proporciona un alias para el sistema de archivos exportado, que forma parte del sistema de archivos pseudo.
- **RPC compuestos** combinan operaciones pertinentes y reducir las conversaciones entre.
- **Sesiones y enlace de sesión** permite una semántica y que la disponibilidad continua y mejorar el rendimiento al usar varias redes entre los clientes de NFS 4.1 y el servidor NFS.

## Infraestructura NFS

A continuación se detallan las mejoras en toda la infraestructura de NFS en Windows Server 2012:

- El **llamada a procedimiento remoto (RPC) y representación de datos externos (XDR)** infraestructura de transporte, que ofrece la tecnología por el protocolo de red de WinSock, está disponible para ambos servidor para NFS y cliente para NFS. This replaces Transport Device Interface (TDI), offers better support, and provides better scalability and Receive Side Scaling (RSS).
- La característica de **puerto RPC multiplexor** está firewalls (menos puertos para administrar) y simplifica la implementación de NFS.
- **Ajusta automáticamente memorias caché y grupos de subprocesos** son funcionalidades de administración de recursos de la nueva infraestructura RPC/XDR que son dinámicos, ajusta automáticamente la memorias caché y grupos en función de la carga de trabajo de subprocesos. Esto elimina completamente las suposiciones implicadas al ajustar los parámetros, proporcionando un rendimiento óptimo tan pronto como se ha implementado NFS.
- **Opciones de implementación y la autenticación de privacidad de Kerberos nuevo** con la adición de compatibilidad con la privacidad (Krb5p) de Kerberos junto con las opciones de autenticación de krb5i y krb5 existente.
- **Módulo de identidad de asignación de Windows PowerShell** cmdlets que sea más fácil administrar la asignación de identidad, configurar Active Directory Lightweight Directory Services (AD LDS) y configurar la contraseña de UNIX y Linux y archivos sin formato.
- **Punto de montaje de volumen** le permite acceder a los volúmenes montados en un recurso compartido NFS con la versión 4.1 NFS.
- La característica de **Multiplexación de puerto** es compatible con el puerto RPC multiplexor (puerto 2049), que se cuentan con asistencia de firewall y simplifica la implementación de NFS.

## Disponibilidad continua de NFS versión 3

Clientes de la versión 3 de NFS pueden tener rápido y transparente conmutaciones planeadas con mayor disponibilidad y el tiempo de inactividad reducido. El proceso de conmutación por error es más rápido para los clientes de la versión 3 de NFS porque:

- La infraestructura de agrupación en clústeres permite a un recurso por nombre de red en lugar de un recurso por recurso compartido, lo que mejora considerablemente el tiempo de conmutación por error los recursos.
- Las rutas de acceso de conmutación por error en un servidor NFS se optimizan para mejorar el rendimiento.
- Registro de caracteres comodín en un servidor NFS ya no es necesario y son más ajustado y las conmutaciones por error.
- Las notificaciones de Monitor de estado (NSM) de red se envían después de un proceso de conmutación por error y los clientes ya no necesitan esperar tiempos de espera TCP a conectar con el error a través de servidor.

Ten en cuenta que el servidor para NFS admite conmutación por error transparente solo cuando se inicie manualmente, por lo general, durante el mantenimiento planeado. Si se produce una conmutación por error no planeada, los clientes NFS perderán sus conexiones. Servidor para NFS también no tiene una integración con el filtro de clave de reanudación. Esto significa que si una sesión SMB o locales de la aplicación intenta acceder al mismo archivo que tiene acceso un cliente para NFS inmediatamente después de una conmutación por error planeada, el cliente para NFS podría perder sus conexiones (no tienen éxito transparente conmutación por error).

## Mejoras de implementación y la facilidad de uso

Implementar y administrar NFS ha mejorado de las siguientes maneras:

- Más de cuarenta nuevos cmdlets de PowerShell de Windows que sea más fácil configurar y administrar recursos compartidos de archivos NFS. Para obtener más información, consulta [Cmdlets NFS en Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Asignación de identidad se ha mejorado con un almacén de asignación de archivo plana local y nuevos cmdlets de Windows PowerShell para configurar la asignación de identidad.
- El Administrador de servidores es más fácil de usar la interfaz gráfica de usuario.
- El nuevo proveedor WMI versión 2 está disponible para facilitar la administración.
- El puerto multiplexor (puerto RPC 2049) es con firewalls y simplifica la implementación de NFS.

## Información sobre el Administrador del servidor

En el administrador del servidor - o el más reciente de [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) - usar el agregar Roles y características Asistente para agregar el servidor para NFS rol servicio (en el archivo y iSCSI rol de servicios). Para obtener información general sobre la instalación de las características, consulte la [instalación o desinstalación de Roles, servicios de rol o características](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). NFS herramientas de servidor para incluir los servicios de complemento MMC de sistema de archivos de red para administrar el servidor para NFS y cliente para componentes NFS. Usar el complemento, puedes administrar el servidor de componentes NFS instalados en el equipo. Servidor para NFS también contiene varias herramientas de línea de comandos de administración de Windows:

- **Montar** un recurso NFS remoto (también conocido como una exportación) se monta localmente y le asigna una letra de unidad local en el equipo cliente de Windows.
- **Nfsadmin** administra las opciones de configuración del servidor para NFS y cliente para componentes NFS.
- **Nfsshare** establece la configuración del recurso compartido NFS para las carpetas que se comparten con servidor para NFS.
- **Nfsstat** muestra o restablece las estadísticas de llamadas recibidas por servidor para NFS.
- **Showmount** muestra los sistemas de archivos montado exportados por servidor para NFS.
- **Umount** quita unidades montadas NFS.

NFS en Windows Server 2012 presenta el módulo NFS para Windows PowerShell con varios nuevos cmdlets específicamente para NFS. Estos cmdlets proporcionan una forma fácil para automatizar las tareas de administración de NFS. Para obtener más información, consulta [cmdlets NFS en Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## Información adicional

La siguiente tabla proporciona recursos adicionales para evaluar NFS.

|Tipo de contenido|Referencias|
|---|---|
|Implementación|[Implementar el sistema de archivos de red](deploy-nfs.md)|
|Operaciones|[Cmdlets NFS en Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologías relacionadas|[Almacenamiento en Windows Server](../storage.md)|