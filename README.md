# Aplica-o-de-cloud-Iot-em-python
Iot 
//inclusão de biblioteca e objetos do módulo relógio
#include <DS3231.h>  
DS3231  rtc(SDA, SCL);
Time t;

//parâmetros de horário que serão atualizados
int horaAtual, minutoAtual;

//parâmetros primeira alimentação
int horaAlimentacao1, minutoAlimentacao1, demosComida1;

//parâmetros segunda alimentação
int horaAlimentacao2, minutoAlimentacao2, demosComida2;

void setup()
{ 
  
 //inicia o módulo relógio
 rtc.begin(); 
 //inicia o monitor serial
 Serial.begin(115200); 

 //determina o pino do relé
 pinMode(8, OUTPUT); 

 //determina o horário da primeira alimentação
 horaAlimentacao1 = 8; 
 minutoAlimentacao1 = 30;

 //determina o horário da segunda alimentação
 horaAlimentacao2 = 18; 
 minutoAlimentacao2 = 30;

 //determina o status de alimentação. 0 equivale a não e 1 a sim
 demosComida1 = 0;   
 demosComida2 = 0; 
 
  //as linhas abaixo devem ser descomentadas para configurar o relógio interno. Descomente, carregue o código para o arduino, comente novamente e suba o código mais uma vez.
  // rtc.setDate(7,5,2019);    // determina a data (dia, mes, ano)
  // rtc.setDOW(TUESDAY);     // determina o dia da semana
  // rtc.setTime(15,41, 0);     // determina o horário (hora, minuto, segundo)

 // desliga o relé para começar.
 digitalWrite(8, HIGH);


} 

void loop()
{
  //determina o horário atual
  t = rtc.getTime();
  horaAtual = t.hour;
  minutoAtual = t.min;

 
  //verifica se é o horário da primeira alimentação
  if (horaAtual == horaAlimentacao1 && minutoAtual == minutoAlimentacao1 && demosComida1 == 0 ){

    digitalWrite(8, LOW);
    delay(20000);
    digitalWrite(8, HIGH);
    demosComida1 = 1; //altera status da comida1
    
  }

  //verifica se é o horário da segunda alimentação
  if (horaAtual == horaAlimentacao2 && minutoAtual == minutoAlimentacao2 && demosComida2 == 0 ){

    digitalWrite(8, LOW);
    delay(20000);
    digitalWrite(8, HIGH);
    demosComida2 = 1; //altera status da comida2
    
  }

  //Imprime o horário da próxima alimentação
  if (demosComida1 == 0 || demosComida1 == 1 && demosComida2 == 1 ){
    Serial.print("Horário atual: ");
    Serial.println(rtc.getTimeStr());
    Serial.print("Próxima alimentação: ");
    Serial.print(horaAlimentacao1);
    Serial.print("h:");
    Serial.print(minutoAlimentacao1);
    Serial.println("min");
    Serial.println("  ");
  }

  if (demosComida1 == 1 && demosComida2 == 0){
     Serial.print("Horário atual: ");
    Serial.println(rtc.getTimeStr());
    Serial.print("Próxima alimentação: ");
    Serial.print(horaAlimentacao2);
    Serial.print("h:");
    Serial.print(minutoAlimentacao2);
    Serial.println("min");
    Serial.println("  ");
  }
 
  //meia noite reseta o status de comida do dia 
  if (horaAtual == 0 && minutoAtual == 0){
    demosComida1 = 0;
    demosComida2 = 0; 
  }
 
  //atualiza monitor serial
  delay (1000);
  
} 

