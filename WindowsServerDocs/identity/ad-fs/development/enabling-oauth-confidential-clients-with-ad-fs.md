---
description: Cree una aplicación del lado servidor que use clientes confidenciales de OAuth. Use AD FS 2016 o posterior.
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Compilar una aplicación del lado servidor que usa clientes confidenciales de OAuth con Servicios de federación de Active Directory (AD FS) 2016 o posterior
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.openlocfilehash: 2302d8fd1d6d00943316a62578d1d8d9ee096169
ms.sourcegitcommit: d1815253b47e776fb96a3e91556fd231bef8ee6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99042581"
---
# <a name="build-a-server-side-app-that-uses-oauth-confidential-clients-by-using-ad-fs-2016-or-later"></a>Compilar una aplicación del lado servidor que usa clientes confidenciales de OAuth con AD FS 2016 o posterior


Servicios de federación de Active Directory (AD FS) (AD FS) 2016 y versiones posteriores admiten clientes que pueden mantener su propio secreto, como una aplicación o un servicio que se ejecuta en un servidor Web.  Estos clientes se conocen como *clientes confidenciales*.

En este artículo se describe una aplicación web que se ejecuta en un servidor Web. La aplicación sirve como cliente confidencial para AD FS.

## <a name="prerequisites"></a>Requisitos previos
Necesitará los siguientes recursos: 

-   Herramientas de cliente de GitHub.

-   AD FS en Windows Server 2016 Technical Preview 4 o posterior. (En este artículo se da por supuesto que se ha instalado AD FS).

-   Visual Studio 2013 o posterior.

## <a name="create-an-application-group"></a>Crear un grupo de aplicaciones

Para crear un grupo de aplicaciones en AD FS 2016 o posterior:

1.  En administración de AD FS, haga clic con el botón secundario en **grupos de aplicaciones**. A continuación, seleccione **Agregar grupo de aplicaciones**.

2.  En el **Asistente para agregar grupos de aplicaciones**: 
    1. En **nombre**, escriba *ADFSOAUTHCC*. 
    1. En **aplicaciones cliente-servidor**, seleccione la plantilla **aplicación de servidor que tiene acceso a una API Web** .  
    1. Seleccione **Next** (Siguiente).

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG" alt-text="Captura de pantalla que muestra dónde seleccionar la plantilla de la aplicación de servidor que tiene acceso a una P I." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG":::

3.  Copie el valor del **identificador de cliente** . La usará más adelante en el archivo de *web.config* de la aplicación. Es el valor de `ida:ClientId` .

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG" alt-text="Captura de pantalla que muestra dónde se debe copiar el valor de identificador de cliente." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG":::

4. En **URI de redirección**, escriba *https://localhost:44323* .  Seleccione **Agregar** y, a continuación, seleccione **siguiente**.

5.  En la página **configurar credenciales de aplicación** : 
    1. Seleccione **generar un secreto compartido**. 
    1. Copie el secreto. Usará este secreto más adelante en el archivo de *web.config* de la aplicación. Es el valor de `ida:ClientSecret` .  
    1. Seleccione **Next** (Siguiente).

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG" alt-text="Captura de pantalla que muestra la página configurar credenciales de aplicación." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG":::

6. En la página **configurar API Web** : 
    1. En **identificador**, escriba *https://contoso.com/WebApp* . Usará este valor más adelante en el archivo de *web.config* de la aplicación. Es el valor de `ida:GraphResourceId` . 
    1. Seleccione **Agregar**. 
    1. Seleccione **Next** (Siguiente).  

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG" alt-text="Captura de pantalla que muestra la página configurar web A P I." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG":::

7. En la página **aplicar Directiva de Access Control** , seleccione **permitir todo el mundo**. A continuación, seleccione **Siguiente**.

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG" alt-text="Captura de pantalla que muestra la página aplicar Directiva de Access Control." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG":::

8. En la página **configurar permisos de aplicación** , asegúrese de que **OpenID** y **user_impersonation** estén seleccionados. A continuación, seleccione **Siguiente**.

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG" alt-text="Captura de pantalla que muestra la página configurar permisos de aplicación." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG":::

9. En la página **Resumen** , seleccione **siguiente**.

10. En la página **completar** , seleccione **cerrar**.

## <a name="upgrade-the-database"></a>Actualizar la base de datos
Este artículo se basa en Visual Studio 2015. Para que el ejemplo funcione con Visual Studio 2015, actualice el archivo de base de datos siguiendo las instrucciones de esta sección.

En esta sección se describe cómo descargar la API Web de ejemplo y cómo actualizar la base de datos en Visual Studio 2015. Usamos el [ejemplo Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).

Para descargar el proyecto de ejemplo, en Git Bash, escriba el siguiente comando:

```
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git
```

![Captura de pantalla que muestra cómo descargar el proyecto de ejemplo.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)

Para actualizar el archivo de base de datos:

1.  Abra el proyecto en Visual Studio. En la ventana que aparece, un mensaje explica que la aplicación requiere SQL Server 2012 Express. Si no tiene SQL Server 2012 Express, debe actualizar la base de datos.  Seleccione **Aceptar**.

    ![Captura de pantalla que muestra un mensaje que explica que la aplicación requiere S Q L Server 2012 Express. De lo contrario, debe actualizar la base de datos.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)

2. En la parte superior de la ventana, compile la aplicación seleccionando compilar **compilar**  >  **solución**. Se restaurarán todos los paquetes NuGet.

    ![Captura de pantalla que muestra que la restauración de los paquetes NuGet se realizó correctamente.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)

3.  En la parte superior de la ventana, seleccione **Ver**  >  **Explorador de servidores**.  En el panel que se abre, en **conexiones de datos**, haga clic con el botón secundario en **DefaultConnection** y seleccione **modificar conexión**.

    ![Captura de pantalla que resalta el elemento de menú modificar conexión.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)

4.  En la ventana **modificar conexión** , en **nombre de archivo de base de datos (nuevo o existente)**, seleccione **examinar**. Escriba *path\filename.MDF*. A continuación, en el cuadro de diálogo, seleccione **sí**.

    ![Captura de pantalla que muestra el cuadro de diálogo para crear el archivo de base de datos.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  En el cuadro de diálogo **modificar conexión** , seleccione **avanzadas**.

    ![Captura de pantalla que muestra el botón Opciones avanzadas.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)

6.  En el cuadro de diálogo **propiedades avanzadas** , en **origen de datos**, cambie **(LocalDb\v11.0)** a **(LocalDb) \MSSQLLocalDB**.

    ![Captura de pantalla que resalta el campo de origen de datos.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)

7.  Seleccione **Aceptar** > **Aceptar**. A continuación, seleccione **sí** para actualizar la base de datos.

    ![Captura de pantalla que muestra el cuadro de diálogo para actualizar la base de datos.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)

8.  Una vez finalizado el proceso, a la derecha, copie el valor en el campo **cadena de conexión** .

    ![Captura de pantalla que muestra el campo de cadena de conexión.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)

9.  Abra el archivo *web.config* y reemplace el `connectionString` valor por el valor que copió anteriormente.  Guarde el archivo de *web.config* .

    > [!NOTE]
    > Los pasos anteriores son necesarios para que pueda obtener la nueva cadena de conexión. De lo contrario, recibirá errores al ejecutar `Update-Database` más adelante en este artículo.

    ![Captura de pantalla que muestra dónde encontrar el valor de la cadena de conexión.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)

10. En la parte superior de la ventana de Visual Studio, seleccione **Ver**  >  **otra**  >  **consola del administrador de paquetes** de Windows.

    ![Captura de pantalla que resalta el elemento de menú de la consola del administrador de paquetes.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)

11. En el panel de la **consola del administrador de paquetes** , escriba  `Enable-Migrations` .

    > [!NOTE]
    > Si recibe un error que indica que "Enable-Migrations no se reconoce como cmdlet", escriba *Install-Package EntityFramework* para actualizar Entity Framework.

    ![Captura de pantalla que muestra dónde escribir enable-Migrations.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)

12. Escriba `Add-Migration <AnyNameHere>`.

    ![Captura de pantalla que muestra dónde escribir la prueba de Add-Migration.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)

13. Escriba `Update-Database`.

    ![Captura de pantalla que muestra dónde escribir Update-Database.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)

## <a name="modify-the-web-api"></a>Modificación de la API Web 

Para modificar la API Web de ejemplo en Visual Studio:

1.  En Visual Studio, abra el ejemplo.

2.  Abra el archivo de *web.config* .  Modifique la siguiente configuración con los valores que copió en el procedimiento [creación de un grupo de aplicaciones](#create-an-application-group) :

    -   `ida:ClientId`: Escriba el identificador de cliente.

    -   `ida:ClientSecret`: Escriba el secreto de cliente.

    -   `ida:GraphResourceId`: Escriba el identificador de recurso del gráfico.

    ![Captura de pantalla que resalta los valores que debe cambiar.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)

3. En **App_Start**, abra el archivo *Startup.auth.CS* . Realice los cambios siguientes:

    -   Comente las líneas siguientes:

        ```
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);
        ```

    -   En lugar de las líneas quitadas, agregue la siguiente línea:

        ```
        public static readonly string Authority = "https://<your_fsname>/adfs";
        ```

        En este caso, reemplace `<your_fsname>` por la parte de DNS de la dirección URL del servicio de Federación. Por ejemplo, escriba *ADFS.contoso.com*.

        ![Captura de pantalla que muestra los cambios en el archivo de punto de autenticación de punto de inicio de C S.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)

4.  Abra el archivo *UserProfileController.CS* . Realice los cambios siguientes:

    -   Comente la línea siguiente:

        ```
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));
        ```

    -   Reemplace ambas apariciones por el código siguiente:

        ```
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));
        ```

        ![Captura de pantalla que muestra los cambios en el archivo de punto C S del controlador de perfiles de usuario.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)

    -   Comente la línea siguiente:

        ```
        //authContext = new AuthenticationContext(Startup.Authority);
        ```

    -   Reemplace ambas apariciones por el código siguiente:

        ```
        authContext = new AuthenticationContext(Startup.Authority, false);
        ```

        ![Captura de pantalla que resalta los cambios realizados en el valor de authContext.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)

    -   Comente todas las instancias de la línea siguiente:

        ```
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");
        ```

    -   Reemplace todas las apariciones por el código siguiente:

        ```
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());
        ```

        ![Captura de pantalla que resalta el valor u R i de redireccionamiento U R I.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)

## <a name="test-the-solution"></a>Probar la solución

Para probar la solución de cliente confidencial:

1. En la parte superior de la ventana de Visual Studio, asegúrese de que está seleccionado **Internet Explorer** . A continuación, seleccione la flecha verde.

   ![Captura de pantalla que resalta el botón de Internet Explorer.](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)

2. En la página **ASP.net** que se abre: 
    1. En la esquina superior derecha, seleccione **registrar**.  
    1. Escriba el nombre de usuario y la contraseña. 
    1. Seleccione **registrar** para crear una cuenta local en la base de datos SQL.

   :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG" alt-text="Captura de pantalla que muestra dónde crear una cuenta local en la base de datos S Q L." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG":::

3. Observe el mensaje **Hola abby \@ contoso.com!**.  Seleccione **Perfil**.

   :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG" alt-text="Captura de pantalla que resalta el perfil." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG":::

4. En la página nueva, verá un mensaje que le pide que inicie sesión.  Seleccione **aquí**.

    :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG" alt-text="Captura de pantalla que muestra la página de Perfil de usuario." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG":::

    Se le pedirá que inicie sesión en AD FS.

   :::image type="content" source="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG" alt-text="Captura de pantalla que muestra la página de inicio de sesión de una D F S." lightbox="media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG":::     
        
## <a name="next-steps"></a>Pasos siguientes
Más información sobre el [desarrollo de AD FS](../../ad-fs/AD-FS-Development.md).
