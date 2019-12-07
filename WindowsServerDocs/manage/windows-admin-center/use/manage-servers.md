---
title: Administrar servidores con el centro de administración de Windows
description: Administrar servidores con el centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 11/21/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9a116cc9d86dfe0bb4450efa0f18580a062af722
ms.sourcegitcommit: 7c7fc443ecd0a81bff6ed6dbeeaf4f24582ba339
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2019
ms.locfileid: "74903724"
---
# <a name="manage-servers-with-windows-admin-center"></a>Administrar servidores con el centro de administración de Windows

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Administrar las máquinas de Windows Server

Puede agregar servidores individuales que ejecuten Windows Server 2012 o posterior al centro de administración de Windows para administrar el servidor con un conjunto completo de herramientas, como certificados, dispositivos, eventos, procesos, roles y características, actualizaciones, Virtual Machines y mucho más.

![Pantalla de información general de conexión de servidor](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Agregar un servidor al centro de administración de Windows

Para agregar un servidor al centro de administración de Windows:

1. Haga clic en **+ Agregar** en todas las conexiones.
2. Elija esta opción para agregar una **conexión de servidor**.
3. Escriba el nombre del servidor y, si se le solicita, las credenciales que se usarán.
4. Haga clic en **Enviar** para finalizar.

El servidor se agregará a la lista de conexiones en la página información general. Haga clic en él para conectarse al servidor.

> [!NOTE]
> También puede agregar [clústeres de conmutación por error](manage-failover-clusters.md) o [clústeres hiperconvergidos](manage-hyper-converged.md) como una conexión independiente en el centro de administración de Windows.

## <a name="tools"></a>Herramientas

Las siguientes herramientas están disponibles para las conexiones de servidor:

| Herramienta | Descripción |
| ---- | ----------- |
| [Introducción](#overview) | Ver detalles del servidor y controlar el estado del servidor |
| [Active Directory](#active-directory-preview) | Administrar Active Directory |
| [Backup](#backup) | Ver y configurar Azure Backup |  
| [Certificados](#certificates) | Ver y modificar certificados |
| [Containers](#containers) | Ver contenedores |
| [Dispositivos](#devices) | Ver y modificar dispositivos |
| [DHCP](#dhcp) | Ver y administrar la configuración del servidor DHCP |
| [DNS](#dns) | Ver y administrar la configuración del servidor DNS |
| [Eventos](#events) | Ver eventos |
| [Archivos](#files) | Examen de archivos y carpetas |
| [Firewall](#firewall) | Ver y modificar las reglas de Firewall |
| [Aplicaciones instaladas](#installed-apps) | Visualización y eliminación de las aplicaciones instaladas |
| [Usuarios y grupos locales](#local-users-and-groups) | Ver y modificar usuarios y grupos locales |
| [Network](#network) | Ver y modificar dispositivos de red |
| [Supervisión de paquetes](https://aka.ms/wac1908) | Supervisión de paquetes de red |
| [Monitor de rendimiento](https://aka.ms/perfmon-blog) | Ver informes y contadores de rendimiento |
| [PowerShell](#powershell) | Interacción con el servidor a través de PowerShell |
| [Procesos](#processes) | Ver y modificar procesos en ejecución |
| [Registry](#registry) | Ver y modificar las entradas del registro |
| [Escritorio remoto](#remote-desktop) | Interacción con el servidor a través de Escritorio remoto |
| [Roles y características](#roles-and-features) | Ver y modificar roles y características |
| [Tareas programadas](#scheduled-tasks) | Ver y modificar las tareas programadas |
| [Servicios](#services) | Ver y modificar servicios |
| [Configuración](#settings) | Ver y modificar servicios |
| [Almacenamiento](#storage) | Visualización y modificación de dispositivos de almacenamiento |
| [Servicio de migración de almacenamiento](#storage-migration-service) | Migración de servidores y recursos compartidos de archivos a Azure o Windows Server 2019 |
| [Réplica de almacenamiento](#storage-replica) | Usar réplica de almacenamiento para administrar la replicación de almacenamiento de servidor a servidor |
| [Conclusiones del sistema](#system-insights) | System Insights proporciona un mayor conocimiento del funcionamiento del servidor. |
| [Actualizaciones](#updates) | Ver instalado y comprobar si hay nuevas actualizaciones |
| [Máquinas virtuales](manage-virtual-machines.md) | Ver y administrar máquinas virtuales |
| [Conmutadores virtuales](#virtual-switches) | Visualización y administración de conmutadores virtuales |

## <a name="overview"></a>Introducción

**Información general** le permite ver el estado actual de la CPU, la memoria y el rendimiento de la red, así como realizar operaciones y modificar la configuración de un equipo o servidor de destino.

### <a name="features"></a>Características

En Administrador del servidor información general, se admiten las siguientes características:

- Ver detalles del servidor
- Ver la actividad de la CPU
- Ver la actividad de memoria
- Ver la actividad de la red
- Reinicio del servidor
- Apagar servidor
- Habilitar las métricas de disco en el servidor
- Editar ID. de equipo en el servidor
- Ver la dirección IP de BMC con hipervínculo (requiere BMC compatible con IPMI).

[**Ver comentarios y características propuestas de información general del servidor**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="active-directory-preview"></a>Active Directory (vista previa)

**Active Directory** es una versión preliminar temprana que está disponible en la [fuente de extensión](../configure/using-extensions.md).

### <a name="features"></a>Características

Están disponibles las siguientes opciones de administración de Active Directory:

- Crear usuario
- Crear grupo
- Buscar usuarios, equipos y grupos
- Panel de detalles de usuarios, equipos y grupos cuando se selecciona en la cuadrícula
- Acciones de cuadrícula global usuarios, equipos y grupos (deshabilitar/habilitar, quitar)
- Restablecer contraseñas de usuario
- Objetos de usuario: configurar propiedades básicas & pertenencias a grupos
- Objetos de equipo: configuración de la delegación en un solo equipo
- Objetos de Grupo: administrar la pertenencia (agregar o quitar un usuario a la vez)  

[**Ver comentarios y características propuestas para Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## <a name="backup"></a>Copia de seguridad

La **copia de seguridad** le permite proteger su servidor de Windows frente a daños, ataques o desastres mediante la copia de seguridad del servidor directamente en Microsoft Azure.
[Más información sobre Azure Backup.](https://aka.ms/windows-admin-center-backup)

[Proporcionar comentarios sobre la copia de seguridad en el centro de administración de Windows](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Características

Las siguientes características se admiten en la copia de seguridad:

- Ver una visión general del estado de Azure backup
- Configurar elementos de copia de seguridad y programar
- Iniciar o detener un trabajo de copia de seguridad
- Ver el estado y el historial de trabajos de copia de seguridad
- Ver puntos de recuperación y recuperar datos
- Eliminar datos de la copia de seguridad

## <a name="certificates"></a>Certificados

Los **certificados** permiten administrar almacenes de certificados en un equipo o servidor.

### <a name="features"></a>Características

En los certificados se admiten las siguientes características:

- Examinar y buscar certificados existentes
- Ver detalles del certificado
- Exportación de certificados
- Renovar certificados
- Solicitar nuevos certificados
- Eliminar certificados

[**Ver comentarios y características propuestas para certificados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Contenedores

Los **contenedores** permiten ver los contenedores de un host de contenedor de Windows Server. En el caso de un contenedor de Windows Server Core en ejecución, puede ver los registros de eventos y acceder a la CLI del contenedor.

[**Ver comentarios y características propuestas para contenedores**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Dispositivos

Los **dispositivos** permiten administrar dispositivos conectados en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en los dispositivos:

- Examinar y buscar dispositivos
- Ver los detalles de dispositivo
- Deshabilitar un dispositivo
- Actualizar controlador en un dispositivo

[**Ver comentarios y características propuestas para dispositivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="dhcp"></a>DHCP

**DHCP** permite administrar dispositivos conectados en un equipo o servidor.

### <a name="features"></a>Características

- Crear/configurar/ver ámbitos IPV4 e IPV6
- Crear exclusiones de direcciones y configurar la dirección IP inicial y final
- Crear reservas de direcciones y configurar la dirección MAC del cliente (IPV4), DUID y IAID (IPV6)

[**Ver comentarios y características propuestas para DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## <a name="dns"></a>DNS

**DNS** permite administrar dispositivos conectados en un equipo o servidor.

### <a name="features"></a>Características

- Ver detalles de las zonas de búsqueda directa de DNS, las zonas de búsqueda inversa y los registros DNS
- Crear zonas de búsqueda directa (principal, secundaria o de código auxiliar) y configurar las propiedades de la zona de búsqueda directa
- Crear host (A o AAAA), CNAME o tipo MX de registros DNS
- Configurar las propiedades de los registros DNS
- Crear zonas de búsqueda inversa IPV4 e IPV6 (principal, secundaria y auxiliar), configurar las propiedades de la zona de búsqueda inversa
- Cree PTR, tipo CNAME de registros DNS en la zona de búsqueda inversa.

[**Ver comentarios y características propuestas para DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## <a name="events"></a>Eventos

**Los eventos** permiten administrar registros de eventos en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en eventos:

- Examinar y buscar eventos
- Ver detalles de eventos
- Borrar eventos del registro
- Exportar eventos del registro

[**Ver comentarios y características propuestas para eventos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>Archivos

**Archivos** permite administrar archivos y carpetas en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en los archivos:

- Examen de archivos y carpetas
- Buscar un archivo o una carpeta
- Crear una carpeta nueva
- Eliminar un archivo o una carpeta
- Descargar un archivo o una carpeta
- Carga de un archivo o una carpeta
- cambiar el nombre de un archivo o una carpeta.
- Extraer un archivo zip
- Ver propiedades de archivo o carpeta
- Agregar, editar o quitar recursos compartidos de archivos
- Modificar permisos de usuario y de grupo en recursos compartidos de archivos

[**Ver comentarios y características propuestas para los archivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Firewall

**Firewall** le permite administrar la configuración y las reglas de firewall en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en el Firewall:

- Ver información general de la configuración del firewall
- Ver las reglas de Firewall entrantes
- Ver las reglas de Firewall salientes
- Buscar reglas de Firewall
- Ver detalles de la regla de Firewall
- Crear una nueva regla de firewall
- Habilitar o deshabilitar una regla de Firewall
- Eliminar una regla de firewall
- Editar las propiedades de una regla de Firewall

[**Ver comentarios y características propuestas para el Firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>Aplicaciones instaladas

**Aplicaciones instaladas** le permite enumerar y desinstalar la aplicación que está instalada.

[**Ver comentarios y características propuestas para las aplicaciones instaladas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Usuarios y grupos locales

**Usuarios y grupos locales** permite administrar grupos de seguridad y usuarios que existen de forma local en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en usuarios y grupos locales:

- Ver y buscar usuarios y grupos
- Crear un nuevo usuario o grupo
- Administrar la pertenencia a grupos de un usuario
- Eliminar un usuario o grupo
- Cambia la contraseña de un usuario
- Editar las propiedades de un usuario o grupo

[**Ver comentarios y características propuestas para usuarios y grupos locales**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Red

La **red** le permite administrar la configuración y los dispositivos de red en un equipo o servidor.

### <a name="features"></a>Características

En la red se admiten las siguientes características:

- Examinar y buscar adaptadores de red existentes
- Ver los detalles de un adaptador de red
- Editar propiedades de un adaptador de red
- Creación de un [adaptador de red de Azure (característica de vista previa)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Ver comentarios y características propuestas para la red**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** le permite interactuar con un equipo o un servidor a través de una sesión de PowerShell.

### <a name="features"></a>Características

Las siguientes características se admiten en PowerShell:

- Creación de una sesión de PowerShell interactiva en el servidor
- Desconexión de la sesión de PowerShell en el servidor

[**Ver comentarios y características propuestas para PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processes

**Procesos** permite administrar procesos en ejecución en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en los procesos:

- Examinar y buscar procesos en ejecución
- Ver detalles del proceso
- Iniciar un proceso
- Terminar un proceso
- Crear un volcado de proceso
- Buscar identificadores de proceso

[**Ver comentarios y características propuestas para los procesos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registro

El **registro** permite administrar valores y claves del registro en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en el registro:

- Examinar claves y valores del registro
- Agregar o modificar valores del registro
- Eliminar valores del registro

[**Ver comentarios y características propuestas para el registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Escritorio remoto

**Escritorio remoto** le permite interactuar con un equipo o servidor a través de una sesión de escritorio interactiva.

### <a name="features"></a>Características

Las siguientes características se admiten en Escritorio remoto:

- Iniciar una sesión de escritorio remoto interactiva
- Desconectarse de una sesión de escritorio remoto
- Enviar CTRL + ALT + SUPR a una sesión de escritorio remoto

[**Ver comentarios y características propuestas para escritorio remoto**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Roles y características

**Roles y características** permite administrar roles y características en un servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en roles y características:

- Examinar la lista de roles y características de un servidor
- Ver detalles de roles o características
- Instalar un rol o una característica
- Quitar un rol o una característica

[**Ver comentarios y características propuestas para roles y características**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>TAREAS PROGRAMADAS

**Tareas programadas** permite administrar tareas programadas en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en tareas programadas:

- Examinar la biblioteca del programador de tareas
- Editar tareas programadas
- Habilitar & deshabilitar tareas programadas
- Iniciar & detener tareas programadas
- Crear tareas programadas

[**Ver comentarios y características propuestas para las tareas programadas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Servicios

**Servicios** permite administrar los servicios en un equipo o servidor.

### <a name="features"></a>Características

En los servicios se admiten las siguientes características:

- Examinar y buscar servicios en un servidor
- Ver los detalles de un servicio
- Iniciar un servicio
- Pausar un servicio
- Editar las propiedades de un servicio

[**Ver comentarios y características propuestas para los servicios**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="settings"></a>Configuración

La **configuración** es una ubicación central para administrar la configuración de un equipo o servidor.

### <a name="features"></a>Características

- Ver y modificar las variables de entorno del sistema y del usuario
- Permite ver la configuración de las alertas de supervisión desde [Azure monitor](azure-monitor.md)
- Ver y modificar la configuración de energía
- Ver y modificar la configuración de Escritorio remoto
- Ver y modificar la configuración de control de acceso basado en roles
- Ver y modificar la configuración del host de Hyper-V, si procede

## <a name="storage"></a>Almacenamiento

El **almacenamiento** le permite administrar dispositivos de almacenamiento en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en el almacenamiento:

- Examinar y buscar discos existentes en un servidor
- Ver detalles del disco
- Crear un volumen
- Inicializar un disco
- Crear, adjuntar y desasociar un disco duro virtual (VHD)
- Desconectar un disco
- Dar formato a un volumen
- Cambiar el tamaño de un volumen
- Editar propiedades de volumen
- Eliminar un volumen
- Instalar la administración de cuotas
- Administrar [el almacenamiento de las cuotas del servidor de archivos Administrador de recursos > crear/actualizar cuota](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)

[**Ver comentarios y características propuestas para el almacenamiento**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Servicio de migración de almacenamiento

El **servicio de migración de almacenamiento** permite migrar servidores y recursos compartidos de archivos a Azure o Windows Server 2019, sin necesidad de que las aplicaciones o los usuarios cambien nada.
[Obtener información general sobre el servicio de migración de almacenamiento](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>El servicio de migración de almacenamiento requiere Windows Server 2019.

## <a name="storage-replica"></a>Réplica de almacenamiento

Use **réplica de almacenamiento** para administrar la replicación de almacenamiento de servidor a servidor.
[Más información acerca de réplica de almacenamiento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Conclusiones del sistema

**System Insights** incorpora análisis predictivos de forma nativa en Windows Server para ayudarle a mejorar el funcionamiento del servidor.
[Obtener información general de System Insights](https://aka.ms/systeminsights)

>[!NOTE]
>System Insights requiere Windows Server 2019.

## <a name="updates"></a>Actualizaciones

**Actualizaciones** le permite administrar las actualizaciones de Microsoft y/o Windows en un equipo o servidor.

### <a name="features"></a>Características

Las siguientes características se admiten en las actualizaciones de:

- Ver las actualizaciones de Windows o de Microsoft disponibles
- Ver una lista de historial de actualizaciones
- Instalación de actualizaciones
- Buscar actualizaciones en línea desde Microsoft Update
- Administración de la integración de [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Ver comentarios y características propuestas para actualizaciones**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Máquinas virtuales

Vea [administrar virtual machines con el centro de administración de Windows](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Conmutadores virtuales

Los **conmutadores virtuales** permiten administrar conmutadores virtuales de Hyper-V en un equipo o servidor.

### <a name="features"></a>Características

Se admiten las siguientes características en conmutadores virtuales:

- Examinar y buscar conmutadores virtuales en un servidor
- Crear un nuevo conmutador virtual
- Cambiar el nombre de un conmutador virtual
- Eliminar un conmutador virtual existente
- Editar las propiedades de un conmutador virtual

[**Ver comentarios y características propuestas para conmutadores virtuales**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
