---
title: 'Servicios de escritorio remoto: almacenamiento de datos seguro'
description: Información de planeación para almacenar datos de forma segura mediante el uso de discos de perfil de usuario (UPD) de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: 242d09e49141e9fb789a83421714366105730cc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870596"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Servicios de escritorio remoto: almacenamiento de datos seguro con los UPD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Recursos empresariales de Store, los datos de personalización de usuario y configuración de forma segura en el entorno local o en Azure. Hosts de sesión de escritorio remoto usa la autenticación de Active Directory y capacitar a los usuarios con los recursos que necesitan en un entorno personalizado, de forma segura. 

Los usuarios cómo asegurarse de que tienen una experiencia coherente, independientemente del punto de conexión desde el que obtener acceso a sus recursos remotos, es un aspecto importante de la administración de una implementación de RDS. Discos de perfil de usuario (UPD) permiten la configuración de la aplicación para seguir un usuario en una sola colección, las personalizaciones y datos de usuario. UPD es un archivo de disco duro virtual por recopilación guarda en un recurso compartido central que se monta en una sesión de usuario cuando inician sesión: el UPD se trata como una unidad local para el tiempo que dure esa sesión por usuario. 

Desde la perspectiva del usuario, el UPD proporciona una experiencia de famililar - que guardarán sus documentos en sus documentos en la carpeta (en lo que parece ser una unidad local), cambiar su configuración de la aplicación como de costumbre y hacen las personalizaciones realizadas en su entorno de Windows. Todos estos datos, incluidos el subárbol del registro, se almacenan en el UPD y continúa en un recurso compartido de red central. Los UPD solo están disponibles para el usuario cuando el usuario está conectado activamente a un escritorio o una conexión de RemoteApp. Los UPD solo pueden moverse dentro de una colección dado del usuario C:\Users todo&#92;\<username\> directorio (incluyendo AppData\Local) se almacena en el UPD.

Puede usar [cmdlets de PowerShell](https://technet.microsoft.com/library/jj215443.aspx) para designar la ruta de acceso para el recurso compartido central, el tamaño de cada UPD y las carpetas que deben incluirse o excluirse el perfil de usuario que se guardan en el UPD. Como alternativa, puede habilitar los UPD mediante Administrador del servidor, vaya a **Remote Desktop Services > colecciones > colecciones de escritorios > Propiedades de la colección > discos de perfil de usuario**. Tenga en cuenta que habilite o deshabilite los UPD para todos los usuarios de una colección completa, pero no para usuarios específicos de la colección. Los UPD deben almacenarse en un recurso compartido de archivo central donde los servidores de la colección tienen permisos control total. 

Se puede lograr una alta disponibilidad para sus UPD guardándolos en Azure con [espacios de almacenamiento directo](rds-storage-spaces-direct-deployment.md). 