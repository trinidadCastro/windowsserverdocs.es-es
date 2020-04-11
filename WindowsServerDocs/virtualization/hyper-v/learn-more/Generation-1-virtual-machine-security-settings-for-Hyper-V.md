---
title: Configuración de seguridad de las máquinas virtuales de generación 1 para Hyper-V
description: Describe la configuración de seguridad disponible en el Administrador de Hyper-V para las máquinas virtuales de generación 1.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f86c4fe9222f08b3ef3719080deeb4fbda6edd33
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860818"
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

Para más información sobre los tejidos protegidos, consulta la sección Introducción a las máquinas virtuales blindadas en [Seguridad y control](../../../security/Security-and-Assurance.md).

Puedes agregar una unidad de almacenamiento de claves a una ranura vacía en uno de los controladores IDE de la máquina virtual. Para ello, haz clic en **Agregar unidad de almacenamiento de claves** para agregar una unidad de almacenamiento de claves a la primera ranura del controlador IDE disponible en esta máquina virtual.

## <a name="see-also"></a>Consulta también

- [Configuración de seguridad de las máquinas virtuales de generación 2 del Administrador de Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Seguridad y control](../../../security/Security-and-Assurance.md)