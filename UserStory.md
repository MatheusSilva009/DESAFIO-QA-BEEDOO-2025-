# User Story — Cadastro de Curso

## Resumo
Como membro do time de QA
Quero validar o fluxo de cadastro de um curso
Para garantir que o formulário aceite entradas válidas, rejeite entradas inválidas e apresente mensagens claras ao usuário

## Critérios de Aceitação (derivados dos cenários Gherkin)
- Dado que eu estou na página "/Cadastrar curso.html", ao preencher todos os campos obrigatórios com valores válidos e enviar, devo ver confirmação de cadastro ou redirecionamento para a lista de cursos.
- O campo "Nome do curso" é obrigatório; tentativa de envio sem este campo exibe mensagem de erro específica.
- O campo "Data de início" deve ser anterior à "Data de fim"; caso contrário, exibir erro indicando o problema.
- O campo "Número de vagas" deve aceitar apenas inteiros positivos; valores não numéricos ou negativos devem ser rejeitados com mensagem apropriada.
- O campo "Url da imagem de capa" deve aceitar apenas URLs válidas; URLs inválidas devem gerar erro de validação.
- O campo "Tipo de curso" é obrigatório; envio sem seleção deve exibir erro.
- O campo "Descrição do curso" é opcional; cadastro sem descrição deve ser permitido.
- Há validação de tamanho para descrição (ex.: bloquear acima de X caracteres — cenário considera 2000+ como inválido).
- O formulário deve aceitar números de vagas grandes dentro do limite do sistema; se houver limite, exibir erro claro ao exceder.
- Todos os campos devem ter labels acessíveis; o botão de envio deve ser alcançável por teclado e possuir texto claro (ex.: "CADASTRAR CURSO").

## Regras de Negócio e Restrições
- Nome do curso: texto não vazio, limite razoável (ex.: 1–255 caracteres).
- Descrição: opcional, limite máximo (ex.: 2000 caracteres).
- Datas: formato local dd/mm/aaaa; lógica: início < fim.
- Número de vagas: inteiro positivo; validar mínima (>=1) e máxima conforme regra do produto (ex.: <= 1000).
- URL da imagem: validação de esquema (http/https) e formato de URL.
- Tipo do curso: enumerado (ex.: Online, Presencial, Híbrido).

## Decisões tomadas ao criar a user story

- Priorizei validações que causam falha grave no fluxo (nome ausente, datas inconsistentes, número de vagas inválido, tipo não selecionado) porque são as que impactam a integridade do cadastro.
- Adicionei regras de tamanho e limites (descrição, vagas) para cobrir casos de borda e proteger o sistema contra entradas excessivas ou abusivas.
- Mantive a descrição como opcional porque nem sempre é obrigatória para que um curso exista; isso evita bloquear cadastros mínimos.
- Incluí requisitos de acessibilidade (labels, alcance por teclado e texto claro do botão) para garantir conformidade básica de usabilidade e inclusão.
- Sugeri valores de limite (ex.: 2000 caracteres, vagas <=1000) como exemplos; esses valores devem ser alinhados com produto/back-end e ajustados conforme políticas da plataforma.
- Mantive os critérios observáveis (mensagens de erro, redirecionamento, presença de elementos) para facilitar automação de testes e validação manual.

## Observações para QA e Desenvolvimento
- Confirmar com o time de produto e backend os limites exatos (tamanhos máximos, máximo de vagas).
- Definir as mensagens de erro padrão (texto) para que os testes verifiquem conteúdo exato quando necessário.
- Implementar testes automatizados (E2E) cobrindo os cenários principais: sucesso, campos obrigatórios ausentes, datas inválidas, URL inválida e validação de vagas.

