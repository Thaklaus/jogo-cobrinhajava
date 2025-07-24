import javax.swing.*; // Para criar a janela do jogo
import java.awt.*; // Para desenhar gráficos na tela
import java.awt.event.*; // Para tratar eventos como pressionar teclas
import java.util.Random; // Para gerar posições aleatorias da comida

public class Cobrinha ActionListener {
    //Tela
    private static final int LARGURA_TELA = 1280; // Largura do jogo
    private static final int ALTURA_TELA = 720; // Altura do jogo
    private static final int TAMANHO_BLOCO = 50; // Tamanho de grades de cobra/comidas

    private static final int UNIDADES = LARGURA_TELA * ALTURA_TELA / (TAMANHO_BLOCO * TAMANHO_BLOCO); // Total de blocos possiveis na tela

    private static final int INTERVALO = 200; // Tempo de cada movimento da cobra em milisegundos
    private static final String NOME_FONTE = "Ink Free"; // Fonte usada para os textos (Fonte é o tipo de texto)

    //Variaveis da Cobra e do jogo
    private  final int[]  eixoX = new int[UNIDADES]; // Posições Horinzontais da cobra (X é horizontal e Y é vertiical)
    private  final int[]  eixoY = new int[UNIDADES]; // Posições Verticais da cobra

    private int corpoCobra = 6; // Começa com 6 partes
    private int blocosComidos; // Quantos blocos de comidas a cobra comeu (abaixo está a conta)

    private char direcao = 'D'; // Direção inicial (Direita) C = Para cima B = Baixo E = Esquerda
    private boolean  estaRodadando = false; // se o jogo esta rodando ou não

    Timer timer; // Controlar o tempo do jogo
    Random random; // Gerar posições aleatorias da comida

    Cobrinha() {
        random = new Random(); // Inicializa o gerador aleatório (Random = aleatório)
        setpreferredSize(new Dimension(LARGURA_TELA, ALTURA_TELA)); // Defina o tamanho da tela
        setBackground(Color.BLUE); // Cor de fundo da tela Vermelho
        setFocussable(true); // Recebe as informações das teclas do teclado
        addKeyListener(new LeitorDeTeclas()); // Ler as teclas
        iniciarJogo(); //
    }

    public  void iniciarJogo(){
        criarBloco(); // Criar a primeira comida
        estaRodadando = true;
        timer = new Timer(INTERVALO, this); // Criar o temporizador para remover a cobre
        timer.start(); // Comeca a contagem
    }

    @Override
    public void paintComponente(Graphics g){
        super.paintComponente(g); // Limpar a tela
        desenharTela(g); //  Chama metodo que desenha tudo
    }

    public void desenharTela(Graphics g) {
        if (estaRodadando) {
            g.setColor(Color.black); // Cor da comida
            g.fillOval(blockX, blockY, TAMANHO_BLOCO, TAMANHO_BLOCO); // Desenha a comida

            for (int i = 0; i < corpoCobra; i++){
                if (i == 0 ){
                    g.setColor(Color.red); // Pinta a cabeça da cobra
                } else {
                    g.setColor(new Color(45,180, 0)); // Cor do corpo da cobra
                }
                g.fillRect(eixoX[i],eixoY[i], TAMANHO_BLOCO, TAMANHO_BLOCO);
            }

            // Desenha a pontuação
            g.setColor(Color.BLUE);
            g.setFont(new font(NOME_FONTE, font.BOLD, 40));
            FontMetrics metrics = g.getFontMetrics(g.getFont());
            g.drawString("Pontos: " + blocosComidos);
        }
    }
}
