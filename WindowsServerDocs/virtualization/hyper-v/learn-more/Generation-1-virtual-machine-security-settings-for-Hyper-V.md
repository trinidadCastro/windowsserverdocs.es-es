---
title: Configuración de seguridad de máquina virtual de generación 1 para Hyper-V
description: Describe la configuración de seguridad disponible en el administrador de Hyper-V para máquinas virtuales de generación 1.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: ceb3c2628546815f9b0af35946e173f4276130d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392789"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Configuración de seguridad de máquina virtual de generación 1

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use la configuración de seguridad de la máquina virtual de generación 1 en el administrador de Hyper-V para ayudar a proteger los datos y el estado de una máquina virtual.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configuración de compatibilidad de cifrado en el administrador de Hyper-V

Puede ayudar a proteger los datos y el estado de la máquina virtual seleccionando la siguiente opción de compatibilidad con el cifrado.

- **Cifrar el tráfico de migración de máquina virtual y estado** : cifra el estado guardado de la máquina virtual cuando se escribe en el disco y el tráfico de migración en vivo.

Para habilitar esta opción, debe agregar una unidad de almacenamiento de claves para la máquina virtual.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Unidad de almacenamiento de claves en el administrador de Hyper-V

Una unidad de almacenamiento de claves proporciona una unidad pequeña a la máquina virtual para que se almacene una clave de BitLocker. Esto permite que la máquina virtual Cifre su disco del sistema operativo sin necesidad de un chip de Módulo de plataforma segura virtualizado (TPM). El contenido de la unidad de almacenamiento de claves se cifra mediante un protector de clave. El protector de claves crea el host de Hyper-V para ejecutar la máquina virtual. Tanto el contenido de la unidad de almacenamiento de claves como el protector de clave se almacenan como parte del estado de tiempo de ejecución de la máquina virtual.

Para descifrar el contenido de la unidad de almacenamiento de claves e iniciar la máquina virtual, el host de Hyper-V debe ser:

- Parte de un tejido protegido autorizado para esta máquina virtual, o bien
- Tener la clave privada de uno de los tutores de la máquina virtual.

Para más información sobre los tejidos protegidos, consulte la sección Introducción a las máquinas virtuales blindadas en [seguridad y garantía](../../../security/Security-and-Assurance.md).

Puede Agregar una unidad de almacenamiento de claves a una ranura vacía en una de las controladoras IDE de la máquina virtual. Para ello, haga clic en **Agregar unidad de almacenamiento de claves** para agregar una unidad de almacenamiento de claves a la primera ranura de la controladora IDE disponible en esta máquina virtual.

## <a name="see-also"></a>Vea también

- [Configuración de seguridad de máquina virtual de generación 2 en el administrador de Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Seguridad y control](../../../security/Security-and-Assurance.md)