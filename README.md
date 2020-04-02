# docker-jitsi

Solução para instalação de videoconferência de código aberto Jitsi em containers Docker, utilizando docker-compose. Já habilitada para utilizar certificados digitais válidos da CA gratuita Let's Encrypt (https://letsencrypt.org). 

## Requisitos

### Portas

Para que esta solução funcione, é fundamental abrir as seguintes portas de entrada em seu firewall corporativo, ou ainda no seu Sistema Operacional:

- 80/tcp (HTTP, para redirecionar para HTTPS)
- 443/tcp (HTTPS, interface Web do Jitsi)
- 4443/tcp (Jitsi Videobridge, RTP sobre TCP)
- 10000/udp (Jitsi Videobridge, RTP sobre UDP) 


### Docker

Para instalar o Jitsi em Docker, você precisa ter o Docker daemon instalado. No Ubuntu 18.04 LTS:

```
apt update
apt install docker.io
```

Para outros cenários, verifique a documentação em https://docs.docker.com/install/.

### Docker-compose

Neste repositório, utilizamos o docker-compose para configurar os containers da solução. Para instalá-lo:

```
curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

Você pode ver qual a última release do docker-compose em https://github.com/docker/compose/releases.

Para verificar a versão instalada do docker-compose, e se ele está funcionando:

```
docker-compose -v 
```

### Este repositório

Com o Git instalado, clone este repositório:

```
apt install git
git clone https://github.com/interlegis/docker-jitsi.git
```

## Começando

Para começar, entre no diretório docker-jitsi e copie o arquivo env.example para que seja modificado. Edite com seu editor de textos favorito (como o Vi):

```
cd docker-jitsi
cp env.example .env
vi .env
```

Modifique agora as variáveis que estão logo abaixo da marcação "### MUDAR AQUI ###":

- PUBLIC_URL: Coloque a URL (com https://) que os usuários utilizarão para acessar seu servidor Jitsi.
- DOCKER_HOST_ADDRESS: Coloque aqui o endereço IP do servidor que estará rodando esta solução Jitsi.
- LETSENCRYPT_DOMAIN: Coloque aqui o domínoi (URL, mas sem https://)  que os usuários utilizarão para acessar seu servidor Jitsi.
- LETSENCRYPT_EMAIL: E-mail utilizado pelo Let's Encrypt para avisos importantes sobre seu certificado.

## Subindo os containers:

Após a edição do arquivo, chegou a hora de rodar os containers da solução. 

Se quiser ver os logs, basta usar o docker-compose logs. No exemplo abaixo, utilizamos o parâmetro "-f" para ver os logs em tempo real e o parâmetro "--tail" para limitar o número de linhas antigas a mostrar.

```
cd docker-jitsi
docker-compose up -d
docker-compose logs -f --tail 500
```

Aguarde alguns minutos para que os containers possam subir e o certicado Let's Encrypt possa ser emitido com sucesso. Então, acesse a interface Web do Jitsi na URL configurada na variável PUBLIC_URL.

Agora é só conversar com seus amigos por vídeo! 

