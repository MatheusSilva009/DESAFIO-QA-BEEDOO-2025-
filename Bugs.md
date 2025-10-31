# Bugs da plataforma Beedoo QA Chalenge

## Bug 1
**Resumo do problema:**  
_Foi verificado que o sistema permitiu cadastrar um curso sem informar o nome do curso._

**Cenário de teste:**  
_CT 02 - Falha ao cadastrar sem o nome do curso (campo obrigatório)_ 

**Passos para reproduzir:**  
1. Acessar a página https://creative-sherbet-a51eac.netlify.app/
2. Clicar no botão 'cadastrar curso' 
3. Preencher todos os campos menos o campo: nome do curso
4. Clicar no botão 'cadastrar curso'

**Resultado obtido (técnico):**  
O sistema aceitou a submissão do formulário e criou um registro de curso com o campo 'nome' vazio; o dado foi persistido no backend sem validação de presença no campo obrigatório.

**Resultado esperado (técnico):**  
O frontend ou backend deve validar obrigatoriedade do campo 'nome' e rejeitar a submissão. A API deve retornar erro de validação (ex.: HTTP 400) com mensagem 'Por favor informar o nome do curso' ou o frontend deve exibir validação inline impedindo o envio.

**Impacto:**  
Alto — Permite persistir registros incompletos que comprometem a integridade dos dados, buscas, relatórios e fluxo de inscrição.

**Navegador utilizado:**  
Google Chrome v.141.0.7390.123 (64-bit)

## Bug 2
**Resumo do problema:**  
_Não foi apresentado a mensagem de erro na plataforma de cadastro indicando que a data de início deve ser anterior à data de fim._

**Cenário de teste:**  
_CT 03.2 (BUG) - Preenchimento de datas — variações de sucesso e falha._ 

**Passos para reproduzir:**  
1. Acessar a página https://creative-sherbet-a51eac.netlify.app/
2. Clicar no botão 'cadastrar curso' 
3. Preencher todos os campos
4. Inserir data de início posterior à data de término
5. Clicar no botão 'cadastrar curso'

**Resultado obtido (técnico):**  
O formulário foi aceito e o curso foi criado mesmo com a data de início posterior à data de término; não houve validação de consistência temporal na camada cliente nem no backend.

**Resultado esperado (técnico):**  
Deve haver uma validação de consistência de datas que impeça a submissão quando data de início >= data de término, retornando erro de validação com mensagem 'Data de início deve ser anterior à data de término' e status HTTP 400, ou exibindo erro no frontend antes do envio.

**Impacto:**  
Alto — Gera cursos com períodos inconsistentes que podem causar problemas de agendamento, relatórios incorretos e falha na lógica de inscrição.

**Navegador utilizado:**  
Google Chrome v.141.0.7390.123 (64-bit)

## Bug 3
**Resumo do problema:**  
_Valor inválido sendo permitido cadastro no número de vagas da plataforma._

**Cenário de teste:**  
_CT 04. 4.2 (BUG) - Validação do campo 'Número de vagas' para valores inválidos_ 

**Passos para reproduzir:**  
1. Acessar a página https://creative-sherbet-a51eac.netlify.app/
2. Clicar no botão 'cadastrar curso' 
3. Preencher todos os campos
4. Informar -5 no campo 'Número de vagas'
5. Clicar no botão 'cadastrar curso'

**Resultado obtido (técnico):**  
O sistema aceitou e persistiu um valor negativo para 'número de vagas' (-5) sem validação de tipo/intervalo no cliente ou no servidor.

**Resultado esperado (técnico):**  
Validação numérica deve aceitar apenas inteiros não negativos (>= 0). Submissões com valores negativos devem ser rejeitadas com erro de validação (HTTP 400) e mensagem 'Número de vagas inválido'.

**Impacto:**  
Médio — Pode corromper métricas de capacidade e lógica de inscrição; não causa perda imediata de serviço, mas compromete dados e funcionalidades relacionadas.

**Navegador utilizado:**  
Google Chrome v.141.0.7390.123 (64-bit)

## Bug 4
**Resumo do problema:**  
_Plataforma de cadastro não reconhece que a url da imagem está inválida._

**Cenário de teste:**  
_CT 05 (BUG) - Campo 'Url da imagem de capa' aceita apenas URLs válidas_ 

**Passos para reproduzir:**  
1. Acessar a página https://creative-sherbet-a51eac.netlify.app/
2. Clicar no botão 'cadastrar curso' 
3. Preencher todos os campos
4. Inserir a string 'not-a-url' no campo de URL da imagem
5. Clicar no botão 'cadastrar curso'

**Resultado obtido (técnico):**  
A entrada inválida foi aceita e persistida; não houve verificação de formato de URL (cliente/servidor) e o sistema registrou a URL inválida no banco.

**Resultado esperado (técnico):**  
Deve haver validação de formato de URL (ex.: regex ou parser URL) que impeça a persistência de valores inválidos. O sistema deve rejeitar a submissão com mensagem 'URL inválida' ou substituir por um fallback se aplicável.

**Impacto:**  
Baixo — Afeta principalmente a exibição de imagens (experiência do usuário). Não compromete dados críticos, mas pode gerar erros de renderização em conteúdos que dependem da imagem.

**Navegador utilizado:**  
Google Chrome v.141.0.7390.123 (64-bit)

## Bug 5
**Resumo do problema:**  
_Card do curso não está sendo deletado após clicar na opção excluir curso._

**Cenário de teste:**  
_CT 03  (BUG) - Excluir curso remove o card da lista (fluxo básico)_ 

**Passos para reproduzir:**  
1. Acessar a página https://creative-sherbet-a51eac.netlify.app/
2. Selecionar o curso a ser excluído
3. Clicar em 'excluir curso' e confirmar

**Resultado obtido (técnico):**  
A ação de exclusão não removeu o curso da listagem; o frontend não atualizou a UI corretamente ou a requisição de exclusão não foi processada pelo backend, resultando na permanência do card na interface.

**Resultado esperado (técnico):**  
Ao confirmar exclusão, a API deve retornar status 200/204 e o frontend deve remover o card da lista atualizando o estado local. Deve ser exibido feedback de sucesso ao usuário.

**Impacto:**  
Alto — Falha em operação CRUD compromete a confiabilidade da aplicação, gera inconsistências entre frontend/backend e prejudica a experiência do usuário.

**Navegador utilizado:**
Google Chrome v.141.0.7390.123 (64-bit)
