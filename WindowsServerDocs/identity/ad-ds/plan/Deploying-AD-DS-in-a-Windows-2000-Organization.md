---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: "Implementación de AD DS en una organización de Windows 2000"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c46ff6aee03eee4169dbfe0cdff99febad312d2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Implementación de AD DS en una organización de Windows 2000

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si la organización se está ejecutando Windows 2000 Active Directory, puedes implementar Windows Server 2008 Active Directory Domain Services (AD DS) realizando una actualización en contexto de todas o parte de los sistemas operativos de los controladores de dominio a Windows Server 2008 o mediante la introducción de controladores de dominio ejecutan Windows Server 2008 en tu entorno.  
  
Antes de poder agregar un controlador de dominio que ejecutan Windows Server 2008a un dominio de Active Directory de Windows 2000 existente, debes ejecutar **adprep**, una herramienta de línea de comandos. Adprep extiende el esquema de AD DS, actualiza los descriptores de seguridad predeterminados de los objetos seleccionados y agrega nuevos objetos de directorio como sea necesario para algunas aplicaciones. Adprep está disponible en el disco de instalación de Windows Server 2008 (\sources\adprep\adprep.exe). Para obtener más información, consulta Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Si quieres realizar una actualización en contexto de un controlador de dominio de AD DS de Windows 2000 existente a Windows Server 2008, debes actualizar el servidor para Windows Server 2003 y, a continuación, la actualización a Windows Server 2008.  
  
La siguiente ilustración muestra los pasos para implementar Windows Server 2008 AD DS en un entorno de red que se está ejecutando Windows 2000 Active Directory.  
  
![Implementar en una organización de windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Si quieres establecer el nivel funcional de dominio o bosque a Windows Server 2008, todos los controladores de dominio en el entorno deben ejecutar el sistema operativo Windows Server 2008.  
  
Consolidar los dominios de recursos y la cuenta que se han actualizado en lugar de un entorno de Windows 2000 como parte de tu equipo con Windows Server 2008 AD DS implementación puede requerir bosques o reestructuración dentro del dominio. Reestructuración de dominios de AD DS entre bosques te ayuda a reducir la complejidad de la organización y los costos asociados administrativos. Reestructuración de dominios de AD DS en un bosque te ayuda a reducir la sobrecarga administrativa para la organización en reducir el tráfico de replicación, reducir la cantidad de usuario y la administración de grupo que se necesita y simplificar la administración de directiva de grupo. Para obtener más información, consulta la Guía de migración de ADMT v3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obtener una lista de las tareas detalladas que puedes usar para planear e implementar AD DS en una organización que se está ejecutando Windows 2000 Active Directory, consulte [lista de comprobación: implementación de AD DS en una organización de Windows 2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


