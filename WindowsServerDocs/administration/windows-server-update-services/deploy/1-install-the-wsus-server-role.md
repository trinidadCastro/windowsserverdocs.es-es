---
title: 'Paso 1: instalar el rol de servidor WSUS'
description: 'Tema de Windows Server Update Service (WSUS): describe cómo instalar el rol de servidor con el administrador del servidor'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: fabc8619-350e-403b-96f8-116424931300
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4027f7656c17b2ba46fdf44e3af4de42bb056a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814006"
---
# <a name="step-1-install-the-wsus-server-role"></a>Paso 1: Instalar el rol de servidor WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El siguiente paso en la implementación del servidor WSUS es instalar el rol de servidor de WSUS. En el siguiente procedimiento, se describen los pasos para instalar el rol de servidor de WSUS mediante el Administrador del servidor.

> [!IMPORTANT]
> Este procedimiento solo abarca la instalación de WSUS con Windows Internal Database (WID). El procedimiento para instalar WSUS con Microsoft SQL Server aparece en [este artículo](https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx).

### <a name="to-install-the-wsus-server-role"></a>Para instalar el rol de servidor de WSUS

1.  Inicie sesión en el servidor donde tiene previsto instalar el rol de servidor de WSUS, con una cuenta que sea miembro del grupo local Administradores.

2.  En **administrador del servidor**, haga clic en **administrar**y, a continuación, haga clic en **agregar Roles y características**.

3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.

4.  En el **Seleccionar tipo de instalación** página, confirme que **instalación basada en roles o basada en características** opción está seleccionada y haga clic en **siguiente**.

5.  En el **Seleccionar servidor de destino** página, elija dónde se encuentra el servidor (desde un grupo de servidores o desde un disco duro virtual). Una vez que selecciones la ubicación, elige el servidor en el que quieres instalar el rol de servidor de WSUS y después haz clic en **Siguiente**.

6.  En el **seleccionar roles de servidor** página, seleccione **Windows Server Update Services**.  Se abre **Agregar características requeridas para Windows Server Update Services**. Haz clic en **Agregar características requeridas**y, a continuación, haz clic en **Siguiente**.

7.  En el **seleccionar características**página. conservar las selecciones predeterminadas y, a continuación, haga clic en **siguiente**.

    > [!IMPORTANT]
    > WSUS solo requiere la configuración predeterminada del rol de servidor web. Si se le solicita una configuración adicional para el rol de servidor web mientras configura WSUS, acepte con seguridad los valores predeterminados y continúe la instalación de WSUS.

8.  En la página **Windows Server Update Services** , haga clic en **Siguiente**.

9. En la página **Seleccionar servicios de rol** , deje la configuración predeterminada y haga clic en **Siguiente**.

    > [!TIP]
    > Tienes que seleccionar un tipo de base de datos. Si las opciones de base de datos están desactivadas (no seleccionadas), se producirá un error en las tareas posteriores a la instalación.

10. En la página **Selección de ubicación de contenido**, escribe una ubicación válida para almacenar las actualizaciones. Por ejemplo, puedes crear una carpeta llamada WSUS_database en la raíz de la unidad K específicamente para este propósito y escribir **k:\WSUS_database** como una ubicación válida.

11. Haz clic en **Siguiente**. Se abre la página **Rol Servidor web (IIS)** . Revisa la información y, a continuación, haz clic en **Siguiente**. En **seleccionar servicios de rol para instalar para el servidor Web (IIS)**, conserva los valores predeterminados y, a continuación, haga clic en **siguiente**.

12. En la página **Confirmar selecciones de instalación** , revise las opciones seleccionadas y haga clic en **Instalar**. Se ejecuta el Asistente para la instalación de WSUS. Esto puede llevar varios minutos.

13. Una vez finalizada la instalación de WSUS, en la ventana resumen de la página de **Progreso de la instalación** , haz clic en **Tareas de inicio posterior a la instalación**. El texto cambia y muestra: **Espera mientras se configura el servidor**. Cuando haya terminado la tarea, el texto cambia a: **La configuración se completó correctamente**. Haga clic en **Cerrar**.

14. En **Administrador del servidor**, compruebe si aparece una notificación que le informa que es necesario reiniciar el servidor. Esta puede variar según el rol de servidor instalado. Si se requiere el reinicio, asegúrese de reiniciar el servidor para que finalice la instalación.

> [!IMPORTANT]
> En este punto se finaliza el proceso de instalación, sin embargo, para que WSUS ser funcional que necesita continuar a [paso 2: Configurar WSUS](2-configure-wsus.md).

