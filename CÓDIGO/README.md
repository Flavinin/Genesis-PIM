
#include <stdio.h>
#include <string.h>
#include <time.h>

#define MAX 100

typedef struct {
    int matricula;
    char nome[50];
} Funcionario;

typedef struct {
    int matricula;
    char tipo[30];
    char dataHora[30];
} Registro;

Funcionario funcionarios[MAX];
Registro registros[MAX];

int totalFuncionarios = 0;
int totalRegistros = 0;

void salvarFuncionarios() {
    FILE *arquivo = fopen("funcionarios.txt", "w");

    if (arquivo == NULL) {
        printf("Erro ao salvar funcionarios!\n");
        return;
    }

    for (int i = 0; i < totalFuncionarios; i++) {
        fprintf(arquivo, "%d;%s\n",
                funcionarios[i].matricula,
                funcionarios[i].nome);
    }

    fclose(arquivo);
}

void salvarRegistros() {
    FILE *arquivo = fopen("registros.txt", "w");

    if (arquivo == NULL) {
        printf("Erro ao salvar registros!\n");
        return;
    }

    for (int i = 0; i < totalRegistros; i++) {
        fprintf(arquivo, "%d;%s;%s\n",
                registros[i].matricula,
                registros[i].tipo,
                registros[i].dataHora);
    }

    fclose(arquivo);
}

void carregarDados() {
    FILE *arquivo;
    char linha[150];

    arquivo = fopen("funcionarios.txt", "r");

    if (arquivo != NULL) {
        while (fgets(linha, sizeof(linha), arquivo)
               != NULL && totalFuncionarios < MAX) {
            Funcionario f;

            if (sscanf(linha, "%d;%49[^\n]",
                       &f.matricula, f.nome) == 2) {
                funcionarios[totalFuncionarios++] = f;
            }
        }

        fclose(arquivo);
    }

    arquivo = fopen("registros.txt", "r");

    if (arquivo != NULL) {
        while (fgets(linha, sizeof(linha), arquivo)
               != NULL && totalRegistros < MAX) {
            Registro r;

            if (sscanf(linha, "%d;%29[^;];%29[^\n]",
                       &r.matricula, r.tipo,
                       r.dataHora) == 3) {
                registros[totalRegistros++] = r;
            }
        }

        fclose(arquivo);
    }
}

int buscarFuncionario(int matricula) {
    for (int i = 0; i < totalFuncionarios; i++) {
        if (funcionarios[i].matricula == matricula) {
            return i;
        }
    }

    return -1;
}

void cadastrarFuncionario() {
    if (totalFuncionarios >= MAX) {
        printf("Limite de funcionarios atingido!\n");
        return;
    }

    Funcionario f;

    printf("Matricula: ");
    if (scanf("%d", &f.matricula) != 1) {
        printf("Matricula invalida!\n");
        while (getchar() != '\n');
        return;
    }

    if (buscarFuncionario(f.matricula) != -1) {
        printf("Essa matricula ja existe!\n");
        return;
    }

    printf("Nome: ");
    scanf(" %49[^\n]", f.nome);

    funcionarios[totalFuncionarios++] = f;
    salvarFuncionarios();

    printf("Funcionario cadastrado!\n");
}

void listarFuncionarios() {
    printf("\n--- FUNCIONARIOS ---\n");

    for (int i = 0; i < totalFuncionarios; i++) {
        printf("Matricula: %d | Nome: %s\n",
               funcionarios[i].matricula,
               funcionarios[i].nome);
    }

    if (totalFuncionarios == 0) {
        printf("Nenhum funcionario cadastrado.\n");
    }
}

void registrarPonto() {
    if (totalRegistros >= MAX) {
        printf("Limite de registros atingido!\n");
        return;
    }

    int matricula, opcao;
    char tipos[4][30] = {
        "Entrada",
        "Saida intervalo",
        "Retorno intervalo",
        "Saida final"
    };

    printf("Matricula do funcionario: ");
    if (scanf("%d", &matricula) != 1) {
        printf("Matricula invalida!\n");
        while (getchar() != '\n');
        return;
    }

    if (buscarFuncionario(matricula) == -1) {
        printf("Funcionario nao encontrado!\n");
        return;
    }

    printf("\n1 - Entrada\n");
    printf("2 - Saida para intervalo\n");
    printf("3 - Retorno do intervalo\n");
    printf("4 - Saida final\n");
    printf("Escolha: ");

    if (scanf("%d", &opcao) != 1) {
        printf("Opcao invalida!\n");
        while (getchar() != '\n');
        return;
    }

    if (opcao < 1 || opcao > 4) {
        printf("Opcao invalida!\n");
        return;
    }

    Registro r;
    r.matricula = matricula;
    strcpy(r.tipo, tipos[opcao - 1]);

    time_t agora = time(NULL);
    struct tm *data = localtime(&agora);

    strftime(r.dataHora, sizeof(r.dataHora),
             "%d/%m/%Y %H:%M:%S", data);

    registros[totalRegistros++] = r;
    salvarRegistros();

    printf("Ponto registrado com sucesso!\n");
    printf("Data e hora: %s\n", r.dataHora);
}

void consultarRegistros() {
    int matricula, encontrou = 0;

    printf("Matricula para consulta: ");
    if (scanf("%d", &matricula) != 1) {
        printf("Matricula invalida!\n");
        while (getchar() != '\n');
        return;
    }

    printf("\n--- HISTORICO DE PONTO ---\n");

    for (int i = 0; i < totalRegistros; i++) {
        if (registros[i].matricula == matricula) {
            printf("%s - %s\n",
                   registros[i].tipo,
                   registros[i].dataHora);
            encontrou = 1;
        }
    }

    if (!encontrou) {
        printf("Nenhum registro encontrado.\n");
    }
}

int main() {
    int opcao;

    carregarDados();

    do {
        printf("\n===== GENESIS PONTO =====\n");
        printf("1 - Cadastrar funcionario\n");
        printf("2 - Listar funcionarios\n");
        printf("3 - Registrar ponto\n");
        printf("4 - Consultar historico\n");
        printf("0 - Sair\n");
        printf("Escolha uma opcao: ");

        if (scanf("%d", &opcao) != 1) {
            printf("Opcao invalida!\n");
            while (getchar() != '\n');
            continue;
        }

        switch (opcao) {
            case 1:
                cadastrarFuncionario();
                break;
            case 2:
                listarFuncionarios();
                break;
            case 3:
                registrarPonto();
                break;
            case 4:
                consultarRegistros();
                break;
            case 0:
                printf("Encerrando o Genesis Ponto...\n");
                break;
            default:
                printf("Opcao invalida!\n");
        }

    } while (opcao != 0);

    return 0;
}

