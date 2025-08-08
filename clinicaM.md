#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAX_PACIENTES 100
#define MAX_PRONTUARIOS 100
#define USER "admin"
#define PASS "1234"

struct Paciente
{
    char nome[50];
    char cpf[15];
    int idade;
    char sexo[10];
};

struct Prontuario
{
    char cpf[15];         // CPF do paciente
    char historico[1000]; // Histórico médico
};

struct Paciente pacientes[MAX_PACIENTES];
int totalPacientes = 0;

struct Prontuario prontuarios[MAX_PRONTUARIOS];
int totalProntuarios = 0;

// Função limpar tela
void limparTela()
{
    system("clear");
}

// espera uma tecla para continuar
void esperarTecla()
{
    printf("Aperte qualquer tecla para continuar...\n");
    getchar();
    getchar();
}

// Função para cadastrar um paciente
void cadastrarPaciente()
{
    if (totalPacientes >= MAX_PACIENTES)
    {
        printf("Limite de pacientes atingido.\n");
        return;
    }

    printf("\n--- Cadastro de Paciente ---\n");
    printf("Nome: ");
    scanf(" %[^\n]", pacientes[totalPacientes].nome);
    printf("CPF: ");
    scanf(" %s", pacientes[totalPacientes].cpf);
    printf("Idade: ");
    scanf("%d", &pacientes[totalPacientes].idade);
    printf("Sexo: ");
    scanf(" %s", pacientes[totalPacientes].sexo);

    totalPacientes++;

    printf("\033[0;32m---Cadastro efetuado com sucesso!\033[0m---\n");
    esperarTecla();
    limparTela();
}

// Função para exibir um paciente
void exibirPaciente(struct Paciente p)
{
    printf("\n--- Dados do Paciente ---\n");
    printf("Nome: %s\n", p.nome);
    printf("CPF: %s\n", p.cpf);
    printf("Idade: %d\n", p.idade);
    printf("Sexo: %s\n", p.sexo);
}

// Função para buscar paciente por CPF
void buscarPacientePorCPF()
{
    char cpfBusca[15];
    int encontrado = 0;

    printf("\nDigite o CPF para busca: ");
    scanf(" %s", cpfBusca);

    for (int i = 0; i < totalPacientes; i++)
    {
        if (strcmp(pacientes[i].cpf, cpfBusca) == 0)
        {
            exibirPaciente(pacientes[i]);
            encontrado = 1;
            esperarTecla();
            limparTela();
            break;
        }
    }

    if (!encontrado)
    {
        printf("Paciente com CPF %s não cadastrado.\n", cpfBusca);
        esperarTecla();
        limparTela();
    }
}

// Função para listar todos os pacientes
void listarPacientes()
{
    // char cpfBusca[15];
    int encontrado = 0;
    printf("\n--- Lista de Pacientes ---\n");
    for (int i = 0; i < totalPacientes; i++)
    {
        if (totalPacientes > 0)
        {
            printf("%d. %s (CPF: %s)\n", i + 1, pacientes[i].nome, pacientes[i].cpf);
            encontrado = 1;
        }
    }

    if (!encontrado)
    {
        printf("Nenhum Paciente cadastrado.\n");
        esperarTecla();
        encontrado = 0;
        // limparTela();
    }
}

void criarProntuario()
{
    char cpfBusca[15];
    printf("\nDigite o CPF do paciente para criar o prontuário: ");
    scanf(" %s", cpfBusca);

    // Verifica se o paciente existe
    int pacienteExiste = 0;
    for (int i = 0; i < totalPacientes; i++)
    {
        if (strcmp(pacientes[i].cpf, cpfBusca) == 0)
        {
            pacienteExiste = 1;
            break;
        }
    }

    if (!pacienteExiste)
    {
        printf("Paciente não encontrado.\n\n");
        esperarTecla();
        return;
    }

    // Verifica se já existe prontuário
    for (int i = 0; i < totalProntuarios; i++)
    {
        if (strcmp(prontuarios[i].cpf, cpfBusca) == 0)
        {
            printf("Prontuário já existe para este paciente.\n\n");
            esperarTecla();
            return;
        }
    }

    // Cria prontuário
    strcpy(prontuarios[totalProntuarios].cpf, cpfBusca);
    prontuarios[totalProntuarios].historico[0] = '\0'; // Inicializa vazio
    totalProntuarios++;

    printf("\033[0;34mProntuário criado com sucesso!\033\[0m\n\n");
    esperarTecla();
}

void adicionarInformacaoProntuario()
{
    char cpfBusca[15];
    printf("\nDigite o CPF do paciente: ");
    scanf(" %s", cpfBusca);

    for (int i = 0; i < totalProntuarios; i++)
    {
        if (strcmp(prontuarios[i].cpf, cpfBusca) == 0)
        {
            char novaInfo[500];
            printf("Digite a nova informação médica: ");
            scanf(" %[^\n]", novaInfo);

            strcat(prontuarios[i].historico, "\n");
            strcat(prontuarios[i].historico, novaInfo);

            printf("Informação adicionada com sucesso!\n\n");
            esperarTecla();
            return;
        }
    }

    printf("Prontuário não encontrado.\n\n");
    esperarTecla();
}

void exibirProntuario()
{
    char cpfBusca[15];
    printf("\nDigite o CPF do paciente: ");
    scanf(" %s", cpfBusca);

    for (int i = 0; i < totalProntuarios; i++)
    {
        if (strcmp(prontuarios[i].cpf, cpfBusca) == 0)
        {
            printf("\n--- Prontuário Médico ---\n");
            printf("CPF: %s\n", prontuarios[i].cpf);
            printf("Histórico:%s\n\n", prontuarios[i].historico);
            esperarTecla();
            return;
        }
    }

    printf("Prontuário não encontrado.\n\n");
    esperarTecla();
}

// Menu principal
void mostrarMenu()
{
    int opcao;
    do
    {
        printf("\n=== MENU PRINCIPAL ===\n");
        printf("1. Cadastrar Paciente\n");
        printf("2. Buscar Paciente por CPF\n");
        printf("3. Listar Pacientes\n");
        printf("4. Criar Prontuário\n");
        printf("5. Adicionar Informação ao Prontuário\n");
        printf("6. Exibir Prontuário\n");
        printf("7. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        limparTela();

        switch (opcao)
        {
        case 1:
            cadastrarPaciente();
            break;
        case 2:
            buscarPacientePorCPF();
            break;
        case 3:
            listarPacientes();
            esperarTecla();
            limparTela();
            break;
        case 4:
            criarProntuario();
            limparTela();
            esperarTecla();
            break;
        case 5:
            adicionarInformacaoProntuario();
            limparTela();
            break;
        case 6:
            exibirProntuario();
            limparTela();
            break;
        case 7:
            printf("Encerrando o sistema...\n\n");
            esperarTecla();
            break;
        default:
            printf("Opção inválida. Tente novamente.\n\n");
            esperarTecla();
            limparTela();
        }
    } while (opcao != 7);
}

// Autenticação
int autenticar()
{
    char usuario[20], senha[20];

    printf("====== LOGIN =====\n\n");
    printf("Usuário: ");
    scanf("%s", usuario);
    printf("Senha: ");
    scanf("%s", senha);

    return strcmp(usuario, USER) == 0 && strcmp(senha, PASS) == 0;
}

// Função principal
int main()
{
    if (autenticar())
    {
        printf("\033[0;34mLogin bem-sucedido!\033\[0m\n");
        esperarTecla();
        limparTela();
        mostrarMenu();
    }
    else
    {
        printf("\033[0;31mUsuário ou senha incorretos.\033\[0m\n");
        esperarTecla();
        limparTela();
    }

    return 0;
}
