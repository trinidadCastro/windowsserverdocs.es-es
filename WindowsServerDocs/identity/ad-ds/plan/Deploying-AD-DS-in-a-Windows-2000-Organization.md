---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Implementación de ADDS en una organización de Windows 2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cad5deb32a31f15277c3e0e985d5b7d9b07856aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408915"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Implementación de ADDS en una organización de Windows 2000

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si su organización ejecuta actualmente Windows 2000 Active Directory, puede implementar Windows Server 2008 Active Directory Domain Services (AD DS) mediante la realización de una actualización local de algunos o de todos los sistemas operativos de los controladores de dominio en Windows. Server 2008 o mediante la introducción de controladores de dominio que ejecutan Windows Server 2008 en su entorno de.  
  
Para poder agregar un controlador de dominio que ejecute Windows Server 2008 a un dominio existente de Windows 2000 Active Directory, debe ejecutar **Adprep**, una herramienta de línea de comandos. Adprep extiende el esquema de AD DS, actualiza los descriptores de seguridad predeterminados de los objetos seleccionados y agrega nuevos objetos de directorio según lo requieran algunas aplicaciones. Adprep está disponible en el disco de instalación de Windows Server 2008 (\sources\adprep\adprep.exe). Para obtener más información, consulte Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Si desea realizar una actualización en contexto de un controlador de dominio existente de Windows 2000 AD DS a Windows Server 2008, primero debe actualizar el servidor a Windows Server 2003 y, a continuación, actualizarlo a Windows Server 2008.  
  
En la ilustración siguiente se muestran los pasos para implementar el AD DS de Windows Server 2008 en un entorno de red que ejecuta actualmente Windows 2000 Active Directory.  
  
![implementación en una organización de Windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Si desea establecer el nivel funcional de dominio o bosque en Windows Server 2008, todos los controladores de dominio del entorno deben ejecutar el sistema operativo Windows Server 2008.  
  
La consolidación de los dominios de cuentas y recursos que se actualizan en su lugar desde un entorno de Windows 2000 como parte de la implementación de AD DS de Windows Server 2008 puede requerir la reestructuración de dominios entre bosques o dentro de un bosque. La reestructuración de dominios de AD DS entre bosques ayuda a reducir la complejidad de la organización y los costos administrativos asociados. La reestructuración de dominios de AD DS dentro de un bosque ayuda a reducir la sobrecarga administrativa de la organización, ya que reduce el tráfico de replicación, reduce la cantidad de administración de usuarios y grupos necesaria y simplifica la administración de directiva de grupo. Para obtener más información, vea la guía de migración de ADMT v 3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obtener una lista de tareas detalladas que puede usar para planear e implementar AD DS en una organización que ejecuta actualmente Windows 2000 Active Directory, consulte [Checklist: deploying AD DS in a windows 2000 Organization](https://technet.microsoft.com/library/cc732737.aspx).  
  


