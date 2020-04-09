---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Escenario de implementación de la retención de información en servidores de archivos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2f6b55d8a98a0f4fb0c286e16d752a18e61dce1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861108"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Scenario: Implement Retention of Information on File Servers

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un período de retención es la cantidad de tiempo que un documento debería conservarse antes de que expire. El período de retención puede ser distinto en función de la organización. Puedes clasificar archivos en una carpeta según tengan un período de retención a corto, medio o largo plazo y, después, asignar un período de tiempo para cada tipo. Quizás desee conservar un archivo indefinidamente mediante una retención legal.  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descripción del escenario  
La infraestructura de clasificación de archivos y el Administrador de recursos del servidor de archivos utilizan tareas de administración de archivos y la clasificación de archivos para aplicar períodos de retención para un conjunto de archivos. Puede asignar un período de retención en una carpeta y, a continuación, usar una tarea de administración de archivos para configurar la duración de un período de retención asignado. Cuando los archivos de la carpeta están a punto de expirar, el propietario del archivo recibe un correo electrónico de notificación. También puede clasificar un archivo con el estado de retención legal para que la tarea de administración de archivos no haga expirar el archivo.  
  
Encontrarás más información sobre cómo configurar la retención en [Planificar la retención de información en servidores de archivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Puede encontrar los pasos para clasificar archivos para la retención legal y la configuración de un período de retención en implementación de la [implementación &#40;de la&#41;retención de información en servidores de archivos pasos](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md)de la demostración.  
  
> [!NOTE]  
> En ese escenario solo se describe cómo clasificar manualmente un documento para retención legal. Sin embargo, en Windows Server 2012 es posible clasificar documentos automáticamente para la retención legal. Una manera de hacerlo es crear un clasificador de Windows PowerShell que compare el propietario del archivo con una lista de cuentas de usuario que están en retención legal. Si el propietario del archivo forma parte de la lista de cuentas de usuario, el archivo se clasifica para retención legal.  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario forma parte del escenario de control de acceso dinámico. Para obtener más información sobre el control de acceso dinámico, consulta:  
  
-   [Access Control dinámico: información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Características incluidas en este escenario  
En la tabla siguiente, se enumeran las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Característica|Compatibilidad con este escenario|  
|-----------|---------------------------------|  
|[Información general de Administrador de recursos de servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|La infraestructura de clasificación de archivos es una característica incluida en el Administrador de recursos del servidor de archivos.|  
|[Introducción a los servicios de archivos y almacenamiento](https://technet.microsoft.com/library/hh831487.aspx)|Administrador de recursos del servidor de archivos es una característica incluida en el rol de servidor Servicios de archivo.|  
  
  


