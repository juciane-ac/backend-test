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
| Possíveis erros             |          |
| Melhorias de performance    |          |
| Vulnerabilidades            |        |
| Problemas arquiteturais     |           |
| Problemas de nomeação e legibilidade     |           |

---

## 1. Possíveis Erros

| Local (arquivo:linha)      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
|  |  |            |
|                            |                                                                           |                                                |

---

## 2. Melhorias de Performance

| Local                      | Descrição                                                                 | Sugestão                                       |
|----------------------------|---------------------------------------------------------------------------|------------------------------------------------|
|  |                               |                        |
|                            | `                                            |   |

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






