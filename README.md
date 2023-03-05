# Desafio-linux-docker
## Desafio, utilizando arquivos de execução e instalação, como Docker, implementando uma estrutura de microsserviços.

#### Utilizei 3 VMs criados no VirtualBox para estruturar o projeto.

![virtualbox](https://user-images.githubusercontent.com/48320787/222936090-9d72ae1f-a772-47f7-9842-7fce6111a80c.png)

- ubuntu-aws1  ➤ *Servidor principal*
- ubuntu-aws2 ➤ *Servidor worker*
- ubuntu-aws3 ➤ *servidor worker*

#### Executando o Docker no arquivo nas 3 VMs no VirtualBox.

- chmod +x iac-install-docker.sh ➤ *Comando para permitir a execução*
- ./iac-install-docker.sh ➤ *Comando para a instalação do Docker*

##### Executando o arquivo no servidor “ ubuntu-aws1 “ no modo Stand Alone:

- chmod +x iac-alone.sh ➤ *Comando para permitir a execução*
- ./iac-alone.sh ➤ *Comando para execução do arquivo*

#### Alterar o arquivo “ index.php “ e atualizar o IP principal.

- nano /var/lib/docker/volumes/app/_data/index.php ➤ *Caminho do arquivo*
- $servername = "192.168.0.144"; ➤ *Alterar o IP*

#### Executar no modo cluster.

Para continuar é preciso estar:

1. Os 3 VMs ligados
2. O Docker já estarem instalados nas 3 VMs
3. As 3 VMs estarem na mesma rede

#### Configurando os IPs no arquivo de configuração proxy Nginx.

nano /var/lib/docker/volumes/app/_data/nginx.conf

#### Atualizando os três IPs dos servidores criados. Na porta: 4500.

http {

  

  upstream all {

    server 192.168.0.144:80;
    
    server 192.168.0.161:80;
    
    server 192.168.0.177:80;

  }

  server {

     listen 4500;
    
     location / {
    
       proxy_pass http://all/;
    
     }

  }

}

#### Executar o arquivo com script.

- chmod +x iac-manager.sh ➤ *Comando para permitir a execução*
- ./iac-manager.sh ➤ *Comando para executar*

*Ao executar o arquivo, vai pedir que executar um comando **Docker swarm..** nas outras duas VMs antes de concluir a execução do arquivo.*

*Depois de executar o comando nas duas VMs, volte ao servidor principal e conclua a execução do arquivo.*

#### Executar o arquivo com script.

Esse procedimento deve ser feito somente nas duas VMs .

nano iac-workers.sh ➤ *Atualizar o IP do servidor principal*

#### Atualizar o IP do servidor principal.

mount -o v3 192.168.0.144:/var/lib/docker/volumes/app/_data /var/lib/docker/volumes/app/_data

#### Execute o arquivo de script.

- chmod +x iac-workers.sh ➤ *Comando para permitir a execução*
- ./iac-workers.sh ➤ *Comando para execução do arquivo*

#### Teste final para o projeto.

Para confirmar se está funcionando bem o cluster, podemos acessar o banco de dados.

Acessando o banco de dados com comando no servidor principal

mysql -u root -p -h 192.168.0.144 meubanco

Senha do banco de dados “ Senha123 “ ➤ *Deixei a mesma do vídeo do professor.*

#### Já dentro do banco de dados, execute o comando para ver os dados registrados.

select * from dados;

Final de projeto do desafio com Docker: Utilização prática no cenário de microsserviços
