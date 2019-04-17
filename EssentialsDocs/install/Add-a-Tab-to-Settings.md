---
title: "Agregar una pestaña a configuración"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9eaa1aa5a9c5e8d4c2e36f2000e0adecc83245d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-tab-to-settings"></a>Agregar una pestaña a configuración

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes agregar una pestaña a configuración en el panel por crear e instalar a un ensamblado de código que se usa el Administrador de configuración en el sistema operativo.  
  
## <a name="add-a-tab-to-settings"></a>Agregar una pestaña a configuración  
 Agregar una pestaña a configuración mediante la realización de las siguientes tareas:  
  
-   [Agregar una implementación de la interfaz ISettingsData al ensamblado](Add-a-Tab-to-Settings.md#BKMK_ISettingsData).  
  
-   [Firmar el ensamblado con una firma Authenticode](Add-a-Tab-to-Settings.md#BKMK_SignAssembly).  
  
-   [Instalar el ensamblado en el equipo de referencia](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly).  
  
###  <a name="BKMK_ISettingsData"></a>Agregar una implementación de la interfaz ISettingsData al ensamblado  
 La interfaz ISettingsData se incluye en el espacio de nombres Microsoft.WindowsServerSolutions.Settings del ensamblado AdminCommon.dll, que se encuentra en Files\Windows Server\Bin.  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>Para agregar el código ISettingsData al ensamblado  
  
1.  Abre Visual Studio 2010 como administrador haciendo clic en el programa en el **inicio** menú y seleccionando **ejecutar como administrador**.  
  
2.  Haz clic en **archivo**, haz clic en **nueva**y, a continuación, haz clic en **proyecto**.  
  
3.  En la **nuevo proyecto** cuadro de diálogo, haz clic en **Visual C#**, haz clic en **biblioteca de clases**, escribe **DashboardSettingsPage** para el nombre de la solución y, a continuación, haz clic en **Aceptar**.  
  
    > [!IMPORTANT]
    >  El ensamblado instalado en el servidor debe llamarse DashboardSettingsPage.dll y, a continuación, copia el archivo dll en %ProgramFiles%\Windows Server\Bin\OEM.  
  
4.  Crea el control que quieras que se usará en la pestaña. En este ejemplo, el control de configuración se llama MySettingsControl.  
  
5.  Cambia el nombre del archivo Class1.cs. Por ejemplo, MySettingTab.cs.  
  
6.  Agrega una referencia al archivo AdminCommon.dll.  
  
7.  Agrega la siguiente instrucción using:  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  Cambia el espacio de nombres y el encabezado de clase para que coincida con el siguiente ejemplo:  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. Crea una instancia del control que creaste para la pestaña. Por ejemplo:  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. Agregar el constructor de la clase. El siguiente ejemplo de código muestra el constructor:  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. Agrega el método Commit, que envía los cambios de configuración. El siguiente ejemplo de código muestra el método Commit:  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. Agrega el método TabControl, que identifica el control de la pestaña. El siguiente ejemplo de código muestra el método TabControl:  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. Agrega el método TabId, que proporciona un identificador único para la pestaña. El siguiente ejemplo de código muestra el método TabId:  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. Agrega el método TabOrder, que devuelve el orden de la pestaña. El siguiente ejemplo de código muestra el método TabOrder:  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  El orden de tabulación se define usando números a partir del 0. Las pestañas de configuración integradas de Microsoft se muestran primero y, a continuación, se muestran tus pestañas, según el orden de tabulación que definas. Por ejemplo, si tienes tres pestañas de configuración, especifican el orden de tabulación como 0, 1 y 2 en función del orden en que quieres que aparezcan las pestañas.  
  
15. Agrega el método TabTitle, que proporciona el título de la pestaña. El siguiente ejemplo de código muestra el método TabTitle:  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  El texto del título también puede proceder de un archivo de recursos para satisfacer las necesidades de localización.  
  
16. Guarda y compila la solución.  
  
###  <a name="BKMK_SignAssembly"></a>Firmar el ensamblado con una firma Authenticode  
 Es necesario que el ensamblado tenga una firma Authenticode para que se use en el sistema operativo. Para obtener más información sobre cómo firmar el ensamblado, consulta [firma y comprobación de código con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Instalar al ensamblado en el equipo de referencia  
 Después de compilar correctamente la solución, coloca una copia del archivo DashboardSettingsPage.dll en la siguiente carpeta del equipo de referencia:  
  
 **%ProgramFiles%\Windows Server\Bin\OEM**  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)