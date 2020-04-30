---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Implementación de AD DS en una organización de Windows Server 2003
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 747daaafede4ef06cfd8aa59a48be9de56c23da4
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624293"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Implementación de AD DS en una organización de Windows Server 2003

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si su organización ejecuta actualmente Windows Server 2003 Active Directory, puede implementar Windows Server 2008 Active Directory Domain Services (AD DS), ya sea mediante una actualización local de algunos o todos los sistemas operativos de los controladores de dominio a Windows Server 2008 o mediante la introducción de controladores de dominio que ejecuten Windows Server 2008 en su entorno.

Antes de poder agregar un controlador de dominio que ejecute Windows Server 2008 a un dominio existente de Windows Server 2003 Active Directory, debe ejecutar **Adprep**, una herramienta de línea de comandos. Adprep extiende el esquema de AD DS, actualiza los descriptores de seguridad predeterminados de los objetos seleccionados y agrega nuevos objetos de directorio según lo requieran algunas aplicaciones. Adprep está disponible en el disco de instalación de Windows Server 2008 (\sources\adprep\adprep.exe). Para obtener más información, consulte [Adprep](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731728(v=ws.11)).

En la ilustración siguiente se muestran los pasos para implementar Windows Server 2008 AD DS en un entorno de red que ejecuta actualmente Windows Server 2003 Active Directory.

![implementar en una organización de Windows 2003](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)

> [!NOTE]
> Si desea establecer el nivel funcional de dominio o bosque en Windows Server 2008, todos los controladores de dominio del entorno deben ejecutar el sistema operativo Windows Server 2008.

La consolidación de dominios de recursos y de cuentas que se actualizan en su lugar desde un entorno de Windows Server 2003 como parte de la implementación de AD DS de Windows Server 2008 puede requerir la reestructuración de dominios entre bosques o dentro de un bosque. La reestructuración de dominios de AD DS entre bosques ayuda a reducir la complejidad de la representación de la organización en AD DS y ayuda a reducir los costos administrativos asociados. La reestructuración de dominios de AD DS dentro de un bosque ayuda a reducir la sobrecarga administrativa de la organización al reducir el tráfico de replicación, lo que reduce la cantidad de administración de usuarios y grupos necesaria y simplifica la administración de directiva de grupo. Para obtener más información, vea la [Guía de ADMT: migración y reestructuración de dominios de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)).

Para obtener una lista de tareas detalladas que puede usar para planear e implementar AD DS en una organización que ejecuta Windows Server 2003 Active Directory, consulte [Checklist: deploying AD DS in a Windows server 2003 Organization](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771407(v=ws.10)).

