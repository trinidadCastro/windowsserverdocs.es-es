1.  Ejecute [Install HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/install-hgsserver) para unirse al dominio y promocionar el nodo a un controlador de dominio.

    ```powershell
    $adSafeModePassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

    $cred = Get-Credential 'relecloud\Administrator'

    Install-HgsServer -HgsDomainName 'bastion.local' -HgsDomainCredential $cred -SafeModeAdministratorPassword $adSafeModePassword -Restart
    ```

2.  Cuando se reinicia el servidor, inicie sesión con una cuenta de administrador de dominio.

<!-- Appears twice in guarded-fabric-configure-additional-hgs-nodes.md 
-->