---
title: Administración de servidores con Windows Admin Center
description: Administración de servidores con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 04ade4a14272c7840b5036ca6ad5a3bf3d09bcf9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891156"
---
# <a name="manage-servers-with-windows-admin-center"></a>Administración de servidores con Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Administración de máquinas de Windows Server

Puede agregar servidores individuales que ejecutan Windows Server 2012 o posterior a Windows Admin Center para administrar el servidor con un conjunto completo de herramientas, incluidos certificados, dispositivos, eventos, procesos, Roles y características, actualizaciones, las máquinas virtuales y mucho más.

![Pantalla de información general de conexión de servidor](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Agregar un servidor a Windows Admin Center

Para agregar un servidor a Windows Admin Center:

1. Haga clic en **+ agregar** en todas las conexiones.
2. Optar por agregar una **conexión de servidor**.
3. Escriba el nombre del servidor y, si se le pide las credenciales para usar.
4. Haga clic en **enviar** para finalizar.

El servidor se agregará a la lista de conexiones en la página información general. Haga clic en él para conectarse al servidor.

> [!NOTE]
> También puede agregar [clústeres de conmutación por error](manage-failover-clusters.md) o [clústeres hiperconvergidos](manage-hyper-converged.md) como una conexión independiente en Windows Admin Center.

## <a name="tools"></a>Herramientas

Las siguientes herramientas están disponibles para las conexiones del servidor:

| Herramienta | Descripción |
| ---- | ----------- |
| [Información general](#overview) | Ver los detalles del servidor y controlar el estado del servidor |
| [Backup](#backup) | Ver y configurar Azure Backup |  
| [Certificados](#certificates) | Ver y modificar certificados |
| [Contenedores](#containers) | Ver contenedores |
| [Dispositivos](#devices) | Ver y modificar los dispositivos |
| [Eventos](#events) | Ver eventos |
| [Archivos](#files) | Examinar archivos y carpetas |
| [Firewall](#firewall) | Ver y modificar las reglas de firewall |
| [Aplicaciones instaladas](#installed-apps) | Ver y quitar las aplicaciones instaladas |
| [Usuarios y grupos locales](#local-users-and-groups) | Ver y modificar usuarios y grupos locales |
| [Red](#network) | Ver y modificar los dispositivos de red |
| [PowerShell](#powershell) | Interactuar con el servidor a través de PowerShell |
| [Procesos](#processes) | Ver y modificar los procesos en ejecución |
| [Registry](#registry) | Ver y modificar las entradas del registro |
| [Escritorio remoto](#remote-desktop) | Interactuar con el servidor a través de escritorio remoto |
| [Roles y características](#roles-and-features) | Ver y modificar los roles y características |
| [Tareas programadas](#scheduled-tasks) | Ver y modificar las tareas programadas |
| [Servicios](#services) | Ver y modificar servicios |
| [Almacenamiento](#storage) | Ver y modificar los dispositivos de almacenamiento |
| [Servicio de migración de almacenamiento](#storage-migration-service) | Migrar servidores y recursos compartidos de archivos en Azure o en Windows Server 2019 |
| [Réplica de almacenamiento](#storage-replica) | Usar réplica de almacenamiento para administrar la replicación de almacenamiento de servidor a servidor |
| [Información del sistema](#system-insights) | Sistema Insights proporciona una mayor información sobre el funcionamiento del servidor. |
| [Updates](#updates) | Vista instalado y compruebe si hay nuevas actualizaciones |
| [Máquinas virtuales](manage-virtual-machines.md) | Ver y administrar máquinas virtuales |
| [Conmutadores virtuales](#virtual-switches) | Ver y administrar los conmutadores virtuales |

## <a name="overview"></a>Información general

**Información general sobre** también le permite ver el estado actual de la CPU, memoria y rendimiento de la red, como realizar operaciones y modificar la configuración en un servidor o equipo de destino.

### <a name="features"></a>Características

En información general del administrador del servidor, se admiten las siguientes características:

- Vista Detalles del servidor
- Actividad de CPU de la vista
- Ver actividad de memoria
- Ver actividad de red
- Reiniciar servidor
- Apagar servidor
- Habilitar las métricas de disco en el servidor
- Editar el identificador de equipo en el servidor

[**Ver los comentarios y las características propuestas para información general sobre servidor**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="backup"></a>Copias de seguridad

**Copia de seguridad** le permite proteger el servidor de Windows de los daños, ataques o desastres haciendo una copia de seguridad del servidor directamente a Microsoft Azure.
[Más información sobre la copia de seguridad de Azure.](https://aka.ms/windows-admin-center-backup)

[Proporcionar comentarios para copia de seguridad en Windows Admin Center](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Características

Se admiten las siguientes características de copia de seguridad:

- Ver información general sobre el estado de copia de seguridad de Azure
- Configurar elementos de copia de seguridad y programación
- Iniciar o detener un trabajo de copia de seguridad
- Ver el estado y el historial de trabajos de copia de seguridad
- Ver puntos de recuperación y recuperar datos
- Eliminar datos de copia de seguridad

## <a name="certificates"></a>Certificados

**Certificados** le permite administrar almacenes de certificados en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en los certificados:

- Examinar y buscar certificados existentes
- Ver detalles del certificado
- Exportación de certificados
- Renovar certificados
- Solicitar certificados nuevos
- Eliminar certificados

[**Ver los comentarios y las características propuestas para certificados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Contenedores

**Contenedores** le permite ver los contenedores en un host de contenedor de Windows Server. En el caso de un contenedor en ejecución Windows Server Core, puede ver los registros de eventos y obtener acceso a la CLI del contenedor.

[**Ver los comentarios y las características propuestas para contenedores**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Dispositivos

**Dispositivos** le permite administrar los dispositivos conectados en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en los dispositivos:

- Exploración y búsqueda de dispositivos
- Ver detalles del dispositivo
- Deshabilitar un dispositivo
- Actualización del controlador en un dispositivo

[**Ver los comentarios y las características propuestas para dispositivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="events"></a>Eventos

**Eventos** le permite administrar registros de eventos en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en los eventos:

- Eventos de exploración y búsqueda
- Ver detalles del evento
- Borrar eventos del registro
- Eventos de exportación del registro

[**Ver los comentarios y las características propuestas para eventos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>Archivos

**Archivos** le permite administrar archivos y carpetas en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en los archivos:

- Examinar archivos y carpetas
- Buscar un archivo o carpeta
- Cree una nueva carpeta
- Eliminar un archivo o carpeta
- Descargar un archivo o carpeta
- Cargar un archivo o carpeta
- Cambiar el nombre de un archivo o carpeta
- Extraer un archivo zip
- Ver las propiedades de archivo o carpeta
- Administrar el Administrador de recursos del servidor de archivos [cuotas](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- Agregar, editar o quitar recursos compartidos de archivos
- Modificar permisos de usuario y grupo de recursos compartidos de archivos

[**Ver los comentarios y las características propuestas para archivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Firewall

**Firewall** le permite administrar la configuración de firewall y reglas en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en el Firewall:

- Una visión general de configuración de firewall
- Ver las reglas de firewall entrantes
- Ver reglas de firewall de salida
- Buscar reglas de firewall
- Vista Detalles de la regla de firewall
- Crear una nueva regla de firewall
- Habilitar o deshabilitar una regla de firewall
- Eliminar una regla de firewall
- Editar las propiedades de una regla de firewall

[**Ver los comentarios y las características propuestas para Firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>Aplicaciones instaladas

**Las aplicaciones instaladas** le permite enumerar y desinstalar la aplicación que se instalan.

[**Ver los comentarios y las características propuestas para aplicaciones instaladas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Usuarios y grupos locales

**Los usuarios y grupos locales** le permite administrar los grupos de seguridad y los usuarios que existen localmente en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en usuarios y grupos locales:

- Ver y buscar usuarios y grupos
- Crear un nuevo usuario o grupo
- Administrar la pertenencia a grupos del usuario
- Eliminar un usuario o grupo
- Cambiar una contraseña de usuario
- Editar las propiedades de un usuario o grupo

[**Ver los comentarios y las características propuestas para usuarios y grupos locales**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Red

**Red** le permite administrar dispositivos de red y la configuración en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características de red:

- Examinar y buscar los adaptadores de red existente
- Ver detalles de un adaptador de red
- Editar propiedades de un adaptador de red
- Crear un [adaptador de red de Azure (vista previa)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Ver los comentarios y las características propuestas de red**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** le permite interactuar con un equipo o servidor a través de una sesión de PowerShell.

### <a name="features"></a>Características

Las siguientes características se admiten en PowerShell:

- Crear una sesión de PowerShell interactiva en el servidor
- Desconectarse de la sesión de PowerShell en el servidor

[**Ver los comentarios y las características propuestas para PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Procesos

**Procesos** le permite administrar los procesos en ejecución en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en los procesos:

- Examinar y buscar los procesos en ejecución
- Ver detalles del proceso
- Iniciar un proceso
- Finalizar un proceso
- Crear un volcado del proceso
- Encontrar identificadores de proceso

[**Ver los comentarios y las características propuestas para procesos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registro

**Registro** le permite administrar las claves del registro y los valores en un equipo o servidor.

### <a name="features"></a>Características

En el registro, se admiten las siguientes características:

- Examinar los valores y claves del registro
- Agregar o modificar los valores del registro
- Eliminar los valores del registro

[**Ver los comentarios y las características propuestas para registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Escritorio remoto

**Escritorio remoto** le permite interactuar con un equipo o servidor a través de una sesión de escritorio interactiva.

### <a name="features"></a>Características

En el escritorio remoto, se admiten las siguientes características:

- Iniciar una sesión interactiva de escritorio remota
- Desconectarse de una sesión de escritorio remota
- Enviar Ctrl + Alt + Supr a una sesión de escritorio remoto

[**Ver los comentarios y las características propuestas para escritorio remoto**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Roles y características

**Roles y características** le permite administrar roles y características en un servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en Roles y características:

- Examinar la lista de roles y características en un servidor
- Ver los detalles de rol o característica
- Instalar un rol o característica
- Quitar un rol o característica

[**Ver los comentarios y las características propuestas para Roles y características**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>TAREAS PROGRAMADAS

**Las tareas programadas** le permite administrar las tareas programadas en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en las tareas programadas:

- Examinar la biblioteca del programador de tareas
- Editar tareas programadas
- Habilitar y deshabilitar las tareas programadas
- Iniciar y detener las tareas programadas
- Crear tareas programadas

[**Ver los comentarios y las características propuestas para tareas programadas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Servicios

**Servicios** le permite administrar servicios en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en servicios:

- Servicios de exploración y búsqueda en un servidor
- Ver detalles de un servicio
- Iniciar un servicio
- Pausar un servicio
- Editar las propiedades de un servicio

[**Ver los comentarios y las características propuestas de servicios**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="storage"></a>Almacenamiento

**Almacenamiento** le permite administrar dispositivos de almacenamiento en un equipo o servidor.

### <a name="features"></a>Características

En el almacenamiento, se admiten las siguientes características:

- Examinar y buscar los discos existentes en un servidor
- Ver los detalles del disco
- Crear un volumen
- Inicializar un disco
- Crear, adjuntar y desasociar un disco duro virtual (VHD)
- Desconectar un disco
- Formatear un volumen
- Cambiar el tamaño de un volumen
- Editar propiedades de volumen
- Eliminar un volumen
- Instalar Administración de cuotas

[**Ver los comentarios y las características propuestas para almacenamiento**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Servicio de migración de almacenamiento

**Servicio de migración de almacenamiento** le permite migrar los servidores y recursos compartidos en Azure o en Windows Server 2019 de archivos, sin necesidad de aplicaciones o a los usuarios cambiar nada.
[Obtenga información general acerca del servicio de migración de almacenamiento](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Servicio de migración de almacenamiento requiere Windows Server 2019.

## <a name="storage-replica"></a>Réplica de almacenamiento

Use **réplica de almacenamiento** para administrar la replicación de almacenamiento de servidor a servidor.
[Más información sobre réplica de almacenamiento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Información del sistema

**Información del sistema** presenta el análisis predictivo de forma nativa en Windows Server que le ayudarán a tener mayor información sobre el funcionamiento del servidor.
[Obtenga información general de información del sistema](http://aka.ms/systeminsights)

>[!NOTE]
>Información del sistema requiere Windows Server 2019.

## <a name="updates"></a>Actualizaciones

**Las actualizaciones** le permite administrar las actualizaciones de Windows o de Microsoft en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en las actualizaciones:

- Ver disponibles de Windows o Microsoft Updates
- Ver una lista de historial de actualización
- Instalar actualizaciones
- Buscar actualizaciones desde Microsoft Update en línea
- Administrar [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management) integración

[**Ver los comentarios y las características propuestas para las actualizaciones**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Máquinas virtuales

Consulte [administración de máquinas virtuales con Windows Admin Center](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Conmutadores virtuales

**Conmutadores virtuales** le permite administrar conmutadores virtuales de Hyper-V en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en los conmutadores virtuales:

- Examinar y buscar los conmutadores virtuales en un servidor
- Crear un nuevo conmutador Virtual
- Cambiar el nombre de un conmutador Virtual
- Eliminar un conmutador Virtual existente
- Editar las propiedades de un conmutador Virtual

[**Ver los comentarios y las características propuestas para conmutadores virtuales**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
