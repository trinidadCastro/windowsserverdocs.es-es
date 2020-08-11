---
title: Volver a generar el archivo Tokens.dat
description: Cómo volver a generar el archivo Tokens.dat al solucionar problemas de activación de Windows
ms.topic: troubleshooting
ms.date: 09/18/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: bc44dae97422e4d9d9e55b32004f806bbb7860f7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941787"
---
# <a name="rebuild-the-tokensdat-file"></a>Volver a generar el archivo Tokens.dat

Al solucionar problemas de activación de Windows, es posible que tengas que volver a generar el archivo Tokens.dat. En este artículo se describe en detalle cómo hacerlo.

## <a name="resolution"></a>Solución

Para volver a generar el archivo Tokens.dat, sigue estos pasos:

1. Abre una ventana del símbolo del sistema con permisos elevados: **Para Windows 10**

   1. Abre el menú **Inicio** y escribe **cmd**.
   1. En los resultados de la búsqueda, haz clic con el botón derecho en **Símbolo del sistema** y, después, selecciona **Ejecutar como administrador**.

   **Para Windows 8.1**
   1. Desliza el dedo desde el borde derecho de la pantalla y, luego, pulsa **Buscar**. O bien, si estás usando un mouse, señala a la esquina inferior derecha de la pantalla y, luego, selecciona **Buscar**.
   1. En el cuadro de búsqueda, escribe **cmd**.
   1. Desliza el dedo o haz clic con el botón derecho en el icono del **Símbolo del sistema** que aparece.
   1. Pulsa o haz clic en **Ejecutar como administrador**.

   **Para Windows 7**
   1. Abre el menú **Inicio** y escribe **cmd**.
   1. En los resultados de la búsqueda, haz clic con el botón derecho en **cmd.exe** y selecciona **Ejecutar como administrador**.

1. Escribe la lista de comandos que sea adecuada para tu sistema operativo.

   En Windows 10, Windows Server 2016 y versiones posteriores de Windows, escribe los siguientes comandos en secuencia:
   ```cmd
   net stop sppsvc
   cd %windir%\system32\spp\store\2.0
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   En Windows 8.1, Windows Server 2012 y Windows Server 2012 R2, escribe los siguientes comandos en secuencia:
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\LocalService\AppData\Local\Microsoft\WSLicense
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   En Windows 7, Windows Server 2008 y Windows Server 2008 R2, escribe los siguientes comandos en secuencia:
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SoftwareProtectionPlatform
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
1. Reinicie el equipo.

## <a name="more-information"></a>Información adicional

Después de volver a generar el archivo Tokens.dat, debes reinstalar la clave de producto con uno de los métodos siguientes:

- En el mismo símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione Entrar:

   ```cmd
   cscript.exe %windir%\system32\slmgr.vbs /ipk <Product key>
   ```

  > [!IMPORTANT]
  > No uses el modificador **/upk** para desinstalar una clave de producto. Para instalar una clave de producto sobre una clave de producto existente, usa el modificador **/ipk**.
- Haga clic con el botón derecho en **Mi equipo**, selecciona **Propiedades** y, luego, selecciona **Cambiar clave de producto**.

Para obtener más información acerca de las claves de configuración de cliente KMS, consulta [Claves de configuración del cliente KMS](kmsclientkeys.md).
