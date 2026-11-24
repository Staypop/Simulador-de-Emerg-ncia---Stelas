# Simulador-de-Emerg-ncia---Stelas

Um simulador interativo desenvolvido para treinar e capacitar pessoas a tomarem decisões rápidas e seguras diante de situações críticas. O projeto oferece cenários realistas baseados na web, funcionando como uma ferramenta de aprendizado prático e preventivo.

 Sobre o Projeto ::

Em situações de emergência, o tempo de reação e o conhecimento dos protocolos corretos salvam vidas. Este simulador foi criado com o objetivo de democratizar o acesso ao treinamento preventivo, permitindo que qualquer pessoa pratique como agir sob pressão diretamente do navegador, sem necessidade de instalação.

Principais Objetivos ::

• Educação Prática: Transformar manuais teóricos de segurança em experiências interativas.
• Tomada de Decisão: Avaliar o impacto das escolhas do usuário em tempo real.
• Acessibilidade: Plataforma 100% online, leve e responsiva.

 Funcionalidades ::

• Cenários Interativos: Simulações passo a passo (ex: incêndios, primeiros socorros, evacuação de prédios, desastres naturais).
• Sistema de Consequências: Cada escolha altera o rumo da simulação, mostrando os resultados positivos ou os riscos de decisões incorretas.
• Temporizador de Pressão: Desafios com tempo limitado para simular a urgência real de uma crise.
• Feedback Detalhado: Ao final de cada cenário, o usuário recebe uma análise do seu desempenho e dicas baseadas em normas oficiais de segurança.

Tecnologias Utilizadas ::

• HTML, CSS, JavaScript
• Github, Tinkercad, Canva

------------------------------------------------------------------------------------------------------------------

Código Usado no Tinkercad ::

// Definição dos pinos
const int LED_ESQUERDO = 12;
const int LED_DIREITO = 13;
const int BUZZER = 8;

// Variáveis de tempo para os LEDs (sem travar o código)
unsigned long tempoAnteriorLED = 0;
int estadoLED = 0;
int contadorPiscadas = 0;

// Variáveis de tempo para o S.O.S. (3 curtos, 3 longos, 3 curtos)
// Valores em milissegundos: positivo = som ligado, negativo = pausa
int sequenciaSOS[] = {
  150, -150, 150, -150, 150, -400, // S (3 curtos) + pausa entre letras
  450, -150, 450, -150, 450, -400, // O (3 longos) + pausa entre letras
  150, -150, 150, -150, 150, -1500 // S (3 curtos) + pausa longa no final
};
int tamanhoSOS = 18;
int passoSOS = 0;
unsigned long tempoAnteriorSOS = 0;

void setup() {
  pinMode(LED_ESQUERDO, OUTPUT);
  pinMode(LED_DIREITO, OUTPUT);
  pinMode(BUZZER, OUTPUT);
}

void loop() {
  unsigned long tempoAtual = millis();

  // ==========================================
  // 1. CONTROLE DOS LEDS (Efeito Estroboscópico)
  // ==========================================
  if (estadoLED == 0 && tempoAtual - tempoAnteriorLED >= 50) {
    digitalWrite(LED_ESQUERDO, HIGH);
    tempoAnteriorLED = tempoAtual;
    estadoLED = 1;
  }
  else if (estadoLED == 1 && tempoAtual - tempoAnteriorLED >= 50) {
    digitalWrite(LED_ESQUERDO, LOW);
    tempoAnteriorLED = tempoAtual;
    contadorPiscadas++;
   
    if (contadorPiscadas >= 3) {
      contadorPiscadas = 0;
      estadoLED = 2; // Vai para a pausa entre lados
    } else {
      estadoLED = 0; // Repete o LED esquerdo
    }
  }
  else if (estadoLED == 2 && tempoAtual - tempoAnteriorLED >= 100) {
    tempoAnteriorLED = tempoAtual;
    estadoLED = 3; // Inicia o LED direito
  }
  else if (estadoLED == 3 && tempoAtual - tempoAnteriorLED >= 50) {
    digitalWrite(LED_DIREITO, HIGH);
    tempoAnteriorLED = tempoAtual;
    estadoLED = 4;
  }
  else if (estadoLED == 4 && tempoAtual - tempoAnteriorLED >= 50) {
    digitalWrite(LED_DIREITO, LOW);
    tempoAnteriorLED = tempoAtual;
    contadorPiscadas++;
   
    if (contadorPiscadas >= 3) {
      contadorPiscadas = 0;
      estadoLED = 5; // Vai para a pausa final
    } else {
      estadoLED = 3; // Repete o LED direito
    }
  }
  else if (estadoLED == 5 && tempoAtual - tempoAnteriorLED >= 100) {
    tempoAnteriorLED = tempoAtual;
    estadoLED = 0; // Reinicia o ciclo dos LEDs
  }

  // ==========================================
  // 2. CONTROLE DO BUZZER (Sinal S.O.S.)
  // ==========================================
  int duracaoAtual = abs(sequenciaSOS[passoSOS]);

  if (tempoAtual - tempoAnteriorSOS >= duracaoAtual) {
    tempoAnteriorSOS = tempoAtual;
    passoSOS++;
   
    if (passoSOS >= tamanhoSOS) {
      passoSOS = 0; // Reinicia o S.O.S.
    }

    // Se o valor na lista for positivo, toca o som; se for negativo, silencia
    if (sequenciaSOS[passoSOS] > 0) {
      tone(BUZZER, 1000); // Apita na frequência de 1000Hz
    } else {
      noTone(BUZZER);     // Desliga o som
    }
  }
}

------------------------------------------------------------------------------------------------------------------

Canva: https://canva.link/b4zmmlgw75dulix
Codepen.io: https://codepen.io/editor/Yasmin-Gabrielle-the-decoder/pen/01a1ccc9-6a6c-73df-aa07-80870d06acb2

------------------------------------------------------------------------------------------------------------------

Integrantes: Ana Júlia, Maria Eduarda Oliveira, Maria Eduarda Souza, Yasmin Gabrielle.
