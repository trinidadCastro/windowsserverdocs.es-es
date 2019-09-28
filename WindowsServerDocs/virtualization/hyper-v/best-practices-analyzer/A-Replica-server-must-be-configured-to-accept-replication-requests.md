---
title: Un servidor réplica debe estar configurado para aceptar solicitudes de replicación
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 54868d4db2dccc893bd2897134d9125446873384
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366725"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>Un servidor réplica debe estar configurado para aceptar solicitudes de replicación

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.
  
## <a name="issue"></a>Problema  
*Este equipo se designa como un servidor de réplica de Hyper-V, pero no está configurado para aceptar los datos de replicación entrantes de los servidores principales.*  
  
## <a name="impact"></a>Impacto  
*Este servidor no puede aceptar el tráfico de replicación de los servidores principales.*  
  
## <a name="resolution"></a>Resolución  
*Use el administrador de Hyper-V para especificar de qué servidores principales deben aceptar este servidor réplica los datos de replicación.*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Crear entradas de autorización mediante el administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. (En Administrador del servidor, haga clic en **herramientas** > **Administrador de Hyper-V**).  
  
2.  En la lista de hosts, haga clic con el botón secundario en el que desee y, a continuación, haga clic en **configuración de Hyper-V**.  
  
3.  En el panel de navegación, haga clic en **configuración de replicación**.  
  
4.  En **autorización y almacenamiento**, haga clic en **permitir la replicación desde los servidores especificados**.  
  
5.  Debajo de la lista de servidores, haga clic en **Agregar**.  
  
6.  En **Agregar entrada de autorización**:  
  
    -   Escriba el nombre completo del primer servidor.  
  
    -   Especifique una ubicación dedicada para almacenar solo los archivos del servidor.  
  
7.  Haga clic en **Aceptar**.  
  
8.  Repita el procedimiento para cada servidor principal.  
  
9. Vuelva a hacer clic en **Aceptar** para finalizar y cerrar la ventana.  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>Crear entradas de autorización con Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en Inicio y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Ejecute un comando similar al siguiente, reemplazando:  
  
    -   El nombre del servidor principal de server01.domain01.contoso.com con el nombre de dominio completo del servidor.  
  
    -   La ubicación de D:\ReplicaVMStorage con su ubicación.  
  
    -   El grupo de confianza denominado predeterminado con el nombre de su grupo, si ha creado uno. Si no es así, utilice el valor predeterminado.  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  


