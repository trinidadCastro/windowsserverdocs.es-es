---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: "Retención de implementación de escenario de información en servidores de archivos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62339170895190435adb9a97d565877ff78d97d7
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Escenario: Implementar la retención de información en servidores de archivos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un período de retención es la cantidad de tiempo que debe ser un documento conserva antes de que ha caducado. Dependiendo de la organización, el período de retención pueden diferir. Puedes clasificar los archivos en una carpeta como si tuvieran un período de retención cortos, mediano o a largo plazo y, a continuación, asigna un período de tiempo para cada período. Quieres mantener un archivo indefinidamente poniéndolos en suspensión legal.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
Infraestructura de clasificación de archivo y el Administrador de recursos del servidor de archivos usa tareas de administración de archivos y la clasificación de archivo para aplicar períodos de retención de un conjunto de archivos. Puedes asignar un período de retención en una carpeta y, a continuación, usar una tarea de administración de archivos para configurar cuánto tiempo es de un período de retención asignado al último. Cuando los archivos en la carpeta están a punto de expirar, el propietario del archivo recibe una notificación por correo electrónico. También puedes restringir el acceso de un archivo como si fueran legalmente para que la tarea de administración de archivos no caduca el archivo.  
  
Puedes encontrar información de planificación para configurar el período de retención de [planear la retención de información en los servidores de archivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Puedes encontrar pasos de clasificación de archivos para sostener legal y configuración de un período de retención en [implementar implementar retención de la información en los servidores de archivos & #40; pasos de demostración & #41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Ese escenario solo describe cómo clasificar manualmente un documento de retención legal. Sin embargo, es posible en Windows Server 2012 clasificar automáticamente los documentos de retención legal. Una forma de hacerlo consiste en crear un clasificador de Windows PowerShell que compara el propietario del archivo a una lista de cuentas de usuario que están en suspensión legal. Si el propietario del archivo es una parte de la lista de cuentas de usuario, el archivo se clasifica para sostener legal.  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario es parte del escenario de Control de acceso dinámico. Para obtener más información sobre el Control de acceso dinámico, consulta:  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Características incluidas en este escenario  
La siguiente tabla enumera las características que forman parte de este escenario y describe cómo apoyan.  
  
|Característica|¿Cómo admite este escenario|  
|-----------|---------------------------------|  
|[Información general del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|Infraestructura de clasificación de archivo es una característica que se incluye en el Administrador de recursos del servidor de archivos.|  
|[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)|Administrador de recursos del servidor de archivos es una característica que se incluye con el rol de servidor de servicios de archivo.|  
  
  


