<?php


class Carta {
  
    private $numeroCarta;
    private $nomeCarta;
    private $dicaCarta;
    
  
    public function __construct($numero, $nome, $dica) {
        $this->numeroCarta = $numero;
        $this->nomeCarta = $nome;
        $this->dicaCarta = $dica;
    }
    

    public function pegarNumero() {
        return $this->numeroCarta;
    }
    
    public function pegarNome() {
        return $this->nomeCarta;
    }
    
    public function pegarDica() {
        return $this->dicaCarta;
    }
    
   
    public function mudarNumero($novo_numero) {
        $this->numeroCarta = $novo_numero;
    }
    
    public function mudarNome($novo_nome) {
        $this->nomeCarta = $novo_nome;
    }
    
    public function mudarDica($nova_dica) {
        $this->dicaCarta = $nova_dica;
    }
}


$meuBaralho = array(
    new Carta(1, "Ás de Copas", "É a carta mais baixa ou mais alta, dependendo do jogo"),
    new Carta(2, "Dois de Ouros", "Tem o menor número possível"),
    new Carta(3, "Três de Espadas", "Número pequeno"),
    new Carta(4, "Quatro de Paus", "Número médio baixo"),
    new Carta(5, "Cinco de Copas", "Número do meio"),
    new Carta(6, "Seis de Ouros", "Número médio alto"),
    new Carta(7, "Sete de Espadas", "Número da sorte para alguns"),
    new Carta(8, "Oito de Paus", "Número intermediário"),
    new Carta(9, "Nove de Copas", "Número alto"),
    new Carta(10, "Dez de Ouros", "Valor redondo, perfeito para contar"),
    new Carta(11, "Valete de Espadas", "Representa um soldado"),
    new Carta(12, "Dama de Paus", "Representa uma figura feminina"),
    new Carta(13, "Rei de Copas", "O mais valioso do baralho")
);


$indiceCorteado = rand(0, count($meuBaralho) - 1);
$cartasorteada = $meuBaralho[$indiceCorteado];


$pontosDoJogador = 100; 
$quantidadeTentativas = 0;
$acertos = false;


echo "#############################################\n";
echo "#    JOGO DE ADIVINHAR A CARTA SORTEADA     #\n";
echo "#############################################\n\n";

echo "REGRAS DO JOGO:\n";
echo "1. Você começa com 100 pontos\n";
echo "2. A cada erro, perde 20 pontos\n";
echo "3. O jogo acaba quando acertar ou chegar a 0 pontos\n\n";


echo "DICA SOBRE A CARTA SORTEADA:\n";
echo "----------------------------\n";
echo $cartasorteada->pegarDica() . "\n\n";


echo "CARTAS DISPONÍVEIS NO BARALHO:\n";
echo "------------------------------\n";
foreach ($meuBaralho as $cartaIndividual) {
    echo "Número: " . $cartaIndividual->pegarNumero() . " - Nome: " . $cartaIndividual->pegarNome() . "\n";
}
echo "\n";


while (!$acertos && $pontosDoJogador > 0) {
    echo "=====================================\n";
    echo "SUA PONTUAÇÃO ATUAL: " . $pontosDoJogador . " pontos\n";
    echo "TENTATIVAS: " . $quantidadeTentativas . "\n";
    echo "-------------------------------------\n";
    echo "Digite o número da carta que você acha que foi sorteada\n";
    echo "(ou digite 0 para sair do jogo): ";
    

    $palpite = trim(fgets(STDIN));
    

    if ($palpite == 0) {
        echo "\nVocê decidiu sair do jogo!\n";
        echo "A carta sorteada era: " . $cartasorteada->pegarNome() . "\n";
        break;
    }
    

    if (!is_numeric($palpite)) {
        echo "\nERRO: Você deve digitar um número!\n\n";
        continue;
    }
    
    $palpite = (int)$palpite;
    

    $cartaDopalpite = null;
    foreach ($meuBaralho as $cartaDobaralho) {
        if ($cartaDobaralho->pegarNumero() == $palpite) {
            $cartaDopalpite = $cartaDobaralho;
            break;
        }
    }
    

    if ($cartaDopalpite === null) {
        echo "\nERRO: Não existe carta com esse número!\n";
        echo "Por favor, escolha um número entre 1 e 13.\n\n";
        continue;
    }
    
    $quantidadeTentativas++;
    

    if ($cartaDopalpite->pegarNumero() == $cartasorteada->pegarNumero()) {
        $acertos = true;
        echo "\n#############################################\n";
        echo "#           PARABÉNS! VOCÊ ACERTOU!         #\n";
        echo "#############################################\n";
        echo "A carta sorteada era realmente: " . $cartasorteada->pegarNome() . "\n";
        echo "Você precisou de " . $quantidadeTentativas . " tentativa(s).\n";
        echo "SUA PONTUAÇÃO FINAL: " . $pontosDoJogador . " pontos\n\n";
    } else {

        $pontosDoJogador = $pontosDoJogador - 20;
        if ($pontosDoJogador < 0) {
            $pontosDoJogador = 0;
        }
        
        echo "\nXXXXX QUE PENA! VOCÊ ERROU! XXXXX\n";
        

        if ($cartaDopalpite->pegarNumero() < $cartasorteada->pegarNumero()) {
            echo "DICA: A carta sorteada tem um número MAIOR que " . $cartaDopalpite->pegarNumero() . "\n";
        } else {
            echo "DICA: A carta sorteada tem um número MENOR que " . $cartaDopalpite->pegarNumero() . "\n";
        }
        
        echo "Tente novamente!\n\n";
    }
}


if (!$acertos & $pontosDoJogador <= 0) {
    echo "\n#############################################\n";
    echo "#         SUA PONTUAÇÃO CHEGOU A ZERO!      #\n";
    echo "#           INFELIZMENTE VOCÊ PERDEU!       #\n";
    echo "#############################################\n";
    echo "A carta sorteada era: " . $cartasorteada->pegarNome() . "\n\n";
}

echo "FIM DO JOGO!\n";
echo "Obrigado por jogar!\n";
echo "Desenvolvido por: [Seu Nome]\n";
?>
