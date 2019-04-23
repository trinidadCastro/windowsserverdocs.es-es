---
title: Instale copias de seguridad de servidor en el servidor MultiPoint
description: Le guía por los pasos necesarios para instalar las herramientas de copia de seguridad y recuperación
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4331370-ba07-4529-92ab-db14a41bfc3b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 51932c5f0796cfd757d3322e10c17de2a3081f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832496"
---
# <a name="install-server-backup-on-your-multipoint-server"></a>Instale copias de seguridad de servidor en el servidor MultiPoint
Se recomienda que considere un plan de copia de seguridad y recuperación para los servidores MultiPoint.
  
Un buen plan de recuperación y copia de seguridad es importante para cualquier entorno de tamaño. Copia de seguridad de Windows Server es una característica de Windows Server 2016 que proporciona un conjunto de asistentes y otras herramientas para realizar tareas básicas de copia de seguridad y recuperación para el servidor donde está instalado. Puede usar copias de seguridad de Windows Server para realizar copias de seguridad de un servidor completo (todos los volúmenes), los volúmenes seleccionados, el estado del sistema, o determinados archivos o carpetas y para crear una copia de seguridad que puede usar para volver a generar el sistema.  
  
Puede recuperar volúmenes, carpetas, archivos, determinadas aplicaciones y el estado del sistema. Además, para desastres como errores de disco duro, puede volver a generar un sistema desde el principio o mediante el uso de hardware alternativo. Para ello, debe tener una copia de seguridad del servidor completo o solo de los volúmenes que contienen los archivos del sistema operativo y el entorno de recuperación de Windows. Esto restaura el sistema completo en el sistema antiguo o en un nuevo disco duro.  
  
Una característica clave de copia de seguridad de Windows Server es la capacidad de programar copias de seguridad que se ejecute automáticamente.  
  
Utilice los procedimientos siguientes para configurar el tipo de copia de seguridad que requiere.  
  
## <a name="install-backup-and-recovery-tools"></a>Instalar las herramientas de copia de seguridad y recuperación  
  
1.  Desde el **iniciar** pantalla, abra **administrador del servidor**.  
  
2.  Haga clic en **agregar Roles y características** para iniciar el Asistente para agregar Roles. A continuación, haga clic en **siguiente** después de revisar la **antes de comenzar** notas.  
  
3.  Seleccione el **basado en rol o característica de instalación basada en** opción y, a continuación, haga clic en **siguiente**.  
  
4.  Seleccione el equipo local que está administrando y haga clic en **siguiente**.  
  
    Se abre el Asistente para agregar características.  
  
5.  En el **seleccionar características** página, expanda las características de copia de seguridad de Windows Server, active las casillas de verificación de **copias de seguridad de Windows Server** y **herramientas de línea de comandos**y, a continuación, haga clic en  **Siguiente**.  
  
    > [!NOTE]  
    > O bien, si desea instalar el complemento y la herramienta de línea de comandos de Wbadmin, expanda **características de copia de seguridad de Windows Server**y, a continuación, seleccione el **copias de seguridad de Windows Server** solo la casilla de verificación, asegúrese de que el **Herramientas de línea de comandos** casilla está desactivada.  
  
6.  En el **Confirmar selecciones de instalación** , revise las opciones seleccionadas y, a continuación, haga clic en **instalar**.  
  
    Si se produce algún error durante la instalación, el **resultados de la instalación** página observarán los errores.  
  
7.  Cuando la instalación finalice correctamente, debe tener acceso a estas herramientas de copia de seguridad y recuperación:  
  
    -   Para abrir la copia de seguridad de Windows Server complemento, en el **iniciar** , escriba **copia de seguridad**y, a continuación, haga clic en **copias de seguridad de Windows Server** en los resultados.  
  
    -   Para iniciar la herramienta de Wbadmin y ver la sintaxis de comandos: En el **iniciar** , escriba **comando**. En los resultados, haga clic en **símbolo**, haga clic en **ejecutar como administrador** en la parte inferior de la página y, a continuación, haga clic en **Sí** en el mensaje de confirmación. ¿En el símbolo del sistema, escriba **wbadmin /?** y presione ENTRAR. Debería ver la sintaxis de comandos y las descripciones de la herramienta.  
  
## <a name="configure-backups-using-windows-server-backup"></a>Configurar copias de seguridad mediante copias de seguridad de Windows Server  
  
-   Siga las instrucciones de [seguridad del servidor](https://technet.microsoft.com/library/cc753528.aspx). 