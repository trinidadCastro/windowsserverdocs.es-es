---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: Implementación de ADDS en una organización de Windows 2000
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5015c0ca2c01fca966927af4207a7e3501d9cd39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854286"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Implementación de ADDS en una organización de Windows 2000

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si su organización se está ejecutando Windows 2000 Active Directory, puede implementar Windows Server 2008 Active Directory Domain Services (AD DS) mediante la realización de una actualización en contexto de algunos o todos los sistemas operativos de los controladores de dominio Windows Server 2008 o mediante la introducción de los controladores de dominio que ejecutan Windows Server 2008 en su entorno.  
  
Antes de poder agregar un controlador de dominio que ejecutan Windows Server 2008 a un dominio de Active Directory de Windows 2000 existente, debe ejecutar **adprep**, una herramienta de línea de comandos. Adprep extiende el esquema de AD DS, actualiza los descriptores de seguridad predeterminados de los objetos seleccionados y agrega nuevos objetos de directorio según sea necesario por determinadas aplicaciones. Adprep está disponible en el disco de instalación de Windows Server 2008 (\sources\adprep\adprep.exe). Para obtener más información, consulte Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)).  
  
> [!NOTE]  
> Si desea realizar una actualización en contexto de un controlador de dominio de AD DS de Windows 2000 existente a Windows Server 2008, primero debe actualizar el servidor a Windows Server 2003 y, a continuación, actualizarlo a Windows Server 2008.  
  
La ilustración siguiente muestra los pasos para implementar Windows Server 2008 AD DS en un entorno de red que se está ejecutando Windows 2000 Active Directory.  
  
![implementación en una organización de windows 2000](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> Si desea establecer el nivel funcional del dominio o bosque a Windows Server 2008, todos los controladores de dominio en su entorno deben ejecutar el sistema operativo Windows Server 2008.  
  
Consolidación de los dominios de recursos y cuentas que se actualizan in situ de un entorno de Windows 2000 como parte de su AD DS de Windows Server 2008 puede requerir implementación reestructuración o la reestructuración de dominios dentro del mismo bosque. Reestructuración de dominios de AD DS entre bosques le ayuda a reducir la complejidad de su organización y los costos administrativos asociados. Reestructuración de dominios de AD DS en un bosque le ayuda a reducir la sobrecarga administrativa para su organización, por lo que reduce el tráfico de replicación, lo que reduce la cantidad de usuario y la administración de grupos que se necesita y lo que simplifica la administración de Directiva de grupo. Para obtener más información, consulte la Guía de migración de ADMT v3.1 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Para obtener una lista de tareas detalladas que puede usar para planear e implementar AD DS en una organización que se está ejecutando Windows 2000 Active Directory, consulte [lista de comprobación: Implementación de AD DS en una organización de Windows 2000](https://technet.microsoft.com/library/cc732737.aspx).  
  


