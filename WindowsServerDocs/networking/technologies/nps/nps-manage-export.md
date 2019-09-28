---
title: Exportar una configuración de NPS para importar en otro servidor
description: Puede usar este tema para obtener información sobre cómo exportar una configuración del servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d268dc57-78f8-47ba-9a7a-a607e8b9225c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cbebd0388ccd5dd2540a20f5d325d7f97c7e2bb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405433"
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

    Export-NpsConfiguration -Path <filename>

En la tabla siguiente se enumeran los parámetros del cmdlet **Export-NpsConfiguration** en Windows PowerShell. Los parámetros en negrita son obligatorios.

|Parámetro|Descripción|
|---------|-----------|
|Path|Especifica el nombre y la ubicación del archivo XML al que desea exportar la configuración de NPS.|

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo administradores.

### <a name="export-example"></a>Ejemplo de exportación 

En el ejemplo siguiente, la configuración de NPS se exporta a un archivo XML ubicado en la unidad local. Para ejecutar este comando, ejecute Windows PowerShell como administrador en el NPS de origen, escriba el siguiente comando y presione Entrar.

`Export-NpsConfiguration –Path c:\config.xml` 

Para obtener más información, vea [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx).

Después de exportar la configuración de NPS, copie el archivo XML en el servidor de destino.

La sintaxis de comando para importar la configuración de NPS en el servidor de destino es la siguiente.

    Import-NpsConfiguration [-Path] <String> [ <CommonParameters>]

### <a name="import-example"></a>Ejemplo de importación

El siguiente comando importa la configuración del archivo denominado C:\Npsconfig.xml a NPS. Para ejecutar este comando, ejecute Windows PowerShell como administrador en el NPS de destino, escriba el siguiente comando y presione Entrar.

    PS C:\> Import-NpsConfiguration -Path "C:\Npsconfig.xml"

Para obtener más información, consulte [Import-NpsConfiguration](https://technet.microsoft.com/library/jj872750.aspx).

## <a name="export-and-import-the-nps-configuration-by-using-netsh"></a>Exportar e importar la configuración de NPS mediante netsh

Puede usar el shell de red \(Netsh @ no__t-1 para exportar la configuración de NPS mediante el comando **netsh NPS Export** .

Cuando se ejecuta el comando **netsh NPS Import** , NPS se actualiza automáticamente con las opciones de configuración actualizadas. No es necesario detener NPS en el equipo de destino para ejecutar el comando **netsh NPS Import** ; sin embargo, si la consola NPS o el complemento MMC NPS están abiertos durante la importación de la configuración, los cambios en la configuración del servidor no estarán visibles hasta que se actualice la vista. 

> [!NOTE]
> Cuando se usa el comando **netsh NPS Export** , es necesario proporcionar el parámetro de comando **exportPSK** con el valor **yes**. Este parámetro y valor indican explícitamente que entiende que está exportando la configuración de NPS y que el archivo XML exportado contiene secretos compartidos sin cifrar para clientes RADIUS y miembros de grupos de servidores RADIUS remotos.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo administradores.

### <a name="to-copy-an-nps-configuration-to-another-nps-using-netsh-commands"></a>Para copiar una configuración de NPS en otro NPS mediante comandos Netsh

1. En el NPS de origen, abra el **símbolo del sistema**, escriba **netsh**y, a continuación, presione Entrar.

2. En el símbolo del sistema de **netsh** , escriba **NPS**y, a continuación, presione Entrar. 

3. En el símbolo del sistema **netsh NPS** , escriba **Export FILENAME =** "*path\file.XML*" **exportPSK = Yes**, donde *path* es la ubicación de la carpeta donde desea guardar el archivo de configuración de NPS y *File* es el nombre del archivo XML que que desea guardar. Presiona Entrar. 

Almacena los valores de configuración \(including configuración del registro @ no__t-1 en un archivo XML. La ruta de acceso puede ser relativa o absoluta, o puede ser una Convención de nomenclatura universal \(UNC @ no__t-1. Después de presionar entrar, aparece un mensaje que indica si la exportación al archivo se realizó correctamente.

4. Copie el archivo que creó en el NPS de destino.

5. En un símbolo del sistema en el NPS de destino, escriba **netsh NPS Import FILENAME =** "*path\file.XML*" y, a continuación, presione Entrar. Aparece un mensaje que indica si la importación del archivo XML se ha realizado correctamente.

Para obtener más información acerca de Netsh, vea [Shell de red (netsh)](../netsh/netsh.md).

