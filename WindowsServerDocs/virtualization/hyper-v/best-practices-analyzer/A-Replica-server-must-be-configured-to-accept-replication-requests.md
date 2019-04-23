---
title: Un servidor de réplica debe configurarse para aceptar solicitudes de replicación
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c5e30ddc50b176b83db081a29c6427356ab946c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827526"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>Un servidor de réplica debe configurarse para aceptar solicitudes de replicación

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>Problema  
*Este equipo está designado como un servidor de réplica de Hyper-V, pero no está configurado para aceptar los datos de replicación entrantes desde los servidores principales.*  
  
## <a name="impact"></a>Impacto  
*Este servidor no puede aceptar tráfico de replicación de servidores principales.*  
  
## <a name="resolution"></a>Resolución  
*Utilice el Administrador de Hyper-V para especificar qué servidores principales en este servidor de réplica debe acepta datos de replicación.*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>Crear entradas de autorización con el Administrador de Hyper-V  
  
1.  Abre el Administrador Hyper-V. (Desde el administrador del servidor, haga clic en **herramientas** > **Administrador de Hyper-V**.)  
  
2.  En la lista de hosts, haga clic en lo que desee y después haga clic en **configuración de Hyper-V**.  
  
3.  En el panel de navegación, haga clic en **configuración de replicación**.  
  
4.  En **autorización y almacenamiento**, haga clic en **permitir la replicación desde los servidores especificados**.  
  
5.  Debajo de la lista de servidores, haga clic en **agregar**.  
  
6.  En **Agregar entrada de autorización**:  
  
    -   Escriba el nombre completo del primer servidor.  
  
    -   Especifique una ubicación dedicada para almacenar solo los archivos de ese servidor.  
  
7.  Haga clic en **Aceptar**.  
  
8.  Repetir para cada servidor principal.  
  
9. Haga clic en **Aceptar** nuevo para finalizar y cerrar la ventana.  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>Crear entradas de autorización con Windows PowerShell  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en Inicio y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Ejecute un comando similar al siguiente, reemplazando:  
  
    -   El nombre del servidor principal de server01.domain01.contoso.com con el nombre de dominio completo del servidor.  
  
    -   La ubicación de D:\ReplicaVMStorage con su ubicación.  
  
    -   El grupo de confianza denominado de forma predeterminada con el nombre de su grupo, si ha creado uno. Si no es así, utilice de forma predeterminada.  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  


