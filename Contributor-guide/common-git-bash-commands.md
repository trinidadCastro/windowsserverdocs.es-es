---
title: Comandos comunes de Git Bash para su uso con GitHub
description: Una lista de algunos de los comandos usados con más frecuencia en Git Bash al trabajar con GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 210acaf2b7911892bcfd81b6bbe1975f141308a1
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461688"
---
# <a name="common-git-bash-commands"></a>Comandos comunes de Git Bash

Estos son algunos de los comandos más usados en Git Bash, en función de cuándo se va a utilizar en la creación de contenido y el proceso de edición.

## <a name="master-branch-related"></a>Patrón relacionadas con la rama

Siempre debe utilizar a master como base para cualquier nueva rama.

| Comando | Descripción |
|---------|-------------|
| `git checkout master` | Cambiar al patrón de cualquier otra rama |
| `git pull upstream master` | Actualice su copia local del maestro desde el repositorio de producción |

## <a name="branch-related"></a>Relacionadas con la rama

| Comando | Descripción |
|---------|-------------|
| `git branch` | Ver las ramas existentes |
| `git checkout -B <name-of-branch>` | Crear una nueva rama |
| `git checkout <name-of-branch>` | Cambiar a otra bifurcación |
| `git status` | Compruebe lo que está ocurriendo en su rama |
| `git branch -D <name-of-branch>` | Eliminar una rama existente (asegurándose de que no está en ella) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Verificación en relacionadas (realiza como un grupo, en orden)

| Comando | Descripción |
|---------|-------------|
| `git add --all` | Después de guardar su trabajo, puede agregarlo a una rama |
| `git commit -m “public comment, including quotes”` | Confirme los cambios en la rama |
| `git pull upstream master` | Actualice su copia local del maestro desde el repositorio de producción |
| `git push origin <name-of-branch>` | Insertar en la versión remota de la rama local |