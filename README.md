# Wordpress + NGINX



![ARQUITECTURA](./doc/arch.drawio.svg)

- **Wordpress**: CMS *Open Source* .
  
  - URL: https://wp.localdev.me:8443
  - Usuario: se genera en el primer ingreso

- **PhpMyAdmin**: UI Web de administración de MySQL
  
  - URL : https://admin.localdev.me:8443

- **MySQL**: Motor de base de datos de Wordpress

**Ejecutar el proyecto**
```bash
docker compose --project-name wp up -d
```
**Detener el proyecto**
```bash
docker compose --project-name wp up -d
```

## CHECKLIST HTTPS

**NGINX** es uno de los Proxy Reversos mas utilizados, a diferencia de la mayoria de los contenedores se configura por medio de un archivo de configuracion (hay variantes de la imagen que permiten configurar con variables de entorno pero para este ejemplo es mejor dejarlo simple).

**LISTA DE TAREAS PARA HTTPS**

- [ ] Agregar NGINX al docker-compose
- [ ] Configurar todos los contenedores en la red interna excepto NGINX que esta en ambas redes
- [ ] Generar la configuracion de NGINX y montarla en el directorio correspondiente
- [ ] Generar Certificados y montarlos en el directorio correspondiente (fijense que la configuracion de nginx hace referencia a ellos)


## Generacion de certificado auto-firmado (self-signed) para HTTPS

Una de las formas mas comunes de generar Certificados HTTPS es utilizando el programa **OpenSsl**.

Utilizaremos docker para ejecutar openssl sin necesitad de instalarlo en nuestras maquinas. La imagen [alpine/openssl](https://hub.docker.com/r/alpine/openssl) contiene lo minimo basico e indispensable para realizar esta tarea.

La siguiente linea de comandos ejecuta un contenedor de docker con la imagen `alpine/openssl` genera los certificados y finaliza el contenedor. Puede variar si usan un sistema linux , especialmente la referencia al .\proxy\certs 

```powershell
docker run -ti --rm -v .\proxy\certs:/apps -w /apps alpine/openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes -keyout /apps/localdev.me.key -out /apps/localdev.me.crt -subj "/CN=localdev.me" -addext "subjectAltName=DNS:localdev.me,DNS:*.localdev.me"

```







