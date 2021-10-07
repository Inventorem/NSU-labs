#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define min 2
#define max 16
//Структура для перевода из десятичной, целая и дробная части, вторая сдвинута по разрядам до целых умножением на scope
struct Dec                  {
    long long Integer;
    long long Fractional;
    long long int Scope;    };

// Получает на вход цифру или букву символом, возвращает целочисленный эквивалент
int Digit (char Symbol)                             {
    int Digit = 0;
    if (Symbol - '0' <= 9 && Symbol - '0' >= 0)     {
        Digit = (int)(Symbol - '0');                }
    if (Symbol >= 'A' && Symbol <= 'F')             {
        Digit = (int)(Symbol - 'A'+10);             }
    return Digit;                                   }

//Как следует из названия, обратная прошлой функции, возвращает символ, а не число
char ReverseDigit (int Digit)             {
    char x=0;
    if (Digit <= 9 && Digit >= 0)         {
        x = (char)(Digit + (int)'0');     }
    if (Digit <= 15 && Digit >= 10)       {
        x = (char)(Digit + (int)'A'-10);  }
    return x;                             }

//Проверка оснований
int FirstCheck(int FirstBase, int SecondBase)                                    {
    if (FirstBase>max|| FirstBase < min || SecondBase > max || SecondBase < min) {
        return 1;                                                                }
    else {return 0;}                                                             }

//Вторая проверка на корректность ввода и соответствие первой системе счисления
int SecondCheck(int FirstBase, char * String)
{
    int DotIndex = -1;
    int dots = 0;
    char * Alphabet = "1234567890ABCDEF.\0";
    for (int i = 0; i < (int)strlen(String); i++) {
        if (String[i]=='.')
        {
            dots++;
            DotIndex = i;
        }
        else if (Digit(String[i]) >= 0 && Digit(String[i]) <16)
        {
            if (Digit(String[i]) >= FirstBase)
            {
                return 1;
            }
        }
        if (DotIndex == 0 || DotIndex == (int)(strlen(String)-1) || dots > 1)
        {
            return 1;
        }
        int flag = 1;
        int k = 0;
        while (k < (int)strlen(Alphabet))           //Проверка на недопустимые символы
        {
            if (String[i] == Alphabet[k])
            {
                flag = 0;
                break;
            }
            k++;
        }
        if (flag == 1)
        {
            return 1;
        }
    }
    return 0;
}

//Из строки ввода переводит в десятичную запись, возвращает целую и дробные части, вторую домноженной до целых
struct Dec XtoDec (char * string, int FirstBase) {
    long long Integer = 0;
    long double Fractional = 0;
    int IntegerAmount;
    int FractionalAmount;
    if (strchr(string, '.') == NULL)
    {
        IntegerAmount = (int)strlen(string);
        FractionalAmount = 0;
    }
    else
    {
        IntegerAmount = (int)(strchr(string,'.') - string);
        FractionalAmount = (int)(strlen(string) - (strchr(string,'.')-string));
    }
    if (strchr(string, '.') - string == 1 && string[0] == '0')
    {
        FractionalAmount = (int)strlen(string);
    }

    if (IntegerAmount !=0)
    {
        for (int i = 0; i < IntegerAmount; i++) {
            Integer = Integer * FirstBase + Digit(string[i]);
        }
    }
    if (FractionalAmount !=0)
    {
        for (int i = (int)(strlen(string) - 1); i > strchr(string, '.') - string; i--)
        {
            Fractional = (Fractional + Digit(string[i]));
            Fractional /= FirstBase;
        }
    }

    long long int FractionalScoped = 0;
    long long int scope = 100000000000;
    if (Fractional !=0)
    {
        FractionalScoped = (long long int)(Fractional * scope);
    }
    struct Dec answer = {Integer, FractionalScoped, scope};
    return answer;
}

//Перевод целой части из десятичной во вторую систему
char * DecToSecondInt (long long int Integer, int SecondBase)
{
    char BufferInt[100];
    int Index = 0;
    long long Number = Integer;
    while (Number / SecondBase)         //Считаю остатки(цифры в новой системе)
    {
        int x = (int)(Number % SecondBase);
        Number /= SecondBase;
        BufferInt[Index++] = ReverseDigit(x);
    }
    BufferInt[Index++]= (char)(ReverseDigit((int)Number)); //Превращаю в человеческую строку
    BufferInt[Index++]='\0';
    char * OutputInt;                    //Копирую в выходной массив с выделением памяти, проверяю malloc
    if ((OutputInt = malloc(sizeof(char)*Index)) == NULL) {
        perror("Обидно, что я живу в стране с пустыми строками ©\n  Джейсон Стетэм\n");
    }
    strcpy(OutputInt,BufferInt);
    int i = 0;                          //Переставляю цифры в обратном порядке
    int j = (int)(strlen(OutputInt)-1);
    while(i < j) {
        char temp = OutputInt[i];
        OutputInt[i++] = OutputInt[j];
        OutputInt[j--] = temp;
    }
    return OutputInt;
}

//Переводит во вторую систему, возвращает часть строки вывода после точки
char * DecToSecondFractional (long long Fractional, long long int scope, int SecondBase)
{   //Если дробной части не было, навешиваем ноль и выходим из функции
    if (Fractional == 0)
    {
        char *nullSrt = (char *) malloc(2 * sizeof (char));
        strcpy(nullSrt, "0");
        return nullSrt;
    }
    char BufferFrac[14];
    long long NumberFrac = Fractional;
    int i = 0;
    while ( i < 13)        //Считаю в цикле цифры в новой системе
    {
        long long int a = NumberFrac * SecondBase ;
        int x = (int)(a / scope);
        BufferFrac[i++] = ReverseDigit(x);
        NumberFrac = NumberFrac*SecondBase - (a/scope)*scope;
        if (NumberFrac == 0)
        {
            break;
        }
    }
    BufferFrac[i++] = '\0';
    char * OutputFrac;      //На всякий проверяю, как отработал malloc
    if ((OutputFrac = (char *)malloc(sizeof(char) * i)) == NULL) {
        perror("Обидно, что я живу в стране с пустыми строками ©\n  Джейсон Стетэм\n");
    }
    strcpy(OutputFrac,BufferFrac);
    return OutputFrac;
}

int main()
{
    char InputString[14];
    int FirstSystem; int SecondSystem;
    if ((scanf("%d%d%13s", &FirstSystem, &SecondSystem, InputString))!=3)  //Считывание, проверка ввода
    {
        return 1;
    }
    if (FirstCheck(FirstSystem,SecondSystem)) //Запускаю первую проверку
    {
        printf("bad input");
        return 0;
    }
    int length = (int)strlen(InputString);
    for (int i = 0; i < length; ++i)            //Делаю все буквы прописными
    {
        InputString[i] = toupper((unsigned) InputString[i]);
    }
    if (SecondCheck(FirstSystem,InputString))
    {
        printf("bad input");
        return 0;
    }
    struct Dec DecView = XtoDec(InputString,FirstSystem);       //Три строки подсчетов для перевода числа
    char * OutputInteger = DecToSecondInt(DecView.Integer, SecondSystem);
    char * OutputFractional = DecToSecondFractional(DecView.Fractional,DecView.Scope,SecondSystem);
    printf("%s.%s",OutputInteger,OutputFractional); //Вывод строки с числом
    free(OutputInteger);        //Освобождаю память после malloc'а
    free(OutputFractional);
    return 0;
}
