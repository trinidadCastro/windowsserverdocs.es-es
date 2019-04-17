---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: "Implementación de AD DS en una organización de Windows Server 2003"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: df1267ded5ece95dd5a3ab17e4ec6ad5d87a2f12
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Implementación de AD DS en una organización de Windows Server 2003

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si la organización se está ejecutando Active Directory de Windows Server 2003, puedes implementar Windows Server 2008 Active Directory Domain Services (AD DS) realizando una actualización en contexto de todas o parte de los sistemas operativos de los controladores de dominio a Windows Server 2008 o mediante la introducción de controladores de dominio ejecutan Windows Server 2008 en tu entorno.  
  
Antes de poder agregar un controlador de dominio que ejecutan Windows Server 2008a un dominio de Active Directory de Windows Server 2003 existente, debes ejecutar **adprep**, una herramienta de línea de comandos. Adprep extiende el esquema de AD DS, actualiza los descriptores de seguridad predeterminados de los objetos seleccionados y agrega nuevos objetos de directorio como sea necesario para algunas aplicaciones. Adprep está disponible en el disco de instalación de Windows Server 2008 (\sources\adprep\adprep.exe). Para obtener más información, consulta Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
La siguiente ilustración muestra los pasos para implementar Windows Server 2008 AD DS en un entorno de red que se está ejecutando Windows Server 2003 Active Directory.  
  
![Implementar en una organización de windows 2003](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Si quieres establecer el nivel funcional de dominio o bosque a Windows Server 2008, todos los controladores de dominio en el entorno deben ejecutar el sistema operativo Windows Server 2008.  
  
Consolidar los dominios de recursos y dominios de cuenta que se actualizan en lugar de un entorno de Windows Server 2003 como parte de la implementación de Windows Server 2008 AD DS puede requerir bosques o reestructuración dentro del dominio. Reestructuración de dominios de AD DS entre bosques ayuda a reducir la complejidad de la representación de la organización en AD DS y ayuda a reducir los costos asociados administrativos. Reestructuración de dominios de AD DS en un bosque te ayuda a reducir la sobrecarga administrativa para la organización en reducir el tráfico de replicación, reducir la cantidad de usuario y la administración de grupo que se necesita y simplificar la administración de directiva de grupo. Para obtener más información, consulta la Guía de migración de ADMT v3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obtener una lista de las tareas detalladas que puedes usar para planear e implementar AD DS de una organización que ejecuta Active Directory de Windows Server 2003, vea [lista de comprobación: implementación de AD DS en una organización de Windows Server 2003](https://technet.microsoft.com/library/cc771407.aspx).  
  


