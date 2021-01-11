---
title: Límite de credenciales por aplicación
description: Solución del problema por el que solo se pueden usar 20 credenciales por aplicación.
ms.topic: troubleshooting
author: HeidiLohr
manager: lizross
ms.author: helohr
ms.date: 12/14/2020
ms.localizationpriority: medium
ms.openlocfilehash: 5ce9b606e9205758b819dfe89cadbd0187357243
ms.sourcegitcommit: 4f7308430a69fe7965e16aa5b31f87c5d68e4a09
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97578276"
---
# <a name="credential-limit-per-app"></a>Límite de credenciales por aplicación

Windows solo permite un máximo de 20 credenciales por aplicación. Si necesita tener más de 20 credenciales por aplicación, siga las instrucciones incluidas en este artículo.

## <a name="bypass-the-20-credential-limit"></a>Omisión del límite de 20 credenciales

Para omitir el límite de 20 credenciales:

1. Abra PowerShell como administrador.
2. Establezca el valor de entrada del Registro DWORD **HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Vault\MaxPerAppCredentialNumber** en un número superior a 20.
3. Reinicie el equipo.
4. Pruebe la configuración mediante la creación de un conjunto de credenciales en el cliente de Escritorio remoto.

## <a name="potential-risks"></a>Posibles riesgos

Al cambiar este valor del Registro, es importante tener en cuenta estas cuestiones:

- Esta es una operación de administración. Si se introducen errores en el Registro, el equipo podría volverse inestable. Los usuarios que cambien las entradas del Registro lo harán bajo su propia responsabilidad.
- Este cambio del Registro afectará a todas las aplicaciones del equipo.
