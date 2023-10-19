# - Enlace al circuito de Tinkercat (Segunda Parte):
<p>link del proyecto: <a href=https://www.tinkercad.com/things/hFSaITErf7I?sharecode=878SWXr9QUDC1B1SzO5B_KCgYc7ymc55bTR-hZGb7DA>PARTE 2</a></p>

# - Enlace al PDF con las consignas📑:
<p>Link del pdf: <a href="https://drive.google.com/file/d/1iMSGl6bfeqBlWUpIrIHQhtZe7RmZ900B/view?usp=sharing">PDF PARTE 2</p>

# -.2 Investiga un componente electrónico adicional que podría mejorar este proyecto.
<h2>Proporciona una descripción detallada del componente, su función y cómo se podría
integrar en el proyecto.</h2>
<p> -La funcionalidad del motor de CC en Arduino implica el control de la velocidad y la dirección del motor. <br>
Esto se puede lograr utilizando un puente H o un driver de motor, que permite controlar la polaridad de la corriente que fluye a través del motor para determinar la dirección del giro, así como el control de la magnitud de la corriente para ajustar la velocidad del motor.<br><br>
El motor de corriente continua (CC) es un componente que se utiliza comúnmente con Arduino y otros microcontroladores para controlar el movimiento de objetos, como ruedas en robots, persianas, ventiladores, etc.<br>
El motor de CC es un componente fundamental en muchas aplicaciones de robótica y automatización controladas por Arduino.
</p>

# - Codigo del proyecto: 
```
// integrantes: AGUSTIN LANDEIRA, IARA PASQUARIO Y IVO BARINSTEIN
//Explicación del código:

//Se definen los pines para los segmentos del display y los botones
#define A 12
#define B 13
#define C 7
#define D 8
#define E 9
#define F 11
#define G 10

//Luego definimos las constantes para los segmentos y botones
//(incluyendo el tiempo de visualización de cada número)
#define SUBE 4
#define BAJA 3
#define RESET 5
#define UNIDAD A4
#define DECENA A5
#define APAGADOS 0
#define TIMEDISPLAYON 10

//Aqui se declaran las variables que se van a usar:
//countdigit: almacena el número que se muestra en el display.
//sube, baja y reset:  leer el estado de los botones.
int countdigit = 0;
int sube = 1;
int subprevia = 1;
int baja = 1;
int bajaprevia = 1;
int reset = 1;
int resetprevia = 1;

//En la función setup, se configuran los pines de entrada y salida.
// Luego se inicializan los digitos UNIDAD y DECENA como apagados,y se
//muestra el número "0" en la pantalla al comienzo.
void setup()
{
    pinMode(3, INPUT_PULLUP);
    pinMode(4, INPUT_PULLUP);
    pinMode(5, INPUT_PULLUP);
    pinMode(6, OUTPUT);
    pinMode(7, OUTPUT);
    pinMode(8, OUTPUT);
    pinMode(9, OUTPUT);
    pinMode(10, OUTPUT);
    pinMode(11, OUTPUT);
    pinMode(12, OUTPUT);
    pinMode(13, OUTPUT);
    pinMode(UNIDAD, OUTPUT);
    pinMode(DECENA, OUTPUT);
    digitalWrite(UNIDAD,0);
    digitalWrite(DECENA,0);
    printDigit(0);

    Serial.begin(9600);
}

//En la función loop, se lee si se presionó uno de los botones:
void loop()
{
    int pressed = keypress();// revisa si se presiono una tecla y tambien que no estaba presionada desde antes

    if (pressed == SUBE)
    {

        countdigit++;
        if (countdigit > 99)
        {
            countdigit = 0;
        }

    }
    else if( pressed == BAJA)
    {
        countdigit--;
        if (countdigit < 0)
        {
            countdigit = 99;
        }

    }
    else if (pressed == RESET)
    {

        countdigit = 0;
    }

    printCount(countdigit);
    delay(TIMEDISPLAYON);

}

//Usando la función keypress: si se presiona SUBE, se incrementa
//el número countdigit, si se presiona BAJA, se disminuye,
//y si se presiona RESET, se reinicia a 0.
int keypress(void)
{

    sube = digitalRead(SUBE);// lee el estado de la variable y si no lo presiono devuelve uno
    baja = digitalRead(BAJA);// si presiono me devuelve 0
    reset = digitalRead(RESET);

    if(sube == 1)  // no presione sube
    {
        subprevia = 1; // entonces antes tampoco estaba presionada
    }
    if(baja == 1)
    {
        bajaprevia = 1;
    }
    if(reset == 1)
    {

        resetprevia = 1;
    }

    if(sube == 0 && sube != subprevia)
    {
        subprevia = sube;// hago que el estado anterior tenga el mismo valor
        return SUBE;

    }

    if(baja == 0 && baja != bajaprevia)
    {
        bajaprevia = baja;
        return BAJA;

    }

    if(reset == 0 && reset != resetprevia)
    {
        resetprevia = reset;
        return RESET;

    }

    return 0; // si no presione nada o una que ya estaba presionada

}
//Luego se muestra el número actual en la pantalla usando
//la función printCounty, introduciendose un delay antes de mostrar el número siguiente.
void printCount(int count)
{
    prendeDigito(APAGADOS); // apago los dos
    printDigit(count/10);
    prendeDigito(DECENA);
    prendeDigito(APAGADOS);
    printDigit(count - 10*((int)count/10));
    prendeDigito(UNIDAD);


}

//La función prendeDigito enciende uno de los dígitos
//del display (UNIDAD o DECENA) y apaga el otro
void prendeDigito(int digito)
{

    if (digito == UNIDAD)
    {
        digitalWrite(UNIDAD,LOW); // pongo el unidad en 0(enciende)
        digitalWrite(DECENA,HIGH);// pongo el comun en 1(no enciende)
        delay(TIMEDISPLAYON);

    }
    else if (digito == DECENA)
    {

        digitalWrite(DECENA,LOW);
        digitalWrite(UNIDAD,HIGH);
        delay(TIMEDISPLAYON);
    }
    else
    {
        digitalWrite(DECENA,HIGH); // no prende ninguno de los dos
        digitalWrite(UNIDAD,HIGH);
    }

}

//En la funcion printDigity se definen los patrones de
//segmentos para cada dígito del 0 al 9 y se encienden  los
// segmentos del display según el dígito que se quiera mostrar.

void printDigit(int digit)
{
    apagar();
    switch(digit)
    {

    case 0:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);

        break;
    case 1:
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;

    case 2:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(G, HIGH);
        break;
        break;

    case 3:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 4:
        digitalWrite(B,HIGH);
        digitalWrite(C,HIGH);
        digitalWrite(F,HIGH);
        digitalWrite(G,HIGH);
        break;

    case 5:
        digitalWrite(A, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 6:
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 7:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;

    case 8:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 9:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);

        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;


        break;
    }
}
//La función apagarse utiliza para apagar todos los segmentos
//antes de mostrar un nuevo dígito en la pantalla
void apagar()
{

    digitalWrite(A,LOW);
    digitalWrite(B,LOW);
    digitalWrite(C,LOW);
    digitalWrite(D,LOW);
    digitalWrite(E,LOW);
    digitalWrite(F,LOW);
    digitalWrite(G,LOW);
// PARTE 2
// integrantes: AGUSTIN LANDEIRA, IARA PASQUARIO Y IVO BARINSTEIN
//Explicación del código:

//Se definen los pines para los segmentos del display y los botones
# define A 12
# define B 13
# define C 7
# define D 8
# define E 9
# define F 11
# define G 10

//Se definen los leds
# define LED_1 6
# define LED_2 2
# define TEMPERATURA A0

//Luego definimos las constantes para los segmentos y botones
//(incluyendo el tiempo de visualización de cada número)
# define SUBE 4
# define BAJA 3
# define SWITCH 5
# define UNIDAD A4
# define DECENA A5
# define APAGADOS 0
# define TIMEDISPLAYON 10

//Aqui se declaran las variables que se van a usar:
//countdigit: almacena el número que se muestra en el display.
//sube, baja y reset:  leer el estado de los botones.
int countdigit = 0;
int sube = 1;
int subprevia = 1;
int baja = 1;
int bajaprevia = 1;
int reset = 1;
int resetprevia = 1;
int interruptor = 1;
int switch_previa = 1;
bool flagMostrar = true;
int leerTemperatura = 0;

//En la función setup, se configuran los pines de entrada y salida.
// Luego se inicializan los digitos UNIDAD y DECENA como apagados,y se
//muestra el número "0" en la pantalla al comienzo.
void setup()
{
    pinMode(3, INPUT_PULLUP);
    pinMode(4, INPUT_PULLUP);
  	pinMode(5, OUTPUT);
    pinMode(6, OUTPUT);
    pinMode(7, OUTPUT);
    pinMode(8, OUTPUT);
    pinMode(9, OUTPUT);
    pinMode(10, OUTPUT);
    pinMode(11, OUTPUT);
    pinMode(12, OUTPUT);
    pinMode(13, OUTPUT);
    pinMode(LED_1, OUTPUT);
    pinMode(LED_2, OUTPUT);
    pinMode(UNIDAD, OUTPUT);
    pinMode(DECENA, OUTPUT);
    digitalWrite(UNIDAD,0);
    digitalWrite(DECENA,0);
    printDigit(0);

    Serial.begin(9600);
}

//En la función loop, se lee si se presionó uno de los botones:
void loop()
{
    int pressed = keypress();// revisa si se presiono una tecla y tambien que no estaba presionada desde antes
    interruptor = digitalRead(SWITCH);// Lee el estado del interruptor (el pin SWITCH) para determinar si está encendido (valor 1) o apagado (valor 0)
    
  	//Se lee la temperatura analógica del sensor  mediante analogRead. 
  	leerTemperatura = analogRead(TEMPERATURA);
  	
  	//Se usa la funcion map para convertir esta lectura  en un rango de temperaturas  entre -40 y 125 grados Celsius
    leerTemperatura = map(leerTemperatura, 20, 358,-40, 125);
	calcularTemperatura(leerTemperatura); 
  
 	// Código para aumentar el número mostrado en el display
    if (pressed == SUBE && interruptor == 1)
    {
		// Se verifica si el interruptor está encendido y si presiono el botón "SUBE"
		// Si es así, se aumenta el número mostrado en el display y se verifica si se ha alcanzado el límite máximo.
    
      if(switch_previa == 0)
      {
      	switch_previa = 1;
        countdigit = 0;
      }
      //Se imprime un mensaje en el puerto serie y se ajusta
      //la bandera flagMostrar.
      
        flagMostrar = true;
        Serial.println("Aumentar");
        countdigit++;
        
        if (countdigit > 99)
        {
            countdigit = 0;
        }
    }
	
	// Código para disminuir el número mostrado en el display
    //Similar al caso anterior, pero se disminuye el numero y se verifica el límite minimo

    else if( pressed == BAJA && interruptor == 1)
    {
      if(switch_previa == 0)
      {
      	switch_previa = 1;
        countdigit = 0;
      }
        flagMostrar = true;
        Serial.println("Restar");
        countdigit--;
        if (countdigit < 0)
        {
            countdigit = 99;
        }
        switch_previa = 1;
    }
	
	// Se aumenta el número mostrado en el display si el interruptor está apagado
    if(pressed == SUBE && interruptor == 0)
    {
		// El comportamiento es diferente cuando el interruptor está apagado.
		// Se incrementa el número si el interruptor no estaba apagado antes.
        if(switch_previa == 1)
        {
            switch_previa = 0;
            countdigit = 0;
            flagMostrar = false;
        }

        if(switch_previa == 0)
        {
            countdigit++;
        }
		//se verifica si el número resultante es primo.
        if(esPrimo(countdigit) == 1)
        {
            flagMostrar = true;
            Serial.println("\nEs primo");
        }
        else
        {
            Serial.println("\nNo es primo");
            flagMostrar = false;
        }
    }
	// Se disminuye el número mostrado en el display si el interruptor está apagado
    else if(pressed == BAJA && interruptor == 0)
    {	// Similar al caso anterior, pero se disminuye el número.
        if(switch_previa == 1)
        {
            switch_previa = 0;
            countdigit = 0;
            flagMostrar = false;
        }

        if(switch_previa == 0)
        {
            countdigit--;
        }
		// Y se verifica si es primo.
        if(esPrimo(countdigit) == 1)
        {
            flagMostrar = true;
            Serial.println("\nEs primo");
        }
        else
        {
            Serial.println("\nNo es primo");
            flagMostrar = false;
        }
    }

    if(flagMostrar)
    {
        printCount(countdigit); // Muestra el número actual en el display
        delay(TIMEDISPLAYON);// Espera durante un tiempo antes de mostrar el siguiente número.
    }
    else
    {
        apagar();// Apaga todos los segmentos del display
        delay(TIMEDISPLAYON); // Espera durante un tiempo antes de apagar el display.
    }
}

//Usando la función keypress: si se presiona SUBE, se incrementa
//el número countdigit, si se presiona BAJA, se disminuye,
//y si se presiona RESET, se reinicia a 0.
int keypress(void)
{

    sube = digitalRead(SUBE);// lee el estado de la variable y si no lo presiono devuelve uno
    baja = digitalRead(BAJA);// si presiono me devuelve 0

    if(sube == 1)  // no presione sube
    {
        subprevia = 1; // entonces antes tampoco estaba presionada
    }
    if(baja == 1)
    {
        bajaprevia = 1;
    }

    if(sube == 0 && sube != subprevia)
    {
        subprevia = sube;// hago que el estado anterior tenga el mismo valor
        return SUBE;

    }

    if(baja == 0 && baja != bajaprevia)
    {
        bajaprevia = baja;
        return BAJA;

    }
    return 0; // si no presione nada o una que ya estaba presionada
}
//Luego se muestra el número actual en la pantalla usando
//la función printCounty, introduciendose un delay antes de mostrar el número siguiente.
void printCount(int count)
{
    prendeDigito(APAGADOS); // apago los dos
    printDigit(count/10);
    prendeDigito(DECENA);
    prendeDigito(APAGADOS);
    printDigit(count - 10*((int)count/10));
    prendeDigito(UNIDAD);


}

//La función prendeDigito enciende uno de los dígitos
//del display (UNIDAD o DECENA) y apaga el otro
void prendeDigito(int digito)
{

    if (digito == UNIDAD)
    {
        digitalWrite(UNIDAD,LOW); // pongo el unidad en 0(enciende)
        digitalWrite(DECENA,HIGH);// pongo el comun en 1(no enciende)
        delay(TIMEDISPLAYON);

    }
    else if (digito == DECENA)
    {

        digitalWrite(DECENA,LOW);
        digitalWrite(UNIDAD,HIGH);
        delay(TIMEDISPLAYON);
    }
    else
    {
        digitalWrite(DECENA,HIGH); // no prende ninguno de los dos
        digitalWrite(UNIDAD,HIGH);
    }

}

//En la funcion printDigit se definen los patrones de
//segmentos para cada dígito del 0 al 9 y se encienden  los
// segmentos del display según el dígito que se quiera mostrar.

void printDigit(int digit)
{
    apagar();
    switch(digit)
    {

    case 0:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);

        break;
    case 1:
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;

    case 2:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(G, HIGH);
        break;
        break;

    case 3:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 4:
        digitalWrite(B,HIGH);
        digitalWrite(C,HIGH);
        digitalWrite(F,HIGH);
        digitalWrite(G,HIGH);
        break;

    case 5:
        digitalWrite(A, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 6:
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 7:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;

    case 8:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 9:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);

        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;


        break;
    }
}
//La función apagarse utiliza para apagar todos los segmentos
//antes de mostrar un nuevo dígito en la pantalla
void apagar()
{

    digitalWrite(A,LOW);
    digitalWrite(B,LOW);
    digitalWrite(C,LOW);
    digitalWrite(D,LOW);
    digitalWrite(E,LOW);
    digitalWrite(F,LOW);
    digitalWrite(G,LOW);

}

int esPrimo(int numero)
{
    int retorno = 1;
    /*
    El número 4 es un caso especial, pues al dividirlo entre
    2 el resultado es 2, y el ciclo nunca se cumple, indicando que
    el 4 SÍ es primo, pero realmente NO lo es, así que si el número es 4
    inmediatamente indicamos que no es primo (regresando 0)
    Nota: solo es para el 4, los demás funcionan bien
    */
    if (numero == 0 || numero == 1 || numero == 4)
    {
        retorno = 0;
    }

    for (int x = 2; x < numero / 2; x++)
    {
        // Si es divisible por cualquiera de estos números, no
        // es primo
        if (numero % x == 0)
        {
            retorno = 0;
        }
    }
    // Si no se pudo dividir por ninguno de los de arriba, sí es primo
    return retorno;
}

//La función calcularTemperaturacontrola los LED según la temperatura 
//leída y ajusta los LED_1 y LED_2 según el valor de la temperatura.
void calcularTemperatura(int temperaturaLeida)
{
	//Si es menor a 20 grados celcius
    if(temperaturaLeida < 20)
    {
      //se prende el led 1
        digitalWrite(LED_1,HIGH);
      // y se apaga el led 2
        digitalWrite(LED_2,LOW);
    }
    else //sino es menor a 20 grados
    {
      //se prende el led 2
        digitalWrite(LED_2,HIGH);
      //y se apaga el led 1
        digitalWrite(LED_1,LOW);
    }
}
```
