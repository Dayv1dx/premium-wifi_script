# Premium Wi-Fi Script

Este repositório mostra como adicionar ONTs na Plataforma via CLI na OLT Huawei.

Requisitos:
- OLT Huawei
- ONT
- Servidor FTP

## Funcionamento do processo

Para que uma ONT seja gerenciavél pela plataforma, é necessário realizar o apontamento dos três seguintes parâmetros:

~~~ xml
<X_HW_AppRemoteManage 
CurrentMgtURL=" br-cloud.openlife.huawei.com" PhoneAppURL="br-cloud.openlife.huawei.com" CurrentPort="9002"/>
~~~

Existem algumas formas de fazer esse apontamento, entretanto irei descrever uma, a qual consiste em adicionar um arquivo `.xml` na ONT utilizando linhas de comando na OLT. O processo é similar a uma atualização de firmware.

Segue passo a passo:

1. ****Configurar um servidor FTP e adicionar o arquivo  [changeNCEURL](./arquivos/changeNCEUrl.xml)**
 nele.**

2. **Fazer login na OLT no _privilege mode_, entrar no _diagnose_ e configurar as credenciai FTP server**

~~~ bash
ftp set 
~~~

3. **Rodar o seguinte comando:**

~~~ bash
# ont-load info configuration {nome_arquivo.xml} {ftp/sftp/tftp} {ip address} {user_ftp} {password_ftp}

  ont-load info configuration changeNCEUrl.xml ftp 10.168.152.168 username ******
~~~

4. **Carregar o script**
~~~ bash
# load script {ftp/sftp/tftp} {ip address} {nome_arquivo.xml}

  load script ftp 10.168.152.168 changeNCEURL.xml
~~~ 

5. **Selecionar as ONTs**

~~~ bash
# ont-load select {frame/slot port ont-id}

  ont-load select 0/1 0 0
~~~ 

6. **Carregar as ONTs**

~~~ bash
# ont start

  ont-load start
~~~ 

7. **Verificar o status do load**

~~~ bash
  Command:
          display ont-load select 0/1 0 0
 --------------------------------------------------------------
   F/S/P          ONT ID         Load state      Load progress
 --------------------------------------------------------------
   0/1/0               0            Success               100%
 --------------------------------------------------------------
~~~ 

Com essa operação bem sucedida, basta verificar o status do equipamento na Plataforma. Para dúvidas consulte o suporte técnico.