---
title: Novedades en la consola de Windows en Windows Server 2016
description: Se enumeran las nuevas características importantes de la consola de Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: server-general
ms.reviewer: na
ms.suite: na
ms.date: 10/04/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9fc582-033b-4973-84e7-0c6024ecfcbc
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 2f05bcffa7c8c4f9e74f3699b9838b8a627af1b1
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63688672"
---
# <a name="whats-new-in-the-windows-console-in-windows-server-2016"></a>Novedades en la consola de Windows en Windows Server 2016
>Se aplica a: Windows Server 2016

El host de consola (el código subyacente que brinda soporte a todas las aplicaciones de modo de carácter, incluido el símbolo del sistema de Windows, el símbolo del sistema de Windows PowerShell y otros) se ha actualizado de varias maneras para agregar una gama de nuevas funcionalidades.  

## <a name="controlling-the-new-features"></a>Control de las nuevas características  
La nueva funcionalidad está habilitada de forma predeterminada, pero puede activar y desactivar cada una de las nuevas características, o revertir al host de consola anterior a través de la interfaz de propiedades (principalmente en la pestaña **Opciones**) o con estas claves del Registro (todas las claves son valores DWORD en **HKEY_CURRENT_USER\Console**):  

|Clave del Registro|Descripción|  
|----------------|---------------|  
|ForceV2|1 habilita todas las características nuevas de la consola; 0 deshabilita todas las características nuevas. Nota: Este valor no se almacena en los accesos directos, sino solo en esta clave del Registro.|  
|LineSelection|1 habilita la selección de línea; 0 para usar solo el modo de bloque.|  
|FilterOnPaste|1 habilita el nuevo comportamiento de pegado.|  
|LineWrap|1 ajusta el texto cuando cambia el tamaño de las ventanas en la consola.|  
|CtrlKeyShortcutsDisabled|0 habilita los nuevos accesos directos; 1 los deshabilita.|  
|Claves de ExtendedEdit|1 habilita el conjunto completo de teclas de selección de teclado; 0 lo deshabilita.|  
|TrimLeadingZeros|1 recorta los ceros iniciales en las selecciones realizadas haciendo doble clic; 0 mantiene los ceros iniciales.|  
|WindowsAlpha|Establece el valor de opacidad entre 30 % y 100 %. Especifique un valor entre 0x4C y 0xFF o entre 76 y 255.|  
|WordDelimiters|Define el carácter que se utiliza para delimitar al seleccionar una palabra completa de texto a la vez con CTRL+MAYÚS+FLECHA (el valor predeterminado es el carácter de espacio). Establezca este valor REG_SZ para contener todos los caracteres que desea que se traten como delimitadores. Nota: Este valor no se almacena en los accesos directos, sino solo en esta clave del Registro.|  

Estos valores se almacenan por cada título de la ventana en el Registro bajo HKCU\Console. Las ventanas de la consola que se abren con un acceso directo tienen estos valores almacenados en el acceso directo; si el acceso directo se copia a otro equipo, los ajustes se trasladan con él al nuevo equipo. La configuración de accesos directos invalida todas las demás opciones, incluida la configuración global y los valores predeterminados. Sin embargo, si revierte a la consola original por medio de **Use legacy console** (Usar consola heredada) en la pestaña **Opciones**, esta configuración es global y se mantiene para todas las ventanas después, incluso después de reiniciar el equipo.  

Puede configurar previamente estos valores o aplicarles un script mediante la configuración correspondiente del Registro en un archivo de instalación desatendida o con Windows PowerShell.  

Las aplicaciones NTVDM de 16 bits siempre revierten al host de consola anterior.  

> [!NOTE]  
> Si encuentra problemas con la nueva configuración de la consola y no los puede resolver con ninguna de las opciones específicas que se muestran aquí, siempre puede revertir a la consola original estableciendo ForceV2 en 0 o con el control **Use legacy console** (Usar consola heredada) en **Opciones**.  

## <a name="console-behavior"></a>Comportamiento de la consola  
Ahora puede cambiar el tamaño de la ventana de la consola a voluntad con solo arrastrar un borde con el mouse. Las barras de desplazamiento aparecen solo si establece las dimensiones de la ventana manualmente (mediante el uso de la pestaña **Diseño** en **Propiedades**) o si la línea más larga de texto en el búfer es mayor que el tamaño de la ventana actual.  

La nueva ventana de la consola ahora es compatible con el ajuste automático de línea. Sin embargo, si utiliza las API de consola para cambiar el texto de un búfer, la consola deja el texto tal y como se insertó originalmente.  

Las ventanas de consola ahora pueden ser semitransparentes (el valor de transparencia mínima es del 30 %). Puede ajustar la transparencia en el menú de propiedades o con estos comandos de teclado:  

|Para ello:|Utilice esta combinación de teclas:|  
|---------------|-----------------------------|  
|Aumentar la transparencia|CTRL+MAYÚS+Más (+) o CTRL+MAYÚS+desplazamiento hacia arriba del mouse|  
|Disminuir la transparencia|CTRL+MAYÚS+Menos (-) o CTRL+MAYÚS+desplazamiento hacia abajo del mouse|  
|Cambiar al modo de pantalla completa|ALT+ENTRAR|  

## <a name="selection"></a>Selección  
Hay muchas opciones nuevas para la selección de texto y líneas, así como para marcar el texto y utilizar el historial del búfer. La consola intenta evitar conflictos con las aplicaciones que puedan estar usando las mismas teclas.  

**Para desarrolladores:** si se produce un conflicto, normalmente puedes controlar el comportamiento del uso de entrada de línea, la entrada procesada y los modos de entrada de eco de la aplicación con la API etConsoleMode(). Si se ejecuta en modo de entrada procesada, se aplican los siguientes métodos abreviados; sin embargo, en otros modos, la aplicación los debe controlar. Las combinaciones de teclas que no aparecen aquí funcionan igual que en las versiones anteriores de la consola. También puede intentar resolver conflictos con distintas configuraciones en la pestaña **Opciones**. Si todo lo demás provoca error, siempre puede revertir a la consola original.  

Ahora puede usar la selección "hacer clic y arrastrar" fuera del modo de Edición rápida, y así puede seleccionar texto en líneas como en el Bloc de notas, en lugar de obtener simplemente un bloque rectangular. Las operaciones de copia ya no requieren que quite los saltos de línea. Además de la selección de "hacer clic y arrastrar", tiene a su disposición estas combinaciones de teclas:  

**Selección de texto**  

|Para ello:|Utilice esta combinación de teclas:|  
|---------------|-----------------------------|  
|Mover el cursor a la izquierda un carácter, ampliando la selección.|MAYÚS+FLECHA IZQUIERDA|  
|Mover el cursor a la derecha un carácter, ampliando la selección.|ALT+FLECHA DERECHA|  
|Seleccionar texto línea a línea hacia arriba desde el punto de inserción.|MAYÚS+FLECHA ARRIBA|  
|Ampliar la selección de texto una línea hacia abajo desde el punto de inserción.|MAYÚS+FLECHA ABAJO|  
|Si el cursor está en la línea que se está editando actualmente, use este comando una vez para ampliar la selección hasta el último carácter en la línea de entrada. Úselo una segunda vez para ampliar la selección hasta el margen derecho.|MAYÚS+FIN|  
|Si el cursor **no** está en la línea que se está editando actualmente, use este comando para seleccionar todo el texto desde el punto de inserción hasta el margen derecho.|MAYÚS+FIN|  
|Si el cursor en la línea que se está editando actualmente, use este comando una vez para ampliar la selección al carácter inmediatamente después del símbolo del sistema. Úselo una segunda vez para ampliar la selección hasta el margen derecho.|MAYÚS+INICIO|  
|Si el cursor **no** está en la línea que se está editando actualmente, use este comando para ampliar la selección hasta el margen izquierdo.|MAYÚS+INICIO|  
|Ampliar la selección una pantalla hacia abajo.|MAYÚS + AV PÁG|  
|Ampliar la selección una pantalla hacia arriba.|MAYÚS + RE PÁG|  
|Ampliar la selección una palabra a la derecha. (Puede definir los delimitadores para "palabra" con la clave del Registro WordDelimiters).|CTRL+MAYÚS+FLECHA DERECHA|  
|Ampliar la selección una palabra a la izquierda.|CTRL+MAYÚS+INICIO|  
|Ampliar la selección hasta el principio del búfer de pantalla.|CTRL+MAYÚS+FIN|  
|Seleccionar todo el texto después del símbolo, si el cursor está en la línea actual y la línea no está vacía.|CTRL+A|  
|Seleccionar el búfer completo, si el cursor **no** está en la línea actual.|CTRL+A|  

**Edición de texto**  

Puede copiar y pegar texto en la consola mediante comandos de teclado. CTRL+C ahora realiza dos funciones. Si no hay texto seleccionado cuando se usa, envía el comando BREAK como de costumbre. Si hay texto seleccionado, el primer uso copia el texto y elimina la selección; el segundo uso envía BREAK. Aquí hay otros comandos de edición:  

|Para ello:|Utilice esta combinación de teclas:|  
|---------------|-----------------------------|  
|Pegar el texto en la línea de comandos.|CTRL + V.|  
|Copiar el texto seleccionado en el Portapapeles.|CTRL+INS|  
|Copiar el texto seleccionado en el Portapapeles; enviar BREAK.|CTRL+C|  
|Pegar el texto en la línea de comandos.|MAYÚS+INS|  

**Modo de marcado**  

Para entrar en el modo de marcado en cualquier momento, haga clic con el botón derecho en cualquier parte en la barra de título de la consola, seleccione **Editar**, y seleccione **Marcar** en el menú que se abre. También puede utilizar CTRL+M. En el modo de marcado, utilice la tecla ALT para identificar el inicio de una selección de ajuste de línea. (Si está deshabilitada la opción **Habilitar la selección de ajuste de línea** el modo de marcado selecciona texto en un bloque). En el modo de marcado, CTRL+MAYÚS+FLECHA selecciona por carácter y no por palabra como en el modo normal. Además de las teclas de selección de la sección **Modificar texto**, estas combinaciones están disponibles en el modo de marcado:  

|Para ello:|Utilice esta combinación de teclas:|  
|---------------|-----------------------------|  
|Entrar en el modo de marcado para mover el cursor en la ventana.|CTRL+M|  
|Comenzar la selección de ajuste de línea en el modo de marcado, junto con otras combinaciones de teclas.|ALT|  
|Mover el cursor en la dirección especificada.|Teclas de dirección|  
|Mover el cursor una página en la dirección especificada.|Teclas de página|  
|Mover el cursor al principio del búfer.|CTRL+INICIO|  
|Mover el cursor al final del búfer.|CTRL+FIN|  

**Historial de navegación**  

|Para ello:|Utilice esta combinación de teclas:|  
|---------------|-----------------------------|  
|Subir una línea en el historial de salida.|CTRL+FLECHA ARRIBA|  
|Bajar una línea en el historial de salida.|CTRL+FLECHA ABAJO|  
|Mover la ventanilla al principio del búfer (si la línea de comandos está vacía) o eliminar todos los caracteres a la izquierda del cursor (si la línea de comandos no está vacía).|CTRL+INICIO|  
|Mover la ventanilla a la línea de comandos (si la línea de comandos está vacía) o eliminar todos los caracteres a la derecha del cursor (si la línea de comandos no está vacía).|CTRL+FIN|  

**Comandos de teclado adicionales**  

|Para ello:|Utilice esta combinación de teclas:|  
|---------------|-----------------------------|  
|Abrir el cuadro de diálogo Buscar.|CTRL+F|  
|Cerrar la ventana de la consola.|ALT+F4|  
