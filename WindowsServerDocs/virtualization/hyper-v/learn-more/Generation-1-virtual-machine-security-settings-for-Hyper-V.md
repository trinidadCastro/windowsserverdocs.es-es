---
title: Configuración de seguridad de máquina virtual de generación 1 de Hyper-V
description: Describe la configuración de seguridad disponible en el Administrador de Hyper-V para máquinas virtuales de generación 1
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 923216142a45071bc3623e3f37b37cc6a2361f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812146"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Configuración de seguridad de máquina virtual de generación 1

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

Use la configuración de seguridad de la máquina virtual de generación 1 en el Administrador de Hyper-V para ayudar a proteger los datos y el estado de una máquina virtual.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configuración de compatibilidad de cifrado en el Administrador de Hyper-V

Puede ayudar a proteger los datos y el estado de la máquina virtual seleccionando la opción de soporte técnico de cifrado siguientes.

- **Cifrar el tráfico de migración de estado y la máquina virtual** -cifra el estado de la máquina virtual que guardó cuando se escribe en disco y el tráfico de migración en vivo.

Para habilitar esta opción, debe agregar una unidad de almacenamiento de claves para la máquina virtual.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Unidad de almacenamiento de claves en el Administrador de Hyper-V

Una unidad de almacenamiento de claves proporciona una unidad de pequeña a la máquina virtual almacenar una clave de BitLocker. Esto permite que la máquina virtual cifrar su disco del sistema operativo sin necesidad de un chip de módulo de plataforma segura (TPM) virtualizado. El contenido de la unidad de almacenamiento de claves se cifra mediante el uso de un Protector de clave. El host de Hyper-V del Protector de clave authories para ejecutar la máquina virtual. Tanto el contenido de la unidad de almacenamiento de claves y el Protector de clave se almacena como parte del estado de tiempo de ejecución de la máquina virtual.

Para descifrar el contenido de la unidad de almacenamiento de claves e iniciar la máquina virtual, el host de Hyper-V debe ser:

- Parte de un tejido protegido autorizado para esta máquina virtual, o
- Tiene una clave privada de una de las protecciones de la máquina virtual.

Para obtener más información sobre los tejidos protegidos, consulte la sección de introducción a las máquinas virtuales blindadas en [seguridad y control](../../../security/Security-and-Assurance.md).

Puede agregar una unidad de almacenamiento de claves en una ranura vacía en una de las controladoras IDE de la máquina virtual. Para ello, haga clic en **agregar la unidad de almacenamiento de la clave** para agregar una unidad de almacenamiento de claves a la primera ranura de controlador IDE gratuita de esta máquina virtual.

##<a name="see-also"></a>Vea también

- [Configuración de seguridad de máquina virtual de generación 2 en el Administrador de Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Seguridad y control](../../../security/Security-and-Assurance.md)