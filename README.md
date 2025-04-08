# Teste Técnico – Análise de Código

## Objetivo

Identificar locais de possíveis alterações no código, incluindo:

- Possíveis erros
- Melhorias de performance
- Vulnerabilidades
- Códigos com responsabilidades em camadas/contextos diferentes do ideal

---

## Sumário da Análise

| Tipo                        | Quantidade |
|-----------------------------|------------|
| Possíveis erros             |  1        |
| Melhorias de performance    | 1         |
| Vulnerabilidades            |  1      |
| Problemas arquiteturais     |  2         |
| Problemas de nomeação e legibilidade     |    3       |

---

## 1. Possíveis Erros

| Local      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
| Requests/User | A validação atual dos campos de document (CPF,CNPJ da companhia) apenas verifica a quantidade de dígitos, não verificando a integridade dos dados, permitindo que dados como  como 00000000000 sejam passados, podendo contribuir para fraudes | criar rules customizadas ou utilizar pacotes de validações existentes, que sejam capazes de validar o formato, o dígito verificador e impedir inserir sequências de números repetidos.           |


---

## 2. Melhorias de Performance

| Local                      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
| database > tabelas users e companies | campos de documentos com tamanho 255, ocupando mais espaço do que precisam e deveriam, isso pode ser um gasto desnecessário de espaço em disco principalmente se considerar o cenário de um BD com uma grande quantidade de dados, além disso o campo modelado assim pode impactar negativamente nas consultas e também fazer com que validações ou buscas por esses documentos sejam comprometidas                           | limitar apenas ao tamanho necessário para a numeração padrão do documento (CNPJ, CPF, etc)                       |


---

## 3. Vulnerabilidades

| Local                      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
|app/Repositories/User   | Possível vulnerabilidade a sql injection, através do uso de whereRaw nas consultas, uma vez que instruções brutas serão injetadas na consulta como strings, e o laravel não garante que consultas assim estejam protegidas contra ataques de SQL Injection                          | utilizar os bindings do laravel, exemplo:  $this->builder->whereNull('accounts.id');          |
|  |                 |         |

---

## 4. Problemas de Arquitetura / Responsabilidade

| Local                      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
| app/Domains/User/create e app/Domains/User/Update   |  Uso de dependência direta do Laravel (Hash::make()) viola o princípio de separação de responsabilidades | criar um Service de Integração com a dependência do laravel para gerar o hash da senha                     |
|app/Domains/User/create e app/Domains/User/Update         | validação de tipo de usuário utilizando in_array com strings fixas diretamente no método, gerando repetição de codigo em lugares diferentes           |  criar uma class de constants (UserType) e refatorar a chamada no domain, que passaria a usar algo como (!UserType::isValid($this->type))            |

---

## 5. Problemas de nomeação e legibilidade 

| Local                      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
| app/UseCases/User/show   |    Nome do arquivo não segue convenção de nomenclatura                             | Renomear para Show.php                     |
| app/UseCases/User/show   |    Nome de variávies genéricas, sem significado real ($a,$b,$c)                          | Usar nomes concretos, condizentes com o que a variável armazena ($userid,$companyId,$user) |
| app/Domains/User e app/Domains/Company | Mensagens de erro genéricas (Não é possível adicionar o...) no caso de adicionar dados que são únicos e já estão associados a um cadastro, dificultando o tratamento preciso dos erros | utilizar exceções específicas como por exemplo CnpjAlreadyinUseException |






