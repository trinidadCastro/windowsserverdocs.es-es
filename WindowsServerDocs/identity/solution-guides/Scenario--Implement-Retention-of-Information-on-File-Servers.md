---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Escenario implementar retención de información en servidores de archivos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880256"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Escenario: Implementar la retención de información en servidores de archivos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un período de retención es la cantidad de tiempo que un documento debería conservarse antes de que expire. El período de retención puede ser distinto en función de la organización. Puedes clasificar archivos en una carpeta según tengan un período de retención a corto, medio o largo plazo y, después, asignar un período de tiempo para cada tipo. Quizás desee conservar un archivo indefinidamente mediante una retención legal.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
La infraestructura de clasificación de archivos y el Administrador de recursos del servidor de archivos utilizan tareas de administración de archivos y la clasificación de archivos para aplicar períodos de retención para un conjunto de archivos. Puede asignar un período de retención en una carpeta y, a continuación, usar una tarea de administración de archivos para configurar la duración de un período de retención asignado. Cuando los archivos de la carpeta están a punto de expirar, el propietario del archivo recibe un correo electrónico de notificación. También puede clasificar un archivo con el estado de retención legal para que la tarea de administración de archivos no haga expirar el archivo.  
  
Encontrarás más información sobre cómo configurar la retención en [Planificar la retención de información en servidores de archivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Puede encontrar pasos para clasificar archivos para retención legal y configurar un período de retención en [implementar implementar retención de información en servidores de archivos &#40;pasos de demostración&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> En ese escenario solo se describe cómo clasificar manualmente un documento para retención legal. Sin embargo, es posible en Windows Server 2012 para clasificar documentos automáticamente para retención legal. Una manera de hacerlo es crear un clasificador de Windows PowerShell que compare el propietario del archivo con una lista de cuentas de usuario que están en retención legal. Si el propietario del archivo forma parte de la lista de cuentas de usuario, el archivo se clasifica para retención legal.  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario forma parte del escenario de control de acceso dinámico. Para obtener más información sobre el control de acceso dinámico, consulta:  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Características incluidas en este escenario  
En la tabla siguiente, se enumeran las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Característica|Compatibilidad con este escenario|  
|-----------|---------------------------------|  
|[Información general del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|La infraestructura de clasificación de archivos es una característica incluida en el Administrador de recursos del servidor de archivos.|  
|[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)|Administrador de recursos del servidor de archivos es una característica incluida en el rol de servidor Servicios de archivo.|  
  
  


