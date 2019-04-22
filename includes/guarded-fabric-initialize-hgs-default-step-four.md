Si los certificados que proporcionó en HGS mediante huellas digitales, le pedirá que conceda acceso de lectura HGS a la clave privada de tales certificados. En un servidor con experiencia de escritorio instalado, complete los pasos siguientes:

1.  Abra el Administrador de certificados del equipo local (**certlm.msc**)
2.  Buscar los certificados > haga clic en > todas las tareas > Administrar claves privadas
3.  Haga clic en **Agregar**.
4.  En la ventana del selector de objetos, haga clic en **tipos de objeto** y habilitar **cuentas de servicio**
5.  Escriba el nombre de la cuenta de servicio que se ha mencionado en el texto de advertencia desde `Initialize-HgsServer`
6.  Asegúrese de que la gMSA tiene acceso de "Lectura" a la clave privada.

En server core, deberá descargar un módulo de PowerShell para ayudar en la configuración de los permisos de clave privados.

1.  Ejecute `Install-Module GuardedFabricTools` en el servidor HGS si tiene conectividad a Internet, o ejecutar `Save-Module GuardedFabricTools` en otro equipo y copie el módulo a través de un servidor HGS.
2.  Ejecute `Import-Module GuardedFabricTools`. Esto agregará propiedades adicionales para el objeto de certificado que se encuentre en PowerShell.
3.  Encontrar la huella digital del certificado en PowerShell con `Get-ChildItem Cert:\LocalMachine\My`
4.  Actualizar la ACL, reemplazando la huella digital con su propio y la cuenta de gMSA en el código siguiente con la cuenta aparece en el texto de advertencia de `Initialize-HgsServer`.

    ```powershell
    $certificate = Get-Item "Cert:\LocalMachine\1A2B3C..."
    $certificate.Acl = $certificate.Acl | Add-AccessRule "HgsSvc_1A2B3C" Read Allow
    ```

Si está utilizando certificados respaldada por HSM o los certificados almacenan en un proveedor de almacenamiento de claves de terceros, no es posible que estos pasos se aplican a usted. Consulte la documentación del proveedor de su almacenamiento de claves para obtener información sobre cómo administrar los permisos de la clave privada. En algunos casos, no hay ninguna autorización o autorización se proporciona a todo el equipo cuando se instala el certificado.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->