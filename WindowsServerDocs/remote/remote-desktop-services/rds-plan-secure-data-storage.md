---
title: 'Servicios de Escritorio remoto: almacenamiento de datos seguro'
description: Información de planificación para almacenar datos de forma segura mediante el uso de discos de perfil de usuario (UPD) en RDS.
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
ms.openlocfilehash: d681530f849fe55495de0f4d4b564921d3b0bb3d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870872"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Servicios de Escritorio remoto: almacenamiento de datos seguro con UPD

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016

Recursos empresariales de Store, datos de personalización del usuario y configuración segura en el entorno local o en Azure. Los hosts de sesión de Escritorio remoto usan la autenticación de AD y otorgan a los usuarios los recursos que necesitan en un entorno personalizado, de forma segura. 

Asegurar que los usuarios tengan una experiencia consistente, independientemente del punto de conexión desde el que acceden a sus recursos remotos, es un aspecto importante de la administración de una implementación del Servicio de Escritorio remoto. Los Discos de perfil de usuario (UPD) permiten que los datos de los usuarios, las personalizaciones y la configuración de la aplicación sigan a un usuario en una sola colección. Un UPD es un archivo VHD creado en función de la colección y el usuario, que se guarda en un recurso compartido central que se monta en la sesión de un usuario cuando este inicia sesión: el UPD se trata como una unidad de disco local durante esa sesión. 

Desde la perspectiva del usuario, el UPD proporciona una experiencia familiar: guarda sus documentos en la carpeta Documentos (en lo que parece ser una unidad de disco local), cambia la configuración de la aplicación como de costumbre y realiza las personalizaciones en el entorno de Windows. Todos estos datos, incluido el subárbol del Registro, se almacenan en el UPD y persisten en un recurso compartido de red central. Los UPD solo están disponibles para el usuario cuando está conectado a un escritorio o RemoteApp. Igualmente, los UPD solo pueden desplazarse dentro de una colección porque el directorio completo `C:\Users\<username\>` del usuario (incluido AppData\Local) está almacenado en el UPD.

Puedes usar los [cmdlets de PowerShell](https://technet.microsoft.com/library/jj215443.aspx) para designar la ruta de acceso al recurso compartido central, el tamaño de cada UPD y las carpetas que deben incluirse o excluirse del perfil de usuario guardado en el UPD. Como alternativa, puedes habilitar los UPD a través del Administrador del servidor si accedes a **Servicios de Escritorio remoto** > **Colecciones** > **Colección de escritorio** > **Propiedades de la colección de escritorio** > **Discos de perfil de usuario**. Ten en cuenta que debes habilitar o deshabilitar los UPD de todos los usuarios de una colección completa, no los de usuarios específicos de esa colección. Los UPD deben almacenarse en un recurso compartido de archivos central donde los servidores de la colección tienen permisos de control total. 

Puedes obtener una alta disponibilidad para tus UPD si los almacenas en Azure con [Espacios de almacenamiento directo](rds-storage-spaces-direct-deployment.md). 