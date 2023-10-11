Dentro de la consola de jenkins
en caso de erro por permisos de la carpeta SSH-KEY-Jenkins
    - `chmod 400 SSH-KEY-Jenkins`
acceder a la consola
    - `ssh -i SSH-KEY-Jenkins ubuntu@x.xx.xxx.xxx` ubuntu es el nombre por defecto y la ip pública de la instancía

Una vez dentro:
- `sudo apt update`
- `sudo apt install openjdk-11-jre`
- `curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins`

Para poder ver nuestro jenkins levantado ir a la panel de aws
- Instancía jenkins
- seguridad
- pinchar en el enlace de grupos de seguridad
- Editar reglas de entrada
- agregar regla
    intervalo de puertos: 8080
    Origen: 0.0.0.0/0
    guardar

para saber el estado de la instancía podemos ejecutar el siguiente comando dentro de la consola
`systemctl status jenkins`

- copiar la llave que aparece en la primera línea después de CGroup 
- o ejecutando el comando `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

Para conectar el repo con github
- Crear una tarea en jenkins como proyecto libre
- configurar / configurar el origen del código fuente
    - seleccionar git 
    - añadir la url https del repositorio
    - indicar la rama
y en caso de no ser privado no hará falta añadir clave

- configurar
    - marcar el disparador  "GitHub hook trigger for GITScm polling"

En github ir al repo / settings / webhooks / add
- añadir PayloadUrl, que es el host y puerto de nuestro jenkins y agregandole `github-webhooks` =>  http://xxxxxx:8080/github-webhooks
- marcar let me select indiviual events
    marcar check de pull request y pushes
    guardar

