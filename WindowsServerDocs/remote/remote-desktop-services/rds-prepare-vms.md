---
title: Preparación de máquinas virtuales para Escritorio remoto
description: Preparar las máquinas virtuales para los componentes de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: cb06963c3ae51fb9337827c7b29b93b8c2736c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871886"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creación de máquinas virtuales para Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede instalar los componentes de servicios de escritorio remoto en servidores físicos o en máquinas virtuales. 

El primer paso es [crear máquinas virtuales Windows Server en Azure](/azure/virtual-machines/windows/quick-create-portal). Desea crear tres máquinas virtuales: uno para el Host de sesión de escritorio remoto, uno para el agente de conexión y otra para la Web de escritorio remoto y puerta de enlace de escritorio remoto. Para garantizar la disponibilidad de la implementación de RDS, cree una disponibilidad establecer (en **alta disponibilidad** en el proceso de creación de máquinas virtuales) y agrupar varias máquinas virtuales en que el conjunto de disponibilidad.
 
Después de crear las máquinas virtuales, siga estos pasos para prepararlos para RDS.

1.  Conéctese a la máquina virtual mediante el cliente conexión a Escritorio remoto (RDC):  
    1.  Abra la vista de grupos de recursos en Azure portal y, a continuación, haga clic en el grupo de recursos que se usará para la implementación.  
    2.  Seleccione la nueva máquina virtual RDSH (por ejemplo, Contoso-SHA-1).  
    3.  Haga clic en **Connect > Abrir** para abrir el cliente de escritorio remoto.  
    4.  En el cliente, haga clic en **Connect**y, a continuación, haga clic en **usar otra cuenta de usuario**. Escriba el nombre de usuario y la contraseña para la cuenta de administrador local.  
    5.  Haga clic en **Sí** cuando existe una advertencia sobre el certificado.  
2.  Habilitar la administración remota:  
    1.  En el administrador del servidor, haga clic en **servidor Local > configuración actual de administración remota (deshabilitado)**.  
    2.  Seleccione **habilitar la administración remota para este servidor**.  
    3.  Haga clic en **Aceptar**.  
3.  Opcional: Windows Update para descargar e instalar actualizaciones automáticamente no se puede establecer temporalmente. Esto ayuda a evitar que los cambios y los reinicios del sistema mientras se implementa el servidor RDSH.  
    1.  En el administrador del servidor, haga clic en **servidor Local > configuración actual de Windows Update**.  
    2.  Seleccione **opciones avanzadas > aplazar actualizaciones**.   
4.  Agregue el servidor al dominio:  
    1.  En el administrador del servidor, haga clic en **servidor Local > configuración de grupo de trabajo actual**.  
    2.  Haga clic en **cambio > dominio**y, a continuación, escriba el nombre de dominio (por ejemplo, Contoso.com).  
    3.  Escriba las credenciales de administrador de dominio.  
    4.  Reinicie la máquina virtual.  
5.  Repita los pasos 1 a 4 para la máquina virtual Web de escritorio remoto y puerta de enlace.  
6.  Repita los pasos 1 a 4 para la máquina virtual de agente de conexión a Escritorio remoto.  
7.  Inicializar y formatear el disco conectado en la máquina virtual de agente de conexión a Escritorio remoto:  
    1.  Conéctese a la máquina virtual de agente de conexión a Escritorio remoto (paso 1 anterior).  
    2.  En el administrador del servidor, haga clic en **Herramientas > Administración de equipos**.  
    3.  Haga clic en **administración de discos**.  
    4.  Seleccione el disco conectado, a continuación, **MBR (registro de arranque maestro)** y, a continuación, haga clic en **Aceptar**.  
    5.  Haga clic en el nuevo disco (marcado como **sin asignar**) y haga clic en **nuevo volumen Simple**.  
    6.  En el **nuevo volumen Simple** asistente, acepte los valores predeterminados, pero proporcione un nombre aplicable para el **etiqueta de volumen** (como recursos compartidos).  
8.  Creación de recursos compartidos de archivos para los discos de perfil de usuario y certificados en la máquina virtual de agente de conexión a Escritorio remoto:   
    1.  Abra el Explorador de archivos, haga clic en **este equipo**y abra el disco que ha agregado para recursos compartidos de archivos.  
    2.  Haga clic en **inicio** y **nueva carpeta**.  
    3.  Escriba un nombre para la carpeta de discos de usuario, por ejemplo, **UserDisks**.  
    4.  Haga clic en la nueva carpeta y haga clic en **Propiedades > Compartir > uso compartido avanzado**.  
    5.  Seleccione **compartir esta carpeta** y haga clic en **permisos**.  
    6.  Seleccione **todo el mundo**y, a continuación, haga clic en **quitar**. Ahora haga clic en **agregar**, escriba **Admins. del dominio**y haga clic en **Aceptar**.  
    7.  Seleccione **Control total**y, a continuación, haga clic en **Aceptar > Aceptar > cerrar**.  
    8.  Repita los pasos c. g. Para crear una carpeta compartida para los certificados.   


