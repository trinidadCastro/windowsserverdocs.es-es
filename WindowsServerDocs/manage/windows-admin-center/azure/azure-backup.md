---
title: Copia de seguridad de los servidores de Windows desde Windows Admin Center con Azure Backup
description: Usa Windows Admin Center (proyecto Honolulu) a los servidores de copia de seguridad de Windows con la copia de seguridad de Azure
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 3983d0b65bc69ef9fd40f3c8e196d40534b1b8f9
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297096"
---
# Copia de seguridad de los servidores de Windows desde Windows Admin Center con Azure Backup

>Se aplica a: Windows Admin Center Preview, Windows Admin Center

[Obtén más información acerca de la integración de Azure con Windows Admin Center.](../plan/azure-integration-options.md)

Windows Admin Center simplifica el proceso de copia de seguridad de los servidores de Windows en Azure y protege frente a eliminaciones accidentales o malintencionadas, daños e incluso ransomware. Para automatizar la instalación, puedes conectar la puerta de enlace de Windows Admin Center a Azure.

Usa la siguiente información para configurar la copia de seguridad de Windows Server y crear una directiva de copia de seguridad para la copia de seguridad volúmenes del servidor y el estado del sistema de Windows desde el centro de administración de Windows.

## ¿Qué es Azure Backup y cómo funciona con Windows Admin Center? 

**Copia de seguridad de Azure** es el servicio basados en Azure, puedes usar para la copia de seguridad (o proteger) y restaurar los datos en la nube de Microsoft. Copia de seguridad de Azure reemplaza el existente de forma local o la solución de copia de seguridad fuera del sitio con una solución basada en la nube que es coste competitiva, segura y confiable.
[Obtenga más información acerca de la copia de seguridad de Azure](https://docs.microsoft.com/azure/backup/backup-overview).

Copia de seguridad de Azure ofrece varios componentes que puedes descargarán e implementar en el equipo correspondiente, server, o en la nube. El componente o agente, que implementar depende de lo que quieres proteger. Todos los componentes de copia de seguridad de Azure (independientemente de si estás protegiendo datos locales o en Azure) pueden usarse para cuando se realiza una copia de seguridad de datos en un almacén de servicios de recuperación en Azure.

La integración de copia de seguridad de Azure en el centro de administración de Windows es ideal para la copia de seguridad de volúmenes y el sistema Windows estado local Windows servidores físicos o virtuales. Esto hace que un mecanismo para servidores de archivos, los controladores de dominio y servidores Web IIS de copia de seguridad completa.

Windows Admin Center expone la integración de copia de seguridad de Azure a través de la herramienta de **copia de seguridad** nativa. La herramienta de **copia de seguridad** proporciona de instalación, administración y supervisión de las experiencias para comenzar rápidamente la copia de seguridad de los servidores, realizar copia de seguridad comunes y restaurar las operaciones y para supervisar el estado general de copia de seguridad de los servidores de Windows.

## Requisitos previos y planificación

- Una cuenta de Azure con al menos una suscripción activa
- El destino de los servidores de Windows que quieres copia de seguridad debe tener acceso a Internet a Azure
- [Conecta la puerta de enlace de Windows Admin Center a Azure](azure-integration.md)

Para iniciar el flujo de trabajo de copia de seguridad de Windows Server, abre una conexión de servidor, haz clic en la herramienta de **copia de seguridad** y sigue los pasos que se mencionan a continuación.

## Configurar la copia de seguridad de Azure
Al hacer clic en la herramienta de **copia de seguridad** para la conexión con un servidor en el que no está todavía habilitado Azure Backup, verá la pantalla de **bienvenida de copia de seguridad de Azure** . Haz clic en el botón **establecer una copia de seguridad de Azure** . Esto abrirá al Asistente para instalación de copia de seguridad de Azure. Siga los pasos que aparecen a continuación, en el Asistente de copia de seguridad del servidor.

Si ya está configurado Azure Backup, al hacer clic en la herramienta de **copia de seguridad** se abrirá el **Panel de copia de seguridad**. Consulte la sección ([administración y supervisión](#management-and-monitoring)) para detectar las operaciones y las tareas que pueden realizarse en el panel.

### Paso 1: Inicio de sesión a Microsoft Azure
Inicia sesión en, cuenta de Azure. 

> [!NOTE]
> Si se ha conectado a la puerta de enlace de Windows Admin Center a Azure, debe iniciar sesión automáticamente en Azure. Puedes hacer clic en **Cerrar sesión** aún más inicie sesión en como un usuario diferente.

### Paso 2: Configurar la copia de seguridad de Azure
Selecciona la configuración adecuada para Azure Backup, tal como se describe a continuación

 - **Id. de suscripción:** La suscripción de Azure que quieras usar cuando se realiza una copia de seguridad de Windows Server a Azure. Todos los activos de Azure, como el grupo de recursos de Azure, se creará el almacén de servicios de recuperación en la suscripción seleccionada.
 - **Almacén:** El almacén de servicios de recuperación donde se almacenarán las copias de seguridad de los servidores. Puedes seleccionar entre depósitos existentes o Windows Admin Center creará un almacén de nuevo.  
 - **Grupo de recursos:** El grupo de recursos de Azure es un contenedor para una colección de recursos. El almacén de servicios de recuperación se crea o contenido en el grupo de recurso especificado. Puedes seleccionar entre los grupos de recursos existentes o Windows Admin Center creará una nueva.
 - **Ubicación:** La región de Azure donde se creará el almacén de servicios de recuperación. Se recomienda para seleccionar la región de Azure más cercana a Windows Server.

### Paso 3: Seleccionar elementos de copia de seguridad y programación

- Selecciona la que quieras realizar una copia de seguridad del servidor. Windows Admin Center te permite elegir de una combinación de los **volúmenes** y el **Estado del sistema de Windows** y te dan el tamaño estimado de datos que se ha seleccionado para la copia de seguridad.

> [!NOTE]
> La primera copia de seguridad es una copia de seguridad completa de todos los datos seleccionados. Sin embargo, las copias de seguridad posteriores son incrementales en la naturaleza y transfieren únicamente los cambios a los datos desde la copia de seguridad anterior.

- Selecciona desde varias preestablecido **Programaciones de copia de seguridad de** estado del sistema o volúmenes.

### Paso 4: Escribir la frase de contraseña de cifrado

- Escribe una **Frase de contraseña de cifrado** de tu elección (16 caracteres como mínimos).  **Copia de seguridad de Azure** protege los datos de copia de seguridad con una frase de contraseña de cifrado configurada por el usuario y administrados por el usuario. La frase de contraseña de cifrado es necesaria para recuperar los datos de copia de seguridad de Azure.

> [!NOTE]
> La contraseña que se debe almacenarse en una ubicación segura fuera del sitio como otro servidor o el [Almacén de claves de Azure](https://docs.microsoft.com/azure/key-vault/quick-create-portal). Microsoft no almacenar la frase de contraseña y no se puede recuperar o restablecer la contraseña si se pierde o se olvidado.

- Revisar toda la configuración y haga clic en **Aplicar**

Windows Admin Center, luego, realizará las siguientes operaciones

1. Crear un grupo de recursos de Azure, si no existe ya
2. Crear un almacén de servicios de recuperación de Azure como se especifica
3. Instalar y registrar al agente de servicios de recuperación de Microsoft Azure para el almacén
4. Crear la programación de copia de seguridad y la retención según las opciones seleccionadas y asociarlas con el servidor de Windows.

## Administración y supervisión

Una vez que se ha instalado correctamente Azure Backup, verá el **Panel de copia de seguridad** cuando se abre la herramienta de copia de seguridad de una conexión de servidor existente. Puede realizar las siguientes tareas desde el **Panel de copia de seguridad**

- **Acceder a la caja fuerte en Azure:** Puedes hacer clic en el vínculo de **Almacén de servicios de recuperación** en la pestaña de **información general** del **Panel de copia de seguridad** para ir a la cámara en Azure a realizar un [amplio conjunto de operaciones de administración](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **Realizar una copia de seguridad de ad hoc:** Haz clic en la **Copia de seguridad ahora** para realizar una copia de seguridad ad hoc. 
- **Supervisar los trabajos y configurar las notificaciones de alerta:** Más allá de los trabajos y [configurar las notificaciones de alerta](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) para recibir correos electrónicos de cualquier error trabajos u otras alertas relacionadas con copia de seguridad o navegar a la pestaña de **trabajos** del panel para supervisar continua.
- **Puntos de recuperación de vista y recuperar datos:** Haz clic en la pestaña de **Puntos de recuperación** del panel para ver los puntos de recuperación y haz clic en la **Recuperación de datos** para conocer los pasos para recuperar los datos de Azure.