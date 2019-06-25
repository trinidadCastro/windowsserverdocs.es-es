---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: Escenario Get visión general de los datos mediante el uso de clasificación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881486"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Escenario: Comprender los datos mediante la clasificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En la mayoría de las organizaciones ha ido aumentando la dependencia de los datos y los recursos de almacenamiento. Los administradores de TI se enfrentan al creciente desafío que supone la supervisión de infraestructuras de almacenamiento cada vez más grandes y más complejas, al mismo tiempo que se les asigna la responsabilidad de garantizar que el costo total de la propiedad se mantenga en un nivel razonable. La administración de recursos de almacenamiento no se limita solo al volumen o la disponibilidad de datos, sino que implica además cumplir las directivas de la compañía y saber cómo se consume el almacenamiento para permitir un uso y un cumplimiento eficaces que mitiguen el riesgo. La infraestructura de clasificación de archivos permite obtener una idea clara de los datos mediante la automatización de los procesos de clasificación con el fin de poder administrar los datos de manera más eficaz. La infraestructura de clasificación de archivos proporciona los siguientes métodos de clasificación: manual, mediante programación y automática. Este tema se centra en el método de clasificación automática de archivos.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
La infraestructura de clasificación de archivos usa reglas de clasificación para examinar los archivos y clasificarlos según su contenido. Las propiedades de clasificación se definen de forma centralizada en Active Directory para que estas definiciones se puedan compartir en todos los servidores de archivos de la organización. Puedes crear reglas de clasificación que busquen en los archivos una cadena estándar o una cadena que coincida con un patrón (expresión regular). Cuando se encuentra un parámetro de clasificación configurado en un archivo, ese archivo se clasifica como configurado en la regla de clasificación. Algunos ejemplos de reglas de clasificación son:  
  
-   Clasificar cualquier archivo que contiene la cadena "Contoso confidencial" como si tuviera un fuerte impacto empresarial  
  
-   Clasificar cualquier archivo que contenga al menos 10 números de la seguridad social como archivo que contiene información de identificación personal.  
  
Cuando se clasifica un archivo, puedes usar una tarea de administración de archivos para realizar una acción en cualquier archivo que esté clasificado de una forma específica. Las acciones de una tarea de administración de archivos incluyen proteger los derechos asociados con el archivo, expirar el archivo y ejecutar una acción personalizada (como publicar información en un servicio web).  
  
Encontrarás información sobre planificación para configurar la clasificación automática de archivos en [Planificar la clasificación automática de archivos](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
Puede encontrar los pasos para clasificar archivos automáticamente en [implementar la clasificación automática de archivo &#40;pasos de demostración&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Este escenario forma parte del escenario de control de acceso dinámico. Para obtener más información sobre el control de acceso dinámico, consulta:  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Infraestructura de clasificación de archivos en Windows Server 2012 contribuye al Control de acceso dinámico porque permite a los propietarios de datos empresariales clasificar y etiquetar los datos fácilmente. La información de clasificación que se almacena en la directiva de acceso central permite definir directivas de acceso para las clases de datos que son críticos para la empresa.  
  
## <a name="BKMK_NEW"></a>Características incluidas en este escenario  
En la tabla siguiente, se enumeran las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Característica|Compatibilidad con este escenario|  
|-----------|---------------------------------|  
|[Información general del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831701.aspx)|La infraestructura de clasificación de archivos es una característica incluida en el Administrador de recursos del servidor de archivos.|  
|[Información general de servicios de almacenamiento y de archivo](https://technet.microsoft.com/library/hh831487.aspx)|Administrador de recursos del servidor de archivos es una característica incluida en el rol de servidor Servicios de archivo.|  
  

