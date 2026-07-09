#include <iostream>
#include <string>
#include <vector>
using namespace std;


// ==========================================================
// Hierarquia de Astronauta (Camada 1 - herança e polimorfismo)
// ==========================================================

class Astronauta {
public:
    string cpf;
    string nome;
    int idade;
    bool vivo;
    bool disponivel;
    vector<int> voosParticipados; // códigos dos voos lançados

    Astronauta(string cpf, int idade, string nome)
        : cpf(cpf), nome(nome), idade(idade), vivo(true), disponivel(true) {}

    virtual string funcao() { return "Astronauta"; }

    // destrutor virtual: boa prática ao usar herança com ponteiros,
    // mesmo sem precisarmos dar delete nesta atividade.
    virtual ~Astronauta() {}
};

class Comandante : public Astronauta {
public:
    Comandante(string cpf, int idade, string nome) : Astronauta(cpf, idade, nome) {}
    string funcao() override { return "Comandante"; }
};

class Engenheiro : public Astronauta {
public:
    Engenheiro(string cpf, int idade, string nome) : Astronauta(cpf, idade, nome) {}
    string funcao() override { return "Engenheiro"; }
};

class Cientista : public Astronauta {
public:
    Cientista(string cpf, int idade, string nome) : Astronauta(cpf, idade, nome) {}
    string funcao() override { return "Cientista"; }
};

class Turista : public Astronauta {
public:
    Turista(string cpf, int idade, string nome) : Astronauta(cpf, idade, nome) {}
    string funcao() override { return "Turista"; }
};

// Fábrica simples: cria o tipo certo de astronauta a partir da palavra lida no comando.
// Retorna nullptr se a função informada não for reconhecida.
Astronauta* criarAstronauta(const string& funcao, const string& cpf, int idade, const string& nome) {
    if (funcao == "COMANDANTE") return new Comandante(cpf, idade, nome);
    if (funcao == "ENGENHEIRO") return new Engenheiro(cpf, idade, nome);
    if (funcao == "CIENTISTA")  return new Cientista(cpf, idade, nome);
    if (funcao == "TURISTA")    return new Turista(cpf, idade, nome);
    return nullptr;
}


// ==========================================================
// Nave (Camada 2)
// ==========================================================

struct Nave {
    int codigo;
    string nome;
    int capacidade;

    Nave(int codigo, int capacidade, string nome)
        : codigo(codigo), nome(nome), capacidade(capacidade) {}
};


// ==========================================================
// Voo
// ==========================================================

// Estados: 0=planejado, 1=em curso, 2=sucesso, 3=explosão
struct Voo {
    int codigo;
    vector<string> cpfsPassageiros;
    int estado; // 0,1,2,3
    int codigoNave; // -1 quando o voo ainda não tem nave associada

    Voo(int codigo) : codigo(codigo), estado(0), codigoNave(-1) {}

    string nomeEstado() const {
        if (estado == 0) return "planejado";
        if (estado == 1) return "em curso";
        if (estado == 2) return "finalizado com sucesso";
        return "finalizado com explosão";
    }
};


// ==========================================================
// Dados globais
// ==========================================================

vector<Astronauta*> astronautas;
vector<Voo> voos;
vector<Nave> naves;


// ==========================================================
// Funções de busca
// ==========================================================

int buscarAstronauta(const string& cpf) {
    for (int i = 0; i < (int)astronautas.size(); i++)
        if (astronautas[i]->cpf == cpf) return i;
    return -1;
}

int buscarVoo(int codigo) {
    for (int i = 0; i < (int)voos.size(); i++)
        if (voos[i].codigo == codigo) return i;
    return -1;
}

int buscarNave(int codigo) {
    for (int i = 0; i < (int)naves.size(); i++)
        if (naves[i].codigo == codigo) return i;
    return -1;
}

bool astronautaNoVoo(const Voo& v, const string& cpf) {
    for (int i = 0; i < (int)v.cpfsPassageiros.size(); i++)
        if (v.cpfsPassageiros[i] == cpf) return true;
    return false;
}


// ==========================================================
// Operações
// ==========================================================

void cadastrarAstronauta(const string& funcao, const string& cpf, int idade, const string& nome) {
    if (buscarAstronauta(cpf) != -1) {
        cout << "Erro: já existe um astronauta com o CPF " << cpf << "." << endl;
        return;
    }
    Astronauta* a = criarAstronauta(funcao, cpf, idade, nome);
    if (a == nullptr) {
        cout << "Erro: função \"" << funcao << "\" inválida. Use COMANDANTE, ENGENHEIRO, CIENTISTA ou TURISTA." << endl;
        return;
    }
    astronautas.push_back(a);
    cout << "Astronauta " << nome << " (" << a->funcao() << ") cadastrado com sucesso." << endl;
}

void cadastrarVoo(int codigo) {
    if (buscarVoo(codigo) != -1) {
        cout << "Erro: já existe um voo com o código " << codigo << "." << endl;
        return;
    }
    voos.push_back(Voo(codigo));
    cout << "Voo " << codigo << " cadastrado com sucesso." << endl;
}

void cadastrarNave(int codigo, int capacidade, const string& nome) {
    if (buscarNave(codigo) != -1) {
        cout << "Erro: já existe uma nave com o código " << codigo << "." << endl;
        return;
    }
    if (capacidade <= 0) {
        cout << "Erro: a capacidade da nave deve ser maior que zero." << endl;
        return;
    }
    naves.push_back(Nave(codigo, capacidade, nome));
    cout << "Nave " << nome << " cadastrada com sucesso." << endl;
}

void associarNave(int codigoVoo, int codigoNave) {
    int iv = buscarVoo(codigoVoo);
    if (iv == -1) {
        cout << "Erro: voo " << codigoVoo << " não encontrado." << endl;
        return;
    }
    int in = buscarNave(codigoNave);
    if (in == -1) {
        cout << "Erro: nave " << codigoNave << " não encontrada." << endl;
        return;
    }
    Voo& v = voos[iv];
    if (v.estado != 0) {
        cout << "Erro: o voo " << codigoVoo << " não está planejado." << endl;
        return;
    }
    v.codigoNave = codigoNave;
    cout << "Nave " << naves[in].nome << " associada ao voo " << codigoVoo << "." << endl;
}

void adicionarAstronauta(const string& cpf, int codigoVoo) {
    int ia = buscarAstronauta(cpf);
    if (ia == -1) {
        cout << "Erro: astronauta com CPF " << cpf << " não encontrado." << endl;
        return;
    }
    int iv = buscarVoo(codigoVoo);
    if (iv == -1) {
        cout << "Erro: voo " << codigoVoo << " não encontrado." << endl;
        return;
    }
    Voo& v = voos[iv];
    if (v.estado != 0) {
        cout << "Erro: o voo " << codigoVoo << " não está planejado." << endl;
        return;
    }
    Astronauta* a = astronautas[ia];
    if (!a->vivo) {
        cout << "Erro: o astronauta " << a->nome << " está morto." << endl;
        return;
    }
    if (astronautaNoVoo(v, cpf)) {
        cout << "Erro: o astronauta " << a->nome << " já está no voo " << codigoVoo << "." << endl;
        return;
    }
    // Regra nova (Camada 2): respeitar a capacidade da nave, se houver uma associada.
    if (v.codigoNave != -1) {
        int in = buscarNave(v.codigoNave);
        if (in != -1 && (int)v.cpfsPassageiros.size() >= naves[in].capacidade) {
            cout << "Erro: a nave " << naves[in].nome << " já atingiu sua capacidade máxima ("
                 << naves[in].capacidade << ") no voo " << codigoVoo << "." << endl;
            return;
        }
    }
    v.cpfsPassageiros.push_back(cpf);
    cout << "Astronauta " << a->nome << " adicionado ao voo " << codigoVoo << "." << endl;
}

void removerAstronauta(const string& cpf, int codigoVoo) {
    int ia = buscarAstronauta(cpf);
    if (ia == -1) {
        cout << "Erro: astronauta com CPF " << cpf << " não encontrado." << endl;
        return;
    }
    int iv = buscarVoo(codigoVoo);
    if (iv == -1) {
        cout << "Erro: voo " << codigoVoo << " não encontrado." << endl;
        return;
    }
    Voo& v = voos[iv];
    if (v.estado != 0) {
        cout << "Erro: o voo " << codigoVoo << " não está planejado." << endl;
        return;
    }
    for (int i = 0; i < (int)v.cpfsPassageiros.size(); i++) {
        if (v.cpfsPassageiros[i] == cpf) {
            v.cpfsPassageiros.erase(v.cpfsPassageiros.begin() + i);
            cout << "Astronauta " << astronautas[ia]->nome
                 << " removido do voo " << codigoVoo << "." << endl;
            return;
        }
    }
    cout << "Erro: o astronauta " << astronautas[ia]->nome
         << " não está no voo " << codigoVoo << "." << endl;
}

void lancarVoo(int codigoVoo) {
    int iv = buscarVoo(codigoVoo);
    if (iv == -1) {
        cout << "Erro: voo " << codigoVoo << " não encontrado." << endl;
        return;
    }
    Voo& v = voos[iv];
    if (v.estado != 0) {
        cout << "Erro: o voo " << codigoVoo << " não está planejado." << endl;
        return;
    }
    if (v.cpfsPassageiros.empty()) {
        cout << "Erro: o voo " << codigoVoo << " não tem astronautas." << endl;
        return;
    }
    // Regra nova (Camada 2): precisa ter uma nave associada.
    if (v.codigoNave == -1) {
        cout << "Erro: o voo " << codigoVoo << " não tem nave associada." << endl;
        return;
    }

    bool temComandante = false;
    bool temEngenheiro = false;

    for (int i = 0; i < (int)v.cpfsPassageiros.size(); i++) {
        int ia = buscarAstronauta(v.cpfsPassageiros[i]);
        Astronauta* a = astronautas[ia];
        if (!a->vivo) {
            cout << "Erro: o astronauta " << a->nome << " está morto." << endl;
            return;
        }
        if (!a->disponivel) {
            cout << "Erro: o astronauta " << a->nome << " está indisponível." << endl;
            return;
        }
        if (a->funcao() == "Comandante") temComandante = true;
        if (a->funcao() == "Engenheiro") temEngenheiro = true;
    }

    // Regra nova (Camada 1): precisa de ao menos 1 Comandante e 1 Engenheiro.
    if (!temComandante || !temEngenheiro) {
        cout << "Erro: o voo " << codigoVoo
             << " precisa de pelo menos um Comandante e um Engenheiro na tripulação." << endl;
        return;
    }

    v.estado = 1;
    for (int i = 0; i < (int)v.cpfsPassageiros.size(); i++) {
        int ia = buscarAstronauta(v.cpfsPassageiros[i]);
        astronautas[ia]->disponivel = false;
        astronautas[ia]->voosParticipados.push_back(codigoVoo);
    }
    cout << "Voo " << codigoVoo << " lançado com sucesso." << endl;
}

void explodirVoo(int codigoVoo) {
    int iv = buscarVoo(codigoVoo);
    if (iv == -1) {
        cout << "Erro: voo " << codigoVoo << " não encontrado." << endl;
        return;
    }
    Voo& v = voos[iv];
    if (v.estado != 1) {
        cout << "Erro: apenas voos em curso podem explodir." << endl;
        return;
    }
    v.estado = 3;
    for (int i = 0; i < (int)v.cpfsPassageiros.size(); i++) {
        int ia = buscarAstronauta(v.cpfsPassageiros[i]);
        astronautas[ia]->vivo = false;
        astronautas[ia]->disponivel = false;
    }
    cout << "Voo " << codigoVoo << " explodiu. Todos os astronautas morreram." << endl;
}

void finalizarVoo(int codigoVoo) {
    int iv = buscarVoo(codigoVoo);
    if (iv == -1) {
        cout << "Erro: voo " << codigoVoo << " não encontrado." << endl;
        return;
    }
    Voo& v = voos[iv];
    if (v.estado != 1) {
        cout << "Erro: apenas voos em curso podem ser finalizados com sucesso." << endl;
        return;
    }
    v.estado = 2;
    for (int i = 0; i < (int)v.cpfsPassageiros.size(); i++) {
        int ia = buscarAstronauta(v.cpfsPassageiros[i]);
        astronautas[ia]->disponivel = true;
    }
    cout << "Voo " << codigoVoo << " finalizado com sucesso." << endl;
}

void listarVoos() {

    int ordem[] = {0, 1, 2, 3};
    string titulos[] = {"Planejados", "Em curso", "Finalizados com sucesso", "Finalizados com explosão"};

    for (int g = 0; g < 4; g++) {
        cout << "--- " << titulos[g] << " ---" << endl;
        bool algum = false;
        for (int i = 0; i < (int)voos.size(); i++) {
            if (voos[i].estado != ordem[g]) continue;
            algum = true;
            cout << "  Voo " << voos[i].codigo
                 << " [" << voos[i].nomeEstado() << "]";
            if (voos[i].codigoNave != -1) {
                int in = buscarNave(voos[i].codigoNave);
                if (in != -1) {
                    cout << " | Nave: " << naves[in].nome
                         << " (capacidade " << naves[in].capacidade << ")";
                }
            } else {
                cout << " | Nave: (nenhuma)";
            }
            cout << endl;
            if (voos[i].cpfsPassageiros.empty()) {
                cout << "    (sem astronautas)" << endl;
            } else {
                for (int j = 0; j < (int)voos[i].cpfsPassageiros.size(); j++) {
                    string cpf = voos[i].cpfsPassageiros[j];
                    int ia = buscarAstronauta(cpf);
                    cout << "    CPF: " << cpf;
                    if (ia != -1) {
                        cout << " | Nome: " << astronautas[ia]->nome
                             << " | Função: " << astronautas[ia]->funcao();
                    }
                    cout << endl;
                }
            }
        }
        if (!algum) cout << "  (nenhum)" << endl;
    }
}

void listarMortos() {
    cout << "--- Astronautas mortos ---" << endl;
    bool algum = false;
    for (int i = 0; i < (int)astronautas.size(); i++) {
        if (!astronautas[i]->vivo) {
            algum = true;
            cout << "  CPF: " << astronautas[i]->cpf
                 << " | Nome: " << astronautas[i]->nome
                 << " | Função: " << astronautas[i]->funcao() << endl;
            cout << "  Voos participados: ";
            if (astronautas[i]->voosParticipados.empty()) {
                cout << "(nenhum)";
            } else {
                for (int j = 0; j < (int)astronautas[i]->voosParticipados.size(); j++) {
                    if (j > 0) cout << ", ";
                    cout << astronautas[i]->voosParticipados[j];
                }
            }
            cout << endl;
        }
    }
    if (!algum) cout << "  (nenhum)" << endl;
}


// ==========================================================
// Main
// ==========================================================

int main() {
    string comando;
    while (cin >> comando) {
        if (comando == "FIM") {
            break;
        } else if (comando == "CADASTRAR_ASTRONAUTA") {
            string funcao, cpf, nome;
            int idade;
            cin >> funcao >> cpf >> idade;
            getline(cin >> ws, nome);
            cadastrarAstronauta(funcao, cpf, idade, nome);
        } else if (comando == "CADASTRAR_VOO") {
            int codigo;
            cin >> codigo;
            cadastrarVoo(codigo);
        } else if (comando == "CADASTRAR_NAVE") {
            int codigo, capacidade;
            string nome;
            cin >> codigo >> capacidade;
            getline(cin >> ws, nome);
            cadastrarNave(codigo, capacidade, nome);
        } else if (comando == "ASSOCIAR_NAVE") {
            int codigoVoo, codigoNave;
            cin >> codigoVoo >> codigoNave;
            associarNave(codigoVoo, codigoNave);
        } else if (comando == "ADICIONAR_ASTRONAUTA") {
            string cpf;
            int codigo;
            cin >> cpf >> codigo;
            adicionarAstronauta(cpf, codigo);
        } else if (comando == "REMOVER_ASTRONAUTA") {
            string cpf;
            int codigo;
            cin >> cpf >> codigo;
            removerAstronauta(cpf, codigo);
        } else if (comando == "LANCAR_VOO") {
            int codigo;
            cin >> codigo;
            lancarVoo(codigo);
        } else if (comando == "EXPLODIR_VOO") {
            int codigo;
            cin >> codigo;
            explodirVoo(codigo);
        } else if (comando == "FINALIZAR_VOO") {
            int codigo;
            cin >> codigo;
            finalizarVoo(codigo);
        } else if (comando == "LISTAR_VOOS") {
            listarVoos();
        } else if (comando == "LISTAR_MORTOS") {
            listarMortos();
        } else {
            cout << "Comando desconhecido: " << comando << endl;
        }
    }
    return 0;
}
