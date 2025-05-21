using System;
using System.Collections.Generic;

// Interface para comportamentos específicos de celular
public interface ICelularComportamento
{
    void Ligar();
    void Desligar();
    void FazerLigacao(string numero);
    void EnviarMensagem(string numero, string mensagem);
    void TirarFoto();
    void MostrarConfiguracao();
}

// Classe abstrata base para todos os celulares
public abstract class Celular : ICelularComportamento
{
    // Propriedades comuns a todos os celulares
    public string Marca { get; protected set; }
    public string Modelo { get; protected set; }
    public int AnoFabricacao { get; protected set; }
    public string SistemaOperacional { get; protected set; }
    public bool Ligado { get; protected set; }
    public double TamanhoTela { get; protected set; }
    public int MemoriaRAM { get; protected set; } // em GB
    public int Armazenamento { get; protected set; } // em GB

    // Construtor
    public Celular(string marca, string modelo, int anoFabricacao, string sistemaOperacional, 
                   double tamanhoTela, int memoriaRAM, int armazenamento)
    {
        Marca = marca;
        Modelo = modelo;
        AnoFabricacao = anoFabricacao;
        SistemaOperacional = sistemaOperacional;
        TamanhoTela = tamanhoTela;
        MemoriaRAM = memoriaRAM;
        Armazenamento = armazenamento;
        Ligado = false;
    }

    // Métodos abstratos que devem ser implementados pelas classes derivadas
    public abstract void Ligar();
    public abstract void Desligar();
    
    // Métodos virtuais que podem ser sobrescritos
    public virtual void FazerLigacao(string numero)
    {
        if (!Ligado)
        {
            Console.WriteLine("O celular está desligado. Ligue-o primeiro.");
            return;
        }
        Console.WriteLine($"Ligando para {numero}...");
    }

    public virtual void EnviarMensagem(string numero, string mensagem)
    {
        if (!Ligado)
        {
            Console.WriteLine("O celular está desligado. Ligue-o primeiro.");
            return;
        }
        Console.WriteLine($"Enviando mensagem para {numero}: {mensagem}");
    }

    public virtual void TirarFoto()
    {
        if (!Ligado)
        {
            Console.WriteLine("O celular está desligado. Ligue-o primeiro.");
            return;
        }
        Console.WriteLine("Tirando foto...");
    }

    public virtual void MostrarConfiguracao()
    {
        Console.WriteLine($"Marca: {Marca}");
        Console.WriteLine($"Modelo: {Modelo}");
        Console.WriteLine($"Ano: {AnoFabricacao}");
        Console.WriteLine($"SO: {SistemaOperacional}");
        Console.WriteLine($"Tela: {TamanhoTela}\"");
        Console.WriteLine($"RAM: {MemoriaRAM}GB");
        Console.WriteLine($"Armazenamento: {Armazenamento}GB");
        Console.WriteLine($"Estado: {(Ligado ? "Ligado" : "Desligado")}");
    }
}

// Implementação para celulares Samsung
public class Samsung : Celular
{
    public bool SPenCompativel { get; private set; }

    public Samsung(string modelo, int anoFabricacao, double tamanhoTela, 
                  int memoriaRAM, int armazenamento, bool sPenCompativel)
        : base("Samsung", modelo, anoFabricacao, "Android", tamanhoTela, memoriaRAM, armazenamento)
    {
        SPenCompativel = sPenCompativel;
    }

    public override void Ligar()
    {
        Ligado = true;
        Console.WriteLine("Samsung: Iniciando sistema com animação personalizada...");
    }

    public override void Desligar()
    {
        Ligado = false;
        Console.WriteLine("Samsung: Desligando com confirmação...");
    }

    public override void TirarFoto()
    {
        base.TirarFoto();
        Console.WriteLine("Usando modo de câmera otimizado da Samsung...");
    }

    public override void MostrarConfiguracao()
    {
        base.MostrarConfiguracao();
        Console.WriteLine($"SPen Compatível: {(SPenCompativel ? "Sim" : "Não")}");
    }
}

// Implementação para celulares iPhone
public class IPhone : Celular
{
    public bool FaceID { get; private set; }

    public IPhone(string modelo, int anoFabricacao, double tamanhoTela, 
                 int memoriaRAM, int armazenamento, bool faceID)
        : base("Apple", modelo, anoFabricacao, "iOS", tamanhoTela, memoriaRAM, armazenamento)
    {
        FaceID = faceID;
    }

    public override void Ligar()
    {
        Ligado = true;
        Console.WriteLine("iPhone: Mostrando logo da Apple...");
    }

    public override void Desligar()
    {
        Ligado = false;
        Console.WriteLine("iPhone: Deslizando para desligar...");
    }

    public override void FazerLigacao(string numero)
    {
        if (!Ligado)
        {
            Console.WriteLine("O iPhone está desligado. Ligue-o primeiro.");
            return;
        }
        Console.WriteLine($"iPhone: Ligando para {numero} via FaceTime (se disponível)...");
    }

    public override void MostrarConfiguracao()
    {
        base.MostrarConfiguracao();
        Console.WriteLine($"FaceID: {(FaceID ? "Sim" : "Não")}");
    }
}

// Implementação para celulares Xiaomi
public class Xiaomi : Celular
{
    public bool Infravermelho { get; private set; }

    public Xiaomi(string modelo, int anoFabricacao, double tamanhoTela, 
                 int memoriaRAM, int armazenamento, bool infravermelho)
        : base("Xiaomi", modelo, anoFabricacao, "Android (MIUI)", tamanhoTela, memoriaRAM, armazenamento)
    {
        Infravermelho = infravermelho;
    }

    public override void Ligar()
    {
        Ligado = true;
        Console.WriteLine("Xiaomi: Iniciando MIUI com anúncios...");
    }

    public override void Desligar()
    {
        Ligado = false;
        Console.WriteLine("Xiaomi: Desligando com menu rápido...");
    }

    public override void MostrarConfiguracao()
    {
        base.MostrarConfiguracao();
        Console.WriteLine($"Controle por Infravermelho: {(Infravermelho ? "Sim" : "Não")}");
    }
}

// Programa principal
class Program
{
    static void Main(string[] args)
    {
        // Criando uma lista de celulares de diferentes marcas
        List<Celular> celulares = new List<Celular>
        {
            new Samsung("Galaxy S22", 2022, 6.1, 8, 128, true),
            new IPhone("13 Pro", 2021, 6.1, 6, 256, true),
            new Xiaomi("Redmi Note 11", 2022, 6.43, 6, 128, true)
        };

        // Demonstrando o polimorfismo
        foreach (var celular in celulares)
        {
            Console.WriteLine("\n=== Testando " + celular.Marca + " " + celular.Modelo + " ===");
            
            celular.MostrarConfiguracao();
            celular.Ligar();
            celular.FazerLigacao("123456789");
            celular.EnviarMensagem("987654321", "Olá, como vai?");
            celular.TirarFoto();
            celular.Desligar();
        }

        // Testando um celular específico
        Console.WriteLine("\n=== Teste Específico do Samsung ===");
        var meuSamsung = new Samsung("Galaxy Z Fold 4", 2022, 7.6, 12, 512, true);
        meuSamsung.Ligar();
        meuSamsung.TirarFoto();
        meuSamsung.MostrarConfiguracao();
        meuSamsung.Desligar();
    }
}
