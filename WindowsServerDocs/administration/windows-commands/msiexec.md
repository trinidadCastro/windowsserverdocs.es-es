---
title: msiexec
description: Artículo de referencia para el comando msiexec, que proporciona los medios para instalar, modificar y realizar operaciones en Windows Installer desde la línea de comandos.
ms.topic: reference
ms.assetid: 122eb0ce-ecbc-4909-a52a-15c3413619af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1255cf26ac4dd3f9c28189ce7df76d63c875ee64
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025289"
---
# <a name="msiexec"></a>msiexec

Proporciona los medios para instalar, modificar y realizar operaciones en Windows Installer desde la línea de comandos.

## <a name="install-options"></a>Opciones de instalación

Establezca el tipo de instalación para iniciar un paquete de instalación.

### <a name="syntax"></a>Sintaxis

```
msiexec.exe [/i][/a][/j{u|m|/g|/t}][/x] <path_to_package>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /i | Especifica la instalación normal. |
| /a | Especifica la instalación administrativa. |
| /Ju | Anunciar el producto al usuario actual. |
| /JM | Anuncie el producto a todos los usuarios. |
| /j/g | Especifica el identificador de idioma utilizado por el paquete anunciado. |
| /j/t | Aplica la transformación al paquete anunciado. |
| /x | Desinstala el paquete. |
| `<path_to_package>` | Especifica la ubicación y el nombre del archivo de paquete de instalación. |

#### <a name="examples"></a>Ejemplos

Para instalar un paquete denominado *example.msi* desde la unidad C:, mediante un proceso de instalación normal, escriba:

```
msiexec.exe /i "C:\example.msi"
```

## <a name="display-options"></a>Opciones de presentación

Puede configurar lo que ve un usuario durante el proceso de instalación, en función de su entorno de destino. Por ejemplo, si va a distribuir un paquete a todos los clientes para la instalación manual, debe haber una interfaz de usuario completa. Sin embargo, si va a implementar un paquete mediante directiva de grupo, que no requiere interacción del usuario, no debería haber ninguna interfaz de usuario implicada.

### <a name="syntax"></a>Sintaxis

```
msiexec.exe /i <path_to_package> [/quiet][/passive][/q{n|b|r|f}]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| `<path_to_package>` | Especifica la ubicación y el nombre del archivo de paquete de instalación. |
| /quiet | Especifica el modo silencioso, lo que significa que no se requiere ninguna interacción del usuario. |
| /passive | Especifica el modo desatendido, lo que significa que la instalación solo muestra una barra de progreso. |
| /qn | Especifica que no hay ninguna interfaz de usuario durante el proceso de instalación. |
| /QN + | Especifica que no hay ninguna interfaz de usuario durante el proceso de instalación, excepto para un cuadro de diálogo final al final. |
| /qb | Especifica que hay una interfaz de usuario básica durante el proceso de instalación. |
| /QB + | Especifica que hay una interfaz de usuario básica durante el proceso de instalación, incluido un último cuadro de diálogo al final. |
| /QR | Especifica una experiencia de interfaz de usuario reducida durante el proceso de instalación. |
| /qf | Especifica una experiencia de interfaz de usuario completa durante el proceso de instalación. |

##### <a name="remarks"></a>Observaciones

- No se muestra el cuadro modal si el usuario cancela la instalación. Puede usar **qb +!** o **qb! +** para ocultar el botón **Cancelar** .

#### <a name="examples"></a>Ejemplos

Para instalar el paquete *C:\example.msi*, mediante un proceso de instalación normal y sin interfaz de usuario, escriba:

```
msiexec.exe /i "C:\example.msi" /qn
```

## <a name="restart-options"></a>Opciones de reinicio

Si el paquete de instalación sobrescribe archivos o intenta cambiar los archivos que están en uso, es posible que sea necesario reiniciar el equipo antes de que se complete la instalación.

### <a name="syntax"></a>Sintaxis

```
msiexec.exe /i <path_to_package> [/norestart][/promptrestart][/forcerestart]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| `<path_to_package>` | Especifica la ubicación y el nombre del archivo de paquete de instalación. |
| /norestart | Detiene el reinicio del dispositivo una vez finalizada la instalación. |
| /promptrestart | Pregunta al usuario si se requiere un reinicio. |
| /forcerestart | Reinicia el dispositivo una vez finalizada la instalación. |

#### <a name="examples"></a>Ejemplos

Para instalar el paquete *C:\example.msi*, mediante un proceso de instalación normal sin reiniciar al final, escriba:

```
msiexec.exe /i "C:\example.msi" /norestart
```

## <a name="logging-options"></a>Opciones de registro

Si necesita depurar el paquete de instalación, puede establecer los parámetros para crear un archivo de registro con información específica.

### <a name="syntax"></a>Sintaxis

```
msiexec.exe [/i][/x] <path_to_package> [/L{i|w|e|a|r|u|c|m|o|p|v|x+|!|*}] <path_to_log>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /i | Especifica la instalación normal. |
| /x | Desinstala el paquete. |
| `<path_to_package>` | Especifica la ubicación y el nombre del archivo de paquete de instalación. |
| /Li | Activa el registro e incluye los mensajes de estado en el archivo de registro de salida. |
| /lw | Activa el registro e incluye advertencias no graves en el archivo de registro de salida. |
| /le | Activa el registro e incluye todos los mensajes de error en el archivo de registro de salida. |
| /la | Activa el registro e incluye información sobre cuándo se inicia una acción en el archivo de registro de salida. |
| /lr | Activa el registro e incluye registros específicos de la acción en el archivo de registro de salida. |
| /lu | Activa el registro e incluye la información de la solicitud de usuario en el archivo de registro de salida. |
| /lc | Activa el registro e incluye los parámetros iniciales de la interfaz de usuario en el archivo de registro de salida. |
| /LM | Activa el registro e incluye información de salida irrecuperable o de memoria insuficiente en el archivo de registro de salida. |
| /lo | Activa el registro e incluye mensajes fuera de espacio en disco en el archivo de registro de salida. |
| /lp | Activa el registro e incluye las propiedades de terminal en el archivo de registro de salida. |
| /lp | Activa el registro e incluye las propiedades de terminal en el archivo de registro de salida. |
| /LV | Activa el registro e incluye la salida detallada en el archivo de registro de salida. |
| /lp | Activa el registro e incluye las propiedades de terminal en el archivo de registro de salida. |
| /lx | Activa el registro e incluye información de depuración adicional en el archivo de registro de salida. |
| /l + | Activa el registro y anexa la información a un archivo de registro existente. |
| l! | Activa el registro y vacía cada línea en el archivo de registro. |
| l | Activa el registro y registra toda la información, excepto información detallada (**/LV**) o información de depuración adicional (**/LX**). |
| `<path_to_logfile>` | Especifica la ubicación y el nombre del archivo de registro de salida. |

#### <a name="examples"></a>Ejemplos

Para instalar el paquete *C:\example.msi*, mediante un proceso de instalación normal con toda la información de registro proporcionada, incluida la salida detallada, y el almacenamiento del archivo de registro de salida en *C:\package.log*, escriba:

```
msiexec.exe /i "C:\example.msi" /L*V "C:\package.log"
```

## <a name="update-options"></a>Opciones de actualización

Puede aplicar o quitar actualizaciones mediante un paquete de instalación.

### <a name="syntax"></a>Sintaxis

```
msiexec.exe [/p][/update][/uninstall[/package<product_code_of_package>]] <path_to_package>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /p | Instala una revisión. Si va a instalar silenciosamente, también debe establecer la propiedad REINSTALLMODE en *ecmus* y volver a instalarlo en *todos*. De lo contrario, la revisión solo actualiza el MSI almacenado en caché en el dispositivo de destino. |
| /update | Opción instalar revisiones. Si va a aplicar varias actualizaciones, debe separarlas con un punto y coma (;). |
| /package | Instala o configura un producto. |

#### <a name="examples"></a>Ejemplos

```
msiexec.exe /p "C:\MyPatch.msp"
msiexec.exe /p "C:\MyPatch.msp" /qb REINSTALLMODE="ecmus" REINSTALL="ALL"
msiexec.exe /update "C:\MyPatch.msp"
```

```
msiexec.exe /uninstall {1BCBF52C-CD1B-454D-AEF7-852F73967318} /package {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

Donde el primer GUID es el GUID de la revisión y el segundo es el código de producto MSI al que se aplicó la revisión.

## <a name="repair-options"></a>Opciones de reparación

Puede usar este comando para reparar un paquete instalado.

### <a name="syntax"></a>Sintaxis

```
msiexec.exe [/f{p|o|e|d|c|a|u|m|s|v}] <product_code>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| /fp | Repara el paquete si falta un archivo. |
| /fo | Repara el paquete si falta un archivo o si se ha instalado una versión anterior. |
| /fe | Repara el paquete si falta el archivo o si está instalada una versión igual o anterior. |
| /FD | Repara el paquete si falta el archivo o si se instala una versión diferente. |
| /fc | Repara el paquete si falta el archivo o si la suma de comprobación no coincide con el valor calculado. |
| /FA | Fuerza la reinstalación de todos los archivos. |
| /Fu | Repara todas las entradas del registro específicas del usuario necesarias. |
| /FM | Repara todas las entradas del registro específicas del equipo necesarias. |
| /FS | Repara todos los accesos directos existentes. |
| /fc | Se ejecuta desde el origen y vuelve a almacenar en caché el paquete local. |

#### <a name="examples"></a>Ejemplos

Para forzar la reinstalación de todos los archivos en función del código de producto MSI que se va a reparar, *{AAD3D77A-7476-469F-ADF4-04424124E91D}*, escriba:

```
msiexec.exe /fa {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

## <a name="set-public-properties"></a>Establecer propiedades públicas

Puede establecer propiedades públicas con este comando. Para obtener información sobre las propiedades disponibles y cómo establecerlas, consulte [propiedades públicas](/windows/win32/msi/public-properties).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Msiexec.exe opciones de la línea de comandos](/windows/win32/msi/command-line-options)

- [Opciones de la línea de comandos del instalador estándar](/windows/win32/msi/standard-installer-command-line-options)
