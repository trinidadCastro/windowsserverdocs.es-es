---
title: Realice una copia de seguridad de los servidores de Windows desde el centro de administración de Windows con Azure Backup
description: Usar el centro de administración de Windows (Project Honolulu) para realizar copias de seguridad de servidores de Windows con Azure Backup
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.openlocfilehash: 12b549daeb6cb5f1db53af2bc2cbc08dd6091eae
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994665"
---
# <a name="backup-your-windows-servers-from-windows-admin-center-with-azure-backup"></a>Realice una copia de seguridad de los servidores de Windows desde el centro de administración de Windows con Azure Backup

>Se aplica a: Versión preliminar de Windows Admin Center, Windows Admin Center

[Más información acerca de la integración de Azure con el centro de administración de Windows.](./index.md)

El centro de administración de Windows simplifica el proceso de copia de seguridad de los servidores de Windows en Azure y le protege de eliminaciones accidentales o malintencionadas, daños e incluso ransomware. Para automatizar la instalación, puede conectar la puerta de enlace del centro de administración de Windows a Azure.

Use la siguiente información para configurar la copia de seguridad de Windows Server y crear una directiva de copia de seguridad para realizar una copia de seguridad de los volúmenes del servidor y del estado del sistema de Windows desde el centro de administración de Windows.

## <a name="what-is-azure-backup-and-how-does-it-work-with-windows-admin-center"></a>¿Qué es Azure Backup y cómo funciona con el centro de administración de Windows?

**Azure Backup** es el servicio de Azure que puede usar para hacer una copia de seguridad de los datos (protegerlos) y restaurarlos en Microsoft Cloud. Reemplaza su solución de copia de seguridad local o remota existente por una solución confiable, segura y rentable basada en la nube.
[Más información sobre Azure backup](/azure/backup/backup-overview).

Azure Backup ofrece varios componentes que se descargan e implementan en el equipo o servidor adecuados, o en la nube. El componente, o agente, que se implemente depende de lo que quiera proteger. Todos los componentes de Azure Backup (independientemente de si va a proteger los datos en el entorno local o en Azure) se pueden usar para realizar copias de seguridad de los datos en un almacén de Recovery Services en Azure.

La integración de Azure Backup en el centro de administración de Windows es ideal para realizar copias de seguridad de los volúmenes y de los servidores físicos o virtuales de Windows de estado del sistema de Windows. Esto hace que un mecanismo completo realice copias de seguridad de servidores de archivos, controladores de dominio y servidores Web de IIS.

El centro de administración de Windows expone la integración de Azure Backup a través de la herramienta de **copia de seguridad** nativa. La herramienta de **copia de seguridad** proporciona las experiencias de instalación, administración y supervisión para iniciar rápidamente la copia de seguridad de los servidores, realizar operaciones comunes de copia de seguridad y restauración y supervisar el estado general de copia de seguridad de los servidores de Windows.

## <a name="prerequisites-and-planning"></a>Requisitos previos y planeamiento

- Una cuenta de Azure con al menos una suscripción activa
- Los servidores de destino de Windows que desea copiar deben tener acceso a Internet a Azure
- [Conectar la puerta de enlace del centro de administración de Windows a Azure](azure-integration.md)

Para iniciar el flujo de trabajo para realizar una copia de seguridad de Windows Server, abra una conexión de servidor, haga clic en la herramienta de **copia de seguridad** y siga los pasos que se mencionan a continuación.

## <a name="setup-azure-backup"></a>Azure Backup de instalación
Al hacer clic en la herramienta de **copia de seguridad** para una conexión de servidor en la que aún no se ha habilitado Azure backup, verá la pantalla de **Inicio de Azure backup** . Haga clic en el botón **configurar Azure backup** . Se abrirá el Asistente para la instalación de Azure Backup. Siga los pasos que se indican a continuación en el Asistente para realizar una copia de seguridad del servidor.

Si Azure Backup ya está configurado, al hacer clic en la herramienta **copia de seguridad** se abrirá el panel de **copia de seguridad**. Consulte la sección ([Administración y supervisión](#management-and-monitoring)) para detectar las operaciones y las tareas que se pueden realizar desde el panel.

### <a name="step-1-login-to-microsoft-azure"></a>Paso 1: Inicio de sesión en Microsoft Azure
Inicie sesión en su cuenta de Azure.

> [!NOTE]
> Si ha conectado la puerta de enlace del centro de administración de Windows a Azure, debe iniciar sesión automáticamente en Azure. Puede hacer clic en **Cerrar sesión** para iniciar sesión como un usuario diferente.

### <a name="step-2-set-up-azure-backup"></a>Paso 2: configuración de Azure Backup
Seleccione la configuración adecuada para Azure Backup tal y como se describe a continuación.

 - **Identificador de suscripción:** La suscripción de Azure que quiere usar para realizar una copia de seguridad de Windows Server en Azure. Todos los recursos de Azure, como el grupo de recursos de Azure, se creará el almacén de Recovery Services en la suscripción seleccionada.
 - **Almacén:** El almacén de Recovery Services en el que se almacenarán las copias de seguridad de los servidores. Puede seleccionar entre almacenes existentes o el centro de administración de Windows creará un nuevo almacén.
 - **Grupos de recursos:** El grupo de recursos de Azure es un contenedor para una colección de recursos. El almacén de Recovery Services se crea o contiene en el grupo de recursos especificado. Puede seleccionar entre grupos de recursos existentes o el centro de administración de Windows creará uno nuevo.
 - **Ubicación:** región de Azure en la que se creará el almacén de Recovery Services. Se recomienda seleccionar la región de Azure más cercana a Windows Server.

### <a name="step-3-select-backup-items-and-schedule"></a>Paso 3: seleccionar elementos y programación de copia de seguridad

- Seleccione de qué desea hacer una copia de seguridad desde el servidor. El centro de administración de Windows permite elegir entre una combinación de **volúmenes** y el **Estado del sistema de Windows** , a la vez que proporciona el tamaño estimado de los datos que se seleccionan para la copia de seguridad.

> [!NOTE]
> La primera copia de seguridad es una copia de seguridad completa de todos los datos seleccionados. Sin embargo, las copias de seguridad subsiguientes son incrementales por naturaleza y solo transfieren los cambios a los datos desde la copia de seguridad anterior.

- Seleccione entre varias **programaciones de copia de seguridad** preestablecidas para los volúmenes o el estado del sistema.

### <a name="step-4-enter-encryption-passphrase"></a>Paso 4: escribir la frase de contraseña de cifrado

- Escriba una **frase de contraseña de cifrado** de su elección (16 caracteres como mínimo).  **Azure backup** protege los datos de copia de seguridad con una frase de contraseña de cifrado administrada por el usuario y configurada por el usuario. La frase de contraseña de cifrado es necesaria para recuperar datos de Azure Backup.

> [!NOTE]
> La frase de contraseña debe almacenarse en una ubicación segura fuera de las instalaciones, como otro servidor o el [Azure Key Vault](/azure/key-vault/quick-create-portal). Microsoft no almacena la frase de contraseña y no puede recuperar ni restablecer la frase de contraseña si la pierde o se olvida de ella.

- Revise toda la configuración y haga clic en **aplicar** .

El centro de administración de Windows llevará a cabo las siguientes operaciones:

1. Cree un grupo de recursos de Azure si aún no existe.
2. Creación de un almacén de Azure Recovery Services como se especifica
3. Instalación y registro del agente de Microsoft Azure Recovery Services en el almacén
4. Cree la copia de seguridad y la programación de retención según las opciones seleccionadas y asóciela con el servidor de Windows.

## <a name="management-and-monitoring"></a>Administración y supervisión

Una vez que haya configurado correctamente Azure Backup, verá el **Panel de copia de seguridad** al abrir la herramienta de copia de seguridad para una conexión de servidor existente. Puede realizar las siguientes tareas desde el **Panel de copia de seguridad**

- **Acceder al almacén en Azure:** Puede hacer clic en el vínculo **Recovery Services Vault** en la pestaña **información general** del **Panel de copia de seguridad** para que se lleve al almacén de Azure para realizar un [amplio conjunto de operaciones de administración](/azure/backup/backup-azure-manage-windows-server) .
- **Realice una copia de seguridad ad hoc:** Haga clic en **realizar copia** de seguridad ahora para realizar una copia de seguridad ad hoc.
- **Supervisar trabajos y configurar notificaciones de alerta:** Vaya a la pestaña **trabajos** del panel para supervisar los trabajos en curso o pasados y [configurar notificaciones de alerta](/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) para recibir mensajes de correo electrónico para los trabajos con errores u otras alertas relacionadas con la copia de seguridad.
- **Ver puntos de recuperación y recuperar datos:** Haga clic en la pestaña **puntos de recuperación** del panel para ver los puntos de recuperación y haga clic en **recuperar datos** para obtener los pasos para recuperar datos de Azure.