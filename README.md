<h1 align="center">
   <a href="#"> Script AssumeRole With SessionToken </a>
</h1>

## Sumário

<!--ts-->
   * [Introdução](#Introdução)
   * [Pre-requisitos](#Pre-requisitos)
   * [AssumeRole](#AssumeRole)
   * [RoleSessionName](#RoleSessionName)
   * [TokenCode](#TokenCode)
   * [Como-usar](#Como-usar)
<!--te-->

### Introdução

A AWS fornece AWS Security Token Service (AWS STS) como um serviço da web que permite solicitar credenciais temporárias e de privilégio limitado para usuários do AWS Identity and Access Management (IAM) ou para usuários que você autentica (usuários federados). Este guia descreve a API AWS STS.

### Pre-requisitos

Antes de iniciar, você deve ter essas ferramentas no seu computador o [Git] (https://git-scm.com), [AWS CLI] (https://docs.aws.amazon.com/pt_br/cli/latest/userguide/cli-chap-install.html) e [jq] (https://stedolan.github.io/jq/) 

(Opcional), um bom editor como [VSCode] (https://code.visualstudio.com/)

### AssumeRole

Retorna um conjunto de credenciais de segurança temporárias que você pode usar para acessar recursos da AWS aos quais você normalmente não teria acesso. Essas credenciais temporárias consistem em um AccessKeyID, SecretKeyID e um SessionTokenID. Normalmente, você usa AssumeRole em sua conta ou para acesso entre contas. Para uma comparação de AssumeRole com outras operações de API que produzem credenciais temporárias.

Usando MFA com AssumeRole

Você pode incluir informações de autenticação multifator (MFA) ao chamar AssumeRole. Isso é útil para cenários de contas cruzadas para garantir que o usuário que assume a função foi autenticado com um dispositivo AWS MFA. Nesse cenário, a política de confiança da função que está sendo assumida inclui uma condição que testa a autenticação MFA. Se o chamador não incluir informações válidas de MFA, a solicitação para assumir a função será negada. 

Para usar MFA com AssumeRole, você passa valores para os parâmetros SerialNumber e TokenCode. O valor SerialNumber identifica o hardware do usuário ou o dispositivo MFA virtual. O TokenCode é a senha de uso único baseada em tempo (TOTP) que o dispositivo MFA produz.

### DurationSeconds

A duração, em segundos, da sessão de função. O valor especificado pode variar de 900 segundos (15 minutos) até a duração máxima da sessão definida para a função. A configuração da duração máxima da sessão pode ter um valor de 1 hora a 12 horas. Se você especificar um valor superior a esta configuração ou a configuração do administrador (o que for inferior), a operação falhará. Por exemplo, se você especificar uma duração de sessão de 12 horas, mas seu administrador definir a duração máxima da sessão para 6 horas, sua operação falhará.
    
O encadeamento de funções limita sua sessão de função AWS CLI ou AWS API a no máximo uma hora. Ao usar a operação da API AssumeRole para assumir uma função, você pode especificar a duração de sua sessão de função com o parâmetro DurationSeconds. Você pode especificar um valor de parâmetro de até 43200 segundos (12 horas), dependendo da configuração de duração máxima da sessão para sua função. No entanto, se você assumir uma função usando o encadeamento de funções e fornecer um valor de parâmetro DurationSeconds maior que uma hora, a operação falhará. Para saber como visualizar o valor máximo para sua função, consulte Visualizar a configuração de duração máxima da sessão para uma função no Guia do usuário do IAM.

Por padrão, o valor é definido como 3600 segundos.

## RoleSessionName

Um identificador para a sessão de função assumida.

Use o nome da sessão da função para identificar exclusivamente uma sessão quando a mesma função for assumida por diferentes diretores ou por diferentes motivos. Em cenários de contas cruzadas, o nome da sessão da função é visível e pode ser registrado pela conta que possui a função. O nome da sessão de função também é usado no ARN do principal de função assumido. Isso significa que as solicitações subsequentes da API entre contas que usam as credenciais de segurança temporárias irão expor o nome da sessão da função para a conta externa em seus logs do AWS CloudTrail.

## TokenCode 
    
O valor fornecido pelo dispositivo MFA, se a política de confiança da função que está sendo assumida exigir MFA. (Em outras palavras, se a política inclui uma condição que testa para MFA). Se a função que está sendo assumida exigir MFA e se o valor TokenCode estiver ausente ou expirado, a chamada AssumeRole retornará um erro de "acesso negado".
    
### Como usar

Clone this repository
```bash
git clone https://github.com/datisolucoesemti/ScriptAssumeRoleWithSessionToken.git
```
Instalando o AWSCLI.
```bash
sudo apt install awscli
```
Instalando o AWSCLI e JQ.
```bash
sudo apt install jq
```
Acessar o diretório
```bash
cd ScriptAssumeRoleWithSessionToken
```
Dar a permissão de execução
```bash
chmod +x AssumeRole.sh
```
Rodar o Script
```bash
source AssumeRole.sh
```
