---
title: Agregar una pestaña a Configuración
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d974f4dc53b9ce389254b162a3305b277181ceed
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181541"
---
# <a name="add-a-tab-to-settings"></a>Agregar una pestaña a Configuración

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede agregar una ficha a Configuración en el Panel; para ello, cree e instale un ensamblado de código usado por el Administrador de configuración en el sistema operativo.

## <a name="add-a-tab-to-settings"></a>Agregar una ficha a Configuración
 Complete las tareas siguientes para agregar una ficha a Configuración:

-   [Agregue una implementación de la interfaz de ISettingsData al ensamblado](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).

-   [Firme el ensamblado con una firma Authenticode](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).

-   [Instale el ensamblado en el equipo de referencia](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).

###  <a name="add-an-implementation-of-the-isettingsdata-interface-to-the-assembly"></a><a name="BKMK_ISettingsData"></a>Agregar una implementación de la interfaz ISettingsData al ensamblado
 La interfaz de ISettingsData está incluida en el espacio de nombres de Microsoft.WindowsServerSolutions.Settings del ensamblado AdminCommon.dll que se encuentra en \Archivos de programa\Windows Server\Bin.

##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Para agregar el código ISettingsData al ensamblado

1.  Abra Visual Studio 2010 como administrador; para ello, haga clic con el botón secundario en el menú **Inicio** y seleccione **Ejecutar como administrador**.

2.  Haga clic en **Archivo**, **Nuevo** y a continuación haga clic en **Proyecto**.

3.  En el cuadro de diálogo **Nuevo proyecto**, haga clic en **Visual C#**, **Biblioteca de clases**, escriba **DashboardSettingsPage** como el nombre de la solución y a continuación haga clic en **Aceptar**.

    > [!IMPORTANT]
    >  El conjunto que se instala en el servidor debe nombrarse DashboardSettingsPage.dll y, a continuación, copiar la dll a %ProgramFiles%\Windows Server\Bin\OEM.

4.  Cree el control que desee utilizar en la pestaña. En este ejemplo, el control de configuración se denomina MySettingsControl.

5.  Cambie el nombre del archivo Class1.cs. Por ejemplo, MySettingTab.cs.

6.  Agregue una referencia al archivo AdminCommon.dll.

7.  Agregue la siguiente instrucción using:

    ```c#
    using Microsoft.WindowsServerSolutions.Settings;
    ```

8.  Cambie el espacio de nombres y el encabezado de la clase para que coincida con el ejemplo siguiente:

    ```

    namespace DashboardSettingsPage
    {
        public class MySettingTab : ISettingsData
        {
        }
    }

    ```

9. Cree una instancia del control que ha creado para la pestaña. Por ejemplo:

    ```c#
    private MySettingsControl tab;
    ```

10. Agregue el constructor de la clase. En el siguiente ejemplo de código se muestra el constructor:

    ```

    public MySettingTab()
    {
       tab = new MySettingsControl();
    }
    ```

11. Agregue el método Commit, que envía los cambios en la configuración. En el siguiente ejemplo de código se muestra el método Commit:

    ```

    void ISettingsData.Commit(bool dismissed)
    {
       // Implement the code that is required to submit your setting changes
    }
    ```

12. Agregue el método TabControl, que identifica el control de la pestaña. En el ejemplo de código siguiente se muestra el método TabControl:

    ```

    System.Windows.Forms.Control ISettingsData.TabControl
    {
       get { return tab; }
    }
    ```

13. Agregue el método TabId, que proporciona un identificador único para la pestaña. En el ejemplo de código siguiente se muestra el método TabId:

    ```

    private Guid id = Guid.NewGuid();

    Guid ISettingsData.TabId
    {
       get { return id; }
    }
    ```

14. Agregue el método TabOrder, que devuelve el orden de la pestaña. En el ejemplo de código siguiente se muestra el método TabOrder:

    ```

    int ISettingsData.TabOrder
    {
       get { return 0; }
    }
    ```

    > [!NOTE]
    >  El orden de las fichas se define utilizando números a partir de 0. Las pestañas integradas de configuración de Microsoft se muestran primero y a continuación las pestañas del usuario en función del orden establecido. Por ejemplo, si dispone de tres pestañas de configuración, puede especificar el orden como 0, 1 y 2, dependiendo del orden en que desee que aparezcan.

15. Agregue el método TabTitle, que proporciona el título de la pestaña. En el ejemplo de código siguiente se muestra el método TabTitle:

    ```

    string ISettingsData.TabTitle
    {
      get { return "My Settings Tab"; }
    }
    ```

    > [!NOTE]
    >  El texto del título también puede proceder de un archivo de recursos que se adapte a sus necesidades de localización.

16. Guarde y genere la solución.

###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Firmar el ensamblado con una firma Authenticode
 Para poder utilizar el ensamblado en el sistema operativo es necesario firmarlo mediante Authenticode. Para obtener más información acerca de cómo firmar el ensamblado, consulte [Signing and Checking Code with Authenticode (Firma y comprobación de código con Authenticode)](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).

###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>Instalar el ensamblado en el equipo de referencia
 Después de crear correctamente la solución, coloque una copia del archivo DashboardSettingsPage.dll en la siguiente carpeta del equipo de referencia:

 **%Programfiles%\Windows Server\Bin\OEM**

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)