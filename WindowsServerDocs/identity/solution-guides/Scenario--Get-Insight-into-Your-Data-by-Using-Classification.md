---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: "Escenario Get detenimiento los datos mediante el uso de clasificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Escenario: Obtener información sobre los datos mediante el uso de clasificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dependencia en los recursos de datos y almacenamiento ha continuado creciendo en importancia para la mayoría de las organizaciones. Los administradores de TI tienen el desafío creciente de supervisión de infraestructuras de almacenamiento más grandes y complejas, mientras que al mismo tiempo que cuenta con la responsabilidad asegurarse de que total costo de la propiedad se mantiene en niveles razonables. Administración de recursos de almacenamiento es no solo sobre el volumen o la disponibilidad de los datos; También es acerca de la aplicación de directivas de empresa y saber cómo se consume almacenamiento para habilitar el uso eficaz y cumplimiento para mitigar el riesgo. Infraestructura de clasificación de archivo, se proporciona información sobre los datos mediante la automatización de procesos de clasificación para que puedan administrar los datos de manera más eficaz. Los siguientes métodos de clasificación están disponibles con la infraestructura de clasificación de archivo: manual, mediante programación y automática. En este tema se centra en el método de clasificación automática de archivos.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
Infraestructura de clasificación de archivo usa reglas de clasificación para analizar archivos automáticamente y clasificarlos según el contenido del archivo. Propiedades de la clasificación se definen de forma centralizada en Active Directory para que estas definiciones pueden compartir a través de los servidores de archivos de la organización. Puedes crear reglas de clasificación que examinar los archivos de una cadena estándar o de una cadena que coincide con un modelo (expresión regular). Cuando se encuentra un parámetro de clasificación configurado en un archivo, ese archivo se clasifica como se hayan configurado en la regla de clasificación. Algunos ejemplos de reglas de clasificación:  
  
-   Clasificar cualquier archivo que contiene la cadena "Información confidencial de Contoso" como tener un impacto empresarial alto  
  
-   Clasificar cualquier archivo que contiene al menos 10 números de la seguridad social como si tuvieran información personalmente identificable  
  
Cuando un archivo se clasifica, puedes usar un archivo de tareas de administración para realizar acciones en todos los archivos que están clasifican de forma específica. Las acciones en una tarea de administración de archivos incluyen proteger los derechos asociados con el archivo, que expira el archivo y ejecutan una acción personalizada (por ejemplo, enviar información a un servicio web).  
  
Puedes encontrar información de planificación de la configuración de clasificación automático de los archivos en [planear la clasificación de archivo automática](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
Puedes encontrar los pasos para clasificar automáticamente los archivos en [implementar la clasificación de archivo automática & #40; pasos de demostración & #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario es parte del escenario de Control de acceso dinámico. Para obtener más información sobre el Control de acceso dinámico, consulta:  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Infraestructura de clasificación de archivo en Windows Server 2012 contribuye al Control de acceso dinámico activando los propietarios de datos de empresas clasificar y etiquetar datos fácilmente. La información de clasificación que se almacena en la directiva de acceso central te permite definir las directivas de acceso para las clases de datos que son críticas para el negocio.  
  
## <a name="BKMK_NEW"></a>Características incluidas en este escenario  
La siguiente tabla enumera las características que forman parte de este escenario y describe cómo apoyan.  
  
|Característica|¿Cómo admite este escenario|  
|-----------|---------------------------------|  
|[Información general del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|Infraestructura de clasificación de archivo es una característica que se incluye en el Administrador de recursos del servidor de archivos.|  
|[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)|Administrador de recursos del servidor de archivos es una característica que se incluye con el rol de servidor de servicios de archivo.|  
  


