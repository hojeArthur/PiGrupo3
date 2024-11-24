## Diagrama de Classe
@startuml
actor "Usuário" as Usuario
actor "Universidade" as Universidade
rectangle "Aplicativo de Gestão" {
    usecase "Cadastrar Pessoa" as UC_CadastrarPessoa
    usecase "Cadastrar Fornecedor" as UC_CadastrarFornecedor
    usecase "Cadastrar Pessoa Jurídica" as UC_CadastrarPessoaJuridica
    usecase "Realizar Cadastro de Pessoa Física" as UC_CadastrarPessoaFisica
    usecase "Cadastrar Aluno" as UC_CadastrarAluno
    usecase "Cadastrar Professor" as UC_CadastrarProfessor
    usecase "Validar Campos Obrigatórios" as UC_ValidarCampos
    usecase "Validar Documento" as UC_ValidarDocumento
    usecase "Verificar Duplicidade de Documento" as UC_VerificarDuplicidade
    usecase "Exibir Erro" as UC_ExibirErro
    
    UC_CadastrarPessoa <|-- UC_CadastrarPessoaJuridica
    UC_CadastrarPessoa <|-- UC_CadastrarPessoaFisica
    UC_CadastrarPessoaFisica <|-- UC_CadastrarAluno
    UC_CadastrarPessoaFisica <|-- UC_CadastrarProfessor
    UC_CadastrarPessoaJuridica <|-- UC_CadastrarFornecedor
    
    UC_CadastrarPessoa -- UC_ValidarCampos : <<include>>
    UC_CadastrarPessoa -- UC_ValidarDocumento : <<include>>
    UC_CadastrarPessoa -- UC_VerificarDuplicidade : <<include>>
    UC_CadastrarPessoa ..> UC_ExibirErro : <<extend>>
}

Usuario --> UC_CadastrarPessoa
Universidade --> UC_ExibirErro
@enduml


### 1. Classe Pessoa
- **Atributos:**
  - `nome`: String
  - `cpf`: String
  - `email`: String
  - `dataNascimento`: Date
- **Métodos:**
  - `validarDocumento()`: Boolean
  - `verificarDuplicidadeDocumento()`: Boolean

### 2. Classe Aluno (herda de Pessoa)
- **Atributos:**
  - `numeroMatricula`: String
  - `responsavel`: String (para casos de menores de 18 anos)
- **Métodos:**
  - `realizarCadastro()`: void
  - `verificarMenorDeIdade()`: Boolean

### 3. Classe Professor (herda de Pessoa)
- **Atributos:**
  - `documentoDocencia`: String
  - `rg`: String
- **Métodos:**
  - `validarDocumentoDocencia()`: Boolean
  - `realizarCadastro()`: void

### 4. Classe PessoaJuridica
- **Atributos:**
  - `cnpj`: String
  - `razaoSocial`: String
  - `certidaoNegativa`: Boolean
- **Métodos:**
  - `validarDocumentosReceitaFederal()`: Boolean
  - `vincularPessoaFisica(pessoa: Pessoa)`: void

### 5. Classe Fornecedor (herda de Pessoa)
- **Atributos:**
  - `vinculoEmpresa`: PessoaJuridica
- **Métodos:**
  - `validarVinculoEmpresa()`: Boolean
  - `realizarCadastro()`: void

## Relações
- **Herança:** Aluno, Professor e Fornecedor herdam de Pessoa.
- **Associação:** Fornecedor tem um vínculo com PessoaJuridica.
- **Composição:** PessoaJuridica pode vincular várias Pessoa (representando os funcionários, por exemplo).

### Notas sobre o diagrama:
- Cada entidade reflete as funções de cadastro e validação mostradas nos fluxos do caso de uso.
- Métodos de validação e regras de negócio, como verificar duplicidade de documentos e validar campos obrigatórios, estão representados dentro das classes, reforçando os fluxos do sistema.

---

# DESCRIÇÃO DOS CENÁRIOS DOS CASOS DE USO

## Cenário Principal
### Cadastro de Aluno
**Ator:** Aluno  
**Pré-condições:**  
- Número de matrícula emitido, CPF válido no banco de dados.

**Fluxo normal:**
1. Clicar em realizar cadastro.
2. Inserir informações pessoais.
3. Confirmar e-mail válido.

**Fluxo alternativo:** Aluno menor de 18 anos  
- Apresentar mensagem de que é necessário cadastrar um responsável.

**Pós-condição:**  
- Aluno acessa o sistema com usuário e senha informados no cadastro.

---

## Cenário Alternativo 1
### Cadastro Pessoa Jurídica
**Ator:** Pessoa Jurídica  
**Pré-condições:**  
- Envio de documentos validados pela Receita Federal.

**Fluxo normal:**
1. Clicar em realizar cadastro.
2. Inserir informações da empresa.
3. Confirmar e-mail válido.

**Fluxo alternativo:** Certidão Negativa não consta  
- Apresentar mensagem de que é necessário o envio da Certidão Negativa para finalizar o cadastro.

**Pós-condição:**  
- Acessa o sistema com usuário e senha informados no cadastro e vincula pessoas físicas à empresa.

---

## Cenário Alternativo 2
### Cadastro de Fornecedor
**Ator:** Fornecedor  
**Pré-condições:**  
- CPF vinculado à uma empresa no banco de dados.

**Fluxo normal:**
1. Clicar em realizar cadastro.
2. Inserir informações pessoais.
3. Confirmar vínculo empresarial.
4. Confirmar e-mail válido.

**Fluxo alternativo:** CPF não consta junto à empresa informada  
- Apresentar mensagem de que é necessário conferir vínculo junto à empresa para concluir o cadastro.

**Pós-condição:**  
- Acessa o sistema com usuário e senha informados.

https://github.com/hojeArthur/PiGrupo3/blob/main/prototipo_imagens/professor_4.png?raw=true
