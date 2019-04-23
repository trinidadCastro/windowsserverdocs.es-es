El servicio de protección de Host debe instalarse en un bosque de Active Directory independiente.
Asegúrese de que la máquina HGS **no** unido a un dominio antes de empezar e inicie sesión como el administrador del equipo local.

Ejecute los comandos siguientes para instalar el servicio de protección de Host y configurar su dominio.
La contraseña que especifique aquí solo se aplicará a la contraseña del modo de reparación de servicios de directorio de Active Directory. lo hará *no* cambiar la contraseña de inicio de sesión de su cuenta de administrador.
Puede proporcionar cualquier nombre de dominio de su elección para - HgsDomainName.

```powershell
$adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
```

<!-- Appears in guarded-fabric-install-hgs-default.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->
