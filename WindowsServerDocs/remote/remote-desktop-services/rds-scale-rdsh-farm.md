---
title: Escalado horizontal de la implementación de Servicios de Escritorio remoto (RDS) mediante la incorporación de una granja de hosts de sesión de Escritorio remoto
description: Agrega un segundo host de sesión de Escritorio remoto al entorno de RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: da0dbd4332cd05d580c2b1f4dc5eb0734b36b13e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403886"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Escalado horizontal de la implementación de Servicios de Escritorio remoto (RDS) mediante la incorporación de una granja de hosts de sesión de Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Puedes mejorar la disponibilidad y el escalado de la implementación de RDS mediante la incorporación de una granja de hosts de sesión de Escritorio remoto (RDSH).   
  
 
Usa los pasos siguientes para agregar otro host de sesión de Escritorio remoto a la implementación:  
  
1. Crea un servidor para hospedar al segundo host de sesión de Escritorio remoto. Si vas a usar máquinas virtuales de Azure, no olvides incluir la nueva máquina virtual en el mismo conjunto de disponibilidad que contiene el primer host de sesión de Escritorio remoto.
2. Habilita la administración remota en el nuevo servidor o máquina virtual:
   1. En Administrador del servidor, haz clic en **Servidor local > Configuración actual de administración remota (deshabilitado)** . 
   2. Selecciona **Habilitar la administración remota para este servidor** y, a continuación, haz clic en **Aceptar**. 
   3. Opcional: Puedes configurar temporalmente Windows Update para que no descargue e instale actualizaciones de forma automática. Esto ayuda a evitar cambios y reinicios del sistema mientras implementas el servidor de RDSH. En Administrador del servidor, haz clic en **Servidor local > Configuración actual de Windows Update**. Haz clic en **Opciones avanzadas > Aplazar actualizaciones**. 
3. Agrega el servidor o la máquina virtual al dominio:
   1. En Administrador del servidor, haz clic en **Servidor local > Configuración actual del grupo de trabajo**. 
   2. Haz clic en **Cambiar > Dominio** y, a continuación, escribe el nombre del dominio (por ejemplo, Contoso.com). 
   3. Especifica las credenciales del administrador de dominio. 
   4. Reinicia el servidor o la máquina virtual.
4. Agrega el nuevo host de sesión de Escritorio remoto a la granja:
   >[!NOTE] 
   > El paso 1, de creación de una dirección IP pública para la máquina virtual de RDMS, solo es necesario si vas a usar una máquina virtual para RDMS y si estos no tienen ya asignada una dirección IP.
   
   1. Crea una dirección IP pública para la máquina virtual que ejecuta los servicios de administración de Escritorio remoto (RDMS). La máquina virtual de RDMS normalmente será la máquina virtual que ejecute la primera instancia del rol de Agente de conexión a Escritorio remoto.  
       1. En Azure Portal, haz clic en **Examinar > Grupos de recursos**, haz clic en el grupo de recursos de la implementación y, luego, en la máquina virtual de RDMS (por ejemplo, Contoso-Cb1).  
       2. Haz clic en **Configuración > Interfaces de red** y luego en la interfaz de red correspondiente.   
       3. Haz clic en **Configuración> Dirección IP**.
       4. Para la **dirección IP pública**, selecciona **Habilitado** y haz clic en **Dirección IP**.   
       5. Si tienes una dirección IP pública existente que quieras usar, selecciónala de la lista. De lo contrario, haz clic en **Crear nuevo**, escribe un nombre y haz clic en **Aceptar** y **Guardar**.   
   2. Inicia sesión en RDMS.
   3. Agrega el nuevo servidor RDSH al Administrador del servidor:   
       1. Inicia Administrador del servidor, haz clic en **Administrar > Agregar servidores**.   
       2. En el cuadro de diálogo Agregar servidores, haz clic en **Buscar ahora**.   
       3. Selecciona el servidor que deseas usar para el host de sesión de Escritorio remoto o la máquina virtual recién creada (por ejemplo, Contoso-Sh2) y haz clic en **Aceptar**.
   4. Adición del servidor RDSH a la implementación
       1. Inicia el Administrador del servidor.  
       2. Haz clic en **Servicios de escritorio remoto > Información general > Servidores de implementación > Tareas > Agregar servidores de host de sesión a Escritorio remoto**.   
       3. Selecciona el nuevo servidor (por ejemplo, Contoso-Sh2) y haz clic en **Siguiente**.  
       4. En la página de confirmación, selecciona **Reiniciar equipos remotos según sea necesario** y, a continuación, haz clic en **Agregar**.   
   5. Adición del servidor RDSH a la granja de colecciones:
       1. Inicia el Administrador del servidor.   
       2. Haz clic en **Servicios de Escritorio remoto** y, a continuación, haz clic en la colección a la que deseas agregar el servidor RDSH recién creado (por ejemplo, ContosoDesktop).   
       3. En **Servidores host**, haz clic en **Tareas > Agregar servidores de host de sesión a Escritorio remoto**.   
       4. Selecciona el servidor recién creado (por ejemplo, Contoso-Sh2) y haz clic en **Siguiente**.   
       5. En la página Confirmación, haz clic en **Agregar**.   

