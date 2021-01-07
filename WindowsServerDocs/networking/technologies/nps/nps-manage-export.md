---
title: Exportar una configuración de NPS para importar en otro servidor
description: Puede usar este tema para obtener información sobre cómo exportar una configuración del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: edefeec01e7091c6cb97c1303e2fd978a453e93d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950401"
---
# <a name="export-an-nps-configuration-for-import-on-another-server"></a>Exportar una configuración de NPS para importar en otro servidor

Se aplica a: Windows Server 2016

Puede exportar toda la configuración de NPS, incluidos los clientes y servidores RADIUS, la Directiva de red, la Directiva de solicitud de conexión, el registro y la configuración de registro, desde un NPS para la importación en otro NPS.

Use una de las herramientas siguientes para exportar la configuración de NPS:

- En Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, puede usar netsh, o puede usar Windows PowerShell.
- En Windows Server 2008 R2 y Windows Server 2008, use netsh.

> [!IMPORTANT]
> No use este procedimiento si la base de datos NPS de origen tiene un número de versión mayor que el número de versión de la base de datos NPS de destino. Puede ver el número de versión de la base de datos de NPS en la pantalla del comando **netsh NPS show config** .

Dado que las configuraciones de NPS no están cifradas en el archivo XML exportado, enviarlo a través de una red puede suponer un riesgo para la seguridad, por lo que debe tomar precauciones al mover el archivo XML desde el servidor de origen a los servidores de destino. Por ejemplo, agregue el archivo a un archivo de almacenamiento cifrado protegido por contraseña antes de mover el archivo. Además, almacene el archivo en una ubicación segura para evitar que usuarios malintencionados accedan a él.

> [!NOTE]
> Si SQL Server registro está configurado en el NPS de origen, SQL Server configuración de registro no se exporta al archivo XML. Después de importar el archivo en otro NPS, debe configurar manualmente el registro de SQL Server.

## <a name="export-and-import-the-nps-configuration-by-using-windows-powershell"></a>Exportar e importar la configuración de NPS mediante Windows PowerShell

En Windows Server 2012 y versiones posteriores del sistema operativo, puede exportar la configuración de NPS mediante Windows PowerShell.

La sintaxis de comando para exportar la configuración de NPS es la siguiente.

```powershell
Export-NpsConfiguration -Path <filename>
```

En la tabla siguiente se enumeran los parámetros del cmdlet **Export-NpsConfiguration** en Windows PowerShell. Los parámetros en negrita son obligatorios.

|Parámetro|Descripción|
|---------|-----------|
|Path|Especifica el nombre y la ubicación del archivo XML al que desea exportar la configuración de NPS.|

**Credenciales administrativas**

Para completar este procedimiento, debe pertenecer al grupo Administradores.

### <a name="export-example"></a>Ejemplo de exportación

En el ejemplo siguiente, la configuración de NPS se exporta a un archivo XML ubicado en la unidad local. Para ejecutar este comando, ejecute Windows PowerShell como administrador en el NPS de origen, escriba el siguiente comando y presione Entrar.

```powershell
Export-NpsConfiguration –Path c:\config.xml
```

Para obtener más información, vea [Export-NpsConfiguration](/powershell/module/nps/export-npsconfiguration).

Después de exportar la configuración de NPS, copie el archivo XML en el servidor de destino.

La sintaxis de comando para importar la configuración de NPS en el servidor de destino es la siguiente.

```powershell
Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]
```

### <a name="import-example"></a>Ejemplo de importación

El comando siguiente importa la configuración del archivo denominado C:\Npsconfig.xml a NPS. Para ejecutar este comando, ejecute Windows PowerShell como administrador en el NPS de destino, escriba el siguiente comando y presione Entrar.

```powershell
Import-NpsConfiguration -Path "C:\Npsconfig.xml"
```

Para obtener más información, consulte [Import-NpsConfiguration](/powershell/module/nps/import-npsconfiguration).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar la configuración de NPS mediante netsh

Puede usar el shell de red (netsh) para exportar la configuración de NPS mediante el comando **netsh NPS Export** .

Cuando se ejecuta el comando **netsh NPS Import** , NPS se actualiza automáticamente con las opciones de configuración actualizadas. No es necesario detener NPS en el equipo de destino para ejecutar el comando **netsh NPS Import** ; sin embargo, si la consola NPS o el complemento MMC NPS están abiertos durante la importación de la configuración, los cambios en la configuración del servidor no estarán visibles hasta que se actualice la vista.

> [!NOTE]
> Cuando se usa el comando **netsh NPS Export** , es necesario proporcionar el parámetro de comando **exportPSK** con el valor **yes**. Este parámetro y valor indican explícitamente que entiende que está exportando la configuración de NPS y que el archivo XML exportado contiene secretos compartidos sin cifrar para clientes RADIUS y miembros de grupos de servidores RADIUS remotos.

**Credenciales administrativas**

Para completar este procedimiento, debe pertenecer al grupo Administradores.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Para copiar una configuración de NPS en otro NPS mediante comandos Netsh

1. En el NPS de origen, abra el **símbolo del sistema**, escriba **netsh** y, a continuación, presione Entrar.

2. En el símbolo del sistema de **netsh** , escriba **NPS** y, a continuación, presione Entrar.

3. En el símbolo del sistema **netsh NPS** , escriba **Export filename =**"*path\file.xml*" **exportPSK = Yes**, donde *path* es la ubicación de la carpeta donde desea guardar el archivo de configuración de NPS y *File* es el nombre del archivo XML que desea guardar. Presione ENTRAR.

    Esto almacena los valores de configuración (incluida la configuración del registro) en un archivo XML. La ruta de acceso puede ser relativa o absoluta, o puede ser una ruta de acceso UNC (Convención de nomenclatura universal). Después de presionar entrar, aparece un mensaje que indica si la exportación al archivo se realizó correctamente.

4. Copie el archivo que creó en el NPS de destino.

5. En un símbolo del sistema en el NPS de destino, escriba **netsh NPS Import FILENAME =**"*path\file.xml*" y, a continuación, presione Entrar. Aparece un mensaje que indica si la importación del archivo XML se ha realizado correctamente.

## <a name="additional-references"></a>Referencias adicionales

- [Shell de red (Netsh)](../netsh/netsh.md)
