---
title: Servidores de réplica deben configurarse para identificar los servidores principales específicos autorizados para enviar tráfico de replicación
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aeb1f4b-2e75-430b-9557-fe64738c4992
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 47b215d4c84e68d93ae1189ddd370358e2781eff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822246"
---
# <a name="replica-servers-should-be-configured-to-identify-specific-primary-servers-authorized-to-send-replication-traffic"></a>Servidores de réplica deben configurarse para identificar los servidores principales específicos autorizados para enviar tráfico de replicación

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Como se ha configurado, este servidor de réplica acepta el tráfico de replicación de todos los servidores principales y los almacena en una sola ubicación.*  
  
### <a name="impact"></a>Impacto  
*Toda la replicación de todos los servidores principales se almacena en una ubicación, lo que podría provocar problemas de seguridad o privacidad.*  
  
## <a name="resolution"></a>Resolución  
*Utilice el Administrador de Hyper-V para crear nuevas entradas de autorización para los servidores principales específicos y especificar ubicaciones de almacenamiento separadas para cada uno de ellos. Puede usar caracteres comodín para agrupar servidores principales en conjuntos para cada entrada de autorización.*  
  
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
  
## <a name="see-also"></a>Vea también  
[New-VMReplicationAuthorizationEntry](https://technet.microsoft.com/library/hh848606.aspx)  
  


