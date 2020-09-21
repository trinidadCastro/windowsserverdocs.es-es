---
title: Configuración de seguridad de las máquinas virtuales de generación 1 para Hyper-V
description: Describe la configuración de seguridad disponible en el Administrador de Hyper-V para las máquinas virtuales de generación 1.
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: d7076056462e7fa3e822c49abfb37278f5ea3482
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746000"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Configuración de seguridad de las máquinas virtuales de generación 1

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Usa la configuración de seguridad de las máquinas virtuales de generación 1 del Administrador de Hyper-V para ayudar a proteger los datos y el estado de una máquina virtual.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configuración de Compatibilidad con cifrado en el Administrador de Hyper-V

Para ayudar a proteger los datos y el estado de la máquina virtual, puedes seleccionar la siguiente opción de compatibilidad con el cifrado.

- **Cifrar el estado y el tráfico de migración de la máquina virtual**: cifra el estado guardado de la máquina virtual cuando se escribe en el disco y el tráfico de migración en vivo.

Para habilitar esta opción, debes agregar una unidad de almacenamiento de claves para la máquina virtual.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Unidad de almacenamiento de claves del Administrador de Hyper-V

Una unidad de almacenamiento de claves proporciona una unidad pequeña a la máquina virtual para almacenar una clave de BitLocker. Esto permite que la máquina virtual cifre el disco del sistema operativo sin necesidad de un chip de Módulo de plataforma segura (TPM) virtualizado. El contenido de la unidad de almacenamiento de claves se cifra mediante un protector de clave. El protector de clave permite al host de Hyper-V ejecutar la máquina virtual. Tanto el contenido de la unidad de almacenamiento de claves como el protector de clave se almacenan como parte del estado de tiempo de ejecución de la máquina virtual.

Para descifrar el contenido de la unidad de almacenamiento de claves e iniciar la máquina virtual, el host de Hyper-V debe cumplir una de las siguientes condiciones:

- Formar parte de un tejido protegido autorizado para esta máquina virtual.
- Tener la clave privada de uno de los protectores de la máquina virtual.

Para más información sobre los tejidos protegidos, consulta la sección Introducción a las máquinas virtuales blindadas en [Seguridad y control](../../../security/Security-and-Assurance.yml).

Puedes agregar una unidad de almacenamiento de claves a una ranura vacía en uno de los controladores IDE de la máquina virtual. Para ello, haz clic en **Agregar unidad de almacenamiento de claves** para agregar una unidad de almacenamiento de claves a la primera ranura del controlador IDE disponible en esta máquina virtual.

## <a name="additional-references"></a>Referencias adicionales

- [Configuración de seguridad de las máquinas virtuales de generación 2 del Administrador de Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Seguridad y control](../../../security/Security-and-Assurance.yml)