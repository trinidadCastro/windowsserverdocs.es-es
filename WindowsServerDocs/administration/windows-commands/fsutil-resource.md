---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: Fsutil (recurso)
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2013173978838764b2ff83a8b7cbc35adc485264
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844098"
---
# <a name="fsutil-resource"></a>Fsutil (recurso)
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Crea un Administrador de recursos transaccional secundario, inicia o detiene una Administrador de recursos transaccional, o muestra información acerca de una Administrador de recursos transaccional y modifica el comportamiento siguiente:

-   Si un Administrador de recursos transaccional predeterminado limpiará sus metadatos transaccionales en el siguiente montaje

-   El Administrador de recursos transaccional especificado para preferir la coherencia sobre la disponibilidad

-   La transacción especificada Administrador de recursos para preferir la disponibilidad a través de la coherencia

-   Características de una Administrador de recursos transaccional en ejecución

Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples) .

## <a name="syntax"></a>Sintaxis

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>
```

#### <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                                                                                                                                                                                        Descripción                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         crear          |                                                                                                                                                                                                                    Crea un Administrador de recursos transaccional secundario.                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        Especifica la ruta de acceso completa a un directorio raíz de Administrador de recursos transaccional.                                                                                                                                                                                                         |
|          info           |                                                                                                                                                                                                            Muestra la información del Administrador de recursos transaccional especificado.                                                                                                                                                                                                            |
|      setautoreset       | Especifica si un Administrador de recursos transaccional predeterminado limpiará los metadatos transaccionales en el siguiente montaje.<p>-Establezca el parámetro **setautoreset** en **true** para especificar que la transacción administrador de recursos Limpie los metadatos transaccionales en el siguiente montaje, de forma predeterminada.<br />-Establezca el parámetro **setautoreset** en **false** para especificar que la transacción administrador de recursos no limpiará los metadatos transaccionales en el siguiente montaje, de forma predeterminada. |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       Especifica el nombre de la unidad seguido de dos puntos.                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 Especifica que un Administrador de recursos transaccional preferirá la disponibilidad a través de la coherencia.                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 Especifica que un Administrador de recursos transaccional preferirá la coherencia con respecto a la disponibilidad.                                                                                                                                                                                                 |
|         Setlog          |                                                                                                                                                                                                  Cambia las características de un Administrador de recursos transaccional que ya se está ejecutando.                                                                                                                                                                                                  |
|         crezca          |                                                                                                  Especifica la cantidad por la que puede aumentar el registro de Administrador de recursos transaccional.<p>El parámetro Growth se puede especificar de la manera siguiente:<p>-Número**de contenedores con** el _formato: contenedores containers_<br />-Porcentaje con el _formato: porcentaje%_ **percent**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      Especifica los objetos de datos utilizados por el Administrador de recursos transaccional.                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                Especifica el número máximo de contenedores para el Administrador de recursos transaccional especificado.                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                Especifica el número mínimo de contenedores para el Administrador de recursos transaccional especificado.                                                                                                                                                                                                |
|  modo {full&#124;Undo}  |                                                                                                                                                                                        Especifica si todas las transacciones están registradas ( **completo**) o solo se registran los eventos revertidos (**Deshacer**).                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  Cambia el GUID del Administrador de recursos transaccional.                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              Especifica el porcentaje por el que se puede reducir automáticamente el registro de Administrador de recursos transaccionales.                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              Especifica el tamaño de la Administrador de recursos transaccional como un número especificado de *contenedores*.                                                                                                                                                                                               |
|          inicio          |                                                                                                                                                                                                                    Inicia el Administrador de recursos transaccional especificado.                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    Detiene el Administrador de recursos transaccional especificado.                                                                                                                                                                                                                     |

### <a name="examples"></a><a name="BKMK_examples"></a>Example
Para establecer el registro del Administrador de recursos transaccional que se especifica en c:\test, para tener un crecimiento automático de cinco contenedores, escriba:

```
fsutil resource setlog growth 5 containers c:test
```

Para establecer el registro del Administrador de recursos transaccional que se especifica en c:\test, para que tenga un crecimiento automático de dos por ciento, escriba:

```
fsutil resource setlog growth 2 percent c:test
```

Para especificar que el Administrador de recursos transaccional predeterminado Limpie los metadatos transaccionales en el siguiente montaje de la unidad C, escriba:

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[NTFS transaccional](https://go.microsoft.com/fwlink/?LinkID=165402)


