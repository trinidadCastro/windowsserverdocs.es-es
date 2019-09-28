---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Implementación de ADDS en una organización de Windows Server2003
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7426ba8fa3ea5077510655ea4877d0e0b69226ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408907"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Implementación de ADDS en una organización de Windows Server2003

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si su organización ejecuta actualmente Windows Server 2003 Active Directory, puede implementar Windows Server 2008 Active Directory Domain Services (AD DS) mediante la realización de una actualización local de algunos o todos los sistemas operativos de los controladores de dominio a  Windows Server 2008 o mediante la introducción de controladores de dominio que ejecutan Windows Server 2008 en su entorno de.  
  
Antes de poder agregar un controlador de dominio que ejecute Windows Server 2008 a un dominio existente de Windows Server 2003 Active Directory, debe ejecutar **Adprep**, una herramienta de línea de comandos. Adprep extiende el esquema de AD DS, actualiza los descriptores de seguridad predeterminados de los objetos seleccionados y agrega nuevos objetos de directorio según lo requieran algunas aplicaciones. Adprep está disponible en el disco de instalación de Windows Server 2008 (\sources\adprep\adprep.exe). Para obtener más información, consulte Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
En la ilustración siguiente se muestran los pasos para implementar Windows Server 2008 AD DS en un entorno de red que ejecuta actualmente Windows Server 2003 Active Directory.  
  
![implementar en una organización de Windows 2003](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> Si desea establecer el nivel funcional de dominio o bosque en Windows Server 2008, todos los controladores de dominio del entorno deben ejecutar el sistema operativo Windows Server 2008.  
  
La consolidación de dominios de recursos y de cuentas que se actualizan en su lugar desde un entorno de Windows Server 2003 como parte de la implementación de AD DS de Windows Server 2008 puede requerir la reestructuración de dominios entre bosques o dentro de un bosque. La reestructuración de dominios de AD DS entre bosques ayuda a reducir la complejidad de la representación de la organización en AD DS y ayuda a reducir los costos administrativos asociados. La reestructuración de dominios de AD DS dentro de un bosque ayuda a reducir la sobrecarga administrativa de la organización al reducir el tráfico de replicación, reducir la cantidad de administración de usuarios y grupos necesaria y simplificar la administración de grupos Directivas. Para obtener más información, vea la guía de migración de ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obtener una lista de tareas detalladas que puede usar para planear e implementar AD DS en una organización que ejecuta Windows Server 2003 Active Directory, consulte [Checklist: Implementación de AD DS en una organización de Windows Server 2003 @ no__t-0.  
  


