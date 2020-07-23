---
title: Instalación de la copia de seguridad del servidor en MultiPoint Server
description: Le guía por los pasos necesarios para instalar las herramientas de copia de seguridad y recuperación
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7aa2c422151247d13dcb1dafd474fb70873f54e1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966917"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Instalación de la copia de seguridad del servidor en MultiPoint Server
Se recomienda que considere un plan de copia de seguridad y recuperación para los servidores multipoint.
  
Un buen plan de copia de seguridad y recuperación es importante para cualquier entorno. Copias de seguridad de Windows Server es una característica de Windows Server 2016 que proporciona un conjunto de asistentes y otras herramientas para realizar tareas básicas de copia de seguridad y recuperación para el servidor en el que está instalado. Puede usar Copias de seguridad de Windows Server para hacer una copia de seguridad de un servidor completo (todos los volúmenes), de volúmenes seleccionados, del estado del sistema o de archivos o carpetas específicos, así como para crear una copia de seguridad que pueda usar para recompilar el sistema.  
  
Puede recuperar volúmenes, carpetas, archivos, determinadas aplicaciones y el estado del sistema. Además, en el caso de desastres como errores del disco duro, puede recompilar un sistema desde cero o con hardware alternativo. Para ello, debe tener una copia de seguridad del servidor completo o solo de los volúmenes que contengan los archivos del sistema operativo y el entorno de recuperación de Windows. Esto restaura el sistema completo en el sistema antiguo o en un nuevo disco duro.  
  
Una característica clave de Copias de seguridad de Windows Server es la capacidad de programar copias de seguridad para que se ejecuten automáticamente.  
  
Use los procedimientos siguientes para configurar el tipo de copia de seguridad que necesita.  
  
## <a name="install-backup-and-recovery-tools"></a>Instalar las herramientas de copia de seguridad y recuperación  
  
1.  En la pantalla **Inicio** , Abra **Administrador del servidor**.  
  
2.  Haga clic en **Agregar roles y características** para iniciar el Asistente para agregar roles. Después, haga clic en **siguiente** después de revisar las notas **antes de empezar** .  
  
3.  Seleccione la opción de instalación basada en **características o en roles** y, a continuación, haga clic en **siguiente**.  
  
4.  Seleccione el equipo local que está administrando y haga clic en **siguiente**.  
  
    Se abre el Asistente para agregar características.  
  
5.  En la página **seleccionar características** , expanda copias de seguridad de Windows Server características, active las casillas de **copias de seguridad de Windows Server** y **herramientas de línea de comandos**y, a continuación, haga clic en **siguiente**.  
  
    > [!NOTE]  
    > O bien, si solo desea instalar el complemento y la herramienta de línea de comandos de Wbadmin, expanda **copias de seguridad de Windows Server características**y, a continuación, active la casilla de **copias de seguridad de Windows Server** solo, asegúrese de que la casilla **herramientas de línea de comandos** está desactivada.  
  
6.  En la página **confirmar selecciones de instalación** , revise las opciones seleccionadas y, a continuación, haga clic en **instalar**.  
  
    Si se produce algún error durante la instalación, la página resultados de la **instalación** observará los errores.  
  
7.  Una vez completada la instalación correctamente, debería poder tener acceso a estas herramientas de copia de seguridad y recuperación:  
  
    -   Para abrir el complemento Copias de seguridad de Windows Server, en la pantalla **Inicio** , escriba copia de **seguridad**y, a continuación, haga clic en **copias de seguridad de Windows Server** en los resultados.  
  
    -   Para iniciar la herramienta Wbadmin y ver la sintaxis de sus comandos: en la pantalla **Inicio** , escriba **Command**. En los resultados, haga clic con el botón secundario en **símbolo del sistema**, haga clic en **Ejecutar como administrador** en la parte inferior de la página y, a continuación, haga clic en **sí** en el mensaje de confirmación. En el símbolo del sistema, escriba **Wbadmin/?** y presione Entrar. Debería ver la sintaxis y las descripciones de los comandos de la herramienta.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurar copias de seguridad con Copias de seguridad de Windows Server  
  
-   Siga las instrucciones de [copia de seguridad del servidor](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753528(v=ws.11)). 
