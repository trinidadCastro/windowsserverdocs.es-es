---
title: Administrar servidores con Windows Admin Center
description: Administrar servidores con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93a40345d05a6230596832b2d455d36eee2401b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296687"
---
# Administrar servidores con Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## Administración de máquinas de Windows Server

Puedes agregar servidores individuales que ejecute Windows Server 2012 o posterior para Windows Admin Center para administrar el servidor con un completo conjunto de herramientas, incluidos los certificados, dispositivos, eventos, procesos, Roles y características, actualizaciones, las máquinas virtuales y mucho más.

![Pantalla de información general de conexión de servidor](../media/manage-servers/server-overview.png)

## Agregar un servidor a Windows Admin Center

Para agregar un servidor a Windows Admin Center:

1. Haga clic en **+ Agregar** todas las conexiones.
2. Elegir esta opción para agregar una **Conexión con el servidor**.
3. Escribe el nombre del servidor y, si se te solicite, las credenciales que se usan.
4. Haz clic en **Enviar** para finalizar.

El servidor se agregará a tu lista de conexión en la página información general. Haz clic en él para conectarse al servidor.

> [!NOTE]
> También puedes agregar [los clústeres de conmutación por error](manage-failover-clusters.md) o [clústeres hiperconvergidos](manage-hyper-converged.md) como una conexión independiente en Windows Admin Center.

## Herramientas

Las siguientes herramientas están disponibles para las conexiones de servidor:

| Herramienta | Descripción |
| ---- | ----------- |
| [Introducción](#overview) | Ver detalles del servidor y controlar el estado del servidor |
| [Active Directory](#active-directory-preview) | Administrar Active Directory |
| [Copia de seguridad](#backup) | Ver y configurar la copia de seguridad de Azure |  
| [Certificados](#certificates) | Permite ver y modificar certificados |
| [Contenedores](#containers) | Contenedores de vista |
| [Dispositivos](#devices) | Permite ver y modificar los dispositivos |
| [DHCP](#dhcp) | Ver y administrar la configuración del servidor DHCP |
| [DNS](#dns) | Ver y administrar la configuración del servidor DNS |
| [Eventos](#events) | Eventos de vista |
| [Archivos](#files) | Explorar archivos y carpetas |
| [Firewall](#firewall) | Permite ver y modificar reglas de firewall |
| [Aplicaciones instaladas](#installed-apps) | Ver y quitar aplicaciones instaladas |
| [Usuarios y grupos locales](#local-users-and-groups) | Ver y modificar los usuarios y grupos locales |
| [Red](#network) | Ver y modificar los dispositivos de red |
| [PowerShell](#powershell) | Interactuar con el servidor a través de PowerShell |
| [Procesos](#processes) | Ver y modificar los procesos en ejecución |
| [Registro](#registry) | Permite ver y modificar las entradas del registro |
| [Escritorio remoto](#remote-desktop) | Interactuar con el servidor a través de escritorio remoto |
| [Roles y características](#roles-and-features) | Ver y modificar los roles y características |
| [Tareas programadas](#scheduled-tasks) | Permite ver y modificar las tareas programadas |
| [Servicios](#services) | Permite ver y modificar servicios |
| [Configuración](#settings) | Permite ver y modificar servicios |
| [Almacenamiento](#storage) | Ver y modificar los dispositivos de almacenamiento |
| [Servicio de migración de almacenamiento](#storage-migration-service) | Migrar servidores y recursos compartidos de archivos a Azure o Windows Server 2019 |
| [Réplica de almacenamiento](#storage-replica) | Usar réplica de almacenamiento para administrar la replicación de almacenamiento de servidor a servidor |
| [Información del sistema](#system-insights) | Ofrece de información del sistema que mayor información sobre el funcionamiento del servidor. |
| [Actualizaciones](#updates) | Vista instalado y comprueba si hay nuevas actualizaciones |
| [Máquinas virtuales](manage-virtual-machines.md) | Ver y administrar las máquinas virtuales |
| [Conmutadores virtuales](#virtual-switches) | Ver y administrar los conmutadores virtuales |

## Introducción

**Introducción a** te permite ver el estado actual de la CPU, memoria y rendimiento de la red, así como realizar operaciones y modificar la configuración en un servidor o un equipo de destino.

### Características

Se admiten las siguientes características en Introducción de administrador del servidor:

- Ver detalles del servidor
- Actividad de CPU de vista
- Ver la actividad de memoria
- Ver la actividad de red
- Reinicia el servidor
- Servidor de apagado
- Habilitar las métricas de disco en servidor
- Editar el Id. de equipo en el servidor
- Ver la dirección IP de BMC con hipervínculo (requiere compatible con IPMI BMC).

[**Comentarios de vista y las características propuestas de introducción al servidor**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## Active Directory (versión preliminar)

**Active Directory** es una vista previa que está disponible en la [fuente de extensión](../configure/using-extensions.md).

### Características

La administración de Active Directory siguiente están disponibles:

- Crear usuario
- Crear grupo
- Búsqueda de los usuarios, equipos y grupos
- Panel de detalles de los usuarios, equipos y grupos cuando se selecciona en cuadrícula
- Global Grid acciones que los usuarios, equipos y grupos (quitar, deshabilitar o habilitar)
- Restablecer contraseñas de usuario
- Objetos de usuario: configurar la pertenencia a grupos de & propiedades básicas
- Objetos de equipo: configurar la delegación para un solo equipo
- Objetos de grupo: administrar la pertenencia a (1 Agregar o quitar usuarios a la vez)  

[**Comentarios de vista y características propuestas de Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## Copia de seguridad

**Copia de seguridad** te permite proteger tu servidor de Windows de los daños, los ataques o desastres haciendo una copia de seguridad de tu servidor directamente a Microsoft Azure.
[Más información sobre la copia de seguridad de Azure.](https://aka.ms/windows-admin-center-backup)

[Proporcionar comentarios para copias de seguridad en Windows Admin Center](https://aka.ms/backup-wac-feedback)

### Características

Se admiten las siguientes características de copia de seguridad:

- Una visión general de su estado de copia de seguridad de Azure
- Configurar la programación y elementos de copia de seguridad
- Iniciar o detener un trabajo de copia de seguridad
- Ver el historial de trabajo de copia de seguridad y el estado
- Ver los puntos de recuperación y recuperar datos
- Eliminar los datos de copia de seguridad

## Certificados

**Certificados** te permite administrar almacenes de certificados en un equipo o servidor.

### Características

En los certificados se admiten las siguientes características:

- Explorar y buscar los certificados existentes
- Ver detalles de certificados
- Exportar certificados
- Renovar certificados
- Solicitar certificados nuevos
- Eliminar certificados

[**Comentarios de vista y las características propuestas de certificados**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## Contenedores

**Los contenedores** te permite ver los contenedores en un host de contenedor de Windows Server. En el caso de un contenedor en ejecución Windows Server Core, puedes ver los registros de eventos y acceder a la CLI del contenedor.

[**Comentarios de vista y las características propuestas para contenedores**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## Dispositivos

**Dispositivos** te permite administrar dispositivos conectados en un equipo o servidor.

### Características

Las siguientes características son compatibles con dispositivos:

- Explorar y buscar dispositivos
- Ver detalles del dispositivo
- Deshabilitar un dispositivo
- Actualización del controlador en un dispositivo

[**Comentarios de vista y las características propuestas para dispositivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## DHCP

**DHCP** te permite administrar dispositivos conectados en un equipo o servidor.

### Características

- Ámbitos de crear y configurar o vistas IPV4 e IPV6
- Crear las exclusiones de dirección y configurar la dirección IP de inicio y finalización
- Reservas de direcciones de crear y configurar la dirección MAC del cliente (IPV4), DUID y IAID (IPV6)

[**Comentarios de vista y las características propuestas de DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## DNS

**DNS** te permite administrar dispositivos conectados en un equipo o servidor.

### Características

- Ver detalles de zonas de búsqueda directa de DNS, las zonas de búsqueda inversa y registros DNS
- Crear zonas de búsqueda directas (principal, secundario o código auxiliar) y configurar las propiedades de la zona de búsqueda directa
- Crear la Host (A o AAAA), CNAME o MX el tipo de registros de DNS
- Configurar las propiedades de los registros DNS
- Crear las zonas de IPV4 e IPV6 de búsqueda inversa (principal, secundaria y código auxiliar), configurar las propiedades de zona de búsqueda inversa
- Crear PTR, registra el tipo CNAME de DNS en la zona de búsqueda inversa.

[**Comentarios de vista y las características propuestas de DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## Eventos

**Eventos** te permite administrar los registros de eventos en un equipo o servidor.

### Características

En los eventos, se admiten las siguientes características:

- Eventos de exploración y búsqueda
- Ver detalles del evento
- Borrar eventos del registro
- Exportar eventos desde el registro

[**Comentarios de vista y características propuestas de eventos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## Archivos

**Archivos** te permite administrar archivos y carpetas en un equipo o servidor.

### Características

En los archivos, se admiten las siguientes características:

- Explorar archivos y carpetas
- Buscar un archivo o carpeta
- Crea una nueva carpeta
- Eliminar un archivo o carpeta
- Descargar un archivo o carpeta
- Cargar un archivo o carpeta
- Cambiar el nombre de un archivo o carpeta
- Extraer un archivo zip
- Ver propiedades de archivo o carpeta
- Administrar [cuotas](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management) del Administrador de recursos del servidor de archivos
- Agregar, editar o quitar los recursos compartidos de archivos
- Modificar permisos de usuario y grupo de recursos compartidos de archivos

[**Comentarios de vista y características propuestas de archivos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## Firewall

**Firewall** te permite administrar la configuración de firewall y las reglas en un equipo o servidor.

### Características

Se admiten las siguientes características de Firewall:

- Ver una descripción general de la configuración del firewall
- Ver las reglas de firewall entrantes
- Vista de reglas de firewall de salida
- Reglas de firewall de búsqueda
- Ver detalles de la regla de firewall
- Crear una nueva regla de firewall
- Habilitar o deshabilitar una regla de firewall
- Eliminar una regla de firewall
- Editar las propiedades de una regla de firewall

[**Comentarios de vista y las características propuestas de Firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## Aplicaciones instaladas

**Aplicaciones instaladas** te permite enumerar y desinstalar la aplicación que se instalan.

[**Comentarios de vista y las características propuestas para las aplicaciones instaladas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## Usuarios y grupos locales

**Usuarios y grupos locales** te permite administrar los grupos de seguridad y los usuarios que existen localmente en un equipo o servidor.

### Características

En usuarios y grupos locales, se admiten las siguientes características:

- Ver y buscar usuarios y grupos
- Crear un nuevo usuario o grupo
- Administra la pertenencia de un usuario
- Eliminar un usuario o grupo
- Cambiar la contraseña de un usuario
- Editar las propiedades de un usuario o grupo

[**Ver los comentarios y características propuestas de usuario y grupos locales**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## Red

**Red** te permite administrar dispositivos de red y la configuración en un equipo o servidor.

### Características

Se admiten las siguientes características de red:

- Explorar y buscar los adaptadores de red existentes
- Ver detalles de un adaptador de red
- Editar propiedades de un adaptador de red
- Crear un [Adaptador de red de Azure (característica de vista previa)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Ver los comentarios y características propuestas de red**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## PowerShell

**PowerShell** te permite interactuar con un equipo o servidor a través de una sesión de PowerShell.

### Características

Se admiten las siguientes características de PowerShell:

- Crear una sesión interactiva de PowerShell en el servidor
- Desconecta de la sesión de PowerShell en el servidor

[**Ver los comentarios y características propuestas de PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## Procesos

**Procesos** te permite administrar procesos en ejecución en un equipo o servidor.

### Características

En los procesos, se admiten las siguientes características:

- Explorar y buscar procesos en ejecución
- Ver detalles del proceso
- Iniciar un proceso
- Finaliza un proceso
- Crear un volcado de memoria de proceso
- Buscar los identificadores de proceso

[**Comentarios de vista y las características propuestas para los procesos**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## Registro

**Registro** te permite administrar las claves del registro y los valores en un equipo o servidor.

### Características

En el registro, se admiten las siguientes características:

- Examinar los valores y claves del registro
- Agregar o modificar los valores del registro
- Eliminar los valores del registro

[**Comentarios de vista y las características propuestas de registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## Escritorio remoto

**Escritorio remoto** te permite interactuar con un servidor a través de una sesión interactiva de escritorio o un equipo.

### Características

En el escritorio remoto, se admiten las siguientes características:

- Iniciar una sesión interactiva de escritorio remota
- Desconectarse de una sesión de escritorio remota
- Enviar Ctrl + Alt + Supr en una sesión de escritorio remoto

[**Comentarios de vista y las características propuestas para escritorio remoto**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## Roles y características

**Roles y características** te permite administrar roles y características en un servidor.

### Características

Se admiten las siguientes características de Roles y características:

- Examinar la lista de roles y características en un servidor
- Ver los detalles de rol o característica
- Instalar un rol o característica
- Quitar un rol o característica

[**Comentarios de vista y las características propuestas de Roles y características**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## Tareas programadas

**Las tareas programadas** te permite administrar tareas programadas en un equipo o servidor.

### Características

Tareas programadas se admiten las siguientes características:

- Examinar la biblioteca del programador de tareas
- Editar tareas programadas
- Habilitar & deshabilitar programada tareas
- Iniciar & Stop programada tareas
- Crear las tareas programadas

[**Comentarios de vista y las características propuestas de tareas programadas**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## Servicios

**Servicios** te permite administrar servicios en un equipo o servidor.

### Características

En los servicios, se admiten las siguientes características:

- Explorar y buscar los servicios en un servidor
- Ver detalles de un servicio
- Iniciar un servicio
- Pausar un servicio
- Editar las propiedades de un servicio

[**Comentarios de vista y las características propuestas para los servicios**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## Configuración

**Opciones de configuración** es una ubicación central para administrar la configuración en un equipo o servidor.

### Características

- Permite ver y modificar las variables de entorno del usuario y del sistema
- Ver la configuración para la supervisión de las alertas de [Monitor de Azure](azure-monitor.md)
- Permite ver y modificar la configuración de energía
- Ver y modificar la configuración de escritorio remoto
- Ver y modificar la configuración de control de acceso basado en roles
- Ver y modificar la configuración de host de Hyper-V, si procede

## Almacenamiento

**Almacenamiento** te permite administrar dispositivos de almacenamiento en un equipo o servidor.

### Características

En el almacenamiento, se admiten las siguientes características:

- Explorar y buscar los discos existentes en un servidor
- Ver detalles de disco
- Crear un volumen
- Inicializar un disco
- Crear, exponer y ocultar un disco duro virtual (VHD)
- Desconectar un disco
- Dar formato a un volumen
- Cambiar el tamaño de un volumen
- Editar las propiedades de volumen
- Eliminar un volumen
- Instalar Administración de cuotas

[**Ver los comentarios y características propuestas de almacenamiento**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## Servicio de migración de almacenamiento

**Servicio de migración de almacenamiento** permite migrar servidores y recursos compartidos de Azure o Windows Server 2019 archivos, sin necesidad de aplicaciones o a los usuarios cambiar nada.
[Obtener una visión general de servicio de migración de almacenamiento](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Servicio de migración de almacenamiento requiere Windows Server 2019.

## Réplica de almacenamiento

Usar **Réplica de almacenamiento** para administrar la replicación de almacenamiento de servidor a servidor.
[Más información sobre la réplica de almacenamiento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## Información del sistema

**Información del sistema** presenta análisis predictivo de forma nativa en Windows Server para ayudar a aumenta la información sobre el funcionamiento del servidor.
[Obtener una visión general de la información del sistema](http://aka.ms/systeminsights)

>[!NOTE]
>Información del sistema requiere Windows Server 2019.

## Actualizaciones

**Actualizaciones** te permite administrar las actualizaciones de Windows o de Microsoft en un equipo o servidor.

### Características

En las actualizaciones, se admiten las siguientes características:

- Ver disponibles de Windows o Microsoft Updates
- Ver una lista de historial de actualizaciones
- Instalar actualizaciones
- Buscar actualizaciones de Microsoft Update en línea
- Administrar la integración de [Administración de actualizaciones de Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Ver los comentarios y características propuestas de actualizaciones**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## Máquinas virtuales

Consulta la [administración de máquinas virtuales con Windows Admin Center](manage-virtual-machines.md)

## Conmutadores virtuales

**Conmutadores virtuales** te permite administrar conmutadores virtuales de Hyper-V en un equipo o servidor.

### Características

Se admiten las siguientes características de conmutadores virtuales:

- Explorar y buscar conmutadores virtuales en un servidor
- Crear un nuevo conmutador Virtual
- Cambiar el nombre de un conmutador Virtual
- Eliminar un conmutador Virtual existente
- Editar las propiedades de un conmutador Virtual

[**Ver los comentarios y características propuestas de conmutadores virtuales**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
