#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TAMANHO_TABELA_HASH 100

// Definição da estrutura do Produto
typedef struct {
    char nome[50];
    float preco;
    int quantidade;
} Produto;

// Definição da estrutura do Node da Pilha
typedef struct Node {
    Produto produto;
    struct Node* next;
} Node;

// Definição da estrutura da Pilha
typedef struct {
    Node* topo;
} Pilha;

// Definição da estrutura da Tabela Hash
typedef struct {
    Pilha* tabela[TAMANHO_TABELA_HASH];
} TabelaHash;

// Função para criar um novo Node
Node* criar_node(Produto produto) {
    Node* novo_node = (Node*)malloc(sizeof(Node));
    novo_node->produto = produto;
    novo_node->next = NULL;
    return novo_node;
}

// Função para inicializar a Pilha
void inicializar_pilha(Pilha* pilha) {
    pilha->topo = NULL;
}

// Função para empilhar um produto na Pilha
void empilhar(Pilha* pilha, Produto produto) {
    Node* novo_node = criar_node(produto);
    novo_node->next = pilha->topo;
    pilha->topo = novo_node;
}

// Função para desempilhar um produto da Pilha
Produto desempilhar(Pilha* pilha) {
    if (pilha->topo == NULL) {
        printf("Erro: Pilha vazia\n");
        exit(EXIT_FAILURE);
    }
    Node* node_removido = pilha->topo;
    Produto produto = node_removido->produto;
    pilha->topo = pilha->topo->next;
    free(node_removido);
    return produto;
}

// Função para inicializar a Tabela Hash
void inicializar_tabela_hash(TabelaHash* tabela_hash) {
    for (int i = 0; i < TAMANHO_TABELA_HASH; i++) {
        tabela_hash->tabela[i] = NULL;
    }
}

// Função para calcular o hash de uma chave (nome do produto)
int calcular_hash(char* chave) {
    int hash = 0;
    for (int i = 0; chave[i] != '\0'; i++) {
        hash = (hash * 31 + chave[i]) % TAMANHO_TABELA_HASH;
    }
    return hash;
}

// Função para adicionar um produto na Tabela Hash
void adicionar_produto(TabelaHash* tabela_hash, Produto produto) {
    int indice = calcular_hash(produto.nome);
    if (tabela_hash->tabela[indice] == NULL) {
        tabela_hash->tabela[indice] = (Pilha*)malloc(sizeof(Pilha));
        inicializar_pilha(tabela_hash->tabela[indice]);
    }
    empilhar(tabela_hash->tabela[indice], produto);
}

// Função para encontrar um produto na Tabela Hash
Produto encontrar_produto(TabelaHash* tabela_hash, char* nome) {
    int indice = calcular_hash(nome);
    if (tabela_hash->tabela[indice] == NULL) {
        printf("Produto nao encontrado\n");
        exit(EXIT_FAILURE);
    }
    Pilha* pilha = tabela_hash->tabela[indice];
    Node* node_atual = pilha->topo;
    while (node_atual != NULL) {
        if (strcmp(node_atual->produto.nome, nome) == 0) {
            return node_atual->produto;
        }
        node_atual = node_atual->next;
    }
    printf("Produto nao encontrado\n");
    exit(EXIT_FAILURE);
}

// Função para exibir o menu de opções
void exibir_menu() {
    printf("\n===== Menu =====\n");
    printf("1. Adicionar produto\n");
    printf("2. Remover produto\n");
    printf("3. Visualizar produto\n");
    printf("4. Sair\n");
    printf("Escolha uma opcao: ");
}

// Função para remover um produto da Tabela Hash
void remover_produto(TabelaHash* tabela_hash, char* nome) {
    int indice = calcular_hash(nome);
    if (tabela_hash->tabela[indice] == NULL) {
        printf("Produto nao encontrado\n");
        return;
    }
    Pilha* pilha = tabela_hash->tabela[indice];
    Node* node_atual = pilha->topo;
    Node* node_anterior = NULL;

    while (node_atual != NULL) {
        if (strcmp(node_atual->produto.nome, nome) == 0) {
            if (node_anterior == NULL) {
                // O produto a ser removido está no topo da pilha
                pilha->topo = node_atual->next;
            } else {
                node_anterior->next = node_atual->next;
            }
            free(node_atual);
            printf("Produto removido com sucesso\n");
            return;
        }
        node_anterior = node_atual;
        node_atual = node_atual->next;
    }

    printf("Produto nao encontrado\n");
}

// Função principal
int main() {
    TabelaHash tabela_hash;
    inicializar_tabela_hash(&tabela_hash);

    int opcao;
    while (1) {
        exibir_menu();
        scanf("%d", &opcao);

        switch (opcao) {
            case 1: {
                Produto novo_produto;
                printf("Nome do produto: ");
                scanf("%s", novo_produto.nome);
                printf("Preco do produto: ");
                scanf("%f", &novo_produto.preco);
                printf("Quantidade em estoque: ");
                scanf("%d", &novo_produto.quantidade);
                adicionar_produto(&tabela_hash, novo_produto);
                printf("Produto adicionado com sucesso!\n");
                break;
            }
            case 2: {
              char nome[50];
              printf("Nome do produto a ser removido: ");
              scanf("%s", nome);
              remover_produto(&tabela_hash, nome);
              break;
          }
            case 3: {
                char nome[50];
                printf("Nome do produto a ser visualizado: ");
                scanf("%s", nome);
                Produto produto_encontrado = encontrar_produto(&tabela_hash, nome);
                printf("Nome: %s, Preco: %.2f, Quantidade em estoque: %d\n", produto_encontrado.nome, produto_encontrado.preco, produto_encontrado.quantidade);
                break;
            }
            case 4:
                printf("Saindo...\n");
                exit(EXIT_SUCCESS);
            default:
                printf("Opcao invalida\n");
        }
    }

    return 0;
}
