---
title: Preparación de máquinas virtuales para Escritorio remoto
description: Preparación de las máquinas virtuales para los componentes de Escritorio remoto
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6a1f0bfef21351894d3b9c2cfd8d044491834f6c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387252"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creación de máquinas virtuales para Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Puedes instalar los componentes de Servicios de Escritorio remoto en servidores físicos o en máquinas virtuales. 

El primer paso es [crear máquinas virtuales de Windows Server en Azure](/azure/virtual-machines/windows/quick-create-portal). Necesitarás crear tres máquinas virtuales: una para el host de sesión de Escritorio remoto, una para el agente de conexión y otra para el acceso web y la puerta de enlace de Escritorio remoto. Para garantizar la disponibilidad de la implementación de RDS, crea un conjunto de disponibilidad (en **Alta disponibilidad** durante el proceso de creación de las máquinas virtuales) y agrupa varias máquinas virtuales en ese conjunto de disponibilidad.
 
Después de crear las máquinas virtuales, sigue estos pasos para prepararlas para RDS.

1.  Conéctate a la máquina virtual mediante el cliente de Conexión a Escritorio remoto (RDC):  
    1.  En Azure Portal, abre la vista Grupos de recursos y, después, haz clic en el grupo de recursos que quieras usar para la implementación.  
    2.  Selecciona la nueva máquina virtual de RDSH (por ejemplo, Contoso-Sh1).  
    3.  Haz clic en **Conectar > Abrir** para abrir el cliente de Escritorio remoto.  
    4.  En el cliente, haz clic en **Conectar** y, a continuación, en **Usar otra cuenta de usuario**. Escribe el nombre de usuario y la contraseña de la cuenta de administrador local.  
    5.  Haz clic en **Sí** cuando se te avise sobre el certificado.  
2.  Habilita la administración remota:  
    1.  En Administrador del servidor, haz clic en **Servidor local > Configuración actual de administración remota (deshabilitado)** .  
    2.  Selecciona **Habilitar la administración remota para este servidor**.  
    3.  Haga clic en **Aceptar**.  
3.  Opcional: Puedes configurar temporalmente Windows Update para que no descargue e instale actualizaciones de forma automática. Esto ayuda a evitar cambios y reinicios del sistema mientras implementas el servidor de RDSH.  
    1.  En Administrador del servidor, haz clic en **Servidor local > Configuración actual de Windows Update**.  
    2.  Selecciona **Opciones avanzadas > Aplazar actualizaciones**.   
4.  Agrega el servidor al dominio:  
    1.  En Administrador del servidor, haz clic en **Servidor local > Configuración actual del grupo de trabajo**.  
    2.  Haz clic en **Cambiar > Dominio** y, a continuación, escribe el nombre del dominio (por ejemplo, Contoso.com).  
    3.  Especifica las credenciales del administrador de dominio.  
    4.  Reinicie la máquina virtual.  
5.  Repite los pasos 1 a 4 para la máquina virtual de acceso web y puerta de enlace de Escritorio remoto.  
6.  Repite los pasos 1 a 4 para la máquina virtual del Agente de conexión a Escritorio remoto.  
7.  Inicializa y formatea el disco conectado a la máquina virtual de Agente de conexión a Escritorio remoto:  
    1.  Conéctate a la máquina virtual de Agente de conexión a Escritorio remoto (paso 1 anterior).  
    2.  En el Administrador del servidor, haz clic en **Herramientas > Administración de equipos**.  
    3.  Haz clic en **Administración de discos**.  
    4.  Selecciona el disco conectado, **MBR (registro de arranque maestro)** y, después, haz clic en **Aceptar**.  
    5.  Haz clic con el botón derecho en el nuevo disco (marcado como **Sin asignar**) y haz clic en **Nuevo volumen simple**.  
    6.  En el asistente de **Nuevo volumen simple**, acepta los valores predeterminados, pero proporciona un nombre aplicable para la **etiqueta de volumen** (como "Recursos compartidos").  
8.  En la máquina virtual del Agente de conexión a Escritorio remoto crea recursos compartidos de archivos para los discos y certificados del perfil de usuario:   
    1.  Abra el Explorador de archivos, haz clic en **Este equipo** y abre el disco que agregaste a los recursos compartidos de archivos.  
    2.  Haz clic en **Inicio** y **Nueva carpeta**.  
    3.  Escribe un nombre para la carpeta de discos de usuario, por ejemplo, **UserDisks**.  
    4.  Haz clic con el botón derecho en la nueva carpeta y haz clic en **Propiedades > Uso compartido > Uso compartido avanzado**.  
    5.  Selecciona **Compartir esta carpeta** y haz clic en **Permisos**.  
    6.  Selecciona **Todos** y haz clic en **Quitar**. Ahora, haz clic en **Agregar**, escribe **Admins. del dominio** y haz clic en **Aceptar**.  
    7.  Selecciona **Permitir control total** y, después, haz clic en **Aceptar > Aceptar > Cerrar**.  
    8.  Repite los pasos c a g. para crear una carpeta compartida para los certificados.   


