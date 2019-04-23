---
title: Hyper-V debe ser el único rol habilitado
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bd03554396696a43b4821aff0f4ed893933484c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886466"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-V debe ser el único rol habilitado

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Se habilitan las funciones que no sea de Hyper-V en este servidor.*  
  
En la mayoría de los casos, no es una buena idea para instalar otros roles en un servidor que ejecuta el rol Hyper-V. Servicio de rol Host de virtualización de escritorio remoto es una excepción, ya que es parte del rol de servicios de escritorio remoto y requiere que Hyper-V esté instalado en el mismo servidor.  
  
## <a name="impact"></a>Impacto  
  
*El rol de Hyper-V debe ser el único rol habilitado en un servidor.*  
  
Esta práctica recomendada ayuda a mantener el sistema operativo del host libre de roles, características y las aplicaciones que no son necesarios para ejecutar Hyper-V. Siguiendo este procedimiento recomendado y que ejecuta Hyper-V en Nano Server ayuda a reducir el número de actualizaciones, necesitará porque solo Nano Server, los componentes del servicio de Hyper-V y el hipervisor de Windows estará sujeto a las actualizaciones de software.  
  
## <a name="resolution"></a>Resolución  
  
*Para quitar todas las funciones excepto Hyper-V, use el administrador del servidor.*  
  
El administrador del servidor incluye al Asistente para quitar Roles. Este asistente le permite quitar más de un rol a la vez. Antes de quitar roles, el Asistente para quitar Roles comprueba las dependencias reducir el riesgo de quitar software del que dependen de otros roles. Si se han encontrado dependencias, el asistente le pedirá que apruebe la eliminación de otros roles, servicios de rol o software requerido por las funciones instaladas.   
  
Para usar el administrador del servidor, debe estar iniciado en el equipo como administrador.  
  
#### <a name="to-remove-a-role"></a>Para quitar un rol  
  
1.  Abra el administrador del servidor mediante el uso de métodos abreviados en el **iniciar** menú, en la barra de tareas de Windows o en Herramientas administrativas.  
2.   En el **resumen de funciones** área de la ventana principal del administrador del servidor, haga clic en **quitar Roles**. Siga las instrucciones del Asistente para quitar el rol.   
  
  
  


