---
title: 'Paso 1: instala el rol de servidor de WSUS'
description: 'Tema de Windows Server Update Services (WSUS): describe cómo instalar el rol de servidor mediante el Administrador del servidor'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: fabc8619-350e-403b-96f8-116424931300
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 664e0233d10cbbc526635f1868ea1977c59fea63
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80828908"
---
# <a name="step-1-install-the-wsus-server-role"></a>Paso 1: Instalar el rol de servidor de WSUS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El siguiente paso en la implementación del servidor WSUS es instalar el rol de servidor de WSUS. En el siguiente procedimiento, se describen los pasos para instalar el rol de servidor de WSUS mediante el Administrador del servidor.

> [!IMPORTANT]
> Este procedimiento solo abarca la instalación de WSUS con Windows Internal Database (WID). El procedimiento para instalar WSUS con Microsoft SQL Server aparece en [este artículo](https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx).

### <a name="to-install-the-wsus-server-role"></a>Para instalar el rol de servidor de WSUS

1.  Inicie sesión en el servidor donde tiene previsto instalar el rol de servidor de WSUS, con una cuenta que sea miembro del grupo local Administradores.

2.  En el **Administrador del servidor**, haz clic en **Administrar** y, después, haz clic en **Agregar roles y características**.

3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.

4.  En la página **Seleccionar tipo de instalación**, comprueba que la opción **Instalación basada en características o en roles** esté seleccionada y haz clic en **Siguiente**.

5.  En la página **Seleccionar servidor de destino**, elige la ubicación del servidor (en un grupo de servidores o en un disco duro virtual). Una vez que selecciones la ubicación, elige el servidor en el que quieres instalar el rol de servidor de WSUS y después haz clic en **Siguiente**.

6.  En la página **Seleccionar roles de servidor**, selecciona **Windows Server Update Services**.  Se abre**Agregar características requeridas para Windows Server Update Services** . Haz clic en **Agregar características requeridas**y, a continuación, haz clic en **Siguiente**.

7.  En la página **Seleccionar características**. Deja la configuración predeterminada y haz clic en **Siguiente**.

    > [!IMPORTANT]
    > WSUS solo requiere la configuración predeterminada del rol de servidor web. Si se le solicita una configuración adicional para el rol de servidor web mientras configura WSUS, acepte con seguridad los valores predeterminados y continúe la instalación de WSUS.

8.  En la página **Windows Server Update Services** , haga clic en **Siguiente**.

9. En la página **Seleccionar servicios de rol** , deje la configuración predeterminada y haga clic en **Siguiente**.

    > [!TIP]
    > Tienes que seleccionar un tipo de base de datos. Si las opciones de base de datos están desactivadas (no seleccionadas), se producirá un error en las tareas posteriores a la instalación.

10. En la página **Selección de ubicación de contenido** , escribe una ubicación válida para almacenar las actualizaciones. Por ejemplo, puedes crear una carpeta llamada WSUS_database en la raíz de la unidad K específicamente para este propósito y escribir **k:\WSUS_database** como una ubicación válida.

11. Haga clic en **Siguiente**. Se abre la página **Rol Servidor web (IIS)** . Revisa la información y, a continuación, haz clic en **Siguiente**. En **Seleccionar servicios de rol para instalar para el servidor Web (IIS)** , conserva los valores predeterminados y, a continuación, haz clic en **Siguiente**.

12. En la página **Confirmar selecciones de instalación** , revise las opciones seleccionadas y haga clic en **Instalar**. Se ejecuta el Asistente para la instalación de WSUS. Esto puede llevar varios minutos.

13. Una vez finalizada la instalación de WSUS, en la ventana resumen de la página de **Progreso de la instalación** , haz clic en **Tareas de inicio posterior a la instalación**. El texto cambia y muestra: **Espera mientras se configura el servidor**. Cuando haya terminado la tarea, el texto cambia a: **La configuración se completó correctamente**. Haga clic en **Cerrar**.

14. En **Administrador del servidor**, compruebe si aparece una notificación que le informa que es necesario reiniciar el servidor. Esta puede variar según el rol de servidor instalado. Si se requiere el reinicio, asegúrese de reiniciar el servidor para que finalice la instalación.

> [!IMPORTANT]
> En este punto finaliza el proceso de instalación; no obstante, para que funcione WSUS tienes que continuar con el [Paso 2: Configurar WSUS](2-configure-wsus.md).

