
# Plano de Testes — D&D 5th Edition API
--- 
*Disciplina: Qualidade e Gerência de Produto de Software | Ferramenta: Postman | Data: Março/2026*
*Alunos: preguica*

## 1. Escopo 
---
O sistema a ser testado é a D&D 5th Edition API (https://www.dnd5eapi.co), uma API REST pública que fornece dados do jogo de RPG Dungeons & Dragons em sua 5ª edição. 

A suíte de testes cobrirá os endpoits listados a seguir, validando os seguintes aspectos: 
- Recuperação de magias individuais por índice (ex: acid-arrow)
- Listagem de todas as magias disponíveis
- Integridade estrutural dos campos retornados (level, components, damage, etc)
- Comportamento da API para entradas inválidas, vazias ou nos limites (tipo nível 0 e nível 9)
- Consistência das URLs internas (URLs de tipos de dano e classes retornadas pela API)

**Endpoints Cobertos**
- https://www.dnd5eapi.co/api/2014/equipments/

- https://www.dnd5eapi.co/api/2014/alignments/

- https://www.dnd5eapi.co/api/2014/spells/
	- https://www.dnd5eapi.co/api/2014/spells/{spell}

- https://www.dnd5eapi.co/api/2014/monsters/
	- https://www.dnd5eapi.co/api/2014/monsters/{monster}

- https://www.dnd5eapi.co/api/2014/classes/

## 2. Objetivos 

---
A suíte de testes tem como objetivo garantir que: 
- Garantir a integridade do sistema testado
- Verificar a vulnerabilidade a falhas
- Verificar se o software é suficientemente otimizado
- Garantir a fidelidade ao conteúdo original


## 3. Estratégia de Testes 
---
Abordagem usada, tecncias de particionamento de equivalencia e analise de valor limite utilizadas

A abordagem utilizada para os testes foi a Caixa Preta, visto que testamos um sistema sem ter acesso ao seu código de funcionamento interno e tendo disponível apenas as entradas e saídas dos dados do sistema.
## 4. Ambiente
---
Tecnologias versões e dependências

Foram utilizadas as seguintes tecnologias
- Postman versão 12.+ (framework) 12+ (aplicativo) for windows
- Postman versão 12.+ (framework) 12+ (aplicativo) for linux

## 5. Descrição dos Casos de Teste
---
tabela com tudo, id nome, pre condicao, entrada ação e resultado

| ID | Nome do Teste | Pré-condição | Dados de Entrada | Ação | Resultado Esperado |
|----|---------------|--------------|------------------|------|--------------------|
| TC-020 | Status 200 OK - Alignments | API Online | GET /alignments | Realizar requisição GET | Status Code deve ser 200 |
| TC-021 | Integridade dos campos - Alignments | API Online | GET /alignments | Validar propriedades do JSON | "Todos os itens em results devem ter: index |  name e url" |
| TC-022 | Contagem de alinhamentos (Campo count) | API Online | GET /alignments | Verificar valor da chave count | O valor deve ser exatamente 9 |
| TC-023 | Quantidade na lista de resultados | API Online | GET /alignments | Contar itens no array results | Devem existir 9 itens na lista |
| TC-024 | Bloqueio de combinações inválidas | API Online | GET /alignments | Validar index contra lista proibida | "Não deve haver índices como ""chaotic-chaotic"" ou  |""good-good"""
| TC-025 | Performance - Alignments | API Online | GET /alignments | Medir tempo de resposta | O tempo deve ser inferior a 1000ms |
| TC-026 | Status 200 OK - Todos os Monstros | API Online | GET /monsters | Realizar requisição GET | Status Code deve ser 200 |
| TC-027 | Validação de lista não vazia | API Online | GET /monsters | Verificar array results | O campo results deve ser um array e não estar vazio |
| TC-028 | Quantidade total de monstros | API Online | GET /monsters | Contar itens no array results | A lista deve conter 334 itens |
| TC-029 | Sincronia Count vs Results | API Online | GET /monsters | Comparar count com length do array | Os valores devem ser idênticos |
| TC-030 | Integridade dos Monstros (Amostra) | Detalhes acessíveis | GET /monsters/{index} | "Validar campos essenciais (HP |  AC |  Atributos)" | Campos obrigatórios  |não podem ser vazios e devem ter tipos corretos
| TC-031 | Status 404 - Monstro Inexistente | API Online | GET /monsters/exemplo_nao_existe | Tentar buscar monstro inválido | Status Code deve ser 404 |
| TC-032 | Negativo - Status 200 para erro | API Online | GET /monsters/exemplo_nao_existe | Tentar buscar monstro inválido | O status NÃO deve ser 200 |
| TC-034 | Integridade Detalhada - Aboleth | API Online | GET /monsters/aboleth | Validar valores específicos do Aboleth | "Nome=""Aboleth"" |  Força=21 |   |Alinhamento=""lawful evil"" |  etc."
| TC-035 | Validação de Sub-listas - Aboleth | API Online | GET /monsters/aboleth | Verificar arrays de ações e proficiências | Arrays actions e special_abilities não  |podem estar vazios
| TC-037 | Negativo - Spellcasting Aboleth | API Online | GET /monsters/aboleth | "Buscar ""Spellcasting"" em special_abilities" | O monstro não deve possuir esta  |habilidade
| TC-039 | Método POST não permitido | Endpoint de leitura | POST /monsters/monstro_exemplo | Tentar criar um monstro via POST | Status Code deve ser 404 ou 405 |
| TC-040 | Formato de erro (HTML) | Endpoint de leitura | POST /monsters/monstro_exemplo | Verificar Header Content-Type | O tipo de conteúdo deve ser text/html |
| TC-041 | Mensagem de erro de método | Endpoint de leitura | POST /monsters/monstro_exemplo | Verificar corpo da resposta | "Deve conter o texto ""Cannot POST /api/ |2014/monsters"""
| TC-043 | Método DELETE não permitido | Endpoint de leitura | DELETE /monsters/aboleth | Tentar deletar um monstro | Status Code deve ser 404 ou 405 |

## 6. Critérios de Aceite
---
O sistema será considerado aceito a partir da execução dos testes e sua devida aprovação:

- Todos os end-points testados devem cumprir os requisitos estipulados de tempo de resposta.
- Existência de campos obrigatórios e os mesmos não nulos.
- Testes de valores de dados corretos de acordo com o requisito esperado
- Testes de regras de negócios (lógica interna correta)
- Testes de listas com dimensão e valores internos corretos
- Testes de URL existentes quando necessário e no padrão correto
- Bloqueios de requisições sem permissão ou inválidas
## 7. Riscos e Limitações
---
- Quantidade dos dados fornecidos pela API
- Falta de dados dentro de documentos internos da API ( dados desatualizados e incompletos)
- Falta de cobertura de endpoints, além de bloqueio por alto número de requisições
