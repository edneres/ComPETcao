/////////////////////////////////
//           UFPI              //
//           2022              //
// Equipe #include <coragem.h> //
/////////////////////////////////


#include <Arduino.h>

// Pino usado para disparar os pulsos do sensor
#define PinTRIG1  3 
#define PinTriG2  4
// Pino usado para ler a saida do sensor
#define PinECHO1  5 
#define PinECHO2  6
// Pino usado para disparar o motor de vibração (PWM)
const int PinVibra1 = 10; // PWM
const int PinVibra2 = 11; // PWM

double TempoEcho1 = 0;
double TempoEcho2 = 3;
 
// Em metros por microsegundo
const double VelocidadeSom_mporus = 0.000340; 

///////////////////////////
// Configuração do setup //
///////////////////////////

void setup()
{
  // Configuração do pino TRIG como saída
  // Inicializado em nível baixo
  pinMode(PinTRIG1, OUTPUT);
  pinMode(PinTriG2, OUTPUT);
  digitalWrite(PinTRIG1, LOW);
  digitalWrite(PinTriG2, LOW);
  
  // Configuração do pino ECHO como entrada
  pinMode(PinECHO1, INPUT); 
  pinMode(PinECHO2, INPUT); 

  // Inicializa a porta serial
  //Serial.begin(9600);
}

//////////////////////////////////////
// Função que envia o pulso de TRIG //
//////////////////////////////////////

void DisparaPulsoUltrassonico1()
{
  // Para fazer o HC-SR04 enviar um pulso ultrassônico, nós temos
  // que enviar para o pino de trigger um sinal de nível alto
  // com pelo menos 10us de duração
  digitalWrite(PinTRIG1, HIGH);
  delayMicroseconds(10);
  digitalWrite(PinTRIG1, LOW);
}

void DisparaPulsoUltrassonico2()
{
  // Para fazer o HC-SR04 enviar um pulso ultrassônico, nós temos
  // que enviar para o pino de trigger um sinal de nível alto
  // com pelo menos 10us de duração
  digitalWrite(PinTriG2, HIGH);
  delayMicroseconds(10);
  digitalWrite(PinTriG2, LOW);
}

//////////////////////////////////////////////
// Função que calcula a distância em metros //
//////////////////////////////////////////////

double CalculaDistancia(double tempo_us)
{
  return((tempo_us*VelocidadeSom_mporus)/2);
}

//////////////////////////////////////
// Função que controla as vibrações //
//////////////////////////////////////

void Vibracao(double distancia, int pino)
{
  /*
  Utilizamos a função map para transformar uma escalar de 0 a 200 cm
  em uma tensão de 5V a 3,7V - 3,7 V é a tensão mínima para funciona-
  mento do módulo de vibração. No Arduino, para saidas analógicas, o
  número 255 representa 5V e o número 188 representa 3,7V - 188 é ob-
  tido por regra de três simples: (3,7/5)*255 = 188.
  */
  
  double conversao = 0;
  conversao = map(distancia, 0, 200, 255, 188);
  analogWrite(pino, conversao);
}


//////////////////////
// FUNÇÃO PRINCIPAL //
//////////////////////

void loop()
{
  
  // Envia pulso para disparar os sensores
  DisparaPulsoUltrassonico1();
  
  // Mede o tempo de duração do sinal no pino de leitura(us)/entrada
  
  TempoEcho1 = pulseIn(PinECHO1, HIGH);

  // Codicional para ativar primeiro motor apenas se a distância para
  // uma barreira for menor que 2m. 
  if((CalculaDistancia(TempoEcho1)*100) <= 200) 
  {
    Vibracao(CalculaDistancia(TempoEcho1)*100, PinVibra1);
    //Serial.print("Distancia 1: ");
    //Serial.println(CalculaDistancia(TempoEcho1)*100);
    
  }

  else{
    Serial.println("Fora de Alcance 1!");
    digitalWrite(PinVibra1, LOW);
  }
  

//codicional para ativar segundo motor apenas se a distância para
//uma barreira for menor que 2m. 
  DisparaPulsoUltrassonico2();
  TempoEcho2 = pulseIn(PinECHO2, HIGH); 
  if((CalculaDistancia(TempoEcho2)*100) <= 200)
  {

    Vibracao(CalculaDistancia(TempoEcho2)*100, PinVibra2);
    //Serial.print("    Distancia 2: ");
    //Serial.println(CalculaDistancia(TempoEcho2)*100);
  }

  else{
    Serial.println("Fora de Alcance 2!");
    digitalWrite(PinVibra2, LOW);
  }
}
