---
title: Comandos comunes de Git Bash para su uso con GitHub
description: Una lista de algunos de los comandos de Git bash que se usan con más frecuencia cuando se trabaja con GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 4ce5d4d8ce382e9ba421c20595715ec473cca241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70864998"
---
# <a name="common-git-bash-commands"></a>Comandos comunes de Git Bash

Estos son algunos de los comandos más usados en Git Bash, en función de cuándo los usará en el proceso de creación y edición de contenido.

## <a name="master-branch-related"></a>Principales relacionadas con la rama

Siempre debe usar Master como base para cualquier rama nueva.

| Comando | Descripción |
|---------|-------------|
| `git checkout master` | Cambiar a la maestra desde cualquier otra rama |
| `git pull upstream master` | Actualización de la copia local del maestro desde el repositorio de producción |

## <a name="branch-related"></a>Relacionado con la rama

| Comando | Descripción |
|---------|-------------|
| `git branch` | Ver las ramas existentes |
| `git checkout -B <name-of-branch>` | Crear una nueva rama |
| `git checkout <name-of-branch>` | Cambiar a otra rama |
| `git status` | Comprobar lo que está ocurriendo en la rama |
| `git branch -D <name-of-branch>` | Eliminar una rama existente (asegurándose de que no está en ella) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Protección relacionada (se realiza como un grupo, en orden)

| Comando | Descripción |
|---------|-------------|
| `git add --all` | Después de guardar el trabajo, agréguelo a una rama |
| `git commit -m “public comment, including quotes”` | Confirmar los cambios en la rama |
| `git pull upstream master` | Actualización de la copia local del maestro desde el repositorio de producción |
| `git push origin <name-of-branch>` | Inserte la versión remota de la rama local |