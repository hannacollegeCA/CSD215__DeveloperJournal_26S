# NOTES CLASS

**1. Estrutura do projeto para testes**
- O projeto tem duas pastas principais:  
  `main` (código da aplicação) e `test` (código dos testes).  
- A estrutura de pacotes é idêntica nas duas pastas.  
- O pacote **core** não precisa de testes porque contém apenas **data types*** (sem lógica).

**2. Testes da UI**
Cada view é composta por:
  - um **ViewModel** (dados + callbacks)  
  - uma função **createScene()**, que é um **cálculo puro**  
Como createScene é uma função pura, ela é fácil de testar:
  > você passa um ViewModel e verifica se o Node retornado tem o que deveria ter.  
- A UI **não é desenhada na tela** durante os testes; apenas o Node é criado.  
- JavaFX precisa ser inicializado antes dos testes (feito no starter code com `initFX()`).

**3. Testes do Repository**
- O repository é uma **abstração** que isola o código da aplicação do banco de dados.  
- Ele define operações como `create`, `update`, `delete`, `all`, etc.  
- Os testes verificam se o repository realmente salva e recupera dados corretamente.  
- Os testes usam um banco SQLite temporário (cópia do arquivo original).

**4. Testes do Controller**
O controller coordena:
  - navegação  
  - carregamento de dados  
  - chamadas ao repository  
  - troca de cenas  
- Para testar controllers, usamos:
  - **mock do repository** (Mockito)  
  - **stub do MainWindow** (feito pelo aluno)  
- O objetivo é verificar **se o controller chama os métodos certos na ordem certa**.  
- Não é necessário inicializar a aplicação inteira para testar o controller.

**5. Por que o stub é necessário**
O controller chama métodos como:
  - `setTitle(...)`
  - `setMainScene(...)`
  - `showError(...)`
- Durante os testes, não queremos abrir uma janela real.  
- Por isso criamos um **MainWindowStub** que captura essas chamadas.

**6. O que o professor espera no Lab**
- Não é necessário escrever testes exaustivos.  
- O objetivo é **explorar**:
  - testes de UI  
  - testes de repository  
  - testes de controller  
- Os TODOs no código indicam exatamente o que deve ser implementado.  
- Os testes de Product servem como **modelo** para os testes de Category.

------------

# What should I do?

> Completar os dois testes unitarios para a Categoria UI 

i. newCategorySaveButton_sendsUserInputToCallback()
    Criar um CategoryNewView.ViewModel com
    dados falsos (Unvalidated)
    um callback falso (fake)
    Simular um click no botao SAVE
    Verificar se o callback recebeu os dados nao validados

OBJETIVO: garantir que a UI envia os dados do usuário para o controller.

ii. categoriesView_tableContainsAllCategoriesFromViewModel()
    Criar um CategoriesView.ViewModel com uma lista falsa de categorias
    Criar uma cena com CategoriesView.createScene(viewModel)
    Verificar que a tabela contem exatamente os itens da ViewModel

OBJETIVO: garantir que a UI mostra os dados corretamente.

> Completar o Teste de CategoriaRepositoryIntegrationTest 

i. BeforeEach (criar copia do banco)
    CCopiar o arquivo northwind.db para um arquivo temporário
    Abrir o repositorio usando a copia 
    Garantir q cada teste use um banco limpo

ii. createUpdateDelete_roundTripPersistsExpectedValues
    Criar uma categoria no banco
    Atualizar 
    Deletar
    Verificar se tudo ocorreu corretaente

OBJETIVO: garantir que o repositório funciona com SQLite.


> Completar o teste de CategoriaControladorTeest:

i. BeforeEach criar mock MainWindow
    Criar variaveis de instancia 
    Criar um metodo BeforeEach para testar os Mocks

ii. showCategories_setsTitle
    Chamar controller.showCategories()
    Verificar se o metodo create foi chamado (MainWindow)

OBJETIVO: garantir que o controller atualiza o titulo da janela

## Catefory COntroller Test
 Estava faltando a variavel javafx.scene.Node
 lastTitle: guarda o ultimo titulo definido pelo controller 
 lastScene: guarda o ultimo componente visual
 lastError: Registra qualquer execao eviada para ShowError
 ----------------------------------------------------------------------------------



