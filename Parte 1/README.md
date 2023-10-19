# - Enlace al circuito de Tinkercat (Primera Parte) 💻:
<p>Link del proyecto: <a href="https://www.tinkercad.com/things/7otP8gg8quv?sharecode=drkhtWnJcFmuPu_mF_jme56yLRQ5tE9Zrmg1zA8gEAA">PARTE 1</a></p>

# - Enlace al PDF con las consignas📑:
<p>Link del pdf: <a href="https://drive.google.com/file/d/1UTj8HBPnR7vM235m1BswtL_SMnmYe8nO/view?usp=sharing">PDF PARTE 1</p>

# - Enlace al PDF con las consignas📑:
<p>Link del pdf: <a href="https://drive.google.com/file/d/1UTj8HBPnR7vM235m1BswtL_SMnmYe8nO/view?usp=sharing">PDF PARTE 1</p>

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

}
```
