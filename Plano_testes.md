
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

# Tabela de Casos de Teste — D&D 5e API

| ID     | Nome                                                                                                  | Pré-condição                        | Dados de Entrada                              | Ação                                   | Resultado Esperado                                                                 |
|--------|-------------------------------------------------------------------------------------------------------|-------------------------------------|-----------------------------------------------|----------------------------------------|------------------------------------------------------------------------------------|
| TC-001 | Status code é 200                                                                                     | API online e acessível              | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Status 200 OK                                                                      |
| TC-002 | Mago e clérigo devem ser capazes de utilizar a magia                                                  | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | classes contém objetos com index "wizard" e "cleric"                               |
| TC-003 | O level da magia deve ser 3                                                                           | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Campo level igual a 3                                                              |
| TC-004 | O material necessário para a magia deve ser um drop de sangue, uma peça de carne e uma gota de água  | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Campo material igual a "A drop of blood, a piece of flesh, and a pinch of bone dust." |
| TC-005 | As urls retornadas devem ser válidas                                                                  | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Todas as URLs de classes, subclasses e school seguem o padrão /api/2014/{tipo}/{index} |
| TC-006 | O tempo de casting não deve ser maior que 1 minuto                                                    | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Valor numérico de casting_time não é maior que 1                                   |
| TC-007 | A magia não deve ser um ritual                                                                        | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Campo ritual igual a false                                                         |
| TC-008 | O range da magia não deve ser maior que 10 feet                                                       | TC-001 passou                       | index: animate-dead                           | GET /api/2014/spells/animate-dead      | Valor numérico de range não é maior que 10                                         |
| TC-009 | Status code é 200                                                                                     | API online e acessível              | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Status 200 OK                                                                      |
| TC-010 | Dano de magia no level 2 é 4d4                                                                        | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | damage_at_slot_level['2'] igual a "4d4"                                            |
| TC-011 | Mago deve ser capaz de utilizar a magia                                                               | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | classes contém objeto com index "wizard"                                           |
| TC-012 | O dano deve aumentar a cada level                                                                     | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Para cada slot i (3 a 9), dano no slot i é maior que no slot i-1                   |
| TC-013 | Tamanho da lista de nível é igual a 8 (level 2 ao 9)                                                 | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Object.keys(damage_at_slot_level) tem comprimento 8                                |
| TC-014 | Maior nível de magia é 9                                                                              | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | damage_at_slot_level possui a propriedade '9'                                      |
| TC-015 | O dano deve aumentar de 1d4 a cada nível                                                              | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Para cada slot i (3 a 9), dano no slot i é maior que no slot i-1                   |
| TC-016 | Campos devem retornar uma url válida                                                                  | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Todas as URLs de classes, subclasses, school e damage_type seguem o padrão /api/2014/{tipo}/{index} |
| TC-017 | Classes diferentes de mago não podem usar a magia                                                    | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | classes não contém objeto com index "cleric"                                       |
| TC-018 | O casting time não deve ser maior do que 1 turno                                                      | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Valor numérico de casting_time não é maior que 1                                   |
| TC-019 | O range não deve ser maior do que 90 feet                                                             | TC-009 passou                       | index: acid-arrow                             | GET /api/2014/spells/acid-arrow        | Valor numérico de range não é maior que 90                                         |

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
