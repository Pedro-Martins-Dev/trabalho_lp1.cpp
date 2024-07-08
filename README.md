#include <iostream>
#include <string>
#include <vector>

using namespace std;

// Enumeração para o estado do voo
enum EstadoVoo
{
    emPlanejamento, LANCADO, FINALIZADO, EXPLODIDO
};

// Classe para representar um Astronauta
class Astronauta
{
private:
    string cpf;
    string nome;
    int idade;

public:
    string getCpf() const
    {
        return cpf;
    }

    string getNome() const
    {
        return nome;
    }

    int getIdade() const
    {
        return idade;
    }

    void setAstronauta(const string& cpf, const string& nome, int idade)
    {
        this->cpf = cpf;
        this->nome = nome;
        this->idade = idade;
    }

    void exibirAstronauta() const
    {
        cout << "CPF: " << cpf << ", Nome: " << nome << ", Idade: " << idade << endl;
    }
};

// Classe para representar um Voo
class Voo
{
private:
    int codigoVoo;
    vector<Astronauta> astronautas;
    EstadoVoo estado;
    bool finalizadoComSucesso;

public:
    Voo(int codigo) : codigoVoo(codigo), estado(emPlanejamento), finalizadoComSucesso(false) {}

    int getCodigoVoo() const
    {
        return codigoVoo;
    }

    EstadoVoo getEstado() const
    {
        return estado;
    }

    bool isFinalizadoComSucesso() const
    {
        return finalizadoComSucesso;
    }

    void adicionarAstronauta(const Astronauta& astronauta)
    {
        if (estado == emPlanejamento)
        {
            astronautas.push_back(astronauta);
            cout << "Astronauta adicionado com sucesso." << endl;
        }
        else
        {
            cout << "Nao e possivel adicionar astronautas. O voo nao esta em planejamento!" << endl;
        }
    }

    void listarAstronautas() const
    {
        for (const auto& astronauta : astronautas)
            astronauta.exibirAstronauta();
    }

    vector<Astronauta> getAstronautas() const
    {
        return astronautas;
    }

    bool removerAstronautaPorCpf(const string& cpf)
    {
        if (estado != emPlanejamento)
        {
            cout << "Nao e possivel remover astronautas. O voo nao esta em planejamento!" << endl;
            return false;
        }

        for (auto it = astronautas.begin(); it != astronautas.end(); ++it)
        {
            if (it->getCpf() == cpf)
            {
                astronautas.erase(it);
                cout << "Astronauta removido com sucesso!" << endl;
                return true; // Astronauta removido com sucesso
            }
        }
        cout << "Astronauta com CPF " << cpf << " nao encontrado no voo!" << endl;
        return false; // Astronauta nao encontrado
    }

    void lancar()
    {
        if (astronautas.empty())
        {
            cout << "Voo nao pode ser lancado sem tripulantes!" << endl;
        }
        else
        {
            estado = LANCADO;
            cout << "Voo " << codigoVoo << " lancado com sucesso!" << endl;
        }
    }

    void finalizar()
    {
        if (estado == LANCADO)
        {
            estado = FINALIZADO;
            finalizadoComSucesso = true;
            cout << "Voo " << codigoVoo << " finalizado com sucesso!" << endl;
        }
        else
        {
            cout << "Voo nao pode ser finalizado, pois nao foi lancado!" << endl;
        }
    }

    void explodir()
    {
        if (estado == LANCADO)
        {
            estado = EXPLODIDO;
            finalizadoComSucesso = false;
            cout << "Voo " << codigoVoo << " explodiu!" << endl;
        }
        else
        {
            cout << "Voo nao pode explodir, pois nao foi lancado!" << endl;
        }
    }
};

// Vetores globais para armazenar astronautas e voos
vector<Astronauta> listaAstronautas;
vector<Voo> listaVoos;

// Função para encontrar um voo pelo código
Voo* encontrarVooPorCodigo(int codigoVoo)
{
    for (auto& voo : listaVoos)
    {
        if (voo.getCodigoVoo() == codigoVoo)
        {
            return &voo;
        }
    }
    return nullptr;
}

// Função para encontrar um astronauta pelo CPF
Astronauta* encontrarAstronautaPorCpf(const string& cpf)
{
    for (auto& astronauta : listaAstronautas)
    {
        if (astronauta.getCpf() == cpf)
        {
            return &astronauta;
        }
    }
    return nullptr;
}

// Função para lançar um voo
void lancarVoo()
{
    int codigoVoo;
    cout << "Informe o codigo do voo: ";
    cin >> codigoVoo;

    Voo* voo = encontrarVooPorCodigo(codigoVoo);
    if (!voo)
    {
        cout << "Voo com codigo " << codigoVoo << " nao encontrado!" << endl;
        return;
    }

    voo->lancar();
}

// Função para finalizar um voo
void finalizarVoo()
{
    int codigoVoo;
    cout << "Informe o codigo do voo: ";
    cin >> codigoVoo;

    Voo* voo = encontrarVooPorCodigo(codigoVoo);
    if (!voo)
    {
        cout << "Voo com codigo " << codigoVoo << " nao encontrado!" << endl;
        return;
    }

    voo->finalizar();
}

// Função para explodir um voo
void explodirVoo()
{
    int codigoVoo;
    cout << "Informe o codigo do voo: ";
    cin >> codigoVoo;

    Voo* voo = encontrarVooPorCodigo(codigoVoo);
    if (!voo)
    {
        cout << "Voo com codigo " << codigoVoo << " nao encontrado!" << endl;
        return;
    }

    voo->explodir();
}

// Função para cadastrar um astronauta
void cadastrarAstronauta()
{
    int idade;
    string nome, cpf;
    cout << "Informe o CPF do astronauta: ";
    cin >> cpf;
    cout << "Informe o nome do astronauta: ";
    cin >> nome;
    cout << "Informe a idade do astronauta: ";
    cin >> idade;

    Astronauta astronauta;
    astronauta.setAstronauta(cpf, nome, idade);
    listaAstronautas.push_back(astronauta);

    cout << "Astronauta cadastrado com sucesso!" << endl;
}

// Função para cadastrar um voo
void cadastrarVoo()
{
    int codigo;
    cout << "Digite o codigo do voo: ";
    cin >> codigo;

    Voo voo(codigo);
    listaVoos.push_back(voo);
    cout << "Voo cadastrado com sucesso!" << endl;
}

// Função para adicionar um astronauta a um voo
void adicionarAstronautaEmVoo()
{
    string cpf;
    int codigoVoo;

    // Solicita o CPF do astronauta
    cout << "Informe o CPF do astronauta: ";
    cin >> cpf;

    // Solicita o codigo do voo
    cout << "Informe o codigo do voo: ";
    cin >> codigoVoo;

    // Encontra o astronauta pelo CPF
    Astronauta* astronauta = encontrarAstronautaPorCpf(cpf);
    if (!astronauta)
    {
        // Se o astronauta nao for encontrado, exibe uma mensagem de erro e retorna
        cout << "Astronauta com CPF " << cpf << " nao encontrado!" << endl;
        return;
    }

    // Encontra o voo pelo codigo
    Voo* voo = encontrarVooPorCodigo(codigoVoo);
    if (!voo)
    {
        // Se o voo nao for encontrado, exibe uma mensagem de erro e retorna
        cout << "Voo com codigo " << codigoVoo << " nao encontrado!" << endl;
        return;
    }

    // Adiciona o astronauta ao voo
    voo->adicionarAstronauta(*astronauta);
}

// Função para remover um astronauta de um voo
void removerAstronautaDeVoo()
{
    string cpf;
    int codigoVoo;

    // Solicita o CPF do astronauta
    cout << "Informe o CPF do astronauta: ";
    cin >> cpf;

    // Solicita o codigo do voo
    cout << "Informe o codigo do voo: ";
    cin >> codigoVoo;

    // Encontra o voo pelo codigo
    Voo* voo = encontrarVooPorCodigo(codigoVoo);
    if (!voo)
    {
        cout << "Voo com codigo " << codigoVoo << " nao encontrado!" << endl;
        return;
    }

    // Remove o astronauta do voo
    bool sucesso = voo->removerAstronautaPorCpf(cpf);
    if (sucesso)
    {
        cout << "Astronauta removido do voo com sucesso!" << endl;
    }
}

// Função para listar todos os voos
void listarVoos()
{
    for (const auto& voo : listaVoos)
    {
        cout << "Voo codigo: " << voo.getCodigoVoo() << ", Estado: " << voo.getEstado() << endl;
    }
}

// Função para listar todos os voos detalhadamente
void listarVoosDetalhados()
{
    cout << "Voos em planejamento:" << endl;
    for (const auto& voo : listaVoos)
    {
        if (voo.getEstado() == emPlanejamento)
        {
            cout << "Voo codigo: " << voo.getCodigoVoo() << endl;
            voo.listarAstronautas();
        }
    }

    cout << "Voos lancados:" << endl;
    for (const auto& voo : listaVoos)
    {
        if (voo.getEstado() == LANCADO)
        {
            cout << "Voo codigo: " << voo.getCodigoVoo() << endl;
            voo.listarAstronautas();
        }
    }

    cout << "Voos finalizados:" << endl;
    for (const auto& voo : listaVoos)
    {
        if (voo.getEstado() == FINALIZADO)
        {
            cout << "Voo codigo: " << voo.getCodigoVoo() << endl;
            cout << "Voo codigo: " << voo.getCodigoVoo() << endl;
            voo.listarAstronautas();
            if (voo.isFinalizadoComSucesso())
                cout << "Finalizado com sucesso." << endl;
            else
                cout << "Finalizado sem sucesso." << endl;
        }
    }

    cout << "Voos explodidos:" << endl;
    for (const auto& voo : listaVoos)
    {
        if (voo.getEstado() == EXPLODIDO)
        {
            cout << "Voo codigo: " << voo.getCodigoVoo() << endl;
            voo.listarAstronautas();
            if (voo.isFinalizadoComSucesso())
                cout << "Finalizado com sucesso." << endl;
            else
                cout << "Finalizado sem sucesso." << endl;
        }
    }
}

// Função para listar todos os astronautas mortos
void listarAstronautasMortos()
{
    cout << "Astronautas mortos:" << endl;
    for (const auto& voo : listaVoos)
    {
        if (voo.getEstado() == EXPLODIDO)
        {
            cout << "Do voo codigo: " << voo.getCodigoVoo() << endl;
            for (const auto& astronauta : voo.getAstronautas())
            {
                cout << "CPF: " << astronauta.getCpf() << ", Nome: " << astronauta.getNome() << ", Idade: " << astronauta.getIdade() << endl;
            }
        }
    }
}

// Função principal
int main()
{
    int opcao;

    do
    {
        cout << "\n-----------------------" << endl;
        cout << "Sistema de Gerenciamento de Voos Espaciais" << endl;
        cout << "-----------------------" << endl;
        cout << "1 - Cadastrar Astronauta" << endl;
        cout << "2 - Cadastrar Voo" << endl;
        cout << "3 - Adicionar Astronauta em Voo" << endl;
        cout << "4 - Remover Astronauta de Voo" << endl;
        cout << "5 - Listar Voos" << endl;
        cout << "6 - Listar Voos Detalhados" << endl;
        cout << "7 - Lançar Voo" << endl;
        cout << "8 - Finalizar Voo" << endl;
        cout << "9 - Explodir Voo" << endl;
        cout << "10 - Listar Astronautas Mortos" << endl;
        cout << "0 - Sair" << endl;
        cout << "Escolha uma opcao: ";
        cin >> opcao;

        switch (opcao)
        {
        case 1:
            cadastrarAstronauta();
            break;
        case 2:
            cadastrarVoo();
            break;
        case 3:
            adicionarAstronautaEmVoo();
            break;
        case 4:
            removerAstronautaDeVoo();
            break;
        case 5:
            listarVoos();
            break;
        case 6:
            listarVoosDetalhados();
            break;
        case 7:
            lancarVoo();
            break;
        case 8:
            finalizarVoo();
            break;
        case 9:
            explodirVoo();
            break;
        case 10:
            listarAstronautasMortos();
            break;
        case 0:
            cout << "Saindo do programa..." << endl;
            break;
        default:
            cout << "Opcao invalida. Tente novamente!" << endl;
            break;
        }

    } while (opcao != 0);

    return 0;
}
